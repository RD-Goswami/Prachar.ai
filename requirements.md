# Requirements Document: Prachar.ai - The Autonomous Director's War Room

## Project Overview

**Prachar.ai** is an Autonomous Director's War Room designed for Indian students, creators, and college clubs competing in the AWS "AI for Bharat" Hackathon (Student Track: Media, Content & Creativity). The system uses a 5-tier Diamond Resilience Cascade to autonomously plan, draft, and design culturally relevant social media campaigns in aggressive, high-energy Hinglish, with 100% uptime guarantee and enterprise-grade fault tolerance.

### Development Methodology

This project follows **Kiro's Spec-Driven Development** methodology:

1. **Requirements First**: Comprehensive functional and non-functional requirements with testable acceptance criteria
2. **Design Specification**: Detailed system architecture, component design, and API specifications
3. **Iterative Implementation**: Code developed against specifications with continuous validation
4. **Test-Driven Validation**: Unit, integration, and property-based tests aligned with acceptance criteria
5. **Documentation-Centric**: All decisions, trade-offs, and implementations documented for transparency

**Kiro Artifacts:**
- `specs/requirements.md` - This document (500+ lines)
- `specs/design.md` - System design specification (1000+ lines)
- `specs/COGNITO_AUTHENTICATION.md` - Authentication implementation guide (500+ lines)
- `specs/HACKATHON_CRITERIA_REVIEW.md` - Hackathon alignment review

### AWS Services Integration

**Core Services:**
- **Diamond Cascade**: 5-tier intelligent failover (Gemini 3, Groq, Arcee 400B, Llama 3.3, Mock)
- **Amazon Cognito**: User Pools for merchant authentication, JWT-based authorization
- **AWS Lambda**: Serverless compute for agent execution (Python 3.11)

**Supporting Services:**
- **Amazon DynamoDB**: Campaign storage with user isolation
- **Amazon S3**: Generated image storage and brand guideline PDFs
- **Amazon API Gateway**: REST API with Cognito Authorizer
- **Amazon CloudWatch**: Logging, monitoring, and audit trails
- **AWS Amplify**: Frontend hosting with Next.js SSR and CI/CD

## Glossary

- **Creative_Director_Agent**: The autonomous elite persona that plans and executes campaign workflows with aggressive, high-energy tone
- **Diamond_Cascade**: A 5-tier intelligent failover system (Gemini 3 Flash Preview → Groq GPT-OSS 120B → Arcee Trinity Large 400B → Llama 3.3 70B Shield → Titanium Shield Mock) ensuring 100% uptime and bypassing AWS Bedrock quota limits
- **War_Room_UI**: The split-pane tactical dashboard featuring glassmorphism, real-time status bar (TIER, DB_SYNC, REGION), and advanced JSON parser for campaign generation
- **Aukaat_Engine**: The elite Creative Director persona enforcing aggressive, high-energy Hinglish with power words (Aukaat, Bawaal, Main Character Energy, Level Up)
- **AWS_Amplify**: The AWS-native CI/CD and SSR hosting engine for the Next.js frontend, ensuring 100% AWS-native stack
- **Strands_SDK**: Python orchestration framework for building multi-step agentic workflows
- **Hinglish**: Hindi-English linguistic mix commonly used in Indian social media
- **Campaign_Goal**: User's high-level objective (e.g., "Hype my college fest", "Promote my startup")
- **Agentic_Workflow**: Autonomous multi-step process: Reason → Plan → Act → Validate
- **Campaign_Plan**: Agent-generated structured plan with hooks, offers, and CTAs
- **Brand_Style_Guidelines**: User-uploaded PDF defining tone, voice, and visual preferences
- **Titanium_Shield**: Terminal failover tier providing high-quality mock data with intelligent goal matching
- **Stateful_Agent**: Conversation history tracking with context awareness for iterative refinement


## Requirements

### Requirement 1: Autonomous Campaign Planning with Diamond Cascade

**User Story:** As a student club organizer, I want to provide a high-level goal and have the AI autonomously create a complete campaign plan using intelligent failover, so I get 100% reliable results without manual intervention.

#### Acceptance Criteria

