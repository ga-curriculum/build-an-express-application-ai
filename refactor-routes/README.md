<h1>
  <span class="headline">Refactor routes with GitHub Copilot Concepts</span>
</h1>


## Generate a Controller

__Below is a sample prompt for an Express application. NOTE - Your GitHub Copilot generated code may vary slightly from the screenshot below. Trust but verify!__

Currently, all of our routes reside in `app.js`. As our app grows in complexity we'll want to seperate our code into smaller files. 

[It's a principle known as Separation of Concerns](https://en.wikipedia.org/wiki/Separation_of_concerns).

It helps with testing and maintainability over time. As an example, let's ask Github Copilot to generate a controller file for our app using the prompt below.

__SAMPLE PROMPT:__ "Refactor app.js. move the routes into a separate controller file. In app.js, use Express router to connect to the controller file. All requests to the controller file should be appended with /api"

NOTE: Since we're building an api that serves data only (no views) we're prepending all the routes with `/api`.

![](./assets/refactor-routes.png)

Here is the code that was generated for `controller.js`.

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

  module.exports = router;
  ```

And in `app.js` you should see this addition:

  ```js
  // Use controller routes under /api
  app.use('/api', controller);
  ```


