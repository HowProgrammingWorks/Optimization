# Code Optimization for JavaScript

## How to optimize OOP for JavaScript VMs

- Object layout and consistency
  - Don’t add or delete fields later; initialize all fields in the constructor or factory.
  - Keep class fields consistent (shape) — same order and types across instances.
  - Object shape (hidden class) — runtime layout that enables fast property access.
  - Do not change type of identifier or field; keep the same shape throughout usage.
  - Avoid shape changes via optional properties — use `null` to replace objects and `undefiled` instead of primitive values.
  - Monomorphic code — optimized for a single object shape to stay in fast paths.
  - Inline caching — a JIT optimization that remembers seen shapes for quick property access.
  - Avoid union types in most cases to prevent polymorphic or megamorphic inline caches; unions are ok for same type values, enums (e.g. months, statusCodes), same contracts (e.g. Promise and Thenable).
  - Stable property access — prefer dot notation compared to dynamic `[name]` access.
- Data representation
  - Avoid Holey Arrays — no empty elements, deletes, or resizes.
  - Prefer single-type arrays.
  - Keep numbers as signed small integers to avoid accidental doubles or `NaN`.
  - Use typed arrays for numeric hot paths.
  - Reuse preallocated memory to reduce GC pressure.
- Functions and state
  - Inline-friendly functions — keep them small, pure, with monomorphic parameters.
  - Avoid polymorphic returns so the runtime can specialize call sites.
  - Prefer arrow functions when object context is unnecessary.
  - Don’t change incoming parameters.
  - Avoid bound functions in hot code: `bind`, `apply`, `call`.
- Loops and control flow
  - Avoid functions inside loops.
  - Prefer `for` loops for performance and `for..of` for expressiveness;
  - Avoid `for..in` and `.forEach()`.
  - Choose array iteration method that match semantics (intent).
  - Optimize loop invariants.
  - Prefer iterative solutions over recursive ones when depth is high.
- Execution efficiency
  - Optimize lexical scope — keep identifier visibility tight.
  - Prefer `const`, fall back to `let` when reassignment is required.
  - String building in hot paths: use template strings or arrays plus `join`.
  - Avoid deopt triggers — no `with`, `eval`, or prototype mutation in hot code.
  - Warm up predictable paths before measuring performance.
  - Separate hot and cold paths to keep hot functions small.
