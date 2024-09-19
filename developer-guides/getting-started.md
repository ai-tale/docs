# Getting Started with AI Tale

## Introduction

Welcome to AI Tale! This guide will help you get started with integrating our AI-powered fairy tale generation platform into your applications. Whether you're building a children's app, an educational tool, or a creative writing platform, AI Tale can help you generate engaging, illustrated stories.

## Prerequisites

Before you begin, make sure you have:

- An AI Tale developer account (sign up at [https://developer.aitale.tech](https://developer.aitale.tech))
- An API key (available in your developer dashboard)
- Basic knowledge of REST APIs
- A development environment with your preferred programming language

## Quick Start

Here's a quick example to generate a simple fairy tale:

### 1. Authentication

All requests to the AI Tale API require authentication using your API key. Include it in the `Authorization` header of your requests:

```http
Authorization: Bearer YOUR_API_KEY
```

### 2. Generate a Story

To generate a basic story, make a POST request to the story generation endpoint:

```bash
curl -X POST https://api.aitale.tech/v1/stories \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "theme": "forest adventure",
    "age_group": "children",
    "characters": [
      {
        "name": "Luna",
        "type": "child",
        "traits": ["curious", "brave"]
      },
      {
        "name": "Whiskers",
        "type": "talking_animal",
        "species": "fox",
        "traits": ["wise", "friendly"]
      }
    ],
    "length": "short",
    "illustration_style": "watercolor"
  }'
```

### 3. Response

The API will respond with a story generation job ID:

```json
{
  "job_id": "story_gen_123456",
  "status": "processing",
  "estimated_completion_time": 30
}
```

### 4. Check Generation Status

Story generation can take some time, especially for longer stories with illustrations. Check the status using the job ID:

```bash
curl https://api.aitale.tech/v1/jobs/story_gen_123456 \
  -H "Authorization: Bearer YOUR_API_KEY"
```

### 5. Retrieve the Completed Story

Once the status is `completed`, you can retrieve the full story:

```bash
curl https://api.aitale.tech/v1/stories/story_gen_123456 \
  -H "Authorization: Bearer YOUR_API_KEY"
```

Response:

```json
{
  "id": "story_123456",
  "title": "Luna and the Wise Fox",
  "content": {
    "sections": [
      {
        "text": "Once upon a time, in a village beside an enchanted forest, there lived a curious girl named Luna...",
        "illustration_url": "https://cdn.aitale.tech/illustrations/story_123456/section_1.jpg"
      },
      // More sections...
    ]
  },
  "metadata": {
    "word_count": 512,
    "reading_time": 4,
    "age_group": "children",
    "themes": ["adventure", "friendship", "nature"]
  },
  "created_at": "2023-06-15T14:32:10Z"
}
```

## SDK Installation

For easier integration, we provide SDKs for popular programming languages:

### JavaScript/TypeScript

```bash
npm install aitale-sdk
# or
yarn add aitale-sdk
```

Usage:

```javascript
import { AITaleClient } from 'aitale-sdk';

const client = new AITaleClient('YOUR_API_KEY');

async function generateStory() {
  // Start story generation
  const job = await client.stories.create({
    theme: 'forest adventure',
    age_group: 'children',
    characters: [
      {
        name: 'Luna',
        type: 'child',
        traits: ['curious', 'brave']
      },
      {
        name: 'Whiskers',
        type: 'talking_animal',
        species: 'fox',
        traits: ['wise', 'friendly']
      }
    ],
    length: 'short',
    illustration_style: 'watercolor'
  });
  
  console.log(`Story generation started: ${job.job_id}`);
  
  // Wait for completion
  const story = await client.jobs.waitForCompletion(job.job_id);
  
  console.log('Story generated successfully:');
  console.log(`Title: ${story.title}`);
  console.log(`First section: ${story.content.sections[0].text}`);
}

generateStory();
```

### Python

```bash
pip install aitale-sdk
```

Usage:

```python
from aitale import AITaleClient
import time

# Initialize client
client = AITaleClient(api_key="YOUR_API_KEY")

# Create a story generation job
job = client.stories.create(
    theme="forest adventure",
    age_group="children",
    characters=[
        {
            "name": "Luna",
            "type": "child",
            "traits": ["curious", "brave"]
        },
        {
            "name": "Whiskers",
            "type": "talking_animal",
            "species": "fox",
            "traits": ["wise", "friendly"]
        }
    ],
    length="short",
    illustration_style="watercolor"
)

print(f"Story generation started: {job['job_id']}")

# Poll for completion
while True:
    status = client.jobs.get(job['job_id'])
    if status['status'] == 'completed':
        break
    print(f"Status: {status['status']} - waiting...")
    time.sleep(5)

# Get the completed story
story = client.stories.get(job['job_id'])

print("Story generated successfully:")
print(f"Title: {story['title']}")
print(f"First section: {story['content']['sections'][0]['text']}")
```

## Core Concepts

### Story Parameters

AI Tale offers extensive customization options for story generation:

| Parameter | Description | Example Values |
|-----------|-------------|----------------|
| `theme` | The main theme of the story | "space adventure", "underwater journey", "magical forest" |
| `age_group` | Target audience age | "toddler", "children", "middle_grade", "young_adult" |
| `characters` | Array of character objects | See character structure below |
| `setting` | Where and when the story takes place | "medieval castle", "future city", "enchanted forest" |
| `length` | Approximate story length | "very_short", "short", "medium", "long" |
| `illustration_style` | Visual style for illustrations | "watercolor", "cartoon", "realistic", "pixel_art" |
| `moral` | Optional moral or lesson | "friendship", "courage", "honesty" |
| `language` | Story language | "en", "es", "fr", "de", etc. |

### Character Structure

Characters are defined with the following properties:

```json
{
  "name": "Character name",
  "type": "human/animal/magical/etc.",
  "traits": ["trait1", "trait2"],
  "description": "Optional detailed description",
  "species": "For animal characters",
  "age": "Optional age information",
  "role": "protagonist/antagonist/supporting"
}
```

### Illustration Styles

AI Tale supports various illustration styles:

- `watercolor`: Soft, artistic watercolor paintings
- `cartoon`: Bright, child-friendly cartoon style
- `pixel_art`: Retro pixel art style
- `realistic`: More detailed, realistic illustrations
- `sketch`: Hand-drawn sketch appearance
- `storybook`: Classic storybook illustration style
- `comic`: Comic book style with bold lines
- `3d`: Three-dimensional rendered style

## Advanced Usage

### Story Templates

For consistent story generation, you can create and use templates:

```javascript
// Create a template
const template = await client.templates.create({
  name: "Forest Adventures",
  base_parameters: {
    theme: "forest",
    setting: "enchanted woodland",
    illustration_style: "watercolor",
    age_group: "children"
  }
});

// Generate a story using the template
const job = await client.stories.createFromTemplate(template.id, {
  // Override or add specific parameters
  characters: [
    {
      name: "Oliver",
      type: "child",
      traits: ["adventurous", "kind"]
    }
  ]
});
```

### Story Continuation

You can generate continuations of existing stories:

```javascript
const continuation = await client.stories.continue({
  story_id: "story_123456",
  continuation_prompt: "What happened after Oliver met the talking owl?",
  length: "short"
});
```

### Batch Generation

For generating multiple stories efficiently:

```javascript
const batch = await client.stories.createBatch([
  {
    theme: "space adventure",
    characters: [{ name: "Astro", type: "astronaut" }]
  },
  {
    theme: "underwater journey",
    characters: [{ name: "Marina", type: "mermaid" }]
  }
]);

console.log(`Batch ID: ${batch.batch_id}`);
```

## Webhooks

Instead of polling for job completion, you can set up webhooks to receive notifications:

```javascript
// Register a webhook endpoint
const webhook = await client.webhooks.create({
  url: "https://your-app.com/api/aitale-webhook",
  events: ["story.completed", "illustration.completed"],
  secret: "your_webhook_secret" // Used to verify webhook signatures
});

// Generate a story with webhook notification
const job = await client.stories.create({
  theme: "underwater adventure",
  // other parameters...
  webhook_id: webhook.id
});
```

Your webhook endpoint will receive a POST request when the story is completed:

```json
{
  "event": "story.completed",
  "job_id": "story_gen_123456",
  "story_id": "story_123456",
  "timestamp": "2023-06-15T14:40:23Z"
}
```

## Rate Limits and Quotas

API usage is subject to rate limits based on your subscription plan:

| Plan | Stories per Day | Requests per Minute | Concurrent Jobs |
|------|-----------------|---------------------|----------------|
| Free | 10 | 60 | 1 |
| Basic | 100 | 120 | 5 |
| Professional | 1,000 | 300 | 20 |
| Enterprise | Custom | Custom | Custom |

Rate limit headers are included in API responses:

```http
X-RateLimit-Limit: 60
X-RateLimit-Remaining: 58
X-RateLimit-Reset: 1623761452
```

## Error Handling

The API uses standard HTTP status codes and returns detailed error information:

```json
{
  "error": {
    "code": "invalid_parameters",
    "message": "Invalid character type provided",
    "details": {
      "field": "characters[1].type",
      "allowed_values": ["human", "animal", "magical_creature", "talking_animal"]
    }
  }
}
```

Common error codes:

- `authentication_error`: Invalid API key
- `invalid_parameters`: Invalid request parameters
- `rate_limit_exceeded`: You've exceeded your rate limit
- `quota_exceeded`: You've exceeded your daily quota
- `server_error`: Internal server error

## Next Steps

Now that you're familiar with the basics, you can:

1. Explore the [API Reference](../api-reference/story-generation.md) for detailed endpoint documentation
2. Check out our [Best Practices](best-practices.md) guide for optimization tips
3. Join our [Developer Community](https://community.aitale.tech) to connect with other developers
4. View [Sample Applications](https://github.com/ai-tale/examples) for inspiration

If you have any questions or need assistance, contact our developer support team at dev-support@aitale.tech.