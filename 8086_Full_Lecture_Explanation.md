# Full Simple Explanation — LECTURE 3 (8086 Microprocessor & Related Topics)

This walks through your entire lecture note, section by section, in plain language.
Nothing is skipped — it's just explained the "easy" way.

---

## PART A — THE 8086 ITSELF

### A1. What is the 8086?
A 16-bit microprocessor made by Intel in 1978. "16-bit" describes its **ALU**, its
**registers** (AX, BX, CX, DX...), and its **data bus** — all of them work with 16 bits
(2 bytes) at once. Example: if AX = 1234H and BX = 1111H, `ADD AX, BX` gives 2345H in a
single operation, because the ALU handles all 16 bits together, not one byte at a time.

Its core numbers, all worth memorizing cold:

| Feature | Value |
|---|---|
| Word length | 16-bit |
| Address bus | 20-bit → 2²⁰ = 1,048,576 bytes = **1 MB** memory |
| Data bus | 16-bit → moves 2 bytes per transfer |
| I/O address space | 64 KB |
| Hardware interrupts | 256 |
| Prefetch queue | 6 bytes |
| Package | 40-pin DIP |
| Clock speed | 5 / 8 / 10 MHz |

### A2. Internal Architecture — Two Halves Working Together
The chip is split into two units that run **at the same time**:

**BIU (Bus Interface Unit)** — the "delivery" half. It:
- fetches instructions from memory
- reads/writes data to memory and I/O ports
- generates the physical address
- stores prefetched instructions in a 6-byte queue

Its parts: segment registers (CS, DS, SS, ES), the Instruction Pointer (IP), the
6-byte queue, and address-generation circuitry.

**EU (Execution Unit)** — the "worker" half. It:
- decodes instructions
- does arithmetic and logic
- updates flags
- moves data around internally

Its parts: the ALU, the flag register, the general-purpose registers, pointer/index
registers, and the instruction decoder.

**Why split them at all?** So one half can prepare the next task while the other is
still busy with the current one — see A3.

### A3. Why BIU and EU Work Separately (Pipelining)
While EU is busy *executing* one instruction, BIU is already out *fetching* the next
ones and dropping them into the 6-byte queue. So fetching and executing overlap instead
of happening one-after-another.

**Example:**
```
MOV AX, BX
ADD AX, CX
SUB AX, DX
```
While `MOV AX, BX` is being executed by EU, BIU is *already* fetching `ADD AX, CX` and
`SUB AX, DX` in the background. Nobody sits idle. This is why the queue exists — it's
the whole basis of pipelining, and it's the reason the 8086 is faster than a design
where fetch and execute happen strictly one at a time (see Part A18 below for what
breaks if this queue is removed).

### A4. Register Organization — All 14 Registers
The 8086 has 14 registers, all 16-bit, split into four groups.

**Group 1 — General purpose (usable for math, and splittable into 8-bit halves):**

| Register | Main job | Example |
|---|---|---|
| **AX** (Accumulator) | Arithmetic, multiply/divide, I/O. Splits into AH (high 8 bits) + AL (low 8 bits) | `MOV AX,1234H` / `ADD AX,0002H` |
| **BX** (Base) | Holds addressing offsets, general storage | `MOV BX,2000H` then `MOV AX,[BX]` |
| **CX** (Count) | Loop counters, shift/rotate counts, string repeat counts | `MOV CX,05H` / `LOOP AGAIN` |
| **DX** (Data) | Multiply/divide (pairs with AX as DX:AX), I/O port addressing | `MUL BX` → result lands in DX:AX |

**Group 2 — Pointer and Index registers (for addressing, cannot split into 8-bit):**

| Register | Job |
|---|---|
| **SP** (Stack Pointer) | Points to the top of the stack. Physical stack address = SS×10H + SP |
| **BP** (Base Pointer) | Mainly used to reach data sitting on the stack, e.g. `MOV AX,[BP+04]` |
| **SI** (Source Index) | Source pointer in string instructions, e.g. `MOVSB` reads from DS:SI |
| **DI** (Destination Index) | Destination pointer in string instructions, e.g. `MOVSB` writes to ES:DI |

**Group 3 — Segment registers (memory is chopped into 64 KB "neighborhoods," these say which one):**

| Register | Job |
|---|---|
| **CS** (Code Segment) | Base address of the code. Physical address = CS×10H + IP |
| **DS** (Data Segment) | Base for normal data access |
| **SS** (Stack Segment) | Base for the stack, works with SP or BP |
| **ES** (Extra Segment) | Extra data area, often the *destination* in string ops |

**Group 4 — Special registers:**
- **IP** (Instruction Pointer) — offset of the *next* instruction; combined with CS as CS:IP.
- **Flag Register** — status/control bits (9 active ones — full breakdown next).

