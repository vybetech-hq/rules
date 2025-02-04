# Rules

## 1. ChatGPT and Generated Code

ChatGPT can provide useful ideas but often generates suboptimal or incorrect code. Always review and refine any code suggested by AI tools.

### 1.1 Functions

Avoid using outdated syntax for functions in JavaScript. Modern best practices prefer arrow functions for consistency and clarity.

```javascript
// Bad
function myFunction() {
  // do something
}

// Good
const myFunction = () => {
  // do something
};
```

### 1.2 try/catch

Avoid wrapping large blocks of code in a single `try/catch`. Generated code often nests everything inside a `try` block and then uses a `finally` block to rethrow errors unnecessarily. This approach adds complexity without any benefit. Instead:

- Only use `try/catch` where you expect specific, manageable errors.
- Keep the `try` block as minimal as possible. Include only the code that might throw an error.
- Avoid rethrowing errors unnecessarily. Let the error propagate naturally unless you need custom handling.

```javascript
// Bad
try {
  doSomething();
  doSomethingElse();
  finalize();
} catch (error) {
  console.error('An error occurred:', error);
  throw error; // Unnecessary rethrow
}

// Good
try {
  doSomething();
} catch (error) {
  console.error('Error in doSomething:', error.message);
}

// Best Practice
const processWithErrorHandling = () => {
  try {
    riskyOperation();
  } catch (error) {
    handleSpecificError(error);
  }
};

// Keep non-error-prone logic outside the try block
finalize();
doSomethingElse();
```

### 1.3 Obvious Comments

Do not clutter your code with redundant comments that describe the obvious. If a comment doesn't add value beyond what the code itself conveys, it is unnecessary. The fact that ChatGPT struggles to generate good examples of meaningful comments reflects how rarely comments are genuinely needed in well-written, self-documenting code.

```javascript
// Bad
// This checks if something
checkSomething();

// Doesn't add value
// Only allow admins to access this resource because it modifies critical data
if (!user.hasRole('admin')) {
  throw new Error('Unauthorized');
}
```

### 1.4 Early Return

Use early returns to simplify code and avoid unnecessary nesting.

```javascript
// Good
const processSomething = () => {
  if (!isValid) return;
  // proceed with processing
};
```

### 1.5 Single Responsibility Principle

Functions should do one thing and do it well. Break down complex operations into smaller, reusable functions.

```javascript
// Bad
const handleRequest = (data) => {
  validateData(data);
  saveToDatabase(data);
  sendResponse(data);
};

// Good
const handleRequest = (data) => {
  if (!validateData(data)) return;
  saveToDatabase(data);
  sendResponse(data);
};
```

### 1.7 No default values

Do not add default values, trust that it exists in the parent implementation or check for it:

```
return {
  width: device.viewport_width || 1024,
}
```

Later on you'll spend hours debugging where did this `1024` value come from


## 2. Pull Requests (PRs)

### 2.1 Single Change Per PR

Each pull request should focus on one isolated change. This minimizes side effects and makes code easier to review.

- Avoid combining unrelated changes in a single PR.
- Do not include extra error handling, alternate implementations, or unrelated improvements.
- If you have additional ideas, open tasks in GitHub for prioritization and discussion.

### 2.2 Descriptive Commit Messages

Write clear and concise commit messages that explain the "what" and "why" of your changes.

```text
// Bad
Fixed bug

// Good
Fixed null pointer error when user data is not available during login
```

### 2.3 Keep PRs Slim

Small, focused PRs reduce bugs and make reviews more effective. Avoid bloated PRs with unnecessary changes.

## 3. General Best Practices

### 3.1 Consistent Code Style

Follow the established coding style of the project.

### 3.2 Avoid Global Variables

Do not pollute the global namespace. Encapsulate variables and logic within modules or classes.

### 3.4 Write Self-Documenting Code

Write code that is intuitive and easy to understand without requiring excessive comments. Use descriptive variable and function names.

```javascript
// Bad
const x = (a, b) => a + b;

// Good
const addNumbers = (num1, num2) => num1 + num2;
```

### 3.5 Avoid Hardcoding Values

Use constants, configuration files, or environment variables instead of hardcoding values. Provide default values where appropriate.

```javascript
// Bad
const apiUrl = 'https://api.example.com';

// Good
const API_URL = process.env.API_URL || 'https://default-api.example.com';
```

### 3.6 Optimize for Readability

Prioritize code readability over cleverness. Write code that others can easily maintain and extend.

```javascript
// Bad
const result = arr.reduce((a, b) => a + b);

// Good
const sumArray = (numbers) => {
  return numbers.reduce((sum, current) => sum + current, 0);
};
```

### 3.7 camelCase

Always follow the same capitalization, then you don't have to think if something is upperCASED, it's always camelCased

```javascript
// bad
baseURL
myID

// good
baseUrl
myId
```

### 3.8 File extensions in imports

Always include file extensions when importing files.

```javascript
// bad
const beer = require("../lib/beer")
const pint = require("./pint")

// good
const beer = require("../lib/beer.js")
const pint = require("./pint.json")
```

### 3.9 file_names_in_snake_case

```
// bad
someFileName.js
```

```
// good
some_file_name.js
```
reason: some file systems are case-sensitive and allow `someFile.js` and `SomeFile.js` to co-exist

### 3.10 empty lines between logical blocks

```javascript
// bad
if (!something) return null;
const numbers = input
    .toLowerCase()
    .replace(/\s+/g, "")
    .replace("something", "");
switch (numbers.length) {
  case 3:
  ...

// good
if (!something) return null;

const numbers = input
    .toLowerCase()
    .replace(/\s+/g, "")
    .replace("something", "");

switch (numbers.length) {
  case 3:
  ...
```

By following these guidelines, you’ll ensure a cleaner codebase, easier reviews, and better collaboration across the team.
