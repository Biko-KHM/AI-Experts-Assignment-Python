# Bug Explanation

## What was the bug?

The bug was in the token refresh logic in `http_client.py`. When the system encountered a dictionary-formatted token, it would always refresh it, even if the token was still valid. Additionally, valid dictionary tokens couldn't be used to set the Authorization header because the code only handled OAuth2Token objects.

## Why did it happen?

The original code had `isinstance(self.oauth2_token, dict)` as a refresh condition, which only checked the token's type without verifying whether it was actually expired. This meant any dictionary token triggered a refresh, regardless of its expiration time.

The second issue was that the Authorization header logic only had a branch for OAuth2Token instances, completely ignoring dictionary tokens. So even if a dict token was valid, it couldn't be used.

## Why does the fix solve it?

The fix addresses both issues with minimal changes:

1. **Expiration check for dict tokens**: Added a timestamp comparison `int(datetime.now(tz=timezone.utc).timestamp()) >= self.oauth2_token.get("expires_at", 0)` so dictionary tokens are only refreshed when they're actually expired, just like OAuth2Token objects.

2. **Authorization header for dict tokens**: Added an `elif isinstance(self.oauth2_token, dict)` branch to format the Authorization header from dictionary tokens, ensuring they work the same way as OAuth2Token instances.

Now both token types behave consistently: only refresh when expired, and use the token when it's still valid.

## One realistic edge case not covered

The tests don't cover concurrent access scenarios. If multiple threads detect an expired token simultaneously, they could all call `refresh_oauth2()` at the same time, potentially causing race conditions or unnecessary duplicate refresh requests. In a production environment with high concurrency, this could lead to performance issues or inconsistent token state.