### A5. Flag Register — 9 Active Flags

**Status flags** (they report the *result* of the last operation):

| Flag | Set when... | Example |
|---|---|---|
| **CF** (Carry) | there's a carry-out (addition) or borrow (subtraction) at the top bit | `FFFFH + 0001H = 0000H` with CF=1 |
| **PF** (Parity) | the result has an *even* number of 1-bits | `00000110` has two 1s → PF=1 |
| **AF** (Auxiliary Carry) | there's a carry from bit 3 into bit 4 — matters for BCD math | — |
| **ZF** (Zero) | the result is exactly zero | `5 - 5 = 0` → ZF=1 |
| **SF** (Sign) | shows if result is negative (MSB=1) or positive (MSB=0) | — |
| **OF** (Overflow) | signed arithmetic goes out of its valid range | `+127 + 1 = -128` (8-bit signed) → OF=1 |

**Control flags** (you deliberately set these to change processor behavior):

| Flag | Meaning |
|---|---|
| **TF** (Trap) | TF=1 → processor runs one instruction at a time (single-step / debug mode) |
| **IF** (Interrupt) | IF=1 → maskable interrupts allowed; IF=0 → they're ignored |
| **DF** (Direction) | Controls string instructions: DF=0 → SI/DI count up; DF=1 → SI/DI count down |

### A6. Memory Organization — Why Segmentation Exists
The 8086 can reach **1 MB** of memory because it has 20 address lines (2²⁰ = 1,048,576
bytes). But its registers are only **16-bit**, and a 16-bit register alone can only count
up to 64 KB — nowhere near 1 MB. So a single register *cannot* hold a full 20-bit address
by itself.

**The fix: segmentation.** Memory is split into chunks (segments) up to 64 KB each —
Code, Data, Stack, and Extra segments. Each segment has a **base address** (in a segment
register) and the exact spot inside it is an **offset** (in another register). Combine
the two, and you get a full 20-bit address.

**The formula (memorize this):**
```
Physical Address = Segment × 10H + Offset
```
(Multiplying by 10H is the same as shifting the segment value left by 4 bits.)

**Worked Example 1:** CS = 2000H, IP = 1500H
```
2000H × 10H = 20000H
20000H + 1500H = 21500H  ← next instruction comes from here
```

**Worked Example 2:** DS = 2500H, SI = 1200H
```
2500H × 10H = 25000H
25000H + 1200H = 26200H
```

**Why bother with segmentation at all?**
1. It lets 16-bit registers reach a full 1 MB (segment + offset math does the heavy lifting).
2. It logically separates code, data, and stack instead of dumping everything together.
3. Better organization: code lives in CS, data in DS, stack in SS.
4. Relocation flexibility: you can move a whole program just by changing its segment
   registers, without rewriting every address inside it.

**Which segment pairs with which offset register:**

| Purpose | Segment Register | Offset Register | Written as |
|---|---|---|---|
| Fetch instruction | CS | IP | CS:IP |
| Access data | DS | BX / SI / DI / a plain number | DS:offset |
| Stack access | SS | SP / BP | SS:offset |
| String destination | ES | DI | ES:DI |

### A7. The 6-Byte Instruction Queue
Sits inside the BIU. Working order:
1. BIU fetches instruction bytes from memory.
2. It stores them in the 6-byte queue.
3. EU pulls bytes from the queue and executes them.

**Benefit:** fetching and executing overlap, which is the whole basis of pipelining and
speed gains (see A3 and A18).

### A8. Data Bus and Address Bus
- **Address bus = 20 bits** → used to point at memory locations → 2²⁰ = 1 MB reachable.
- **Data bus = 16 bits** → used to move data in/out → the 8086 can read or write **2
  bytes at once**, not just 1.

### A9. Even and Odd Memory Banks
Because the data bus is 16-bit but memory itself is organized byte-by-byte, the 8086
splits memory into two 8-bit "lanes":
- **Even bank** → stores bytes at even addresses
- **Odd bank** → stores bytes at odd addresses

**Why?** So a full 16-bit word (2 bytes) can be grabbed in **one** memory operation
instead of two — one byte comes from each bank simultaneously.

**Example:** a word starting at 2000H has its low byte at 2000H (even bank) and high
byte at 2001H (odd bank) — both fetched together, in one go.

**Selection signals:** A0 and BHE decide which bank(s) are active:

| A0 | BHE | Bank selected |
|---|---|---|
| 0 | 1 | lower/even bank only |
| 1 | 0 | upper/odd bank only |
| 0 | 0 | both banks (word at an even address) |

