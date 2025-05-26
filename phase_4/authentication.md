# Authentication Design

## Overview
The JuaJobs API uses a safe way to log in users that works well even when internet is slow or not available in Africa.

## Why We Need JWT

### Problems Without JWT
1. **Security Issues**
   - Without JWT, we'd need to check username/password for every request
   - This means storing passwords in the database, which is risky
   - If someone hacks the database, they get all passwords

2. **Performance Problems**
   - Every request would need to check the database
   - This makes the app very slow
   - Uses more internet data (important in Africa where data is expensive)

3. **Offline Problems**
   - Can't work when internet is slow or off
   - Users would need to log in again and again
   - Bad experience in areas with poor internet

### Benefits of Using JWT
1. **Better Security**
   - Passwords are never stored in the database
   - Tokens can't be faked (they're signed)
   - Easy to expire tokens if needed

2. **Works Better**
   - No need to check database every time
   - Works faster with slow internet
   - Uses less data

3. **Works Offline**
   - Can store tokens on phone
   - Can work without internet
   - Syncs when back online

## Ways to Log In

### 1. Regular Login with JWT
- **What is JWT?**
  - JWT means "JSON Web Token"
  - It's like a digital ID card that proves who you are
  - It's made up of three parts:
    1. Header (tells what type of token it is)
    2. Payload (has your info like user ID and permissions)
    3. Signature (makes sure no one can fake it)
  - The token is sent with every request to show you're logged in

- **Types of Tokens**
  - Access Token (works for 15 minutes)
  - Refresh Token (works for 7 days)
  - ID Token (has user info)

- **What's in the Token**
  ```json
  {
    "sub": "user_id",
    "iss": "juajobs_api",
    "aud": "juajobs_client",
    "exp": "when it expires",
    "iat": "when it was made",
    "scope": ["what they can do"],
    "userType": "WORKER|CLIENT|ADMIN"
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
3. System gives them tokens
4. App saves the tokens safely
5. App uses tokens for other requests

### 2. Getting New Token
1. App sees old token is expired
2. App asks for new token
3. System checks refresh token
4. System gives new access token
5. App saves new token

### 3. Phone Number Login
1. User puts in phone number
2. System sends code by SMS
3. User puts in the code
4. System checks if code is right
5. System gives tokens if code is right

## Safety Rules

### Token Safety
- Always use HTTPS
- Keep refresh tokens safe
- Get new tokens often
- Delete tokens when logging out

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

## Working with Bad Internet

### Offline Mode
- Save tokens for offline use
- Save requests when offline
- Send saved requests when back online

### Slow Internet
- Make tokens smaller
- Make responses smaller
- Handle errors better

## What Apps Need to Do

### App Requirements
1. Keep tokens safe
2. Get new tokens automatically
3. Handle errors well
4. Work when offline
5. Try again if failed

### Server Requirements
1. Make tokens safely
2. Check tokens properly
3. Limit how many tries
4. Keep logs of everything
5. Watch for bad activity

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
- AUTH_002: Token expired
- AUTH_003: Bad token
- AUTH_004: Too many tries
- AUTH_005: Account locked

## Safety Tips
1. Use safe CORS settings
2. Use safe headers
3. Check security often
4. Watch for bad activity
5. Lock account after too many wrong tries 