# 8086 Microprocessor — Exam Crash Course

A simplified version of your lecture, built around what your CT questions are actually testing.

---

## 1. The One-Paragraph Version

The 8086 is a **16-bit** microprocessor (1978, Intel) that started the x86 family. It has a
**20-bit address bus** (so it can reach **1 MB** of memory) but only **16-bit registers** —
so it glues a segment + an offset together to make a full address. Internally it's split into
two halves that work **at the same time**: one half fetches instructions, the other executes
them. That overlap is the single most important idea in the whole chapter.

---

## 2. Internal Structure — BIU + EU

Think of it as a **kitchen with two people**:

| Unit | Job | Analogy |
|---|---|---|
| **BIU** (Bus Interface Unit) | Fetches instructions from memory, reads/writes data, generates addresses, holds the 6-byte prefetch queue | The person walking to the fridge to grab the next ingredient |
| **EU** (Execution Unit) | Decodes and executes instructions, does math/logic, updates flags | The person actually cooking |

**Why split them?** While EU is "cooking" (executing), BIU is already "walking to the fridge"
(fetching the next instruction) instead of standing around waiting. This overlap is called
**pipelining**, and it's why the 8086 is faster than if one unit did everything step-by-step.

BIU contains: CS, DS, SS, ES (segment registers) + IP + the 6-byte queue.
EU contains: ALU, flag register, AX/BX/CX/DX, SP/BP/SI/DI, instruction decoder.

---

## 3. Registers — 14 total, all 16-bit

**General purpose (can split into 8-bit halves, e.g. AX → AH+AL):**
- **AX** — Accumulator: math, multiply/divide, I/O
- **BX** — Base: holds memory addresses (`[BX]`)
- **CX** — Counter: loops, shifts, string repeats
- **DX** — Data: extends AX in MUL/DIV (DX:AX), I/O port addressing