**The catch — odd-address words are slower.** If a word starts at an *odd* address (say
2001H), its two bytes (2001H and 2002H) don't line up neatly across the two banks —
they're not on the same 16-bit boundary. So the 8086 needs **two separate memory
operations** instead of one. Rule of thumb: **word at even address = faster, word at odd
address = slower.**

### A10. Pin Configuration — 40 Pins Total
The 40 pins handle: address bus, data bus, control signals, interrupt signals, clock and
power, and bus arbitration (sharing the bus with other devices).

**Key signal groups:**
- **AD0–AD15** — multiplexed address/data lines: they carry the *address* in the first
  part of a bus cycle, then switch to carrying *data* right after. Sharing pins this way
  keeps the pin count down.
- **A16/S3 – A19/S6** — carry the higher address bits during the address phase, then
  switch to carrying status bits the rest of the time.
- **Control signals** — `RD` (read), `WR` (write), `M/IO` (is this a memory or I/O
  access?), `ALE` (address latch enable — "lock in the address now"), `DT/R` (is data
  going out or coming in?), `DEN` (data enable).
- **Interrupt signals** — `INTR` (maskable interrupt request), `NMI` (non-maskable
  interrupt — can't be turned off).
- **DMA/bus request signals** — `HOLD` and `HLDA`, used when another device asks to take
  over control of the system bus for a moment.

---

## PART B — SEGMENT REGISTERS, WITH REAL-LIFE ANALOGIES

### B1. Segment Registers as a Library
Think of memory like a library, organized into sections:
- **CS** = the instructions section (what to do)
- **DS** = the books/data section (information to work with)
- **SS** = your working desk (temporary storage, push/pop)
- **ES** = an extra storage room (extra data, string operations)

This division: (1) breaks a huge memory space into manageable chunks, (2) improves speed
and structure, (3) keeps code, data, and stack from getting mixed up.

### B2. How Segment Registers Actually Interact During Execution
Every segment register pairs with an offset register to build a real address:

**CS + IP → instruction fetching.**
Example: CS=2000H, IP=0100H → Physical Address = 20000H + 0100H = **20100H**

**DS + (BX/SI/DI) → data access.**
Example: DS=3000H, BX=0010H → Physical Address = 30000H + 0010H = **30010H**

**SS + SP → stack operations** (PUSH, POP, CALL, RET all use SS:SP; the stack grows
*downward*, meaning SP gets smaller as you push more data onto it).
Example: SS=4000H, SP=FFEEH → Physical Address = 40000H + FFEEH

**ES + DI → string and extra data operations** (instructions like MOVS, STOS, CMPS,
SCAS). Typical pairing: source comes from DS:SI, destination goes to ES:DI.

---

## PART C — THE STACK IN ACTION

### C1. Two Friends Swapping Secret Numbers (PUSH/POP Example)
Say AX = 1234H (Friend A's number) and BX = 5678H (Friend B's number). You want to swap
them *without* directly copying one into the other. The stack (a "temporary memory box")
does this safely:

```
MOV AX, 1234H
MOV BX, 5678H

PUSH AX         ; store AX temporarily
PUSH BX         ; store BX temporarily

POP AX          ; AX gets what was pushed LAST (BX's value)
POP BX          ; BX gets what's left (AX's original value)
```

**Step by step:**
1. `PUSH AX` → stack top now holds 1234H
2. `PUSH BX` → stack top now holds 5678H, with 1234H sitting just underneath it
3. `POP AX` → grabs the top value (5678H) → **AX = 5678H**
4. `POP BX` → grabs what's left (1234H) → **BX = 1234H**

**Final result: AX = 5678H, BX = 1234H — the two values swapped.** This works because
the stack is **LIFO** (Last In, First Out): whatever went in most recently comes out
first.

### C2. Undo in Microsoft Word = Stack Behavior
Every time you type an edit, imagine it's a **PUSH** onto a stack. Every time you hit
**Undo**, that's a **POP** — it removes the *most recent* change first and restores the
version right before it.

```
MOV AX, 1001H
PUSH AX          ; Save edit 1

MOV AX, 1002H
PUSH AX          ; Save edit 2

MOV AX, 1003H
PUSH AX          ; Save edit 3

POP AX           ; Undo → removes 1003H, back to edit 2
POP AX           ; Undo → removes 1002H, back to edit 1
```

| Word action | Stack operation |
|---|---|
| Typing an edit | PUSH |
| Pressing Undo | POP |
| Most recent edit is removed first | LIFO principle |

---

## PART D — BUS CYCLES, EXPLAINED AS EVERYDAY EVENTS

### D1. READ Cycle = Borrowing a Book from a Library
A student: (1) requests a book, (2) the library finds it, (3) hands it over, (4) student
reads it and leaves. Map that onto the 8086's 4-phase READ cycle:

| Phase | What the CPU does | Library analogy |
|---|---|---|
| **T1** — Addressing | CPU places the memory address on the address bus, activates ALE | Student tells the librarian "I want this book" (the address) |
| **T2** — Control/turnaround | Address bus frees up, `RD` goes active-low, memory starts preparing data | Librarian starts locating the book |
| **T3** — Data transfer | Memory puts data on the data bus, CPU reads it (READY signal checked — if not ready, wait states are inserted) | Library hands the book over |
| **T4** — Completion | CPU stores the data internally, control signals go inactive, bus frees up | Student takes the book, reads it, leaves the counter |

### D2. WRITE Cycle = A Factory Manager Updating Store Records
A WRITE cycle is when the CPU *sends* data to memory or an I/O device to be stored.

| 8086 WRITE step | Factory analogy |
|---|---|
| CPU (the decision-maker) | Factory manager |
| Data being written | An order slip |
| Memory/I/O device | The store department |
| `WR` control signal | Instruction to update the records |
| Data gets stored | Record updated in the store's register |

**Steps:** (1) CPU places the address on the address bus, (2) places the data on the data
bus, (3) activates `WR`, (4) the memory/I/O device stores the data, (5) cycle completes.

### D3. Real-World System Example: Ambulance-Priority Traffic Lights
A system where the CPU adjusts traffic signals whenever an emergency ambulance is detected.

- **CPU (8086)** = the decision maker
- **Memory** = stores the traffic rules and timing program
- **I/O devices** = sensors and the traffic lights themselves
- **Ambulance sensor** = the input device

**Flow:**
1. **Input (READ cycle):** sensor detects the ambulance → sends a signal through an I/O
   port → CPU performs a READ → now the CPU "knows" an ambulance is on Road A.
2. **Processing:** CPU fetches the traffic-control program from memory, compares road
   priorities, and decides to give the ambulance's route a green light.
3. **Output (WRITE cycle):** CPU sends control signals to the traffic lights — green for
   the ambulance lane, red for the others.
4. **Memory's role throughout:** it just sits there holding the rules, timing sequences,
   and emergency logic that the CPU keeps consulting.

---

## PART E — MODES, ADDRESSING, AND THE INSTRUCTION SET

### E1. Minimum Mode vs Maximum Mode

| Feature | Minimum Mode | Maximum Mode |
|---|---|---|
| System type | Single processor | Multiprocessor / coprocessor |
| MN/MX̅ pin | Tied HIGH (+5V) | Tied LOW (GND) |
| Who generates control signals | The 8086 itself (ALE, RD, WR, M/IO, INTA) | An external 8288 bus controller, using the 8086's S2/S1/S0 status lines |
| Complexity/cost | Simple, cheap | More complex |
| Typical use | PCs, embedded/educational systems | Multiprocessor systems, systems with an 8087 coprocessor, industrial control |

### E2. Addressing Modes — 8 Ways to Say "Where's the Data?"
(Full table already in Part A — quick recap with the pattern to notice:)
Immediate (value is baked into the instruction) → Register (value's in a register) →
Direct (a fixed memory offset) → Register indirect (a register *holds* the offset) →
Based (register + a number) → Indexed (index register + a number) → Based-indexed
(base + index) → Based-indexed + displacement (base + index + a number). Each mode adds
one more layer of flexibility for finding data in memory.

### E3. Instruction Set — 7 Categories
1. **Data transfer:** MOV, PUSH, POP, XCHG, IN, OUT, LEA, LDS, LES
2. **Arithmetic:** ADD, ADC, SUB, SBB, INC, DEC, MUL, IMUL, DIV, IDIV, CMP, NEG
3. **Logical:** AND, OR, XOR, NOT, TEST
4. **Shift/rotate:** SHL/SAL, SHR, SAR, ROL, ROR, RCL, RCR
5. **String:** MOVSB/MOVSW, LODSB/LODSW, STOSB/STOSW, CMPSB/CMPSW, SCASB/SCASW
6. **Branch/control transfer:** JMP, CALL, RET, LOOP, JC/JNC/JZ/JNZ etc.
7. **Processor control:** NOP, HLT, CLC, STC, CLI, STI, CLD, STD

### E4. Worked Instruction Example
```
MOV AX, 2500H   ; AX ← 2500H
MOV BX, 1200H   ; BX ← 1200H
ADD AX, BX      ; AX ← AX + BX = 2500H + 1200H = 3700H
```
(Flags get updated based on the ADD's result — check ZF, SF, CF, OF as needed.)

### E5. PUSH/POP Mechanics, Precisely
- **PUSH:** SP decreases by 2, *then* the value is stored at SS:SP.
- **POP:** the value is read from SS:SP, *then* SP increases by 2.

### E6. MOV and XCHG — Restrictions to Never Forget

| Instruction | Restriction |
|---|---|
| MOV | Can't transfer memory-to-memory — `MOV [2000H],[3000H]` is invalid; go through a register |
| MOV | Can't move an immediate value straight into a segment register — go through a GP register first (`MOV AX,1234H` then `MOV DS,AX`) |
| MOV | Operand sizes must match — `MOV AX,BL` is invalid (16-bit can't take an 8-bit source) |
| MOV | CS can never be the destination — `MOV CS,AX` is invalid |
| XCHG | Can't exchange memory-to-memory |
| XCHG | No immediate operand allowed — `XCHG AX,1234H` is invalid |
| XCHG | Operand sizes must match |
| XCHG | Segment registers aren't allowed — `XCHG AX,DS` is invalid |

### E7. The Conditional Jump Range Problem
Instructions like `JE`, `JNE`, `JC`, `JZ` etc. use an **8-bit signed displacement**, so
they can only jump:
```
-128 bytes backward  to  +127 bytes forward
```
If your target label is farther away than that, the assembler throws a "jump out of
range" error — because, unlike JMP, conditional jumps have no 16-bit "far" version.

**The fix:** flip the condition and add an unconditional JMP, which *can* use a 16-bit
displacement:
```
; instead of:  JE FAR_LABEL   (out of range)
CMP AX, BX
JNE SKIP          ; if condition is FALSE, skip the far jump
JMP FAR_LABEL     ; if condition is TRUE, this runs — JMP can reach much farther
SKIP:
```

### E8. Interrupts in 8086
The 8086 supports **256 interrupts**, split into:
- **Hardware interrupts:** `INTR` (maskable — can be turned off) and `NMI` (non-maskable
  — always gets through, used for critical events)
- **Software interrupts:** triggered by the `INT n` instruction (e.g. `INT 21H` for DOS
  services)

### E9. Reset Behavior
When the 8086 is reset, execution restarts from a **predefined memory location**
(typically near the top of memory) — this is part of how the system boots up.

### E10. Advantages of the 8086
1. 16-bit processing = better performance than 8-bit chips
2. 1 MB memory access was huge for its time
3. Segmentation gives flexible memory organization
4. The 6-byte queue speeds things up via pipelining
5. A rich instruction set (arithmetic, logic, branching, strings, stack handling)
6. It became the foundation of the entire x86 family

### E11. Limitations of the 8086
1. No on-chip cache
2. No memory protection
3. No built-in floating-point unit
4. Segmentation adds programming complexity
5. Low clock speed compared to modern chips
6. Multiplexed buses need extra external latch/support chips

### E12. Real-Life Significance
Even though the 8086 is old, it matters because it started the x86 line, many modern
processors still carry ideas from it, and it introduced design concepts still used in
CPU architecture today. The related 8088 chip powered the original IBM PC and helped x86
spread widely.

---

## PART F — NEAR vs FAR PROCEDURES

| Feature | NEAR Procedure | FAR Procedure |
|---|---|---|
| Location | Same code segment as the caller | A *different* code segment |
| What changes on CALL | Only IP | Both CS and IP |
| What's pushed onto the stack | Just the return IP | Both return CS and IP |
| Return instruction | `RET` | `RETF` |
| Speed | Faster (less stack use) | Slightly slower |

**NEAR example:**
```
MYPROC PROC NEAR
   ; instructions
   RET
MYPROC ENDP
CALL MYPROC
```

**FAR example:**
```
MYPROC PROC FAR
   ; instructions
   RETF
MYPROC ENDP
CALL FAR PTR MYPROC
```

---

## PART G — ASSEMBLY PROGRAM WALKTHROUGHS

### G1. Add Two 16-bit Numbers, Store in Memory
```
.MODEL SMALL
.STACK 100H
.DATA
NUM1   DW 1234H
NUM2   DW 0567H
RESULT DW ?
.CODE
MAIN PROC
   MOV AX, @DATA
   MOV DS, AX
   MOV AX, NUM1     ; load first number
   ADD AX, NUM2     ; add the second
   MOV RESULT, AX   ; store the sum
   MOV AH, 4CH
   INT 21H          ; end program
MAIN ENDP
END MAIN
```
Calculation: `1234H + 0567H = 179BH` → **RESULT = 179BH**

The same pattern applied to 2345H + 1234H gives **RESULT = 3579H**.

### G2. Reverse the Bit Pattern of AX (Without Changing AX)
```
MOV DX, AX        ; copy AX so the original stays untouched
MOV BX, 0000H     ; BX will hold the reversed bits
MOV CX, 16        ; 16 bits to process

REVERSE:
   SHR DX, 1       ; shift DX's lowest bit out into Carry Flag
   RCL BX, 1       ; roll that Carry bit into BX's lowest bit
   LOOP REVERSE    ; repeat 16 times
```
If AX = 1234H (binary `0001 0010 0011 0100`), the fully reversed pattern comes out as
BX = 2C48H, while **AX stays exactly 1234H** — untouched, because we only ever worked on
the copy in DX.

### G3. Display the Series 1–9 on Screen
```
.MODEL SMALL
.STACK 100H
.DATA
.CODE
MAIN PROC
   MOV AX, @DATA
   MOV DS, AX
   MOV CX, 9        ; loop 9 times
   MOV DL, '1'       ; start at character '1'
DISPLAY:
   MOV AH, 02H       ; DOS function: display one character
   INT 21H
   INC DL            ; move to next digit character
   LOOP DISPLAY      ; repeat until CX = 0
   MOV AH, 4CH
   INT 21H
MAIN ENDP
END MAIN
```
Output: `123456789`

---

## PART H — SUPPORT CHIPS: 8255 PPI

### H1. What the 8255 Does
The **8255 Programmable Peripheral Interface (PPI)** is a bridge chip that connects a
microprocessor to outside devices — keyboards, displays, printers, sensors, etc.

**Key features:** 24 I/O pins split into Port A, Port B, Port C (8 bits each); each port
can be set as input or output; supports several operating modes; commonly paired with
processors like the 8085.

### H2. The 8255's Operating Modes

| Mode | Name | Key trait | Example use |
|---|---|---|---|
| Mode 0 | Simple I/O | No handshaking | LEDs, switches |
| Mode 1 | Strobed I/O | Uses handshaking signals via Port C | Keyboard, printer |
| Mode 2 | Bidirectional Bus | Two-way data, only on Port A | Communication between two processors |
| BSR | Bit Set/Reset | Controls *individual bits* of Port C | Turning a relay ON/OFF |

### H3. Is the 8255 Good Enough for an ADAS (Advanced Driver Assistance System)?
Mostly no. It's fine for small auxiliary jobs — controlling an indicator light, reading
a basic sensor — since it offers 24 programmable I/O lines and simple handshaking. But a
real ADAS needs high-speed, real-time processing of camera/radar/LiDAR data plus complex
decision-making algorithms. The 8255 has **no processing power of its own**, low data
speed, and no support for modern automotive protocols or safety standards — so it can
only play a minor supporting role, never the core job.

### H4. Address Calculation Example
The 8255 occupies **4 consecutive I/O addresses** starting at its base address:

| Offset from base | Selects |
|---|---|
| +0 | Port A |
| +1 | Port B |
| +2 | Port C |
| +3 | Control Register |

**Given base = 80H:** Port A = 80H, Port B = 81H, Port C = 82H, Control Register = 83H.

### H5. Real System Example: Sorting Defective Products on a Conveyor Belt
- **Port A (input):** connected to sensors (infrared, optical, weight)
- **Port B (output):** controls actuators (motor, diverter gate)
- **Port C:** handshaking / status signals

**Flow:** product moves along the belt → sensor checks it → 8255 relays the reading to
the CPU via Port A → CPU decides accept (keep moving) or reject (activate the diverter
via Port B) → defective items get physically diverted into a rejection bin.

---

## PART I — 8254 TIMER, DMA, AND 8051

### I1. Fixed Time Delay for Elevator Movement (8254 Timer)
```
MVI A, 36H       ; control word to configure the 8254
OUT 43H

MVI A, 00H       ; low byte of the count
OUT 40H
MVI A, 50H       ; high byte of the count (sets the delay length)
OUT 40H

WAIT: IN 40H     ; poll the counter
      ANI 01H
      JNZ WAIT   ; keep waiting until the countdown finishes

MVI A, 01H
OUT 20H          ; activate the elevator motor after the delay
HLT
```
**Idea:** load a count into the timer, let it count down, and only move the elevator
once the countdown hits zero — a clean way to generate a fixed, reliable time delay.

### I2. CPU-Controlled I/O vs DMA
- **CPU-controlled I/O:** the CPU personally shuttles every single byte between the I/O
  device and memory. High CPU usage, slow for big/fast transfers.
- **DMA (Direct Memory Access):** data moves directly between the I/O device and memory
  **without** tying up the CPU the whole time.

**Why DMA wins for high-speed work:** lower CPU workload, faster block transfers, CPU
and I/O can run in parallel.

**When plain CPU I/O is still fine:** small/simple transfers, cheap systems with no DMA
hardware, or when you specifically need the CPU directly in control.

### I3. DMA — The Full Picture
- **What it is:** a technique letting peripherals talk directly to RAM, bypassing the CPU.
- **Main components of a DMA controller:** address register, data count register,
  control register, status register.
- **Role in modern OSes:** enables multitasking by letting I/O run in parallel with CPU
  work — fewer context switches, lower latency, higher throughput (protected via IOMMU
  and drivers).
- **Advantages:** faster transfers, reduced CPU load, efficient resource use.
- **Disadvantages:** more complex hardware, bus contention (fighting for bus access),
  cache coherence issues (memory changes underneath the CPU's cache).

### I4. 8051 Program — Buzzer Turns On When a Switch Is Pressed
```
ORG 0000H
START:
   SETB P3.2        ; P3.2 set up as input (the switch)
   CLR  P1.0        ; buzzer starts OFF
CHECK:
   JB P3.2, CHECK    ; keep looping while the switch is NOT pressed
   SETB P1.0         ; switch pressed → turn buzzer ON
HERE:
   SJMP HERE         ; stay here, buzzer remains ON
END
```
**Logic:** the program sits in a loop constantly checking the switch. The instant it's
pressed, the buzzer turns on and stays on.

---

## PART J — MCU, MEMORY BANKING, INTERRUPTS (DETAILED), RISC vs CISC, ARM, EMBEDDED SYSTEMS

### J1. Microcontroller Unit (MCU)
A compact chip built to control one specific embedded application — essentially a
tiny computer on a single chip. Main pieces:
- **CPU** — runs the instructions
- **Memory** — ROM/Flash (holds the program), RAM (holds temporary data)
- **I/O ports** — connect to sensors, switches, motors, LEDs
- **Timers/counters** — for delays, counting, generating signals
- **Serial interfaces** — UART, SPI, I²C, to talk to other devices
- **Interrupt controller** — lets the MCU react quickly to important events
- **ADC** — turns analog sensor signals into digital numbers

### J2. Memory Banking
A way of splitting available memory into smaller sections ("banks") so a processor with
limited address lines can still reach more total memory, or organize it more cleanly. A
**bank-selection mechanism** decides which bank is currently "active."

**Advantages:** lets a processor access more memory overall, simplifies organization,
improves memory usage, and lets different sections be swapped in as needed.

### J3. Interrupts, in Full Detail
An interrupt pauses normal program flow and hands control to a special routine (the
**Interrupt Service Routine**, or ISR).

**I. Hardware interrupts** (triggered by external devices):
- **INTR (maskable)** — can be turned on/off via IF, checked after every instruction,
  needs external hardware to supply the interrupt type. *Example: keyboard input.*
- **NMI (non-maskable)** — can never be disabled, highest priority, reserved for
  critical events. *Example: power failure.*

**II. Software interrupts** — triggered deliberately by code using `INT n`.
*Example: `INT 21H` calls a DOS service.*

**III. Internal interrupts (exceptions)** — the CPU raises these automatically on
errors. *Examples: divide-by-zero → INT 0, single-step → INT 1, breakpoint → INT 3.*

Regardless of the type: the ISR's address is looked up in the Interrupt Vector Table
(IVT), and control jumps there.

### J4. RISC vs CISC

| Feature | RISC | CISC |
|---|---|---|
| Instruction set | Small, simple | Large, complex |
| Instruction length | Usually fixed | Usually variable |
| Execution speed | Fast — most instructions finish in 1–few cycles | Some instructions take several cycles |
| Hardware design | Simpler | More complex |
| Power use | Generally lower | Generally higher |
| Heat generated | Lower — good for mobile | Higher |
| Typical use | Smartphones, tablets, embedded systems | Traditional PCs, servers |

**Why RISC fits smartphones better:** simple instructions → high energy efficiency →
longer battery life; simpler design → less heat in a small, poorly-ventilated case;
easier to pipeline efficiently → smooth multitasking across multiple cores; overall it
favors **performance per watt**, which is exactly what a battery-powered device needs.

### J5. ARM — Advanced RISC Machine
A family of processors *built on* RISC principles: efficient instruction execution at
relatively low power.

**Key traits:** RISC architecture, low power draw, high performance-per-watt, a large
register set, a compact physical design, and scalability — the same core ideas run
everything from tiny microcontrollers up to powerful smartphone chips.

**Where you'll find ARM chips:** smartphones/tablets, embedded systems, IoT devices,
smart TVs, automotive systems, wearables, networking gear.

### J6. ARM7 Architecture / Data Flow
Main pieces:
- **Register file (R0–R15):** R15 = Program Counter, R14 = Link Register, R13 =
  commonly the Stack Pointer, plus a **CPSR** holding condition flags and status.
- **ALU:** arithmetic (add/subtract), logic (AND/OR/XOR), updates condition flags.
- **Barrel shifter:** shifts or rotates an operand *before* the ALU processes it (shift
  left, shift right, rotate).
- **Instruction decoder:** decodes the fetched instruction and generates control signals.
- **Program Counter:** holds the address of the next instruction (32-bit wide in ARM state).
- **Memory interface:** uses `LDR`/`STR` instructions to move data between registers
  and memory. (ARM7 has no single dedicated "address register" — general-purpose
  registers R0–R12 fill that role as needed.)

**Overall data flow:**
```
Instruction Fetch → Instruction Decode → Register Read → Barrel Shift → ALU Operation → Result Write-back
```

### J7. Basic ARM Assembly Examples

**Add two numbers:**
```
MOV R0, #10
MOV R1, #20
ADD R2, R0, R1     ; R2 = 30
```

**Subtract two numbers:**
```
MOV R0, #20
MOV R1, #10
SUB R2, R0, R1     ; R2 = 10
```

**Compare two numbers (store 1 if equal, 0 if not — no branch needed):**
```
MOV R0, #10
MOV R1, #10
CMP R0, R1          ; sets condition flags
MOVEQ R2, #1        ; runs only if equal
MOVNE R2, #0        ; runs only if not equal
```

**Find the larger of two numbers:**
```
MOV R0, #25
MOV R1, #15
CMP R0, R1
MOVGT R2, R0        ; R0 is greater → keep it
MOVLE R2, R1        ; otherwise take R1
```
Result: R2 = 25.

**Check if a number is even or odd:**
```
MOV R0, #10
AND R1, R0, #1      ; isolate the lowest bit
CMP R1, #0
MOVEQ R2, #0        ; lowest bit = 0 → even
MOVNE R2, #1        ; lowest bit = 1 → odd
```

**Conditional-move cheat sheet:**
`MOVEQ` = equal · `MOVNE` = not equal · `MOVGT` = greater than · `MOVLT` = less than ·
`MOVGE` = greater or equal · `MOVLE` = less or equal.

---

## PART K — EMBEDDED SYSTEMS

### K1. What Is an Embedded System?
A computer-based system built to do **one specific job** (or a small set of jobs) inside
a bigger device — unlike a general-purpose computer, which does anything you ask. It
usually faces tight limits on cost, size, power, and processing time.

**Main components:**
- **Processor/Microcontroller** — the brain, runs the program
- **Memory** — ROM, RAM, Flash — holds program and data
- **Input devices** — sensors, switches, keyboards
- **Output devices** — LEDs, displays, motors, relays, speakers
- **Communication interfaces** — UART, SPI, I²C, CAN, USB
- **Power supply** — provides the electricity
- **Software/Firmware** — the program controlling everything

**Examples:** washing machine controller, microwave oven, digital camera, ATM,
traffic-light controller, car ECU, smart watch, printer, medical monitors, Wi-Fi router.

**Key characteristics:** application-specific, often needs real-time operation, low
power, compact, highly reliable, low cost, limited memory/processing resources.

**Simple example:** a washing machine's controller reads sensors and button presses,
then drives the motor, water valve, heater, and display according to the wash program —
one dedicated job, done reliably.

### K2. Embedded Systems in Modern Life
Because they're small, task-specific, and reliable, embedded systems quietly run huge
parts of daily life:

1. **Washing machines** — control cycles, water level, temperature, spin speed, timing.
2. **Automobiles** — engine control, airbags, ABS, parking sensors, automatic transmission.
3. **Microwave ovens** — control cooking time, temperature, power level, keypad/display.
4. **Smartphones** — manage the camera, touchscreen, sensors, communication, power.
5. **Medical equipment** — heart-rate/blood-pressure monitors, infusion pumps, ECG machines.
6. **Traffic control systems** — traffic lights, pedestrian signals, monitoring for road safety.
7. **Home appliances** — fridges, ACs, TVs, smart speakers with automatic, intelligent control.

**Big picture:** embedded systems make devices smarter, faster, safer, and easier to
use — and they're now present in almost every corner of modern life: transportation,
healthcare, communication, home appliances, and industry.

---

## How to Use This Before the Exam
1. First pass: read Part A (architecture, registers, memory formula) and Part C (stack) —
   these are the "always tested" fundamentals.
2. Second pass: Part E (modes, addressing, restrictions, jump range, interrupts) and
   Part G (assembly programs) — these show up as direct write-a-program questions.
3. Third pass: Parts H–K (8255, DMA, 8051, MCU, RISC/CISC, ARM, embedded systems) — these
   tend to appear as "explain/compare/analyze" essay-style questions.
4. Keep the **crash course** file from earlier for last-minute review — this one is for
   understanding *why*, that one is for quick recall right before the exam.
