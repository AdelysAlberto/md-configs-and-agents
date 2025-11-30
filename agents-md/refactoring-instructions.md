---
description: Refactoring instructions 

---

# JavaScript Refactoring Instructions

- Use early returns to reduce nesting
- Extract validations into separate functions
- Use descriptive and meaningful variable names
- Adopt TypeScript with explicit types for better type safety
- Handle edge cases (e.g., empty arrays, negative values, null/undefined)
- Prefer const and let over var to avoid hoisting issues
- Use arrow functions for concise syntax and lexical scoping
- Implement destructuring for cleaner object and array access
- Replace template literals for string concatenation
- Use async/await over promises or callbacks for asynchronous code
- Remove dead code and unused variables to improve maintainability
- Simplify conditional expressions and avoid deeply nested if statements
- Extract magic numbers and strings into named constants
- Favor functional programming patterns (e.g., map, filter) over imperative loops
- Implement proper error handling with try/catch blocks
- Use strict equality (===) instead of loose equality (==)
- Break down large functions into smaller, single-responsibility functions
- Add JSDoc comments for complex functions to improve readability
- Optimize performance by avoiding unnecessary computations in loops
- Ensure consistent code formatting with tools like Prettier or ESLint

# Choosing the Right Loop Structure

Behavior Guidelines:

When analyzing or refactoring JavaScript/TypeScript code, evaluate whether the selected iteration construct is appropriate. Use the following rules to decide and propose improvements:

Use map
Only when transforming an array into another array of the same length and with no side effects.

Use reduce
Only when the goal is to produce a single accumulated value (number, object, array, map, grouped result, etc.).
Never use it for pure iteration or side effects.

Use forEach
When the primary purpose is executing side effects (logging, pushing to an external collection, calling APIs).
No return value expected.

Use for...of
When you need control flow (break, continue) or sequential asynchronous operations (await inside the loop).

Use classic for
When performance and fine-grained index control matter, or when the logic depends heavily on index operations.

Use for...in
Only when iterating over object keys. Never recommend it for arrays.

Prefer clarity over cleverness
If reduce or map introduce unnecessary cognitive complexity, replace them with a clearer construct (e.g., for or for...of).

Refactor the code strictly

No any

Use strict types

Keep code minimal, readable, and declarative when beneficial

Avoid hidden mutations unless intentional and clearly justified
