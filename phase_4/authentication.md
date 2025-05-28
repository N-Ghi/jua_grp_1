# Authentication Design

## Overview
The JuaJobs API uses session-based authentication with Redis for fast and secure user sessions, designed to work well in African market conditions.

## Why We Use Session-Based Authentication

### Benefits
1. **Better Security**
   - Can instantly invalidate sessions
   - Better control over user sessions
   - Easier to track active users
   - Simpler to handle logout

2. **Works Better**
   - Fast response times with Redis
   - Works well with slow internet
   - Uses less server resources
   - Easy to scale

3. **Simple to Implement**
   - Straightforward session management
   - Easy to debug
   - Clear security boundaries
   - Simple to maintain

## Ways to Log In

### 1. Regular Login with Sessions
- **How Sessions Work**
  - User logs in with credentials
  - Server creates a session ID
  - Session data stored in Redis
  - Session ID sent to user
  - User sends session ID with requests

- **Session Data**
  ```json
  {
    "session_id": "unique_id",
    "user_id": "user_id",
    "role": "WORKER|CLIENT|ADMIN",
    "permissions": ["what they can do"],
    "created_at": "timestamp",
    "expires_at": "timestamp"
  }
  ```

### 2. Phone Number Login
- Send code by SMS
- Code works for 10 minutes
- Can try 3 times per hour
- Good for people without email

### 3. Social Media Login
- Login with Google
- Login with Facebook

## How Login Works

### 1. Regular Email/Password Login
1. User puts in email and password
2. System checks if they're correct
3. System creates a session
4. System sends session ID to user
5. User sends session ID with requests

### 2. Session Management
1. Check session on each request
2. Update session if needed
3. Delete session on logout
4. Handle session expiration
5. Track active sessions

### 3. Phone Number Login
1. User puts in phone number
2. System sends code by SMS
3. User puts in the code
4. System checks if code is right
5. System creates session if code is right

## Safety Rules

### Session Safety
- Always use HTTPS
- Set secure cookie flags
- Use short session timeouts
- Delete sessions on logout

### Password Rules
- At least 8 letters
- Need 1 big letter
- Need 1 small letter
- Need 1 number
- Need 1 special character
- Can't use last 5 passwords

### Rate Limits
- 5 tries to login in 15 minutes
- 3 tries for SMS code per hour
- 1000 API calls per hour

## Working with Slow Internet

### Performance
- Redis is fast and lightweight
- Minimal data transfer
- Quick session checks
- Efficient error handling

### Error Handling
- Clear error messages
- Automatic retry logic
- Session recovery
- Connection timeout handling

## What Apps Need to Do

### App Requirements
1. Store session ID safely
2. Handle session expiration
3. Handle errors well
4. Implement retry logic
5. Manage offline state

### Server Requirements
1. Set up Redis properly
2. Handle session cleanup
3. Limit login attempts
4. Keep security logs
5. Monitor for attacks

## Error Messages

### Login Errors
```json
{
  "error": "login_error",
  "error_description": "What went wrong",
  "error_code": "AUTH_001"
}
```

### Common Errors
- AUTH_001: Wrong email or password
- AUTH_002: Session expired
- AUTH_003: Invalid session
- AUTH_004: Too many tries
- AUTH_005: Account locked

## Safety Tips
1. Use secure Redis configuration
2. Set proper session timeouts
3. Monitor session activity
4. Watch for suspicious patterns
5. Lock account after too many wrong tries