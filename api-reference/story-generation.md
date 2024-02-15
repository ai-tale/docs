# Story Generation API

## Overview

The Story Generation API allows you to create AI-generated fairy tales and stories with customizable parameters. This document provides detailed information about the endpoints, request parameters, and response formats for story generation.

## Endpoints

### Generate Story

```http
POST /v1/stories
```

Generates a new story based on the provided parameters.

#### Request Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `theme` | string | Yes | The main theme of the story |
| `age_group` | string | Yes | Target audience age group |
| `characters` | array | Yes | Array of character objects |
| `length` | string | No | Approximate story length (default: "medium") |
| `setting` | object | No | Details about the story setting |
| `plot_elements` | array | No | Key plot elements to include |
| `tone` | string | No | Emotional tone of the story |
| `language_style` | string | No | Style of language to use |
| `moral_theme` | string | No | Moral or lesson of the story |
| `illustration_style` | string | No | Style for generated illustrations |
| `language` | string | No | Language code (default: "en") |
| `webhook_url` | string | No | URL to receive completion notification |
| `metadata` | object | No | Custom metadata to associate with the story |

##### Character Object

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `name` | string | Yes | Character's name |
| `type` | string | Yes | Character type (human, animal, magical_creature, etc.) |
| `traits` | array | No | Array of character personality traits |
| `description` | string | No | Detailed description of the character |
| `role` | string | No | Character's role in the story (protagonist, antagonist, etc.) |

##### Setting Object

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `location` | string | Yes | Primary location of the story |
| `time_period` | string | No | Time period or era |
| `description` | string | No | Detailed description of the setting |

#### Example Request

```json
{
  "theme": "underwater adventure",
  "age_group": "children",
  "characters": [
    {
      "name": "Marina",
      "type": "human",
      "traits": ["curious", "brave", "kind"],
      "description": "A 10-year-old girl with flowing blue hair and a silver shell necklace",
      "role": "protagonist"
    },
    {
      "name": "Bubbles",
      "type": "talking_animal",
      "traits": ["loyal", "playful", "wise"],
      "description": "A friendly dolphin with a star-shaped mark on his fin",
      "role": "companion"
    },
    {
      "name": "Captain Darkwater",
      "type": "human",
      "traits": ["greedy", "cunning"],
      "description": "A treasure hunter with a mechanical diving suit",
      "role": "antagonist"
    }
  ],
  "setting": {
    "location": "Coral Kingdom",
    "time_period": "modern with magical elements",
    "description": "A vibrant underwater world with colorful coral cities and ancient ruins"
  },
  "plot_elements": [
    "discovery of a magical pearl",
    "a journey to the deepest trench",
    "making friends with sea creatures"
  ],
  "tone": "adventurous with moments of humor",
  "length": "medium",
  "language_style": "simple but engaging",
  "moral_theme": "friendship and environmental protection",
  "illustration_style": "watercolor",
  "language": "en"
}
```

#### Response

```json
{
  "job_id": "story_gen_123456",
  "status": "processing",
  "estimated_completion_time": 30
}
```

### Get Story Generation Status

```http
GET /v1/stories/jobs/{job_id}
```

Retrieve the status of a story generation job.

#### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `job_id` | string | Yes | The ID of the story generation job |

#### Response

```json
{
  "job_id": "story_gen_123456",
  "status": "completed",
  "created_at": "2023-06-15T14:32:10Z",
  "completed_at": "2023-06-15T14:33:25Z",
  "story_id": "story_123456"
}
```

Possible status values:
- `processing`: The story is being generated
- `illustrating`: The story text is complete and illustrations are being generated
- `completed`: The story and illustrations are complete
- `failed`: The story generation failed

### Get Story

```http
GET /v1/stories/{story_id}
```

Retrieve a generated story by its ID.

#### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `story_id` | string | Yes | The ID of the story |

#### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `include_illustrations` | boolean | No | Whether to include illustration data (default: true) |
| `include_metadata` | boolean | No | Whether to include metadata (default: true) |

#### Response

```json
{
  "id": "story_123456",
  "title": "Marina and the Magical Pearl",
  "created_at": "2023-06-15T14:33:25Z",
  "content": {
    "sections": [
      {
        "type": "introduction",
        "text": "Once upon a time, in a small coastal village, there lived a curious girl named Marina. She had flowing blue hair that reminded everyone of the ocean waves, and she always wore a silver shell necklace that her grandmother had given her...",
        "illustration_id": "illust_12345",
        "illustration_url": "https://cdn.aitale.tech/illustrations/illust_12345.jpg"
      },
      {
        "type": "chapter",
        "text": "One sunny morning, Marina was walking along the beach when she spotted something glimmering in the shallow water. She waded in and discovered a small, iridescent pearl unlike any she had ever seen before...",
        "illustration_id": "illust_12346",
        "illustration_url": "https://cdn.aitale.tech/illustrations/illust_12346.jpg"
      },
      // More sections...
      {
        "type": "conclusion",
        "text": "And so, Marina returned the magical pearl to its rightful place in the Coral Kingdom. The sea creatures celebrated her bravery, and Bubbles promised to visit her often. Marina knew that she had made friends she would treasure forever, and that the ocean held many more adventures waiting to be discovered.",
        "illustration_id": "illust_12350",
        "illustration_url": "https://cdn.aitale.tech/illustrations/illust_12350.jpg"
      }
    ]
  },
  "summary": "A brave girl named Marina discovers a magical pearl that allows her to breathe underwater. With her new friend Bubbles the dolphin, she embarks on an adventure to the Coral Kingdom to return the pearl to its rightful place, while stopping the greedy Captain Darkwater from stealing the kingdom's treasures.",
  "metadata": {
    "word_count": 2450,
    "reading_time": 12,
    "age_group": "children",
    "themes": ["adventure", "friendship", "environmental protection"],
    "characters": ["Marina", "Bubbles", "Captain Darkwater"],
    "language": "en"
  },
  "parameters": {
    "theme": "underwater adventure",
    "age_group": "children",
    "length": "medium",
    "illustration_style": "watercolor"
    // Original request parameters...
  }
}
```

### List Stories

```http
GET /v1/stories
```

Retrieve a list of generated stories, with optional filtering.

#### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `theme` | string | No | Filter by theme |
| `age_group` | string | No | Filter by age group |
| `created_after` | string | No | Filter by creation date (ISO 8601 format) |
| `created_before` | string | No | Filter by creation date (ISO 8601 format) |
| `limit` | integer | No | Maximum number of results to return (default: 20, max: 100) |
| `offset` | integer | No | Number of results to skip (for pagination) |

#### Response

```json
{
  "stories": [
    {
      "id": "story_123456",
      "title": "Marina and the Magical Pearl",
      "created_at": "2023-06-15T14:33:25Z",
      "summary": "A brave girl named Marina discovers a magical pearl that allows her to breathe underwater...",
      "metadata": {
        "word_count": 2450,
        "age_group": "children",
        "themes": ["adventure", "friendship", "environmental protection"]
      }
    },
    {
      "id": "story_123457",
      "title": "The Dragon's Riddle",
      "created_at": "2023-06-14T10:15:30Z",
      "summary": "A clever young wizard must solve a dragon's riddle to save his village...",
      "metadata": {
        "word_count": 3200,
        "age_group": "middle_grade",
        "themes": ["adventure", "problem-solving", "courage"]
      }
    }
  ],
  "total": 42,
  "limit": 20,
  "offset": 0
}
```

### Delete Story

```http
DELETE /v1/stories/{story_id}
```

Delete a specific story.

#### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `story_id` | string | Yes | The ID of the story to delete |

#### Response

```json
{
  "id": "story_123456",
  "deleted": true
}
```

### Generate Story Continuation

```http