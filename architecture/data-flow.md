# AI Tale Data Flow

## Introduction

This document describes the flow of data through the AI Tale system, from user input to the final delivery of illustrated stories. Understanding these data flows is essential for developers working on the platform and for troubleshooting issues that may arise.

## Core Data Flows

### Story Generation Flow

```
┌──────────┐     ┌──────────┐     ┌──────────┐     ┌──────────┐     ┌──────────┐
│          │     │          │     │          │     │          │     │          │
│  User    │────►│   API    │────►│  Story   │────►│ Content  │────►│   Web    │
│ Request  │     │ Gateway  │     │Generator │     │ Storage  │     │   App    │
│          │     │          │     │          │     │          │     │          │
└──────────┘     └──────────┘     └──────────┘     └──────────┘     └──────────┘
                                        │
                                        ▼
                                   ┌──────────┐     ┌──────────┐
                                   │          │     │          │
                                   │Illustrat-│────►│ Content  │
                                   │   ion    │     │ Storage  │
                                   │ Service  │     │          │
                                   └──────────┘     └──────────┘
```

#### Process Description

1. **User Request Initiation**
   - User submits a story generation request through the web or mobile application
   - Request includes parameters such as theme, age group, characters, and style preferences
   - Authentication token is included for user identification

2. **API Gateway Processing**
   - Gateway validates the authentication token
   - Request is logged for analytics
   - Rate limiting is applied based on user subscription tier
   - Request is routed to the Story Generation Service

3. **Story Generation**
   - User parameters are validated and normalized
   - Prompt is constructed for the language model
   - Story is generated using the appropriate AI model
   - Content moderation is applied to ensure appropriate content
   - Metadata (title, summary, tags) is extracted or generated
   - Story is segmented into scenes for illustration

4. **Illustration Generation**
   - Story Generation Service sends scene descriptions to Illustration Service
   - For each scene:
     - Scene text is analyzed to extract visual elements
     - Illustration prompt is constructed
     - Image is generated using appropriate AI model
     - Image is post-processed (resizing, style application)
     - Content safety check is performed

5. **Content Storage**
   - Story text is stored in the content database
   - Illustrations are stored in object storage
   - Metadata is indexed for search and retrieval
   - Relationships between story and illustrations are established

6. **Content Delivery**
   - Web/mobile application requests content from Content Storage Service
   - Content is formatted for the appropriate device
   - Illustrations are served via CDN for optimal performance
   - User interaction with content is tracked for analytics

#### Data Transformations

| Stage | Input | Transformation | Output |
|-------|-------|----------------|--------|
| User Request | User parameters (theme, characters, etc.) | Parameter validation and normalization | Validated request object |
| Story Generation | Validated request object | AI model processing | Raw story text |
| Story Post-processing | Raw story text | Segmentation, metadata extraction | Structured story with metadata |
| Illustration Generation | Scene descriptions | AI model processing | Raw images |
| Image Post-processing | Raw images | Resizing, style application | Optimized illustrations |
| Content Storage | Structured story and illustrations | Database and object storage operations | Persistent content |
| Content Delivery | Stored content | Formatting for device | Rendered storybook |

### User Registration and Authentication Flow

```
┌──────────┐     ┌──────────┐     ┌──────────┐     ┌──────────┐
│          │     │          │     │          │     │          │
│  User    │────►│   API    │────►│   User   │────►│ Database │
│ Sign-up  │     │ Gateway  │     │ Service  │     │          │
│          │     │          │     │          │     │          │
└──────────┘     └──────────┘     └──────────┘     └──────────┘
                                        │
                                        ▼
                                   ┌──────────┐
                                   │          │
                                   │  Email   │
                                   │ Service  │
                                   │          │
                                   └──────────┘
```

#### Process Description

1. **User Registration**
   - User submits registration information (email, password, etc.)
   - API Gateway routes request to User Management Service
   - User Management Service validates input data
   - Password is hashed using Argon2
   - User record is created in database
   - Verification email is sent via Email Service
   - JWT token is generated and returned to user

