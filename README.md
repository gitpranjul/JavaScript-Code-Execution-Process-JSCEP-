# JSCEP — JavaScript Code Execution Process

![JSCEP- JavaScript Code Execution Process Image](https://github.com/gitpranjul/JavaScript-Code-Execution-Process-JSCEP-/blob/main/JSCEP-Ultimate.png)

**A comprehensive visual architecture map of JavaScript internals** — covering the V8 engine pipeline, Browser Runtime, Node.js Runtime, Execution Context System, Memory Management, Event Loop, WebAssembly, and the full optimization lifecycle.

📄 **[View the full diagram (SVG)](./JSCEP-Ultimate.svg)** &nbsp;|&nbsp; 🖱️ **[View interactive version (HTML)](./JSCEP-Ultimate.html)**

---

## Overview

JSCEP is an educational and architectural visualization project that explains how JavaScript works — from source code to CPU execution.

Unlike most JavaScript diagrams that focus only on the Event Loop or Call Stack, JSCEP provides a complete ecosystem view covering:

- JavaScript Language Runtime
- V8 Engine Internals
- Browser Architecture
- Node.js Runtime Architecture
- Execution Context System
- Memory Management & Garbage Collection
- Event Loop & Queues
- WebAssembly (WASM)
- JIT Compilation Pipeline
- Optimization / Deoptimization
- Scope & Environment Records

The goal is to help learners understand JavaScript as a **complete execution platform**, not a collection of isolated concepts.

---

## How to View

| File | Best for | Notes |
|---|---|---|
| [`JSCEP-Ultimate.svg`](./JSCEP-Ultimate.svg) | Recommended — fastest, works offline | Self-contained, opens directly in any browser |
| [`JSCEP-Ultimate.html`](./JSCEP-Ultimate.html) | Interactive pan/zoom viewer | Requires internet (loads `diagrams.net` viewer script) |

For the best experience, download `JSCEP-Ultimate.svg` and open it in your browser, or zoom directly on GitHub's file preview.

---

## Project Goal

**How does JavaScript really work?**

```
Source Code
      ↓
V8 Engine
      ↓
Execution Contexts
      ↓
Memory Management
      ↓
Browser / Node Runtime
      ↓
CPU Execution
```

---

## Architecture Coverage

### 1. JavaScript Source Processing Pipeline

```
JavaScript Source Code
        ↓
Lexer / Scanner
        ↓
Parser
        ↓
AST
        ↓
Scope & Binding Analysis
        ↓
Ignition Interpreter
```

Covered: UTF-16 source reading, tokenization, syntax parsing, AST construction, scope analysis, binding resolution.

---

### 2. V8 Engine Pipeline

```
Lexer → Parser → AST → Scope & Binding Analysis → Ignition
                                                       ↓
                                          Sparkplug → Maglev → TurboFan
```

**Ignition** (Interpreter) — Bytecode generation, bytecode execution, feedback collection, runtime profiling.

**JIT Optimization Tiers** — Sparkplug (fast baseline compiler) → Maglev (mid-tier optimizer) → TurboFan (advanced optimizer, internally redesigned around the Turboshaft architecture) — used for hot functions, runtime optimization, and machine code generation.

---

### 3. DeOptimization & ReOptimization

**DeOptimization**
```
Optimized Machine Code → Guard Check Failure → Discard Optimized Code → Fallback to Ignition Bytecode
```

**ReOptimization**
```
Ignition Bytecode → Feedback Vector Updates → JIT Compilation → New Optimized Machine Code
```

Covered: BailOut, guard checks, type changes, feedback vectors, the reoptimization cycle.

---

### 4. Execution Context System

**Global Execution Context (GEC)**
```
GEC
├── Global Lexical Environment
├── Global Environment Record
├── Global 'this' Binding
└── Runtime Metadata
```

**Function Execution Context (FEC)**
```
FEC
├── Runtime State
├── Lexical Environment
├── Variable Environment
├── Private Environment
└── Metadata Links
```

Also covered: Eval Execution Context, Module Execution Context, Generator state linkage (`suspendedStart` → `suspendedYield` → `executing` → `completed`).

---

### 5. Environment Records

| Record | Stores | Used By |
|---|---|---|
| Declarative (DER) | `let`, `const`, `class`, parameters | Block/function scopes |
| Object (OER) | Global object bindings | Global scope, `with()` |
| Function (FER) | `this`, `arguments`, `super`, `new.target` | Function invocation |
| Global (GER) | Composite of DER + OER | Top-level program |
| Module (MER) | Import/export bindings | ES Modules |

Covered: scope chains, identifier resolution, outer environment references.

---

### 6. Scope Chain Architecture

```
Global Scope
      ↑
Function Scope
      ↑
Nested Function Scope
```

Explains lexical scoping, closure resolution, and variable lookup.

---

### 7. Memory Management

**Heap** — stores Objects, Arrays, Functions, Closures, Promises, Prototypes, Error Objects, Hidden Classes, Arguments Objects.

**Call Stack** — LIFO structure holding GEC and nested FECs; explains execution context stack behavior and function invocation flow.

---

### 8. Garbage Collection — V8's Orinoco

- Tri-Color Marking (White / Grey / Black markers)
- Minor GC (Scavenger) for Young Generation
- Major GC (Mark-Compact) for Old Generation
- Incremental, Concurrent, and Parallel collection phases
- `WeakRef` and `FinalizationRegistry`

Covered: reachability, memory cleanup, background GC threads.

---

### 9. Browser Architecture

```
Browser
│
├── Browser Host Services Layer
│     ├── Rendering Engine
│     ├── Networking Stack
│     ├── Resource Fetching System
│     ├── Storage System
│     ├── Security Sandbox Layer
│     └── Module Loader System
│
└── JavaScript Runtime Container
```

**Rendering Pipeline**
```
HTML Parser → DOM   ┐
                     ├→ Render Tree → Layout → Paint → Compositor → Frames
CSS Parser → CSSOM   ┘
```

---

### 10. Node.js Runtime Architecture

```
Node.js Runtime
│
├── Node Host Services Layer
│     ├── File System
│     ├── Network Stack
│     ├── OS Interface
│     ├── Process Manager
│     ├── Native Addons
│     └── Module Loader
│
├── libuv Core
│     ├── Event Loop
│     ├── Thread Pool
│     ├── Timers
│     ├── I/O Polling
│     ├── Async Scheduling
│     └── OS Abstraction Layer
│
└── V8 Engine
      ├── Execution Context System
      ├── Call Stack
      ├── Heap
      ├── GC
      └── Compiler Pipeline
```

---

### 11. Event Loop System

Covered: Call Stack, Microtask Queue, Macrotask Queue, the Event Loop itself, and callback scheduling — including Promise jobs, timers, async operations, and queue prioritization.

---

### 12. WebAssembly (WASM)

```
Source Language (C/C++/Rust/Go/Zig)
        ↓
Compiler / Toolchain
        ↓
.wasm
        ↓
Host Runtime Loader
        ↓
WASM Runtime (Module Loader → Validator → Compiler/JIT → Execution Engine → Linear Memory)
        ↓
Machine Code → CPU
```

Supported runtime models: Browser, Node.js, Bun, Deno.

---

## Intended Audience

- JavaScript learners
- Frontend & backend developers
- Full-stack engineers
- Computer science students
- Runtime internals enthusiasts
- V8 engine learners
- Technical interview preparation

---

## Important Notes

JSCEP is an educational architecture model. Some sections intentionally simplify engine implementation details for learning clarity — for example, execution context visualization, environment record representation, memory layout models, and heap simplification.

Actual implementations vary across engines (V8, SpiderMonkey, JavaScriptCore, and legacy Chakra), and evolve over time as engines are updated.

---

## References

- [ECMAScript Specification](https://tc39.es/ecma262/)
- [V8 Documentation](https://v8.dev/docs)
- [Node.js Documentation](https://nodejs.org/en/docs)
- [MDN Web Docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
- [WebAssembly Documentation](https://webassembly.org/)

---

## License

Educational and Research Use.

---

## Author

**Pranjul Mishra**
*JSCEP — JavaScript Code Execution Process*
Understanding JavaScript from Source Code to CPU Execution.
