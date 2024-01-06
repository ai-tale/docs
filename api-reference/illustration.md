# Illustration API

## Overview

The Illustration API allows you to generate and manage AI-powered illustrations for your stories. This document provides detailed information about the endpoints, request parameters, and response formats for illustration generation.

## Endpoints

### Generate Illustrations

```http
POST /v1/illustrations
```

Generates one or more illustrations based on the provided parameters.

#### Request Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `prompts` | array | Yes | Array of illustration prompt objects |
| `style` | string | No | Visual style for all illustrations (default: "storybook") |
| `resolution` | string | No | Image resolution (default: "medium") |
| `aspect_ratio` | string | No | Image aspect ratio (default: "square") |
| `story_id` | string | No | Associated story ID, if applicable |
| `webhook_url` | string | No | URL to receive completion notification |
| `metadata` | object | No | Custom metadata to associate with the illustrations |

##### Illustration Prompt Object

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `text` | string | Yes | Descriptive prompt for the illustration |
| `style` | string | No | Override the global style for this illustration |
| `characters` | array | No | Character details to include in the illustration |
| `scene` | object | No | Scene details (setting, time of day, etc.) |
| `mood` | string | No | Emotional tone of the illustration |

#### Example Request

```json
{
  "prompts": [
    {
      "text": "A young girl with flowing blue hair standing on a beach, looking out at the ocean with wonder in her eyes. The sun is setting, casting golden light across the water.",
      "characters": [
        {
          "name": "Marina",
          "description": "A 10-