**Pointer/Index (used for addresses, can't split into 8-bit):**
- **SP** — Stack Pointer (top of stack, works with SS)
- **BP** — Base Pointer (used to access stack data)
- **SI** — Source Index (source in string ops)
- **DI** — Destination Index (destination in string ops)

**Segment registers (the "which 64 KB neighborhood" registers):**
- **CS** — Code Segment (with IP → next instruction)
- **DS** — Data Segment (normal data)
- **SS** — Stack Segment (with SP)
- **ES** — Extra Segment (string destination)

**Special:**
- **IP** — Instruction Pointer (offset of next instruction)
- **FLAGS** — 9 status/control bits

---

## 4. Memory & Segmentation — the formula you must never forget

A 16-bit register can only reach 64 KB. To reach the full 1 MB, the 8086 combines two
16-bit values:

```
Physical Address = Segment × 10H + Offset
```

**Worked example:** CS = 2000H, IP = 1500H
```
2000H × 10H = 20000H
20000H + 1500H = 21500H  ← this is where the next instruction is fetched from
```

Common pairings:

| Purpose | Segment | Offset |
|---|---|---|
| Fetch instruction | CS | IP |
| Access data | DS | BX / SI / DI |
| Stack | SS | SP / BP |
| String destination | ES | DI |

---

## 5. Flags — 9 active, two groups

**Status flags** (report what just happened):
- **CF** — Carry (carry out / borrow)
- **PF** — Parity (even number of 1-bits in result)
- **AF** — Auxiliary carry (bit 3→4, used in BCD)
- **ZF** — Zero (result = 0)
- **SF** — Sign (MSB of result: 1 = negative)
- **OF** — Overflow (signed math went out of range)

**Control flags** (you set these on purpose):
- **TF** — Trap (single-step debugging mode)
- **IF** — Interrupt enable/disable
- **DF** — Direction (0 = SI/DI increment, 1 = decrement, for string ops)

---

## 6. The 6-byte Instruction Queue = Pipelining

While EU executes the current instruction, BIU is already fetching the *next* ones into a
6-byte queue. EU just pulls from the queue instead of waiting on memory every time.

**If a jump/branch happens:** the queue is thrown away and refilled from the new address
(since the prefetched bytes are now wrong).

---

## 7. Addressing Modes — how an instruction says "where's the data?"

| Mode | Example | Meaning |
|---|---|---|
| Immediate | `MOV AX, 1234H` | value is right there in the instruction |
| Register | `MOV AX, BX` | value is in a register |
| Direct | `MOV AX, [2000H]` | offset given directly |
| Register indirect | `MOV AX, [BX]` | register *holds* the offset |
| Based | `MOV AX, [BX+05]` | base register + displacement |
| Indexed | `MOV AX, [SI+10]` | index register + displacement |
| Based-indexed | `MOV AX, [BX+SI]` | base + index |
| Based-indexed + disp. | `MOV AX, [BX+SI+08]` | base + index + displacement |

---

## 8. Stack — LIFO (Last In, First Out)

Uses **SS** (segment) + **SP** (offset, points to the top).

- **PUSH**: SP decreases by 2, then value stored at SS:SP
- **POP**: value read from SS:SP, then SP increases by 2

**Golden rule:** whatever was pushed *last* comes out *first*. This is exactly why pushing
two registers and popping them back **swaps** their values — see CT Q2 below.

---

## 9. MOV and XCHG — what you're NOT allowed to do

| Restriction | Applies to |
|---|---|
| No memory → memory | Both MOV and XCHG |
| Operand sizes must match (can't mix 8-bit/16-bit) | Both |
| CS can never be a destination | MOV |
| No immediate value directly into a segment register | MOV (go through a GP register first) |
| No immediate operand at all | XCHG |
| Segment registers not allowed | XCHG |

---

## 10. Minimum vs Maximum Mode

| | Minimum Mode | Maximum Mode |
|---|---|---|
| System | Single processor | Multiprocessor/coprocessor |
| MN/MX̅ pin | +5V (HIGH) | GND (LOW) |
| Who makes control signals | 8086 itself | External 8288 bus controller |
| Complexity | Simple, cheap | More complex |

---

## 11. Advantages & Limitations (this is CT Q3's territory)

**Advantages:** 16-bit processing power, 1 MB addressable memory, flexible segmented
memory, faster execution via the 6-byte prefetch queue, rich instruction set (arithmetic,
logic, branching, strings, stack), became the foundation of the entire x86 family.

**Limitations:** no on-chip cache, no memory protection, no built-in floating-point unit,
segmentation adds programming complexity, low clock speed by modern standards,
multiplexed address/data pins need external latch chips.

---

## 12. Your CT Questions — Worked Answers

### CT01 Q1 — "Introduce the 8086's basic structure" (10 marks)
Structure your answer around: **(1)** it's a 16-bit processor with 20-bit address bus / 16-bit
data bus, **(2)** it's split into BIU + EU that work in parallel, **(3)** list BIU's job
(fetch, address generation, 6-byte queue) and EU's job (decode, execute, flags), **(4)** mention
the 14 registers grouped by type, **(5)** close with the physical address formula as the payoff
for *why* segmentation exists. That's a clean, organized 10-mark answer — see Sections 2–4 above.

### CT01 Q2 — Trace the code

```
MOV AX, 1234H   ; AX = 1234H
MOV BX, 5678H   ; BX = 5678H
PUSH AX         ; stack top = 1234H
PUSH BX         ; stack top = 5678H   (1234H sits just below it)
POP AX          ; AX = 5678H   (pops the LAST thing pushed = BX's value)
POP BX          ; BX = 1234H   (pops what's left = AX's original value)
```

**Final answer: AX = 5678H, BX = 1234H.**

Notice the values got **swapped**. That's the whole point of the question — it's testing
whether you understand LIFO order, not just arithmetic.

### CT01 Q3 — Benefits and challenges (10 marks)
Use Section 11 directly. Structure: 3 benefits, 3 challenges, one closing line ("the 8086 was
a huge leap for its time but modern processors solved these gaps with caching, memory
protection, and built-in FPUs").

### CT2 Q4 — "The instruction queue disappears — what happens to performance?"
Without the queue, BIU can't prefetch ahead — so every single time EU finishes an
instruction, it has to **stop and wait** for BIU to go fetch the next one from memory before
it can continue. Fetch and execute can no longer overlap. Result: **performance drops
significantly** — you lose the whole benefit of pipelining and effectively go back to a
slow, purely sequential fetch → decode → execute cycle for every instruction, with no
memory-bus time being used productively in between.

### CT2 Q5 — "Only using AX for everything — efficient?"
**No, this is inefficient**, and you should say why:
- The 8086 gives you AX, BX, CX, DX, SI, DI, SP, BP specifically so you can hold multiple
  values *at once* without touching memory or the stack repeatedly.
- Using only AX forces constant PUSH/POP or memory read-writes just to "make room" for a
  new value — more instructions, slower execution, more memory traffic.
- Some instructions have *hardwired* register expectations that AX-only code fights against —
  e.g. MUL/DIV automatically use DX:AX, loops conventionally use CX, so cramming everything
  into AX actually causes more conflicts and extra shuffling, not less.
- Conclusion: it's like doing all your cooking using only one pan when you have a full rack —
  technically possible, but slow and clumsy.

### CT2 Q6 — "What if MOV didn't exist?"
You'd have to fake data transfer using other instructions:
- **PUSH / POP** — push the source onto the stack, then pop it into the destination (register
  → register transfer via the stack).
- **XCHG** — swaps two operands; if one side is set to 0 first, an exchange effectively
  becomes a copy.
- **Arithmetic tricks** — e.g. `XOR AX, AX` then `OR AX, BX` copies BX into AX (clear the
  destination, then OR in the source).
- **LEA / LDS / LES** — for moving addresses instead of values.

The honest conclusion: it's *possible* but every transfer would take 2–3 instructions
instead of 1, code would be longer, slower, and much harder to read — which is exactly why
MOV exists as a dedicated, simple instruction in the first place.

---

## Quick Memory Checklist Before the Exam
- Physical Address = Segment × 10H + Offset
- 20-bit address bus → 1 MB, 16-bit data bus → 2 bytes at a time
- BIU fetches, EU executes — they overlap (pipelining)
- Stack = LIFO, uses SS:SP
- 9 flags: CF PF AF ZF SF OF (status) + TF IF DF (control)
- MOV/XCHG can't do memory-to-memory