2. **User Authentication**
   - User submits login credentials
   - API Gateway routes request to User Management Service
   - User Management Service validates credentials
   - JWT token is generated with appropriate claims
   - Token is returned to client application
   - Login event is recorded for security monitoring

3. **Token Validation**
   - Client includes JWT token in API requests
   - API Gateway validates token signature and expiration
   - User identity and permissions are extracted from token
   - Request is enriched with user context
   - Request is forwarded to appropriate service

### Subscription Management Flow

```
┌──────────┐     ┌──────────┐     ┌──────────┐     ┌──────────┐
│          │     │          │     │          │     │          │
│  User    │────►│   API    │────►│   User   │────►│ Payment  │
│ Subscribe│     │ Gateway  │     │ Service  │     │ Processor│
│          │     │          │     │          │     │          │
└──────────┘     └──────────┘     └──────────┘     └──────────┘
                                        │
                                        ▼
                                   ┌──────────┐     ┌──────────┐
                                   │          │     │          │
                                   │ Database │◄───►│ Analytics│
                                   │          │     │ Service  │
                                   │          │     │          │
                                   └──────────┘     └──────────┘
```

#### Process Description

1. **Subscription Purchase**
   - User selects subscription plan
   - API Gateway routes request to User Management Service
   - User Management Service initiates payment process
   - Payment processor handles transaction
   - On successful payment, subscription is recorded in database
   - User permissions are updated
   - Subscription event is sent to Analytics Service

2. **Subscription Renewal**
   - Scheduled job identifies subscriptions due for renewal
   - User Management Service initiates renewal payment
   - Payment processor handles transaction
   - Subscription record is updated based on payment result
   - User is notified of successful renewal or payment issue

3. **Subscription Cancellation**
   - User requests subscription cancellation
   - User Management Service updates subscription status
   - Scheduled cancellation date is set (typically end of billing period)
   - Cancellation event is sent to Analytics Service
   - User is notified of cancellation confirmation

## Event-Driven Data Flows

In addition to the request-response flows described above, the AI Tale system uses event-driven architecture for certain operations.

### Content Generation Events

```
┌──────────┐     ┌──────────┐     ┌──────────┐
│          │     │          │     │          │
│  Story   │────►│  Event   │────►│Illustrat-│
│ Generator│     │   Bus    │     │   ion    │
│          │     │          │     │ Service  │
└──────────┘     └──────────┘     └──────────┘
                      │
                      ▼
                 ┌──────────┐     ┌──────────┐
                 │          │     │          │
                 │Analytics │     │Notificat-│
                 │ Service  │     │   ion    │
                 │          │     │ Service  │
                 └──────────┘     └──────────┘
```

#### Events and Handlers

| Event | Producer | Consumers | Purpose |
|-------|----------|-----------|--------|
| `story.created` | Story Generation Service | Illustration Service | Trigger illustration generation |
| `story.created` | Story Generation Service | Analytics Service | Track story creation metrics |
| `illustration.created` | Illustration Service | Content Storage Service | Store and link illustrations |
| `illustration.created` | Illustration Service | Analytics Service | Track illustration metrics |
| `story.completed` | Content Storage Service | Notification Service | Notify user of completed story |

### Analytics Events

```
┌──────────┐     ┌──────────┐     ┌──────────┐
│          │     │          │     │          │
│   All    │────►│  Event   │────►│Analytics │
│ Services │     │   Bus    │     │ Service  │
│          │     │          │     │          │
└──────────┘     └──────────┘     └──────────┘
                                        │
                                        ▼
                                   ┌──────────┐
                                   │          │
                                   │ Analytics│
                                   │ Database │
                                   │          │
                                   └──────────┘
```

#### Analytics Event Types

