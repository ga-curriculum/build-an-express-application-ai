## Note - Your AI generated solution may vary.

`app.js`

```js
const express = require("express");
const cors = require("cors");
const morgan = require("morgan");
const controller = require("./controller");

const app = express();
const PORT = process.env.PORT || 3000;


// Enable CORS
app.use(cors());

// Enable HTTP request logging
app.use(morgan('dev'));


// Custom Middleware 1
app.use((req, res, next) => {
  console.log('Middleware 1: Logging request details');
  next();
});

// Custom Middleware 2
app.use((req, res, next) => {
  console.log('Middleware 2: Performing some operation');
  next();
});



// Use controller routes under /api
app.use('/api', controller);

// Error handling middleware
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).json({ error: "Something went wrong!" });
});


if (require.main === module) {
  app.listen(PORT, () => {
    console.log(`Server running on port ${PORT}`);
  });
}

module.exports = app;
```

<br>

`app.test.js`

```js
const request = require('supertest');
const express = require('express');
let app;

describe('Express App Routes', () => {
  beforeAll(() => {
    app = require('./app');
  });

  test('GET / should return hello Adobe', async () => {
    const res = await request(app).get('/');
    expect(res.statusCode).toBe(200);
    expect(res.text).toBe('hello Adobe');
  });

  test('GET /home should return home Page in h1', async () => {
    const res = await request(app).get('/home');
    expect(res.statusCode).toBe(200);
    expect(res.text).toBe('<h1>home Page</h1>');
  });

  test('GET /123 should return 123 in h1', async () => {
    const res = await request(app).get('/123');
    expect(res.statusCode).toBe(200);
    expect(res.text).toBe('<h1>123</h1>');
  });

  test('GET /greet/John should return John in h1', async () => {
    const res = await request(app).get('/greet/John');
    expect(res.statusCode).toBe(200);
    expect(res.text).toBe('<h1>John</h1>');
  });

  test('GET /greets/Jane should return Welcome Jane in h1', async () => {
    const res = await request(app).get('/greets/Jane');
    expect(res.statusCode).toBe(200);
    expect(res.text).toBe('<h1>Welcome Jane</h1>');
  });

  test('GET /hello?name=Sam&age=25 should return greeting in h1', async () => {
    const res = await request(app).get('/hello?name=Sam&age=25');
    expect(res.statusCode).toBe(200);
    expect(res.text).toBe('<h1>Hello there, Sam! I hear you are 25 years old!</h1>');
  });

  test('GET /hello missing params should return error', async () => {
    const res = await request(app).get('/hello');
    expect(res.statusCode).toBe(400);
    expect(res.text).toMatch(/Missing name or age/);
  });
});
```

<br>
 
`package.json`

```js
{
  "name": "ai-hello-world-express",
  "version": "1.0.0",
  "description": "Express.js app with CORS and error handling for Adobe lesson.",
  "main": "app.js",
  "scripts": {
    "start": "nodemon app.js",
    "test": "jest"
  },
  "keywords": [
    "express",
    "cors",
    "adobe",
    "lesson"
  ],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "cors": "^2.8.5",
    "express": "^4.18.2",
    "morgan": "^1.10.1",
    "nodemon": "^3.1.10"
  },
  "devDependencies": {
    "jest": "^30.1.3",
    "supertest": "^7.1.4"
  }
}
```

<br>

`controller.js`

```js
const express = require('express');
const router = express.Router();

// Root route
router.get('/', (req, res) => {
  res.send('hello Adobe');
});

// GET /hello?name=...&age=...
router.get('/hello', (req, res) => {
  const { name, age } = req.query;
  if (!name || !age) {
    return res.status(400).send('<h1>Missing name or age query parameter</h1>');
  }
  res.send(`<h1>Hello there, ${name}! I hear you are ${age} years old!</h1>`);
});

// GET /home
router.get('/home', (req, res) => {
  res.send('<h1>home Page</h1>');
});

// GET /:itemNumber
router.get('/:itemNumber', (req, res, next) => {
  // Avoid conflict with /home and /greet/:name
  if (req.params.itemNumber === 'home' || req.params.itemNumber === 'greet') {
    return next();
  }
  res.send(`<h1>${req.params.itemNumber}</h1>`);
});

// GET /greet/:name
router.get('/greet/:name', (req, res) => {
  res.send(`<h1>${req.params.name}</h1>`);
});

// GET /greets/:name
router.get('/greets/:name', (req, res) => {
  res.send(`<h1>Welcome ${req.params.name}</h1>`);
});

module.exports = router;
```