1. WHEN the user submits a Campaign_Goal, THE Creative_Director_Agent SHALL analyze the goal and generate a structured Campaign_Plan using the Diamond_Cascade
2. WHEN planning, THE Creative_Director_Agent SHALL attempt Tier 1 (Gemini 3 Flash Preview) first for best quality reasoning
3. WHEN Tier 1 fails, THE system SHALL automatically cascade to Tier 2 (Groq GPT-OSS 120B) within 1 second
4. WHEN all AI tiers fail, THE system SHALL use Titanium_Shield mock data to guarantee 100% success rate
5. WHEN the plan is created, THE Creative_Director_Agent SHALL include at least 3 components: hook, offer, and call-to-action
6. WHEN planning completes, THE Creative_Director_Agent SHALL autonomously proceed to content generation without user intervention
7. WHEN the agent encounters ambiguity, THE Creative_Director_Agent SHALL make reasonable assumptions based on Indian student context

### Requirement 2: Brand-Aware Content Generation

**User Story:** As a creator with a specific brand voice, I want the AI to read my brand guidelines before generating content, so all outputs match my established style.

#### Acceptance Criteria

1. WHEN generating any content, THE Creative_Director_Agent SHALL first attempt to retrieve Brand_Style_Guidelines from S3 storage
2. WHEN brand guidelines exist, THE Creative_Director_Agent SHALL incorporate tone, voice, and style preferences into generation prompts
3. WHEN no brand guidelines are found, THE Creative_Director_Agent SHALL use the Aukaat_Engine default persona
4. WHEN retrieving guidelines, THE system SHALL use semantic search with fallback to keyword matching
5. WHEN brand context is retrieved, THE Creative_Director_Agent SHALL log the retrieved chunks for transparency

### Requirement 3: Hinglish Copywriting with Aukaat Engine

**User Story:** As an Indian student marketer, I want campaign copy in aggressive, high-energy Hinglish with power words that resonates with my peers, so my content feels authentic and culturally dominant.

#### Acceptance Criteria

1. WHEN generating text content, THE Creative_Director_Agent SHALL use the Diamond_Cascade to produce Hinglish copy with the Aukaat_Engine persona
2. WHEN writing copy, THE system SHALL mix Hindi and English words naturally (40-60% Hindi, 60-40% English)
3. WHEN generating copy, THE system SHALL include power words: "Aukaat" (show your worth), "Bawaal" (create chaos/excitement), "Main Character Energy" (be the protagonist), "Level Up" (upgrade yourself)
4. WHEN writing copy, THE system SHALL use aggressive, elite, high-energy tone and NEVER be "mid"
5. WHEN the Campaign_Goal mentions festivals or events, THE system SHALL incorporate relevant cultural references
6. WHEN copy is generated, THE system SHALL produce exactly 3 caption variations for user selection
7. WHEN generating copy, THE system SHALL include culturally appropriate emojis (🔥, 💯, ✨, 🎉, 🚀)


### Requirement 4: Autonomous Visual Generation

**User Story:** As a busy student, I want the AI to automatically create campaign posters that match my brand colors, so I don't need design skills.

#### Acceptance Criteria

1. WHEN the Campaign_Plan includes visual requirements, THE Creative_Director_Agent SHALL autonomously call the image generation tool
2. WHEN generating images, THE system SHALL use curated Unsplash images with intelligent goal matching
3. WHEN brand colors are specified in Brand_Style_Guidelines, THE system SHALL select images that complement those colors
4. WHEN generating posters, THE system SHALL include the campaign hook text as overlay
5. WHEN image generation completes, THE system SHALL return a publicly accessible URL

### Requirement 5: Enterprise Resilience & 100% Uptime Guarantee

**User Story:** As a hackathon participant, I want the system to never fail during demos, so I can confidently present to judges without technical issues.

#### Acceptance Criteria