| Event Category | Examples | Data Points | Consumers |
|----------------|----------|-------------|----------|
| User Events | `user.registered`, `user.login`, `user.subscription.changed` | User ID, timestamp, device info | User analytics, marketing |
| Content Events | `story.created`, `story.viewed`, `story.shared` | Story ID, user ID, parameters | Content analytics, recommendations |
| Performance Events | `api.request`, `model.inference` | Duration, resource usage, error rate | System monitoring, capacity planning |
| Error Events | `error.api`, `error.model`, `error.database` | Error type, stack trace, user impact | Error monitoring, alerting |

## Data Storage

### Database Schema Overview

The AI Tale system uses multiple databases optimized for different types of data:

#### User Database (PostgreSQL)

Stores user information, authentication, and subscription data:

- `users`: User profiles and authentication information
- `subscriptions`: User subscription details
- `preferences`: User preferences for story generation
- `api_keys`: Developer API keys and usage limits

#### Content Database (MongoDB)

Stores story content and metadata:

- `stories`: Story text, metadata, and generation parameters
- `illustrations`: Metadata about illustrations (actual images in object storage)
- `collections`: User-created collections of stories
- `templates`: Story templates and presets

#### Analytics Database (ClickHouse)

Stores time-series data for analytics:

- `user_events`: User interaction events
- `content_events`: Content creation and consumption events
- `performance_metrics`: System performance data
- `error_events`: System and application errors

### Object Storage

Amazon S3 is used for storing binary data:

- `illustrations`: Generated illustrations in various formats and resolutions
- `user-uploads`: User-uploaded content (profile pictures, custom illustrations)
- `exports`: Exported stories in PDF and other formats
- `backups`: Database backups and system snapshots

## Data Retention and Lifecycle

### Data Retention Policies

| Data Type | Retention Period | Archival Strategy | Deletion Process |
|-----------|------------------|-------------------|------------------|
| User Accounts | Until deletion request or 7 years of inactivity | Anonymized after 2 years of inactivity | Soft delete, then hard delete after 30 days |
| Story Content | Indefinite for active users | Compressed storage after 1 year | Deleted with user account or on explicit request |
| Illustrations | Indefinite for active users | Lower resolution after 1 year | Deleted with associated story |
| Analytics Data | Raw data: 90 days, Aggregated: 7 years | Aggregation and summarization | Automated purge of raw data |
| System Logs | 30 days | Archived for 1 year | Automated purge |

### Data Backup Strategy

| Data Store | Backup Frequency | Retention | Recovery Method |
|------------|------------------|-----------|------------------|
| User Database | Continuous with point-in-time recovery | 30 days | Database restore |
| Content Database | Daily full, hourly incremental | 30 days | Database restore |
| Object Storage | Cross-region replication | Indefinite | S3 replication |
| Configuration | Version controlled | Full history | Infrastructure as code deployment |

## Data Security

### Data Classification

| Classification | Examples | Security Controls | Access Restrictions |
|----------------|----------|-------------------|--------------------|
| Public | Published stories, public user profiles | Integrity controls | No restrictions |
| Internal | Aggregated analytics, system metrics | Access controls, encryption at rest | Authenticated staff only |
| Confidential | User emails, subscription details | Encryption, access logging | Need-to-know basis |
| Restricted | Passwords, payment information | Strong encryption, tokenization | Minimal access, audit logging |

### Encryption Strategy

| Data Type | Encryption at Rest | Encryption in Transit | Key Management |
|-----------|--------------------|-----------------------|---------------|
| User credentials | Argon2 hashing | TLS 1.3 | N/A (one-way hashing) |
| Personal information | AES-256 | TLS 1.3 | AWS KMS with rotation |
| Payment information | Tokenization (via payment processor) | TLS 1.3 | Managed by payment processor |
| Content data | AES-256 for sensitive content | TLS 1.3 | AWS KMS with rotation |

## Conclusion

The data flows in the AI Tale system are designed to ensure efficient processing, secure storage, and reliable delivery of content. Understanding these flows is essential for developers working on the platform, as well as for troubleshooting and optimizing system performance. As the platform evolves, these data flows may be refined to accommodate new features and improved processing techniques.