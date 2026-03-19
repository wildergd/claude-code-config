# SOLID Principles (Frontend / React)

Apply these principles to components, hooks, and functions.  
Favor composition, clarity, and simplicity over strict adherence.

## 1. Single Responsibility Principle (SRP)

**Definition:**  
A component, hook, or function should have one clear responsibility and one reason to change.

**React interpretation:**
- Components → primarily UI rendering (may orchestrate hooks and simple logic)
- Hooks → stateful logic or side effects
- Functions → pure transformations

**Guidelines:**
- Extract hooks when logic mixes with UI or grows beyond simple orchestration
- Isolate side effects (data fetching, subscriptions)
- Keep responsibilities cohesive (not artificially small)

**Red flags:**
- Components handling UI + fetching + business logic
- Hooks managing unrelated concerns
- Files mixing multiple domains

**Do NOT apply when:**
- Splitting reduces readability or increases indirection
- The logic is small and naturally cohesive

## 2. Open/Closed Principle (OCP)

**Definition:**  
Extend behavior without modifying stable code.

**React interpretation:**
- Use composition:
  - props
  - children
  - render functions
  - hook composition

**Guidelines:**
- Design components to be extended via inputs, not edits
- Prefer composition for complex or repeated variation; use conditionals for simple cases

**Red flags:**
- Repeated edits to the same component for new features
- Large conditional blocks (`if/else`, `switch`) for variants

**Do NOT apply when:**
- There is only one known use case
- Abstraction adds complexity without real variation

## 3. Liskov Substitution Principle (LSP)

**Definition:**  
Components or hooks sharing a contract must be swappable without breaking consumers.

**React interpretation:**
- Components with the same props interface must be substitutable without causing breaks
- Hooks must return consistent structure and semantics regardless of internal state

**Guidelines:**
- Keep prop contracts stable
- Ensure consistent return shapes in hooks
- Consumers must not need to know which implementation they're using

**Red flags:**
- Swapping a component with the same props interface breaks the consumer
- Hook returns `null` in some branches and an object in others
- Conditional logic based on component "type"
- Same props but different side effects
- Behavior changes significantly based on presence/absence of optional props

**Do NOT apply when:**
- There is no shared contract or interchangeable usage

## 4. Interface Segregation Principle (ISP)

**Definition:**  
Do not force consumers to depend on unused inputs.

**React interpretation:**
- Keep props minimal and cohesive
- Prefer multiple focused hooks over one large hook
- Split large components by responsibility

**Guidelines:**
- Group related props and keep APIs stable over time
- Design APIs around real usage

**Red flags:**
- Components with many unrelated or optional props
- Hooks requiring unused parameters
- "God components"

**Do NOT apply when:**
- Splitting creates excessive fragmentation or prop drilling

## 5. Dependency Inversion Principle (DIP)

**Definition:**  
Depend on abstractions, not concrete implementations.

**React interpretation:**
- Decouple components from APIs, services, and global state

**Injection preference:**
- props → component-scoped, explicit, easiest to test
- hooks → reusable data access and service abstraction
- context → cross-cutting only (auth, theme, i18n) — last resort

**Guidelines:**
- Abstract dependencies when it improves testability or reuse

**Red flags:**
- Direct API calls inside UI components
- Tight coupling to specific services
- Hardcoded dependencies

**Do NOT apply when:**
- The dependency is simple, stable, and unlikely to change
- he dependency is local to the component and not reused

## Practical Rules

- Prefer hooks over complex component logic
- Prefer composition for complex or repeated variation; use conditionals for simple cases
- Prefer simple solutions over strict SOLID adherence
- Avoid premature abstraction
- Apply SOLID only when it improves clarity, reuse, or maintainability

## Response Format
- Identify relevant principle(s), confirm or reject violations, and refactor if needed.  
- Explain trade-offs only when the right choice isn't obvious from the code.  
- Skip steps that don't add value for simple cases.