1. WHEN any AI tier fails, THE system SHALL automatically cascade to the next tier within 1 second
2. WHEN Tier 1 (Gemini 3 Flash Preview) fails, THE system SHALL cascade to Tier 2 (Groq GPT-OSS 120B)
3. WHEN Tier 2 fails, THE system SHALL cascade to Tier 3 (Arcee Trinity Large 400B)
4. WHEN Tier 3 fails, THE system SHALL cascade to Tier 4 (Llama 3.3 70B Shield)
5. WHEN all AI tiers fail, THE system SHALL use Tier 5 (Titanium Shield Mock) to guarantee 100% success rate
6. WHEN using Titanium Shield, THE system SHALL provide high-quality mock data with intelligent goal matching (tech/fest/workshop categories)
7. WHEN cascading occurs, THE system SHALL log the tier used and reason for failover to CloudWatch
8. WHEN AWS Bedrock quota limits or ThrottlingExceptions occur, THE Diamond_Cascade SHALL bypass these limitations by using alternative providers
9. WHEN the system operates, THE overall success rate SHALL be 100% guaranteed by the 5-tier architecture

**Rationale:** This cascade was implemented to bypass AWS Bedrock Quota Limits and ThrottlingExceptions, demonstrating enterprise fault tolerance and ensuring flawless demo experiences.

### Requirement 6: The Director's War Room UI

**User Story:** As a user, I want a professional tactical dashboard that makes campaign generation feel like commanding a war room, so I have an engaging and intuitive experience.

#### Acceptance Criteria

1. WHEN the user accesses the platform, THE system SHALL display the War_Room_UI with split-pane layout
2. WHEN displaying the UI, THE left sidebar SHALL be fixed at 400px width with glassmorphism effects (backdrop-blur-xl, zinc-900/50)
3. WHEN displaying the UI, THE center canvas SHALL be fluid width and contain the Active Intelligence Feed
4. WHEN the user sends a message, THE system SHALL display pretty chat bubbles (user right/indigo, director left/zinc)
5. WHEN the AI responds with campaign data, THE system SHALL use an Advanced JSON Parser to extract structured data
6. WHEN valid JSON is found, THE system SHALL display "✅ Strategic Campaign Compiled. See the Canvas below." instead of raw JSON
7. WHEN campaign assets are generated, THE system SHALL render them below the chat feed with scanline hover effects
8. WHEN displaying the UI, THE bottom status bar SHALL show real-time indicators: TIER (STANDBY/TIER_1/ACTIVE/ERROR), DB_SYNC (OK/SYNCED), REGION (US-EAST-1)
9. WHEN the user hovers over campaign cards, THE system SHALL display vertical scanline sweep animation (1000ms duration)
10. WHEN the UI loads, THE system SHALL display cyan-indigo radial glow background effect


### Requirement 7: AWS-Native Hosting with Amplify

**User Story:** As a developer, I want the frontend hosted exclusively on AWS infrastructure, so I maintain a 100% AWS-native stack for the hackathon.

#### Acceptance Criteria

1. WHEN deploying the frontend, THE system SHALL use AWS Amplify exclusively
2. WHEN hosting on Amplify, THE system SHALL use Next.js SSR monorepo configuration
3. WHEN deploying, THE system SHALL NOT use Vercel or any non-AWS hosting platforms
4. WHEN Amplify builds, THE system SHALL use automatic CI/CD from the main branch
5. WHEN the frontend is deployed, THE system SHALL be accessible via the Amplify-provided URL (*.amplifyapp.com)
6. WHEN environment variables are needed, THE system SHALL use Amplify Environment Variables
7. WHEN the build completes, THE system SHALL support server-side rendering for optimal performance

**Rationale:** Maintaining a 100% AWS-native stack demonstrates deep AWS integration and aligns with hackathon criteria for AWS service utilization.

### Requirement 8: Serverless Execution

**User Story:** As a hackathon participant with limited budget, I want the system to run serverlessly, so I only pay for actual usage.

#### Acceptance Criteria

1. WHEN a user triggers campaign generation, THE system SHALL execute the Creative_Director_Agent on AWS Lambda
2. WHEN Lambda executes, THE function SHALL complete within 5 minutes (Lambda timeout limit)
3. WHEN the agent runs, THE system SHALL use Python 3.11 runtime with urllib for HTTP requests
4. WHEN execution completes, THE system SHALL store results in DynamoDB
5. WHEN errors occur, THE system SHALL log to CloudWatch and return user-friendly error messages
6. WHEN the Diamond_Cascade executes, THE system SHALL use pure REST API calls without third-party SDKs

### Requirement 9: Campaign History and Retrieval

**User Story:** As a creator, I want to view my past campaigns and reuse successful strategies, so I can iterate and improve over time.

#### Acceptance Criteria

