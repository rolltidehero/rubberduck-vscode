# Describe the code and ask follow-up questions

Describe the selected code.

## Template

### Configuration

```json conversation-template
{"id": "describe-code",
  "engineVersion": 0,
  "label": "Describe Code",
  "description": "Describe the selected code.",
  "tags": ["debug", "understand"],
  "header": {
    "title": "Describe Code ({{location}})",
    "icon": {
      "type": "codicon",
      "value": "book"
    }
  },
  "variables": [
    {
      "name": "selectedText",
      "time": "conversation-start",
      "type": "selected-text",
      "constraints": [{ "type": "text-length", "min": 1 }]
    },
    {
      "name": "location",
      "time": "conversation-start",
      "type": "selected-location-text"
    },
    {
      "name": "firstMessage",
      "time": "message",
      "type": "message",
      "property": "content",
      "index": 0
    },
    {
      "name": "lastMessage",
      "time": "message",
      "type": "message",
      "property": "content",
      "index": -1
    }
  ],
  "initialMessage": {
    "placeholder": "Generating description",
    "maxTokens": 512
  },
  "response": {
    "maxTokens": 1024,
    "stop": ["Bot:", "Developer:"]
  }
}
```

### Initial Message Prompt

```template-initial-message
## Instructions
Summarize the code below (emphasizing its key functionality).

## Selected Code
\`\`\`
{{selectedText}}
\`\`\`

## Task
Summarize the code at a high level (including goal and purpose) with an emphasis on its key functionality.

## Response

```

### Response Prompt

```template-response
## Instructions
Continue the conversation below by asking numbered follow-up questions to gather more details about the current developer request.
Pay special attention to the current developer request.

## Current Request
Developer: {{lastMessage}}

{{#if selectedText}}
## Selected Code
\`\`\`
{{selectedText}}
\`\`\`
{{/if}}

## Code Summary
{{firstMessage}}

## Conversation
{{#each messages}}
{{#if (neq @index 0)}}
{{#if (eq author "bot")}}
Bot: {{content}}
{{else}}
Developer: {{content}}
{{/if}}
{{/if}}
{{/each}}

## Task
Write a response that continues the conversation below by asking numbered follow-up questions to gather more details about the current developer request.
Provide several numbered suggestions to enhance the code such as libraries to use and anything else.
Provide several numbered feature requests and suggestions to add more features to the code.
Provide several numbered suggestions such as libraries to use and anything else to enhance the code and the UI.
Include links with citations for your suggestions if possible
Stay focused on current developer request.
Consider the possibility that there might not be a solution.
Ask for clarification if the message does not make sense or more input is needed.
Use the style of a documentation article.
Include code snippets (using Markdown) and examples where appropriate.
The title of each code snippet should be the expected filename that the code belongs to, and if no filename is expected, then generate it.

## Response
Bot:
```
