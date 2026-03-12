# AI-Assisted Development Session

**Project:** Serverless AI Chatbot Platform  
**Developer:** Julien  
**AI Tool Used:** GitHub Copilot Chat  
**Language:** TypeScript / Node.js  
**Cloud Platform:** Amazon Web Services

---

## 1. Project Context

This development session demonstrates how AI-assisted coding tools were used to accelerate backend API development for a serverless chatbot application deployed on Amazon Web Services.

The system architecture includes:

- API Gateway
- AWS Lambda
- DynamoDB
- Amazon Bedrock

The goal was to create a backend endpoint that receives a user prompt and returns an AI-generated response.

---

## 2. Development Goal

Create a secure REST API endpoint:

POST /chat

Responsibilities:

- Accept user message
- Validate input
- Send prompt to AI model
- Return generated response
- Implement proper error handling

---

## 3. AI Interaction Transcript

### Developer Prompt

Create a Node.js Lambda function in TypeScript that accepts a POST request with a user message and sends the prompt to Amazon Bedrock. The function should return the generated response with input validation and error handling.

### AI Response (Initial Code Scaffold)

```typescript
import { BedrockRuntimeClient, InvokeModelCommand } from "@aws-sdk/client-bedrock-runtime";

const client = new BedrockRuntimeClient({ region: "us-east-1" });

export const handler = async (event) => {
  const body = JSON.parse(event.body);

  if (!body.message) {
    return {
      statusCode: 400,
      body: JSON.stringify({ error: "Message is required" })
    };
  }

  const prompt = body.message;

  const command = new InvokeModelCommand({
    modelId: "anthropic.claude-v2",
    body: JSON.stringify({ prompt })
  });

  const response = await client.send(command);

  return {
    statusCode: 200,
    body: JSON.stringify({ response })
  };
};
```

## 4. Code Validation and Improvements

After reviewing the AI-generated code, the following improvements were implemented:

- Input sanitization
- Request size limits
- Structured logging
- Robust error handling

Example improvement:

```typescript
if (!body.message || body.message.length > 1000) {
  return {
    statusCode: 400,
    body: JSON.stringify({ error: "Invalid input length" })
  };
}
```

Structured logging:

```typescript
console.log("Incoming request", {
  messageLength: body.message.length
});
```

## 5. Security Considerations

Security improvements included:

- Input validation
- Sanitization of incoming data
- Removal of sensitive data from logs
- IAM role restricted to Bedrock invocation
- HTTPS-only API Gateway endpoint


## 6. Testing Strategy

AI-assisted test generation was used to generate initial unit tests.

Example Jest test:

```typescript
test("returns error when message missing", async () => {
  const event = { body: JSON.stringify({}) };

  const response = await handler(event);

  expect(response.statusCode).toBe(400);
});
```
These tests were manually reviewed and expanded.