1. WHEN a campaign is generated, THE system SHALL store the Campaign_Plan, copy, and image URL in DynamoDB
2. WHEN storing campaigns, THE system SHALL include metadata: timestamp, user_id, goal, status, and conversation messages
3. WHEN the user requests history, THE system SHALL retrieve campaigns sorted by creation date
4. WHEN displaying history, THE system SHALL show campaign thumbnails and key metrics
5. WHEN a user selects a past campaign, THE system SHALL allow them to regenerate variations
6. WHEN storing campaigns, THE system SHALL use camelCase keys (campaignId, userId) to match AWS conventions

### Requirement 10: Secure User Authentication

**User Story:** As a user, I want secure login so my campaigns and brand guidelines remain private.

#### Acceptance Criteria

1. WHEN a user accesses the platform, THE system SHALL require authentication via Amazon Cognito
2. WHEN authenticating, THE system SHALL support email/password and social login (Google)
3. WHEN a user logs in, THE system SHALL issue JWT tokens for API access
4. WHEN accessing resources, THE system SHALL validate user_id matches the resource owner
5. WHEN sessions expire, THE system SHALL prompt re-authentication
6. WHEN API calls are made, THE system SHALL include JWT token in Authorization header


### Requirement 11: Stateful Conversation with Context Awareness

**User Story:** As a user, I want to refine my campaigns through conversation, so I can iteratively improve the output without starting over.

#### Acceptance Criteria

1. WHEN the user sends a message, THE system SHALL append it to the conversation history
2. WHEN the AI responds, THE system SHALL append the response to the conversation history
3. WHEN generating campaigns, THE system SHALL pass the full conversation history to the Diamond_Cascade
4. WHEN storing campaigns, THE system SHALL persist the conversation messages array in DynamoDB
5. WHEN the user refines a campaign, THE system SHALL use previous context to generate improved versions
6. WHEN displaying chat, THE system SHALL show the full conversation history with proper formatting

### Requirement 12: Real-Time Progress Updates

**User Story:** As a user waiting for campaign generation, I want to see real-time progress updates, so I know the agent is working.

#### Acceptance Criteria

1. WHEN the Creative_Director_Agent starts execution, THE system SHALL update the status bar to show current tier
2. WHEN each tier executes, THE system SHALL update the TIER indicator (TIER_1, TIER_2, etc.)
3. WHEN generation is in progress, THE system SHALL display "AI REASONING..." indicator
4. WHEN generation completes, THE system SHALL update DB_SYNC to "SYNCED"
5. WHEN errors occur, THE system SHALL show user-friendly error messages with retry options
6. WHEN the status changes, THE system SHALL update within 500ms

### Requirement 13: Advanced JSON Parsing

**User Story:** As a user, I want to see clean, formatted campaign data instead of raw JSON, so I have a professional experience.

#### Acceptance Criteria

1. WHEN the AI responds, THE system SHALL attempt to parse the response as JSON
2. WHEN valid JSON is found, THE system SHALL extract campaign structure (hook, offer, cta, captions)
3. WHEN JSON is successfully parsed, THE system SHALL display "✅ Strategic Campaign Compiled. See the Canvas below."
4. WHEN JSON parsing fails, THE system SHALL try regex extraction to find JSON within text
5. WHEN no valid JSON is found, THE system SHALL display the raw text as a normal chat message
6. WHEN campaign data is extracted, THE system SHALL render it as formatted cards below the chat feed

### Requirement 14: Multi-Language Support (Future)

**User Story:** As a creator targeting regional audiences, I want to generate campaigns in regional languages beyond Hinglish, so I can reach diverse audiences.

#### Acceptance Criteria

1. WHEN the user selects a target language, THE Creative_Director_Agent SHALL generate copy in that language
2. WHEN supported languages include: Hinglish, Tamil, Telugu, Bengali, Marathi
3. WHEN generating regional content, THE system SHALL use culturally appropriate references
4. WHEN brand guidelines specify language preferences, THE system SHALL honor them
5. WHEN language is not specified, THE system SHALL default to Hinglish with Aukaat_Engine


## Non-Functional Requirements

### Performance
- Campaign generation SHALL complete within 3 seconds for 90% of requests in War Room mode
- Campaign generation SHALL complete within 60 seconds for 90% of requests in production mode
- Lambda cold start SHALL not exceed 10 seconds
- DynamoDB queries SHALL return results within 500ms
- Diamond Cascade tier failover SHALL occur within 1 second

