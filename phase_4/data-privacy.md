# Data Privacy & Compliance

## Overview
The JuaJobs API keeps user data safe and follows the rules in different African countries.

## Personal Information 

### Types of Personal Info
1. **Very Private Info**
   - Full name
   - ID numbers
   - Bank details
   - Phone numbers
   - Email addresses

2. **Somewhat Private Info**
   - Where they live
   - Work history
   - School info
   - Skills and certificates

3. **Not Very Private Info**
   - Job preferences
   - Public profile
   - Reviews and ratings

### How We Handle Personal Info

#### Getting Info
- Ask permission first
- Only get what we need
- Tell them why we need it
- Keep it simple

#### Keeping Info Safe
- Lock it up tight
- Send it safely
- Control who can see it
- Check it regularly

#### Using Info
- Only use it for what we said
- Keep track of who sees it
- Delete it when we don't need it
- Follow the rules

## Getting Permission

### Types of Permission
1. **Making an Account**
   - Terms of service
   - Privacy policy
   - Marketing choices
   - Data sharing choices

2. **Profile Info**
   - Who can see it
   - Sharing contact info
   - Showing portfolio
   - Showing reviews

3. **Job Applications**
   - Sharing profile
   - Sharing contact info
   - Sharing work history
   - Sharing references

### How We Track Permission
```json
{
  "consent_id": "uuid",
  "user_id": "uuid",
  "consent_type": "PRIVACY_POLICY",
  "version": "1.0",
  "granted_at": "when they said yes",
  "expires_at": "when it runs out",
  "status": "ACTIVE",
  "preferences": {
    "marketing": true,
    "data_sharing": false,
    "profile_visibility": "PUBLIC"
  }
}
```

## Keeping Data Small

### Filtering Data
- Only show what's needed
- Different views for different users
- Show less to strangers
- Only send what's asked for

### Example
```javascript
const filterUserData = (user, requester) => {
  const baseFields = ['id', 'firstName', 'lastName'];
  
  if (requester.role === 'ADMIN') {
    return user; // Show everything
  }
  
  if (requester.id === user.id) {
    return {
      ...baseFields,
      email: user.email,
      phone: user.phone,
      address: user.address
    };
  }
  
  return {
    ...baseFields,
    profilePicture: user.profilePicture,
    title: user.title,
    skills: user.skills
  };
};
```

## Country Rules

### African Country Rules
1. **Nigeria**
   - Follow NDPR rules
   - Keep data in Nigeria
   - Get permission
   - Tell people if data is lost

2. **Kenya**
   - Follow Data Protection Act
   - Let people see their data
   - Control data moving between countries
   - Keep data safe

3. **South Africa**
   - Follow POPIA rules
   - Have a data officer
   - Make data agreements
   - Check data impact

### How We Follow Rules

#### Data Location
- Keep data in the right country
- Control data moving between countries
- Follow country laws
- Have local helpers

#### User Rights
- Let them see their data
- Let them fix their data
- Let them delete their data
- Let them take their data
- Let them say no

## Keeping Data Safe

### How We Protect Data
1. **Locking Data**
   - Use TLS 1.3 for sending
   - Use AES-256 for storing
   - Keep keys safe
   - Change certificates often

2. **Controlling Access**
   - Different access for different roles
   - Use two-step login
   - Control sessions
   - Block bad IPs

3. **Watching Data**
   - Log all access
   - Keep track of changes
   - Look for strange activity
   - Alert on problems

### What to Do If Data is Lost
1. Find out what happened
2. Stop the problem
3. Tell people
4. Fix the problem
5. Learn from it

## How to Use the API

### Safe Headers
```
X-Content-Type-Options: nosniff
X-Frame-Options: DENY
X-XSS-Protection: 1; mode=block
Content-Security-Policy: default-src 'self'
Strict-Transport-Security: max-age=31536000; includeSubDomains
```

### How Long We Keep Data
```json
{
  "user_data": {
    "keep_for": "5 years",
    "delete_when": "account deleted",
    "archive_as": "locked archive"
  },
  "job_data": {
    "keep_for": "3 years",
    "delete_when": "job done",
    "archive_as": "read only"
  },
  "application_data": {
    "keep_for": "2 years",
    "delete_when": "application rejected",
    "archive_as": "anonymous"
  }
}
```

## Required Documents

### What We Need
1. Privacy Policy
2. Terms of Service
3. Data Agreements
4. Security Rules
5. What to Do If Data is Lost

### Regular Checks
- Check security every 3 months
- Check rules every year
- Test for holes
- Look for problems

## Error Messages

### Privacy Errors
```json
{
  "error": "privacy_error",
  "error_code": "PRIV_001",
  "error_description": "Can't see this data",
  "compliance_reference": "NDPR-2021-001"
}
```

### Error Codes
- PRIV_001: Can't see data
- PRIV_002: Need permission
- PRIV_003: Data too old
- PRIV_004: Not allowed in this country
- PRIV_005: Breaking the rules 