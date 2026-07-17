# HITS API Security Report

## 1. Report Objective
This document combines:
1. API security findings derived from the provided case study, dataset, and API documentation.

Goal: help security teams and developers move from finding -> explanation -> fix.

## 2. Context Summary
The HITS Connect API handles sensitive student information. Based on the provided evidence, the current API design enables unauthorized data exposure through broken authentication and authorization controls.

Primary risk theme: an attacker can access student records without proper identity checks, then expand access through weak session/token controls.

## 3. Key Findings and Python-Oriented Fix Guidance

### F-01 Broken Authentication on Student List Endpoint
- Evidence mapping: V-003
- Endpoint: GET /v1/students
- Issue: Endpoint is accessible without token validation.
- Risk: Enables bulk data exfiltration of student records.

#### High-level remediation
1. Require authentication middleware for all sensitive routes.
2. Restrict list endpoints to approved roles only.


#### Python snippet (FastAPI)
```python
from fastapi import FastAPI, Depends, HTTPException, Header
from jose import jwt, JWTError

app = FastAPI()

# Load these from environment or secret manager in production
JWT_PUBLIC_KEY = "<public-key>"
JWT_ALGORITHM = "RS256"
JWT_ISSUER = "hits-auth"
JWT_AUDIENCE = "hits-connect-api"


def get_current_user(authorization: str = Header(default="")):
    # Validate Authorization header format
    if not authorization.startswith("Bearer "):
        raise HTTPException(status_code=401, detail="Unauthorized")

    token = authorization.split(" ", 1)[1]

    try:
        # Strict JWT validation prevents token misuse
        payload = jwt.decode(
            token,
            JWT_PUBLIC_KEY,
            algorithms=[JWT_ALGORITHM],
            issuer=JWT_ISSUER,
            audience=JWT_AUDIENCE,
        )
        return payload
    except JWTError:
        raise HTTPException(status_code=401, detail="Invalid token")


def require_roles(*allowed_roles):
    def _checker(user=Depends(get_current_user)):
        # Enforce role-based authorization
        if user.get("role") not in allowed_roles:
            raise HTTPException(status_code=403, detail="Forbidden")
        return user

    return _checker


@app.get("/v1/students")
def list_students(user=Depends(require_roles("admin", "registrar"))):
    # Return paginated and minimal student data in real implementation
    return {"items": [], "page": 1, "page_size": 50}
```

### F-02 IDOR/BOLA on Student Profile Endpoint
- Evidence mapping: V-004, also similar pattern in V-050 and V-100
- Endpoint: GET /v1/students/{id}
- Issue: A student token can request another student's profile by changing path ID.
- Risk: Direct privacy breach and policy violation.

#### High-level remediation
1. Enforce object-level authorization on every object lookup.
2. Permit self-access only for student role.
3. Permit cross-record access only for trusted administrative roles.

#### Python snippet (ownership + privileged role check)
```python
from fastapi import Path


@app.get("/v1/students/{student_id}")
def get_student_profile(
    student_id: int = Path(...),
    user=Depends(get_current_user),
):
    # Student may access own profile; admins can access broader records
    privileged_roles = {"admin", "registrar", "support"}
    is_owner = user.get("student_id") == student_id
    is_privileged = user.get("role") in privileged_roles

    if not (is_owner or is_privileged):
        raise HTTPException(status_code=403, detail="Access denied")

    # Replace with real DB/service lookup
    return {
        "id": student_id,
        "name": "REDACTED",
        "cgpa": 7.8,
    }
```

### F-03 No Login Rate Limiting
- Evidence mapping: V-016
- Endpoint: POST /auth/login
- Issue: No anti-automation controls (rate limit, lockout, or slowdown).
- Risk: Enables credential stuffing and password spraying.

#### High-level remediation
1. Apply IP + account-based throttling.
2. Add temporary lockout after repeated failures.
3. Alert SOC on suspicious login spikes.