### Scalability
- System SHALL support 100 concurrent users during hackathon demo
- S3 SHALL store up to 10,000 generated images
- DynamoDB SHALL handle 1,000 writes per second
- Diamond Cascade SHALL handle quota limits by intelligent failover

### Reliability
- System SHALL guarantee 100% success rate via 5-tier Diamond Cascade
- Titanium Shield SHALL provide high-quality mock data when all AI tiers fail
- System SHALL bypass AWS Bedrock quota limits and ThrottlingExceptions
- System SHALL maintain uptime during demos with zero failures

### Security
- All API endpoints SHALL require valid JWT tokens
- S3 objects SHALL use pre-signed URLs with 1-hour expiration
- Lambda SHALL use IAM roles with least privilege
- DynamoDB SHALL enforce user isolation via partition keys
- All user data SHALL be isolated by userId

### Usability
- Frontend SHALL be mobile-responsive
- Campaign generation SHALL require maximum 3 user inputs
- Error messages SHALL be in plain English with actionable guidance
- War Room UI SHALL provide intuitive split-pane layout
- Status bar SHALL show real-time system status

### Compliance
- System SHALL comply with AWS Acceptable Use Policy
- Generated content SHALL be filtered for inappropriate content
- Audit logs SHALL be retained for 90 days
- System SHALL respect user privacy and data isolation

### Infrastructure
- Frontend SHALL be hosted exclusively on AWS Amplify
- Frontend SHALL NOT use Vercel or non-AWS platforms
- System SHALL maintain 100% AWS-native stack
- Amplify SHALL use Next.js SSR monorepo configuration
- All AWS services SHALL be in us-east-1 region

### Documentation
- System SHALL maintain 12,000+ lines of professional documentation
- All requirements SHALL be traceable to implementation
- All design decisions SHALL be documented
- All API endpoints SHALL be documented
- All test results SHALL be documented


## Technical Architecture Summary

### 5-Tier Diamond Resilience Cascade

```
User Request
     ↓
┌─────────────────────────────────────────────────────────────┐
│ TIER 1: GEMINI 3 FLASH PREVIEW (Primary)                   │
│ • Provider: Google                                          │
│ • Parameters: ~2B                                           │
│ • Speed: 2-3s                                               │
│ • Success Rate: 95%                                         │
│ • Cost: Free                                                │
└─────────────────────────────────────────────────────────────┘
     ↓ (if fails)
┌─────────────────────────────────────────────────────────────┐
│ TIER 2: GROQ GPT-OSS 120B (Powerhouse)                     │
│ • Provider: Groq                                            │
│ • Parameters: 120B                                          │
│ • Speed: 0.5-1s                                             │
│ • Success Rate: 98%                                         │
│ • Cost: Free                                                │
└─────────────────────────────────────────────────────────────┘
     ↓ (if fails)
┌─────────────────────────────────────────────────────────────┐
│ TIER 3: ARCEE TRINITY LARGE (400B Creative King)           │
│ • Provider: OpenRouter                                      │
│ • Parameters: 400B                                          │
│ • Speed: 3-5s                                               │
│ • Success Rate: 99%                                         │
│ • Cost: Free                                                │
└─────────────────────────────────────────────────────────────┘
     ↓ (if fails)
┌─────────────────────────────────────────────────────────────┐
│ TIER 4: LLAMA 3.3 70B - THE SHIELD                         │
│ • Provider: OpenRouter                                      │
│ • Parameters: 70B                                           │
│ • Speed: 2-4s                                               │
│ • Success Rate: 99.9%                                       │
│ • Cost: Free                                                │
└─────────────────────────────────────────────────────────────┘
     ↓ (if fails)
┌─────────────────────────────────────────────────────────────┐
│ TIER 5: TITANIUM SHIELD MOCK DATA (Terminal)               │
│ • Provider: Local                                           │
│ • Parameters: N/A                                           │
│ • Speed: <0.1s                                              │
│ • Success Rate: 100%                                        │
│ • Cost: $0                                                  │
└─────────────────────────────────────────────────────────────┘
     ↓
Campaign Response (100% Guaranteed)
```

