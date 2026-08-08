# Mark-Compact Garbage Collection in Ada

## Project Overview
This repository implements the fundamental **Mark-Compact** garbage collection algorithm in Ada. It models heap memory as a strongly-typed block array and executes memory collection to reclaim space taken up by dead (unreachable) objects.

## Features
- **Strong Typing**: Dedicated integer range bindings to prevent pointer corruption out of bounds.
- **Mark Phase**: DFS-based algorithm handling disjoint object graphs and cyclic dependencies to isolate reachable objects.
- **Variant 1: LISP 2 Algorithm (Sliding)**: Uses 3 passes over the heap to slide live objects linearly to the beginning, preserving their historical instantiation order.
- **Variant 2: Two-Finger Algorithm (Swapping)**: Uses 2 passes for high-efficiency compaction by swapping live objects from the end of the heap into dead "holes" at the front. (Applicable here because our block structure models fixed-size allocation).

## Testing
To assure maximum confidence in memory-critical operations, this project employs rigorous **Verification and Validation (V&V)** paradigms.

**The Test Philosophy**: We assume the code is intrinsically *incorrect* or handles edge-cases *dangerously*. The suite is constructed of assertions explicitly designed to disprove these assumptions. A `PASS` means our pessimistic assumption about the codebase was empirically proven false.

The test categories verify:
1. **Functional Correctness (Tests 2, 5, 7):** V&V check that reachable graphs are logically marked, orphaned memory is truly recycled, and forwarding addresses update seamlessly across LISP2 and Two-Finger algorithms. 
2. **Cycle & Graph Safety (Test 3):** Disproves the assumption that circular references lock the process into infinite loops, guaranteeing execution safety.
3. **Edge Case Guarding (Tests 1, 4, 6):** Validates the system against mathematical limits (zero elements, max elements), proving the heap operations survive initialization boundaries.
4. **Error Handling (Test 8):** Guarantees constraints are enforced at runtime (e.g., stopping execution on spoofed root pointers).

These tests matter because critical embedded systems written in Ada require undeniable proof of reliability and memory safety. Verification proves the algorithm matches mathematical literature specs; Validation proves the GC operates perfectly within the simulation sandbox.

## Usage

### Compilation
The codebase requires the GNAT toolchain. A standard `Makefile` is provided. To build both the demo application and the test suite:
```bash
make all
