# Loom 🧵
### A Lightweight Userspace Threading Library in C

![Language](https://img.shields.io/badge/language-C-blue)
![Assembly](https://img.shields.io/badge/assembly-x86__64%20%2F%20Arm64-red)
![Status](https://img.shields.io/badge/status-In%20Development-orange)

**Loom** is a minimal, educational implementation of "Green Threads" (userspace threads) written in C and Assembly. It bypasses the operating system's kernel to manage thread execution, stack allocation, and context switching entirely in userspace.

The goal of this project is to demonstrate low-level systems concepts: **CPU register management**, **stack pointer manipulation**, and **cooperative scheduling**.

---

## 🛠 Architecture & Features

Loom implements a **Many-to-One** threading model (many user threads map to a single kernel thread).

* **Custom Context Switching:** Implemented directly in Assembly (x86-64 / AArch64) to save/restore callee-saved registers.
* **Stack Management:** Manual allocation of separate stack frames (`malloc`) for each thread.
* **Cooperative Scheduler:** A Round-Robin scheduler that relies on threads explicitly yielding control (`loom_yield()`).
* **Thread States:** A simple state machine (RUNNING, READY, FINISHED).

## 🧩 The Core Challenge: Context Switching

At the heart of Loom is the `context_switch` function. Unlike `setjmp`/`longjmp`, Loom manually manipulates the CPU state.

**On x86-64, this involves:**
1.  Saving `rsp`, `rbp`, `rbx`, `r12-r15` to the current thread's control block.
2.  Switching the hardware Stack Pointer (`rsp`) to the target thread's stack.
3.  Restoring the target thread's registers.
4.  Jumping to the stored Instruction Pointer (`rip`).

## 🚀 Roadmap

- [ ] **Phase 1: The Context Struct**
    - Define `struct ThreadContext` to hold register states.
    - Define `struct Thread` (TCB) to hold stack pointers and IDs.
- [ ] **Phase 2: The Assembly Core**
    - Implement `switch_context.S` for x86-64/AArch64.
- [ ] **Phase 3: The Runtime**
    - Implement `loom_create()` to initialize stacks.
    - Implement `loom_yield()` to swap threads.
- [ ] **Phase 4: The Scheduler**
    - Implement a simple circular queue for Round-Robin scheduling.

## 📚 References & Reading

This project explores concepts found in:
* *Operating Systems: Three Easy Pieces* (Concurrency)
* *Computer Systems: A Programmer's Perspective* (Stack Frames & Procedure Calls)

---

> **Author's Note:** This is a portfolio project to demonstrate understanding of the hardware/software interface. It is not intended for production use (yet).# loom
