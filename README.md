# Python OAuth2 HTTP Client - Bug Fix Assignment

This repository contains a Python HTTP client with OAuth2 token management. The project demonstrates debugging skills, test-driven development, and minimal code changes to fix a production bug.

## Project Overview

The HTTP client supports both `OAuth2Token` objects and legacy dictionary-based tokens. A bug was discovered where valid dictionary tokens were being unnecessarily refreshed, causing performance issues and inconsistent behavior.

## What Was Fixed

**Bug**: Dictionary tokens were always refreshed regardless of their expiration status, and couldn't be used to set Authorization headers.

**Solution**: Added expiration checking for dictionary tokens and Authorization header support, ensuring they behave identically to OAuth2Token objects.

See [EXPLANATION.md](EXPLANATION.md) for detailed analysis of the bug, root cause, and fix.

## Project Structure

```
.
├── app/
│   ├── http_client.py      # HTTP client with OAuth2 support (bug fixed here)
│   └── tokens.py            # OAuth2Token implementation
├── tests/
│   ├── test_http_client.py # Test suite including bug reproduction test
│   └── conftest.py          # Test configuration
├── Dockerfile               # Container setup for CI/CD
├── requirements.txt         # Pinned dependencies
└── EXPLANATION.md          # Detailed bug analysis
```

## Running Tests Locally

Make sure you have Python 3.11+ installed, then:

```bash
# Install dependencies
pip install -r requirements.txt

# Run the test suite
python -m pytest -v
```

Expected output: All 7 tests should pass.

## Running Tests with Docker

The project includes a Dockerfile for consistent, reproducible test execution:

```bash
# Build the Docker image
docker build -t python-assignment .

# Run tests in the container
docker run python-assignment
```

The container automatically runs the test suite and displays results.

## Dependencies

All dependencies are pinned for reproducibility:

- `pytest==8.0.0` - Testing framework
- `requests==2.31.0` - HTTP library
- `python-dateutil==2.8.2` - Date parsing utilities

## Key Changes Made

1. **Bug Fix** (`app/http_client.py`):
   - Added expiration check for dictionary tokens
   - Added Authorization header support for dictionary tokens
   - Minimal change: only 2 logical modifications

2. **Test Coverage** (`tests/test_http_client.py`):
   - Added `test_api_request_does_not_refresh_valid_dict_token`
   - This test fails on the buggy code and passes after the fix

3. **Infrastructure**:
   - Created Dockerfile for containerized testing
   - Pinned all dependencies in requirements.txt
   - Added comprehensive documentation

## Testing Approach

The test suite validates:
- Token refresh logic for both OAuth2Token and dict types
- Expiration checking accuracy
- Authorization header formatting
- Edge cases (missing tokens, expired tokens, valid tokens)

The new test specifically reproduces the bug by creating a valid dictionary token and verifying it's not unnecessarily refreshed.

## Notes

- All changes follow the principle of minimal, reviewable modifications
- No refactoring of unrelated code
- Focus on correctness and clarity
- Production-ready fix with comprehensive test coverage

---

**Assignment completed for**: Eskalate AI Software Engineer Position  
**Date**: March 11, 2026
