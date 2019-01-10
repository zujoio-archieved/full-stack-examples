Motivation
===
- Our code must manage more state than ever before.
- Managing this ever-changing state is hard.
- Lost control over the when, why, and how of its state.
- New requirements becoming common in front-end product development.
- We're mixing two concepts that are very hard for the human mind to reason about: mutation and asynchronicity

Three Principles
===
- **Single source of truth**
    - The state of your whole application is stored in an object tree within a single store.
- **State is read-only**
    - The only way to change the state is to emit an action, an object describing what happened.
- **Changes are made with pure functions**
    - To specify how the state tree is transformed by actions, you write pure reducers.

Basic 
===
- Actions 
- Reducers
- Usage with React

Advanced
===
- Async Actions
- Middleware
- Usage with React Router

