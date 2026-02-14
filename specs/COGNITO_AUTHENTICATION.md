# Amazon Cognito Authentication - Design Summary

**Date:** 2026-02-14  
**Status:** ✅ DESIGN COMPLETE

---

## Overview

Prachar.ai now includes **Amazon Cognito User Pools** for secure merchant authentication and **JWT-based authorization** to protect all Amazon Bedrock API calls. This ensures that every AI-generated campaign is traceable to a verified user, enabling audit trails, data isolation, and compliance.

---

## Key Components

### 1. Amazon Cognito User Pool

**Purpose:** Secure merchant sign-ups and authentication

**Configuration:**
- User Pool Name: `prachar-ai-merchants`
- Password Policy: 8+ characters, uppercase, lowercase, numbers, symbols
- Auto-verified Attributes: Email
- MFA: Optional
- Custom Attributes:
  - `brand_name`: Merchant's brand/organization name
  - `organization`: Organization type (College Club, Startup, Creator)

**User Attributes:**
```json
{
    "sub": "a1b2c3d4-e5f6-7890-abcd-ef1234567890",
    "email": "merchant@example.com",
    "email_verified": true,
    "custom:brand_name": "TechFest KIIT",
    "custom:organization": "College Club"
}
```

---

## 2. JWT-Based Authorization Flow

### Authentication Flow

```
1. User Sign-Up/Sign-In
   ↓
Frontend (Next.js) → Cognito User Pool
   ↓
Cognito returns JWT tokens:
   - ID Token (user identity)
   - Access Token (API authorization)
   - Refresh Token (token renewal)
   ↓
2. API Request
   ↓
Frontend → API Gateway (with Authorization header)
   ↓
API Gateway validates JWT with Cognito
   ↓
3. Lambda Invocation
   ↓
Lambda receives validated user context
   ↓
Lambda extracts user_id from JWT claims
   ↓
4. Bedrock API Call
   ↓
Lambda invokes Bedrock with user_id for audit trail
```

### JWT Token Structure

**ID Token Claims:**
```json
{
    "sub": "a1b2c3d4-e5f6-7890-abcd-ef1234567890",
    "email": "merchant@example.com",
    "email_verified": true,
    "cognito:username": "merchant123",
    "custom:brand_name": "TechFest KIIT",
    "custom:organization": "College Club",
    "token_use": "id",
    "exp": 1705323600
}
```

**Access Token Claims:**
```json
{
    "sub": "a1b2c3d4-e5f6-7890-abcd-ef1234567890",
    "cognito:groups": ["merchants"],
    "token_use": "access",
    "scope": "aws.cognito.signin.user.admin",
    "exp": 1705323600
}
```

---

## 3. API Gateway Cognito Authorizer

**Configuration:**
```python
{
    "Name": "CognitoAuthorizer",
    "Type": "COGNITO_USER_POOLS",
    "ProviderARNs": [
        "arn:aws:cognito-idp:us-east-1:ACCOUNT_ID:userpool/us-east-1_XXXXX"
    ],
    "IdentitySource": "method.request.header.Authorization",
    "AuthorizerResultTtlInSeconds": 300
}
```

**Request Flow:**
```
Client Request:
GET /api/campaigns
Headers:
  Authorization: Bearer <JWT_ACCESS_TOKEN>

↓

API Gateway:
1. Extracts JWT from Authorization header
2. Validates JWT signature with Cognito public keys
3. Checks token expiration
4. Verifies token_use = "access"
5. Extracts user context (sub, username, groups)

↓

Lambda receives validated user context
```

---

## 4. Securing Bedrock API Calls

### Lambda Authorization Logic

```python
def lambda_handler(event, context):
    # Extract user identity from Cognito JWT claims
    claims = event['requestContext']['authorizer']['claims']
    user_id = claims['sub']  # Cognito user UUID
    user_email = claims.get('email', 'unknown')
    
    # Log API call with user identity for audit trail
    print(f"[AUDIT] User {user_id} ({user_email}) initiating campaign generation")
    
    # Call Bedrock with user-scoped context
    response = bedrock_runtime.invoke_model(
        modelId="anthropic.claude-3-5-sonnet-20240620-v1:0",
        body=json.dumps({
            "messages": [...],
            "metadata": {
                "user_id": user_id,
                "username": username
            }
        }),
        guardrailIdentifier=os.getenv('GUARDRAIL_ID')
    )
    
    # Store campaign with user_id for data isolation
    campaign_record = {
        'campaign_id': str(uuid.uuid4()),
        'user_id': user_id,  # Ensures data belongs to authenticated user
        'user_email': user_email,
        'goal': campaign_goal,
        'created_at': datetime.utcnow().isoformat()
    }
    
    # Save to DynamoDB with user_id as partition key
    table.put_item(Item=campaign_record)
```

