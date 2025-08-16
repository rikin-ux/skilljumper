# SkillJumper API Documentation

## Overview

The SkillJumper Core Services Platform provides REST and GraphQL APIs for the mobile application and external integrations. All APIs are designed with privacy-first principles and neurodivergent accessibility in mind.

**Base URL**: `https://api.skilljumper.com/v1`  
**Authentication**: OAuth 2.0 with guardian consent flows  
**Rate Limiting**: 1000 requests/hour per user  
**Response Format**: JSON with consistent error handling

## Authentication

### OAuth 2.0 Flow

```http
POST /auth/token
Content-Type: application/json

{
  "grant_type": "authorization_code",
  "client_id": "your_client_id",
  "client_secret": "your_client_secret",
  "code": "authorization_code",
  "redirect_uri": "your_redirect_uri"
}
```

**Response:**
```json
{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "token_type": "Bearer",
  "expires_in": 3600,
  "refresh_token": "refresh_token_here",
  "scope": "read write"
}
```

### Guardian Consent (Under 18)

```http
POST /auth/guardian-consent
Authorization: Bearer {access_token}
Content-Type: application/json

{
  "guardian_email": "parent@example.com",
  "young_person_id": "user_123",
  "consent_level": "full" // "full", "limited", "view_only"
}
```

## Core Endpoints

### Quest Recommendation Engine

#### Get Quest Recommendation

```http
POST /quests/recommend
Authorization: Bearer {access_token}
Content-Type: application/json

{
  "user_context": {
    "energy_level": "medium",
    "available_time_minutes": 15,
    "current_location": "home",
    "emotional_state": "neutral",
    "sensory_environment": "calm"
  },
  "preferences": {
    "difficulty_preference": "adaptive",
    "support_level": "moderate",
    "interaction_style": "visual"
  },
  "constraints": {
    "physical_limitations": [],
    "time_constraints": {
      "must_complete_by": "2025-08-16T15:00:00Z"
    },
    "location_constraints": {
      "indoor_only": true
    }
  }
}
```

**Response:**
```json
{
  "quest": {
    "id": "quest_12345",
    "title": "Organize Your Study Space",
    "description": "Create a calm, organized environment for learning",
    "estimated_duration_minutes": 12,
    "difficulty_level": 3,
    "categories": ["organization", "executive_function"],
    "adaptations": [
      {
        "type": "visual_support",
        "description": "Step-by-step photo guide included"
      },
      {
        "type": "time_modification", 
        "description": "Built-in 2-minute break after 5 minutes"
      }
    ]
  },
  "confidence_score": 87,
  "estimated_success_rate": 0.78,
  "reasoning_explanation": "Selected based on your current energy level and past success with organization tasks",
  "support_recommendations": [
    {
      "type": "environmental",
      "suggestion": "Play background music or nature sounds"
    },
    {
      "type": "social",
      "suggestion": "Consider inviting a family member to help"
    }
  ],
  "fallback_options": [
    {
      "quest_id": "quest_67890",
      "title": "5-Minute Desk Clear",
      "reason": "Shorter alternative if energy drops"
    }
  ]
}
```

#### Submit Quest Feedback

```http
POST /quests/{quest_id}/feedback
Authorization: Bearer {access_token}
Content-Type: application/json

{
  "outcome": "completed", // "completed", "partially_completed", "abandoned", "postponed", "failed"
  "completion_percentage": 85,
  "duration_minutes": 10,
  "user_feedback": {
    "difficulty_rating": 3, // 1-5 scale
    "enjoyment_rating": 4,
    "support_effectiveness": 4,
    "would_recommend": true,
    "open_feedback": "The visual guide was really helpful!"
  },
  "adaptations_used": [
    {
      "adaptation_id": "visual_support_001",
      "effectiveness_rating": 5
    }
  ],
  "context_during_attempt": {
    "energy_level": "medium",
    "interruptions": 1,
    "environment_changes": []
  }
}
```

**Response:**
```json
{
  "feedback_id": "feedback_789",
  "learning_model_updated": true,
  "next_recommendation_adjusted": true,
  "achievements_unlocked": [
    {
      "id": "organization_novice",
      "title": "Organization Novice",
      "description": "Completed 5 organization quests"
    }
  ]
}
```

### User Profile Management

#### Get User Profile

```http
GET /users/profile
Authorization: Bearer {access_token}
```