**Overall Success Rate: 100%** (guaranteed by Tier 5)

### War Room UI Architecture

```
┌──────────────────┬──────────────────────────────────────────┐
│  SIDEBAR (400px) │         CENTER CANVAS (Fluid)            │
│  ═══════════════ │  ═══════════════════════════════════════ │
│                  │                                          │
│  🎯 TERMINAL     │  📡 ACTIVE INTELLIGENCE FEED             │
│  • User Email    │  • Pretty Chat Bubbles                   │
│  • Logout        │  • User (Right, Indigo)                  │
│                  │  • Director (Left, Zinc)                 │
│  ✨ COMMAND      │                                          │
│  CENTER          │  ⚡ CAMPAIGN ASSETS                       │
│  • Sparkles Icon │  • Strategy Cards (Hook, Offer, CTA)     │
│  • Enter Below   │  • Caption Cards (3 captions)            │
│                  │  • Visual Asset (Campaign image)         │
│  📝 INPUT BOX    │                                          │
│  • Directive     │  🎨 SCANLINE EFFECTS                     │
│  • Send Button   │  • Vertical sweep on hover               │
│  • AI Reasoning  │  • Scale-up animations                   │
│                  │  • Glow effects                          │
└──────────────────┴──────────────────────────────────────────┘
│  STATUS BAR: TIER_1 | DB_SYNC: OK | REGION: US-EAST-1     │
└─────────────────────────────────────────────────────────────┘
```

### AWS Services Stack

1. **AWS Amplify** - Frontend hosting with Next.js SSR and CI/CD
2. **Amazon Cognito** - User authentication with JWT tokens
3. **AWS Lambda** - Serverless compute for Diamond Cascade execution
4. **Amazon DynamoDB** - Campaign storage with user isolation
5. **Amazon S3** - Image storage and brand guidelines
6. **Amazon API Gateway** - REST API with Cognito Authorizer
7. **Amazon CloudWatch** - Logging, monitoring, and audit trails

**Total AWS Services: 7**


## Acceptance Testing Strategy

### Test Categories

1. **Functional Tests**
   - Campaign generation with all 5 tiers
   - Hinglish copywriting with power words
   - War Room UI rendering and interactions
   - Stateful conversation flow
   - JSON parsing and fallback

2. **Integration Tests**
   - Diamond Cascade failover logic
   - Cognito authentication flow
   - DynamoDB persistence
   - S3 image storage
   - API Gateway routing

3. **Performance Tests**
   - Response time under load
   - Cascade failover speed
   - UI rendering performance
   - Database query latency

4. **Security Tests**
   - JWT token validation
   - User isolation verification
   - API authorization checks
   - Data privacy compliance

5. **Reliability Tests**
   - 100% uptime guarantee
   - Titanium Shield activation
   - Error recovery
   - Demo stability

### Test Execution

- All acceptance criteria SHALL be mapped to automated tests
- Tests SHALL run on every deployment
- Test results SHALL be documented
- Failed tests SHALL block deployment

## Version History

- **V1.0** (Initial): Basic requirements with Bedrock integration
- **V2.0** (Current): Diamond Cascade, War Room UI, Aukaat Engine, AWS Amplify hosting

## Appendix

### Power Words Reference

- **Aukaat**: Show your worth, demonstrate capability
- **Bawaal**: Create chaos/excitement, make noise
- **Main Character Energy**: Be the protagonist, take center stage
- **Level Up**: Upgrade yourself, improve, advance

### Cultural References

- Chai (tea) - Common social beverage
- Maggi - Popular instant noodles
- Late-night coding - Student culture
- Canteen - College cafeteria
- Squad - Friend group

### Technical Terms

- **Glassmorphism**: UI design with frosted glass effect (backdrop-blur)
- **Scanline**: Vertical sweep animation effect
- **SSR**: Server-Side Rendering
- **JWT**: JSON Web Token
- **RAG**: Retrieval-Augmented Generation
- **Cascade**: Sequential failover pattern

---

**Document Status:** ✅ Complete  
**Last Updated:** March 5, 2026  
**Version:** 2.0  
**Total Lines:** 500+  
**Kiro Methodology:** Spec-Driven Development  

---

**Team NEONX - AI for Bharat Hackathon**