### Security Benefits

1. **User Identity Verification**: Every Bedrock API call is tied to a verified Cognito user
2. **Audit Trail**: All AI-generated content is traceable to specific merchants
3. **Data Isolation**: Campaigns are stored with `user_id` ensuring users only access their own data
4. **Rate Limiting**: Cognito user identity enables per-user rate limiting
5. **Cost Attribution**: Track Bedrock usage costs per merchant for billing
6. **Compliance**: Meet data privacy requirements by associating all AI operations with authenticated users

---

## 5. Frontend Integration (Next.js)

### AWS Amplify Auth Configuration

```typescript
// prachar-ai/src/lib/auth.ts
import { Amplify } from 'aws-amplify';

Amplify.configure({
    Auth: {
        Cognito: {
            userPoolId: process.env.NEXT_PUBLIC_COGNITO_USER_POOL_ID!,
            userPoolClientId: process.env.NEXT_PUBLIC_COGNITO_CLIENT_ID!,
            loginWith: {
                email: true
            },
            signUpVerificationMethod: 'code',
            userAttributes: {
                email: { required: true },
                'custom:brand_name': { required: false }
            },
            passwordFormat: {
                minLength: 8,
                requireLowercase: true,
                requireUppercase: true,
                requireNumbers: true,
                requireSpecialCharacters: true
            }
        }
    }
});
```

### Sign-Up Flow

```typescript
import { signUp, confirmSignUp } from 'aws-amplify/auth';

async function handleSignUp(email: string, password: string, brandName: string) {
    const { userId } = await signUp({
        username: email,
        password,
        options: {
            userAttributes: {
                email,
                'custom:brand_name': brandName
            },
            autoSignIn: true
        }
    });
}

async function handleConfirmSignUp(email: string, code: string) {
    await confirmSignUp({
        username: email,
        confirmationCode: code
    });
}
```

### API Request with JWT

```typescript
import { fetchAuthSession } from 'aws-amplify/auth';

async function generateCampaign(goal: string) {
    // Get current user's JWT tokens
    const session = await fetchAuthSession();
    const accessToken = session.tokens?.accessToken?.toString();
    
    // Make API request with Authorization header
    const response = await fetch('/api/generate', {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json',
            'Authorization': `Bearer ${accessToken}`  // JWT token
        },
        body: JSON.stringify({ goal })
    });
    
    return await response.json();
}
```

---

## 6. DynamoDB Data Model with User Isolation

### Updated Campaign Schema

```python
{
    "campaign_id": "uuid",  # Sort key
    "user_id": "cognito_sub",  # Partition key (ensures data isolation)
    "user_email": "merchant@example.com",
    "goal": "Hype my college fest",
    "plan": {...},
    "captions": [...],
    "image_url": "https://s3.amazonaws.com/...",
    "status": "completed",
    "created_at": "2024-01-15T10:30:00Z",
    "bedrock_invocation_id": "uuid",  # For audit trail
    "guardrail_logs": [
        {
            "step": "copy_generation",
            "action": "ALLOWED",
            "user_id": "cognito_sub"
        }
    ]
}
```

### Query Pattern

```python
# Get all campaigns for authenticated user
response = table.query(
    KeyConditionExpression=Key('user_id').eq(user_id)
)
```

---

## 7. Protected API Endpoints

### Generate Campaign (Protected)

```
POST /api/campaigns/generate
Headers:
  Authorization: Bearer <JWT_ACCESS_TOKEN>
  Content-Type: application/json

Body:
{
    "goal": "Hype my college fest"
}

Response:
{
    "campaign_id": "uuid",
    "user_id": "cognito_sub",
    "plan": {...},
    "captions": [...],
    "image_url": "https://s3.amazonaws.com/...",
    "status": "completed"
}
```

### Get Campaign History (Protected)

```
GET /api/campaigns
Headers:
  Authorization: Bearer <JWT_ACCESS_TOKEN>

Response:
{
    "campaigns": [...],
    "user_id": "cognito_sub"
}
```

### User Profile (Protected)

```
GET /api/user/profile
Headers:
  Authorization: Bearer <JWT_ACCESS_TOKEN>

Response:
{
    "user_id": "cognito_sub",
    "email": "merchant@example.com",
    "email_verified": true,
    "brand_name": "TechFest KIIT",
    "organization": "College Club",
    "total_campaigns": 15
}
```