**Response:**
```json
{
  "user_id": "user_123",
  "basic_info": {
    "age": 16,
    "neurodivergent_conditions": ["ADHD", "Executive Function Challenges"],
    "support_preferences": {
      "communication_style": "direct",
      "feedback_frequency": "immediate",
      "visual_support_preference": "high"
    }
  },
  "learning_model": {
    "strengths": ["visual_processing", "creative_thinking"],
    "challenges": ["time_management", "task_initiation"],
    "optimal_conditions": {
      "time_of_day": "morning",
      "session_length": "10-15_minutes",
      "environment": "quiet_structured"
    }
  },
  "progress_summary": {
    "quests_completed": 47,
    "success_rate": 0.73,
    "favorite_categories": ["creativity", "social_skills"],
    "streak": {
      "current": 5,
      "longest": 12
    }
  },
  "privacy_settings": {
    "data_sharing_consent": "research_only",
    "guardian_visibility": "progress_summary",
    "peer_interaction": "opt_in"
  }
}
```

#### Update User Preferences

```http
PUT /users/preferences
Authorization: Bearer {access_token}
Content-Type: application/json

{
  "support_preferences": {
    "reminder_frequency": "daily",
    "challenge_level": "gradual_increase",
    "social_features": "friends_only"
  },
  "accessibility_settings": {
    "text_size": "large",
    "high_contrast": true,
    "reduced_motion": false,
    "audio_descriptions": true
  },
  "notification_preferences": {
    "quest_reminders": true,
    "achievement_celebrations": true,
    "progress_updates": "weekly"
  }
}
```

### Progress Tracking

#### Get Progress Analytics

```http
GET /users/progress?period=week&include_details=true
Authorization: Bearer {access_token}
```

**Response:**
```json
{
  "period": "2025-08-10T00:00:00Z to 2025-08-16T23:59:59Z",
  "summary": {
    "quests_attempted": 8,
    "quests_completed": 6,
    "success_rate": 0.75,
    "total_time_minutes": 95,
    "categories_explored": ["organization", "social_skills", "self_care"]
  },
  "daily_breakdown": [
    {
      "date": "2025-08-10",
      "quests_completed": 1,
      "mood_avg": 4.2,
      "energy_pattern": "high_morning"
    }
  ],
  "insights": [
    {
      "type": "pattern_recognition",
      "insight": "You perform best on organization tasks in the morning",
      "confidence": 0.85
    },
    {
      "type": "recommendation",
      "insight": "Consider trying social skills quests when energy is medium-high",
      "reasoning": "Based on successful pattern from last month"
    }
  ]
}
```

### Content Management

#### Get Available Quests

```http
GET /quests?category=organization&difficulty=1-3&duration_max=20
Authorization: Bearer {access_token}
```

**Response:**
```json
{
  "quests": [
    {
      "id": "quest_001",
      "title": "10-Minute Room Reset",
      "category": "organization",
      "difficulty": 2,
      "estimated_duration": 10,
      "prerequisites": [],
      "adaptations_available": ["visual", "auditory", "step_reduction"],
      "safety_level": "low_risk",
      "required_materials": ["none"]
    }
  ],
  "pagination": {
    "total": 156,
    "page": 1,
    "per_page": 20,
    "has_more": true
  },
  "filters_applied": {
    "category": "organization",
    "difficulty": "1-3",
    "duration_max": 20
  }
}
```

## GraphQL API

### Schema Overview

```graphql
type Query {
  user: User
  questRecommendation(criteria: QuestCriteria!): QuestRecommendation
  quests(filters: QuestFilters): [Quest!]!
  userProgress(period: TimePeriod): ProgressAnalytics
}

type Mutation {
  updateUserPreferences(input: UserPreferencesInput!): User
  submitQuestFeedback(input: QuestFeedbackInput!): FeedbackResponse
  createGuardianConnection(input: GuardianConnectionInput!): GuardianConnection
}

type User {
  id: ID!
  profile: UserProfile!
  learningModel: LearningModel!
  progress: ProgressSummary!
  preferences: UserPreferences!
}

type Quest {
  id: ID!
  title: String!
  description: String!
  category: QuestCategory!
  difficulty: Int!
  estimatedDuration: Int!
  adaptations: [Adaptation!]!
  safetyLevel: SafetyLevel!
}

type QuestRecommendation {
  quest: Quest!
  confidenceScore: Float!
  estimatedSuccessRate: Float!
  reasoning: String!
  supportRecommendations: [SupportRecommendation!]!
  fallbackOptions: [Quest!]!
}
```

### Example GraphQL Queries

#### Get Personalized Quest Recommendation

```graphql
query GetQuestRecommendation($criteria: QuestCriteria!) {
  questRecommendation(criteria: $criteria) {
    quest {
      id
      title
      description
      difficulty
      estimatedDuration
      adaptations {
        type
        description
      }
    }
    confidenceScore
    estimatedSuccessRate
    reasoning
    supportRecommendations {
      type
      suggestion
    }
    fallbackOptions {
      id
      title
      estimatedDuration
    }
  }
}
```

