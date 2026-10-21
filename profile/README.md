# Petracode: Scalable Formal Verification for Object-Oriented Software

Petracode provides a language-agnostic, scalable formal verification standard for object-oriented programs across mainstream languages (Java, C++, C#, Python, TypeScript, Go and Rust).

Verification is assertion-driven: contracts and system invariants are embedded directly within source programs as executable assertions. Petracode extracts these into a unified intermediate representation, lowers them to equivalent assertion-preserving Java models, and formally proves safety using symbolic execution engines.

**Petra Examples:** [https://docs.petracode.co.uk/guides/whatispetra/](https://github.com/petracode-labs/petra-examples)

**Official Documentation:** [https://docs.petracode.co.uk/guides/whatispetra/](https://docs.petracode.co.uk/guides/whatispetra/)

**Commercialization Site:** [www.petracode.co.uk](https://www.petracode.co.uk)

**Petra Thesis:** [https://eprints.soton.ac.uk/id/eprint/512032/](https://eprints.soton.ac.uk/id/eprint/512032/)

---

## Architecture Overview

```text
+---------------------------------------------------------------------------------------+
|                    Source Languages with Embedded Assertions e.g.                     |
|    [ Java ]              [ Python ]             [ C++ ]              [ C# ]           |
|    (assert cond)        (assert cond)        (assert(cond))      (Debug.Assert)       |
+---------------------------------------------------------------------------------------+
                                           │
                                           │ AST Extraction & Assertion Normalization
                                           ▼
+---------------------------------------------------------------------------------------+
|                                 Petra OOP Subset AST                                  |
|                                                                                       |
|   ┌────────────────────────────────────────┐  ┌───────────────────────────────────┐   |
|   │              Petra Coord               │  │            Petra Base             │   |
|   │ ────────────────────────────────────── │  │ ───────────────────────────────── │   |
|   │ • Hierarchical state coordination      │  │ • Leaf-level algorithm execution  │   |
|   │ • State-machine transitions            │  │ • Complex data structure state    │   |
|   │ • Architectural invariants (asserts)   │  │ • Numerical & floating-point math │   |
|   │ • Structural composition boundaries    │  │ • Algorithmic contracts (asserts) │   |
|   └────────────────────────────────────────┘  └───────────────────────────────────┘   |
+---------------------------------------------------------------------------------------+
                                           │
                                           │ Canonical Lowering (Preserving Assertions)
                                           ▼
+---------------------------------------------------------------------------------------+
|                              Target Java Equivalent Model                             |
|   • Strongly-typed, deterministic semantic anchor                                     |
|   • Direct translation of program logic and operational semantics                     |
|   • Normalized Java assertion statements: `assert <condition> : <context>`            |
+---------------------------------------------------------------------------------------+
                                           │
                                           │ Symbolic State-Space Exploration
                                           ▼
+---------------------------------------------------------------------------------------+
|                               Symbolic Execution Engine                               |
|   • Systematic path exploration & symbolic state evaluation                           |
|   • Reachability analysis against assertion violations                                |
|   • Isolated leaf path exploration (Base) + Hierarchical path reduction (Coord)       |
|                                          │                                            |
|                                          ▼                                            |
|                 [ All Assertions Hold ]  OR  [ Counter Example ]                      |
+---------------------------------------------------------------------------------------+
```

## Core Verification Model: Dual-Subset Architecture
Standard formal verification often collapses under industrial OOP applications due to heap aliasing, dynamic dispatch, and unstructured side effects. PetraCode enforces scalability by segregating programs into two distinct, contractually bound tiers:

### 1. Petra Coord (Coordination Layer)
Purpose: Oversees system architecture, lifecycle management, and high-level behavioral flow.

Characteristics: State-machine transitions, component topologies, and hierarchical routing.

Verification Impact: Abstracted from low-level algorithmic mutation. Coordination properties are checked using lightweight compositional reasoning and structural invariants, eliminating path explosion at the architectural level.

### 2. Petra Base (Algorithmic / Leaf Layer)
Purpose: Executes concrete operations, algorithmic computations, and data transformations.

```text
[Source Code] ──> [Parser / Extractor] ──> [Petra AST] ──> [Java Lowering] ──> [Verifier]
```

Frontend Normalization: Source files from supported languages pass through language-specific frontends to extract the syntax conforming to the Petra OOP subset specifications.

Canonical Petra AST: Source constructs map to unified primitives, explicitly marking constructs as either Coord (structural/state orchestration) or Base (computational leaf).

Java Lowering: The AST lowers to an equivalent, contract-annotated Java model. Java acts as the stabilized target platform, utilizing its deterministic memory and type model as the semantic anchor.

Verification Pass: The verification backend checks the translated Java program.

Verifies that Petra Base leaves satisfy their functional pre- and post-conditions.

Verifies that Petra Coord components preserve hierarchical state invariants across compositions.

* Produces formal verification certificates or actionable counterexample traces.

* Characteristics: Pure mathematical logic, localized data structure mutations (e.g., custom trees, buffers), and floating-point/arithmetic kernels.

* Verification Impact: Completely isolated inside leaf methods. Verified strictly against local pre-/post-conditions without needing the entire global system state.