---

## 8. Updated Architecture Diagram

```
User (Browser)
    ↓
AWS Amplify (Next.js Frontend)
    ↓
Amazon Cognito User Pool (Authentication)
    ↓ (JWT Token)
API Gateway (REST API + Cognito Authorizer)
    ↓ (Validated User Context)
Lambda (agent.py with Strands SDK)
    ↓
├── Amazon Bedrock (Claude + Titan) [Secured with user_id]
├── Bedrock Knowledge Base (RAG)
├── Bedrock Guardrails
├── DynamoDB (campaigns, brands, logs) [Partitioned by user_id]
└── S3 (images, PDFs)
```

---

## 9. IAM Permissions

### Lambda Execution Role

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "bedrock:InvokeModel",
                "bedrock:Retrieve",
                "bedrock:ApplyGuardrail"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "cognito-idp:GetUser",
                "cognito-idp:ListUsers"
            ],
            "Resource": "arn:aws:cognito-idp:*:*:userpool/*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:PutObject",
                "s3:GetObject"
            ],
            "Resource": "arn:aws:s3:::prachar-ai-assets/*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "dynamodb:PutItem",
                "dynamodb:GetItem",
                "dynamodb:Query"
            ],
            "Resource": "arn:aws:dynamodb:*:*:table/prachar-campaigns"
        }
    ]
}
```

---

## 10. Error Handling

### Authentication Errors

**401 Unauthorized:**
```json
{
    "error": "Unauthorized: Missing or invalid authentication token"
}
```

**403 Forbidden:**
```json
{
    "error": "Forbidden: Token expired or user not verified"
}
```

### Authorization Errors

**Scenario:** User attempts to access another user's campaigns

**Response:**
```json
{
    "error": "Forbidden: Access denied to requested resource"
}
```

**Action:** Log security event, return 403 status

---

## 11. Testing Requirements

### Security Tests

1. **Test expired JWT token rejection**
2. **Test invalid JWT signature rejection**
3. **Verify user data isolation** (user A cannot access user B's campaigns)
4. **Test rate limiting per user**
5. **Validate audit trail completeness**

### Property-Based Tests

- **Property 4**: All Bedrock API calls must have valid user_id in audit logs
- **Property 5**: All DynamoDB queries must filter by authenticated user_id

---

## 12. Success Metrics

### Security Targets

- **JWT validation success rate:** 100%
- **Unauthorized access attempts blocked:** 100%
- **Audit trail completeness:** 100% of Bedrock calls logged with user_id
- **Data isolation:** Zero cross-user data leaks

### Performance Targets

- **Authentication latency:** < 500ms
- **Campaign generation:** < 60 seconds
- **Guardrail compliance:** 100%

---

## 13. Implementation Checklist

### Backend
- [ ] Create Cognito User Pool with custom attributes
- [ ] Configure API Gateway Cognito Authorizer
- [ ] Update Lambda to extract user_id from JWT claims
- [ ] Add user_id to all Bedrock API call metadata
- [ ] Update DynamoDB schema with user_id partition key
- [ ] Implement audit logging with user context
- [ ] Add IAM permissions for Cognito access

### Frontend
- [ ] Install AWS Amplify Auth library
- [ ] Configure Amplify with Cognito User Pool
- [ ] Implement sign-up flow with email verification
- [ ] Implement sign-in flow
- [ ] Add JWT token to all API requests
- [ ] Handle token expiration and refresh
- [ ] Implement protected routes

### Testing
- [ ] Unit tests for JWT validation
- [ ] Integration tests for auth flow
- [ ] Security tests for data isolation
- [ ] Load tests with authenticated users
- [ ] Audit trail verification tests

---

## 14. Benefits Summary

### Security
✅ Every Bedrock API call is authenticated  
✅ User data is isolated by partition key  
✅ Complete audit trail for compliance  
✅ Protection against unauthorized access  

### Scalability
✅ Per-user rate limiting  
✅ Cost attribution per merchant  
✅ Multi-tenant architecture ready  

### User Experience
✅ Secure sign-up with email verification  
✅ Seamless JWT-based authentication  
✅ Automatic token refresh  
✅ Protected API endpoints  

---

**Status:** ✅ DESIGN COMPLETE  
**Next Steps:** Implementation  
**Documentation:** Updated in `specs/design.md`

---

**Last Updated:** 2026-02-14  
**Author:** System Design Team  
**Version:** 2.0 (with Cognito Authentication)