**Variables:**
```json
{
  "criteria": {
    "userContext": {
      "energyLevel": "MEDIUM",
      "availableTimeMinutes": 15,
      "currentLocation": "HOME"
    },
    "preferences": {
      "difficultyPreference": "ADAPTIVE",
      "supportLevel": "MODERATE"
    }
  }
}
```

## Error Handling

### Standard Error Response

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid quest criteria provided",
    "details": {
      "field": "available_time_minutes",
      "issue": "Must be between 1 and 120 minutes"
    },
    "request_id": "req_12345",
    "timestamp": "2025-08-16T10:30:00Z"
  }
}
```

### Error Codes

| Code | HTTP Status | Description |
|------|-------------|-------------|
| `INVALID_TOKEN` | 401 | Authentication token is invalid or expired |
| `INSUFFICIENT_PERMISSIONS` | 403 | User lacks required permissions |
| `USER_NOT_FOUND` | 404 | User profile not found |
| `QUEST_NOT_FOUND` | 404 | Requested quest does not exist |
| `VALIDATION_ERROR` | 400 | Request data validation failed |
| `RATE_LIMIT_EXCEEDED` | 429 | Too many requests in time window |
| `GUARDIAN_CONSENT_REQUIRED` | 451 | Action requires guardian approval |
| `ALGORITHM_ERROR` | 500 | Quest recommendation engine error |
| `DATABASE_ERROR` | 500 | Internal database error |

## Rate Limiting

### Limits by Endpoint

| Endpoint | Limit | Window |
|----------|--------|--------|
| `/quests/recommend` | 60 requests | 1 hour |
| `/quests/*/feedback` | 100 requests | 1 hour |
| `/users/progress` | 120 requests | 1 hour |
| All other endpoints | 1000 requests | 1 hour |

### Rate Limit Headers

```http
X-RateLimit-Limit: 60
X-RateLimit-Remaining: 45
X-RateLimit-Reset: 1692187200
```

## Webhooks

### Quest Completion Events

```http
POST https://your-app.com/webhooks/quest-completed
Content-Type: application/json
X-SkillJumper-Signature: sha256=...

{
  "event": "quest.completed",
  "timestamp": "2025-08-16T10:30:00Z",
  "user_id": "user_123",
  "quest_id": "quest_456",
  "data": {
    "completion_percentage": 100,
    "duration_minutes": 12,
    "success_factors": ["visual_support", "appropriate_timing"]
  }
}
```

## SDK Examples

### JavaScript/TypeScript

```typescript
import { SkillJumperClient } from '@skilljumper/sdk';

const client = new SkillJumperClient({
  apiKey: 'your_api_key',
  environment: 'production' // or 'sandbox'
});

// Get quest recommendation
const recommendation = await client.quests.getRecommendation({
  userContext: {
    energyLevel: 'medium',
    availableTimeMinutes: 15
  }
});

// Submit feedback
await client.quests.submitFeedback(questId, {
  outcome: 'completed',
  userFeedback: {
    difficultyRating: 3,
    enjoymentRating: 4
  }
});
```

### Python

```python
from skilljumper import SkillJumperClient

client = SkillJumperClient(
    api_key='your_api_key',
    environment='production'
)

# Get quest recommendation
recommendation = client.quests.get_recommendation(
    user_context={
        'energy_level': 'medium',
        'available_time_minutes': 15
    }
)

# Submit feedback
client.quests.submit_feedback(quest_id, {
    'outcome': 'completed',
    'user_feedback': {
        'difficulty_rating': 3,
        'enjoyment_rating': 4
    }
})
```

## Testing

### Sandbox Environment

**Base URL**: `https://api-sandbox.skilljumper.com/v1`

Use test credentials:
- **Client ID**: `test_client_123`
- **Client Secret**: `test_secret_456`

### Test Users

Predefined test users with different profiles:

```json
{
  "test_user_adhd": {
    "id": "test_001",
    "conditions": ["ADHD"],
    "age": 14
  },
  "test_user_autism": {
    "id": "test_002", 
    "conditions": ["Autism"],
    "age": 18
  }
}
```

## Support

### Developer Support
- **Documentation**: [docs.skilljumper.com](https://docs.skilljumper.com)
- **API Status**: [status.skilljumper.com](https://status.skilljumper.com)
- **Discord**: [discord.gg/skilljumper-dev](https://discord.gg/skilljumper-dev)
- **Email**: api-support@skilljumper.com

### SLA & Availability
- **Uptime**: 99.9% guaranteed
- **Response Time**: <200ms for 95% of requests
- **Support Response**: <24 hours for technical issues