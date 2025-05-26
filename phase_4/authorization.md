# Authorization Framework

## Overview
The JuaJobs API implements a comprehensive role-based access control system with fine-grained permissions and resource ownership validation.

## Role-Based Access Control 

### User Roles
1. **Admin**
   - Full system access
   - User management
   - System configuration
   - Analytics access

2. **Client**
   - Job posting
   - Worker hiring
   - Payment management
   - Review submission

3. **Worker**
   - Profile management
   - Job application
   - Portfolio management
   - Review submission

### Permission Structure
```json
{
  "resource": "jobs",
  "actions": ["create", "read", "update", "delete"],
  "conditions": {
    "ownership": true,
    "status": ["OPEN", "IN_PROGRESS"]
  }
}
```

## Resource Ownership Model

### Ownership Rules
1. **Jobs**
   - Created by clients
   - Workers can view and apply
   - Only owner can modify/delete

2. **Worker Profiles**
   - Owned by workers
   - Clients can view
   - Only owner can modify

3. **Applications**
   - Created by workers
   - Visible to job owner
   - Only applicant can modify

4. **Reviews**
   - Created by both parties
   - Visible to both parties
   - Cannot be modified after submission

## Permission Mapping

### Client Permissions
```json
{
  "jobs": {
    "create": true,
    "read": ["own", "public"],
    "update": ["own"],
    "delete": ["own"]
  },
  "applications": {
    "read": ["own_jobs"],
    "update": ["own_jobs"]
  },
  "reviews": {
    "create": ["completed_jobs"],
    "read": ["own"]
  }
}
```

### Worker Permissions
```json
{
  "jobs": {
    "read": ["public"],
    "create": ["application"]
  },
  "profile": {
    "read": ["own", "public"],
    "update": ["own"]
  },
  "applications": {
    "create": true,
    "read": ["own"],
    "update": ["own"]
  }
}
```

## Delegated Permissions

### Job Delegation
- Clients can delegate job management
- Temporary access tokens
- Time-limited permissions
- Audit logging of all actions

### Team Management
- Project team creation
- Role assignment
- Permission inheritance
- Access revocation

## Implementation Guidelines

### Permission Checking
1. Validate user role
2. Check resource ownership
3. Verify action permission
4. Apply business rules
5. Log access attempt

### Error Responses
```json
{
  "error": "authorization_error",
  "error_description": "User does not have permission to perform this action",
  "error_code": "AUTHZ_001",
  "required_permissions": ["jobs:update"],
  "user_permissions": ["jobs:read"]
}
```

## Security Considerations

### Access Control Best Practices
1. Principle of least privilege
2. Regular permission audits
3. Permission inheritance validation
4. Cross-role access prevention
5. Resource isolation

### Audit Logging
- All permission checks
- Access attempts
- Permission changes
- Role modifications
- Resource ownership changes

## API Endpoints Protection

### Protected Routes
```
POST   /api/v1/jobs
PUT    /api/v1/jobs/{id}
DELETE /api/v1/jobs/{id}
POST   /api/v1/applications
PUT    /api/v1/applications/{id}
POST   /api/v1/reviews
```

### Middleware Implementation
```javascript
const checkPermission = (resource, action) => {
  return async (req, res, next) => {
    const user = req.user;
    const resourceId = req.params.id;
    
    // Check role-based permission
    if (!hasPermission(user.role, resource, action)) {
      return res.status(403).json({
        error: 'authorization_error',
        error_code: 'AUTHZ_001'
      });
    }
    
    // Check resource ownership
    if (requiresOwnership(resource, action)) {
      const isOwner = await checkOwnership(user.id, resource, resourceId);
      if (!isOwner) {
        return res.status(403).json({
          error: 'authorization_error',
          error_code: 'AUTHZ_002'
        });
      }
    }
    
    next();
  };
};
```

## Error Codes

### Authorization Errors
- AUTHZ_001: Insufficient permissions
- AUTHZ_002: Not resource owner
- AUTHZ_003: Invalid role
- AUTHZ_004: Permission denied
- AUTHZ_005: Delegation expired 