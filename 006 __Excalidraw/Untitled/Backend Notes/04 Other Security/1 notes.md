# Helmet
- Helmet is a security middleware for Node.js application that helps to setting HTTP response headers.

```ts
const express = require('express');
const helmet = require('helmet');

const app = express();

// Apply security headers
app.use(helmet());

app.get('/', (req, res) => {
  res.send('Hello, World!');
});

app.listen(3000, () => {
  console.log('Server running on port 3000');
});
```

