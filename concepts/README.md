<h1>
  <span class="headline">GitHub Copilot and AI Concepts</span>
</h1>

## 🔹 What Makes a Good Prompt?

What Makes a Good Prompt? The 4 Cs...

- __Clear__: Prompts should be direct and unambiguous to avoid confusion. Use clear, precise language.
- __Commanding__: Ask for exactly what you want - don’t beat around the bush. Use imperatives like "Explain", "List", "Compare".
- __Concise__: If you’re creating a prompt for a business app, longer prompts may end up being more costly and may slow the user down.
- __Contextual__: Give the model as much information as it needs to respond to your prompt effectively.

[How to write better prompts for GitHub Copilot](https://github.blog/developer-skills/github/how-to-write-better-prompts-for-github-copilot/)

<br>

## 🔹 What is Vibe Coding?

Vibe coding is a style of working with AI code assistants (like GitHub Copilot or ChatGPT) where you don’t write detailed specs or requirements upfront.

Instead, you:

* Write a loose comment, function name, or partial idea.
* Let the AI suggest code.
* Accept, reject, or tweak until it “feels right.”

It’s called vibe coding because you’re coding based on the flow or vibe of the conversation with the AI, rather than carefully planning everything first.

👉 Think of it as jamming with AI the way musicians jam with each other. You throw out an idea, the AI responds, and you refine together.

Got it — let’s break down “vibe coding” in a way that makes sense to someone with about 1 year of programming experience.

<br>

## 🔹 Why Vibe Coding Works

* AI tools are good at filling in details once you point them in a direction.
* You can explore solutions quickly without overthinking.
* Great for brainstorming, prototyping, or exploring patterns.

⚠️ But: It can produce messy or incorrect code if you don’t review and test carefully.

<br>

## 🔹 Example 1: Minimal Prompt → AI Fills the Gaps

__SAMPLE PROMPT:__ "make a todo app backend with express"

Copilot might generate:

```js
const express = require('express');
const app = express();
app.use(express.json());

let todos = [];

app.get('/todos', (req, res) => {
  res.json(todos);
});

app.post('/todos', (req, res) => {
  const todo = { id: todos.length + 1, text: req.body.text };
  todos.push(todo);
  res.status(201).json(todo);
});

app.listen(3000, () => console.log('Server running on port 3000'));
```

👉 You didn’t specify endpoints, data structure, or error handling — Copilot “vibed” a simple backend for you.

<br>

## 🔹 Example 2: Building on the Vibe

Now you add...

__SAMPLE PROMPT:__ "add delete endpoint"

Copilot suggests:

```js
app.delete('/todos/:id', (req, res) => {
  const id = parseInt(req.params.id);
  todos = todos.filter(todo => todo.id !== id);
  res.status(204).send();
});
```

👉 You didn’t explain how to delete — Copilot inferred from the existing vibe of your code.

<br>

## 🔹 Example 3: UI Vibe Coding

Here's a React example...

__SAMPLE PROMPT:__ "make a button that toggles dark mode"

Copilot might generate:

```js
import { useState } from 'react';

export default function DarkModeButton() {
  const [dark, setDark] = useState(false);

  return (
    <button onClick={() => setDark(!dark)}>
      {dark ? 'Switch to Light Mode' : 'Switch to Dark Mode'}
    </button>
  );
}
```

👉 You didn’t describe state, labels, or logic. The AI “vibed” a standard pattern.

<br>

## 🔹 When to Use Vibe Coding

✅ Good for:

* Prototyping
* Exploring APIs
* Generating boilerplate code
* Getting unstuck when you don’t know where to start

❌ Not good for:
* Production-critical code (needs more precision & testing)
* Security-sensitive code (auth, encryption, etc.)
* Complex algorithms that need careful design

<br>

## 🔹 Key Learnings
* Vibe coding = fast, playful, exploratory coding with AI.
* You give AI loose hints → it generates code → you refine together.
* Always review and test what you accept, because AI isn’t always correct.


<br>

## What is “Context” in GitHub Copilot?

When you use GitHub Copilot, it doesn’t just look at the one line you’re typing. It looks at the surrounding code, comments, and files to figure out what you probably want next.

Think of context as the “hints” you give Copilot so it can generate the right code for you.


<br>

## Ways Copilot Gets Context

**Current File**
* Copilot reads the code above and below your cursor.
* Example: If you already have a function signature, it tries to finish the function body in a way that matches.

**Comments / Prompts**
* You can write a natural language comment (like a mini-prompt).
* Example: `function to calculate factorial using recursion`
* Copilot will then generate the function code.

**Other Files in the Project**
* If you have related files (like a `models.js` or `utils.py`), Copilot can peek at those to stay consistent.

**Your Coding Style**
* Copilot learns the patterns in your current file (indentation, naming conventions, etc.) and matches them.

<br>

## 🔹 Example 1: Weak Context (Vague Prompt)

__SAMPLE PROMPT:__ "make a function"

Copilot might generate something random like:

```javascript
function myFunction() {
  // TODO: implement
}
```

👉 Because the context is vague, the result isn’t useful.

<br>

## 🔹  Example 2: Strong Context (Clear Prompt)

__SAMPLE PROMPT:__ "function that takes an array of numbers and returns the average"


Copilot might generate something random like:

```javascript
function average(numbers) {
  if (numbers.length === 0) return 0;
  let sum = 0;
  for (let num of numbers) {
    sum += num;
  }
  return sum / numbers.length;
}
```

👉 Much better — clear instructions in the comment gave Copilot enough context.

<br>

### 🔹  Example 3: Using Existing Code as Context

```javascript
function add(a, b) {
  return a + b;
}
```

Then you type:

```javascript
function subtract(a, b) {
```

Copilot will likely complete it as:

```javascript
  return a - b;
}
```

👉 The context from your add function helped it guess you want similar math functions.

🔹 Key Learnings

* ***Copilot works best when you give it strong context.**
* Use **descriptive comments** instead of vague ones.
* Let Copilot “see” enough surrounding code so it understands the pattern.
* Think of context like giving **directions to a helper**: the clearer you are, the better the results.

<br>

## 📝 Mini Exercise: Copilot Context in Action (Time Permitting)

### 🎯 Goal

Understand how different levels of context (weak vs. strong prompts) affect Copilot’s code generation.

###  Step 1: Weak Prompt

In a JavaScript file, type:

```javascript
// make a function
```

➡️ Ask Copilot to generate code.

**Question**: Is the result useful? Why or why not?


### Step 2: Stronger Prompt

Now replace the comment with:

```javascript
// function that takes a list of names and returns them in alphabetical order
```

➡️ Trigger Copilot again.
You should get something like:

```javascript
function sortNames(names) {
  return names.sort();
}
```

**Discussion**:

* What made this result more useful?
* How did the added context help?


### Step 3: Context from Surrounding Code

Start with this function:

```javascript
function add(a, b) {
  return a + b;
}
```

Now type:

```javascript
function multiply(a, b) {
```

➡️ Copilot will likely suggest:

```javascript
  return a * b;
}
```

**Discussion**:
* Why did Copilot guess correctly?
* What role did the add function play in the suggestion?


### Step 4: Project Context

Create a file called `utils.js` with:

```javascript
export function capitalize(word) {
  return word.charAt(0).toUpperCase() + word.slice(1);
}
```

Now in another file, type:

```javascript
// function that takes a sentence and capitalizes every word
```

➡️ Copilot might generate code using `capitalize()` because it saw it in `utils.js`.

**Discussion**:
* How does Copilot use project-level context?
* Why is this powerful when working on bigger projects?


**Wrap-Up Reflection**
* What happens when prompts are vague?
  * Copilot may generate generic or less useful code because it lacks clear direction.
* What happens when prompts are specific?
  * Copilot produces more accurate and relevant code that matches your intent.
* How does surrounding code and files shape Copilot’s output?
  * Copilot uses the context from nearby code and other project files to generate suggestions that fit your coding style and project structure.