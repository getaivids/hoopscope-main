# OpenAI API Integration Guide

## 1. API Key Handling (Security First)
- Never hardcode keys into HTML/JS files.
- Store your OpenAI API key in server-side environment variables or use a secure secrets manager. (For local: `.env` file, for server: cloud secret vault.)
- Use server endpoints as proxies for all API calls—never expose secret keys to the browser.

## 2. Example Proxy Endpoints (Node/Express)
```js
// (Pseudo-example)
app.post('/api/generate', async (req, res) => {
  const { prompt, model = "gpt-4-turbo" } = req.body;
  const apiKey = process.env.OPENAI_API_KEY;
  const result = await fetch('https://api.openai.com/v1/chat/completions', {
    method: 'POST',
    headers: {
      'Authorization': `Bearer ${apiKey}`,
      'Content-Type': 'application/json',
    },
    body: JSON.stringify({model, messages: [{role: 'user', content: prompt}], max_tokens: 800}),
  });
  const data = await result.json();
  res.json(data);
});
```

## 3. Frontend Fetch Example
```js
fetch('/api/generate', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ prompt })
}).then(r => r.json())
.then(res => handleOutput(res));
```

## 4. Prompt Optimization
- Use clear structure: "Given [data], please..."
- Specify format in the prompt (JSON, bullets, etc.) for reliable output.

## 5. Error Handling
- Always catch network and API errors; show clear user feedback.
- Fallback gracefully ("Sorry, that didn’t work. Please try again.")

## 6. Further Reading
- [OpenAI API Docs](https://platform.openai.com/docs/api-reference/introduction)

## See CHANGELOG.md for dates and project structure updates.