#### Python snippet (demo-level limiter logic)
```python
from time import time

# Demo-only in-memory tracker
# Production should use Redis/APIM/Gateway-based distributed limits
FAILED_ATTEMPTS = {}
WINDOW_SECONDS = 15 * 60
MAX_ATTEMPTS = 10


def is_rate_limited(identity_key: str) -> bool:
    now = time()

    # Keep only timestamps within current rolling window
    recent_attempts = [
        ts for ts in FAILED_ATTEMPTS.get(identity_key, [])
        if now - ts < WINDOW_SECONDS
    ]
    FAILED_ATTEMPTS[identity_key] = recent_attempts

    # True means request should be blocked
    return len(recent_attempts) >= MAX_ATTEMPTS


def record_failed_attempt(identity_key: str) -> None:
    FAILED_ATTEMPTS.setdefault(identity_key, []).append(time())
```

### F-04 JWT Validation and Secret Weakness
- Evidence mapping: V-027, V-098
- Issue: Weak token handling can allow token abuse or forged claims.
- Risk: Breaks trust model across all protected endpoints.

#### High-level remediation
1. Enforce explicit allowed algorithms only.
2. Reject none algorithm and malformed claims.
3. Validate issuer, audience, exp, nbf.
4. Rotate keys and store secrets in a secret manager.

#### Python snippet (strict verifier helper)
```python
from jose import jwt, JWTError
from fastapi import HTTPException


def verify_access_token(token: str) -> dict:
    try:
        # Claim checks should be strict and explicit
        return jwt.decode(
            token,
            JWT_PUBLIC_KEY,
            algorithms=["RS256"],
            issuer="hits-auth",
            audience="hits-connect-api",
        )
    except JWTError:
        raise HTTPException(status_code=401, detail="Token verification failed")
```

### F-05 Verbose Error Responses (Information Leakage)
- Evidence mapping: V-013
- Issue: Stack traces or internal exceptions may be exposed to clients.
- Risk: Helps attackers refine payloads and discover backend behavior.

#### High-level remediation
1. Return sanitized error messages to API consumers.
2. Log full stack traces internally only.
3. Disable debug mode in production.

#### Python snippet (safe global error handler)
```python
from fastapi.responses import JSONResponse
from fastapi.requests import Request


@app.exception_handler(Exception)
async def safe_exception_handler(request: Request, exc: Exception):
    # Replace print with structured centralized logging in production
    print(f"Unhandled error at {request.url}: {exc}")

    # Never expose stack trace details to the client
    return JSONResponse(
        status_code=500,
        content={"error": "Internal server error"},
    )
```

## 4. Recommended Security Baseline for API Services
Use this baseline for all high-value routes:
1. Authentication required by default (Contineous verify, Layered security approach).
2. Authorization required for every object and action (MFA/Trust no one principle).
3. Rate limits on auth and data-heavy endpoints.
4. Centralized token validation with strict claim checks.
5. Standardized error handling and response sanitization.
6. Audit logs for access to sensitive objects.

### Optional Python snippet: Security headers middleware
```python
@app.middleware("http")
async def add_security_headers(request, call_next):
    response = await call_next(request)

    # Add baseline protective headers
    response.headers["X-Content-Type-Options"] = "nosniff"
    response.headers["X-Frame-Options"] = "DENY"
    response.headers["Referrer-Policy"] = "strict-origin-when-cross-origin"
    response.headers["Content-Security-Policy"] = "default-src 'self'"

    return response
```

## 5. Risk Prioritization
| Finding | Likelihood (1-5) | Impact (1-5) | Score | Priority |
|---|---:|---:|---:|---|
| F-01 Broken auth on student list | 5 | 5 | 25 | P1 Immediate |
| F-02 IDOR/BOLA on student profile | 5 | 5 | 25 | P1 Immediate |
| F-03 No login throttling | 4 | 4 | 16 | P1 High |
| F-04 JWT handling weaknesses | 4 | 5 | 20 | P1 Immediate |
| F-05 Verbose errors/info leak | 3 | 3 | 9 | P2 Medium |

## 6. Practical Validation Checklist (After Fixes)
1. Unauthenticated GET /v1/students returns 401.
2. Student token requesting another student ID returns 403.
3. Excessive failed login attempts return 429.
4. Invalid/forged tokens are rejected with 401.
5. API errors never expose stack traces in production responses.

## 7. Final Note
This combined report is intentionally implementation-oriented: each finding is paired with a Python pattern that development teams can directly adapt into production-grade controls.
