# 🗺️ K-Map Solver (2, 3, 4 Variables) — C Program

> A console-based Karnaugh Map solver written in **C** that takes minterm values as input and outputs a minimized Boolean expression using **Gray Code grouping** — from Double Octets all the way down to single terms.

---

## 📖 Table of Contents

- [What is a K-Map?](#what-is-a-k-map)
- [What This Program Does](#what-this-program-does)
- [Key Concepts You Must Know First](#key-concepts-you-must-know-first)
- [Requirements](#requirements)
- [How to Compile and Run](#how-to-compile-and-run)
- [How to Use the Program — Step by Step](#how-to-use-the-program--step-by-step)
- [Input Format Explained](#input-format-explained)
- [Worked Examples](#worked-examples)
- [Understanding the Output](#understanding-the-output)
- [How the Program Works Internally](#how-the-program-works-internally)
- [Priority Order of Groupings](#priority-order-of-groupings)
- [Reading the Boolean Expression](#reading-the-boolean-expression)
- [Gray Code and K-Map Cell Order](#gray-code-and-k-map-cell-order)
- [Troubleshooting](#troubleshooting)

---

## What is a K-Map?

A **Karnaugh Map (K-Map)** is a grid-based tool used to simplify Boolean algebra expressions without tedious algebraic steps. You fill the grid with `1`s and `0`s from a truth table, group the `1`s together in powers of 2, and read off a simplified expression from those groups.

K-Maps are the foundation of **digital circuit design** — every gate, chip, and logic board starts with Boolean simplification like this.

```
Example 3-Variable K-Map:

      BC
  A  | 00 | 01 | 11 | 10 |
  ---+----+----+----+----|
   0 |  1 |  0 |  1 |  1 |
   1 |  1 |  0 |  1 |  1 |

Grouped → Simplified: C' + AC (example)
```

---

## What This Program Does

1. Asks you how many variables you have: **2, 3, or 4**
2. Asks you to enter `1` or `0` for each minterm (each combination of inputs)
3. Automatically checks for the largest possible groups first
4. Prints the **minimized Sum of Products (SOP)** Boolean expression

No drawing needed. No manual grouping. The program handles it all.

---

## Key Concepts You Must Know First

### Minterm
A **minterm** is one row of a truth table where the output is `1`.  
For 2 variables A and B, there are 4 possible minterms: `00, 01, 10, 11`.

### Sum of Products (SOP)
The output format of this program. It's an expression where:
- Each **product term** is a group of ANDed variables (e.g., `AB'C`)
- All terms are **ORed** together with `+` (e.g., `A'B + AC' + BD`)

### Complement (apostrophe `'`)
A variable written as `A'` means **A is 0** (NOT A).  
A variable written as `A` means **A is 1** (plain A).

### Grouping (the core of K-Map)
Groups of `1`s are combined to eliminate variables.  
The **larger the group, the fewer variables** in the resulting term.

| Group Size | Name         | Variables Eliminated |
|---|---|---|
| 16 cells   | Double Octet | All → Output = `1`   |
| 8 cells    | Octet        | 3 variables gone     |
| 4 cells    | Quad         | 2 variables gone     |
| 2 cells    | Pair         | 1 variable gone      |
| 1 cell     | Single       | 0 variables gone     |

---

## Requirements

| Tool | Details |
|---|---|
| C Compiler | GCC (recommended) or any standard C compiler |
| OS | Windows, Linux, or macOS |
| Terminal | Command Prompt, PowerShell, Bash, or any shell |

### Install GCC (if not already installed)

**Windows:** Download [MinGW](https://www.mingw-w64.org/) and install GCC  
**Linux:** `sudo apt install gcc`  
**macOS:** `xcode-select --install`

Verify it works:
```bash
gcc --version
```

---

## How to Compile and Run

### Step 1 — Save the file
Make sure your file is saved as `K-Map-Solver.cpp` (or rename it to `kmap.c`).

### Step 2 — Open a terminal in the same folder

### Step 3 — Compile
```bash
gcc K-Map-Solver.cpp -o kmap
```
This creates an executable called `kmap` (or `kmap.exe` on Windows).

### Step 4 — Run
```bash
./kmap
```
On Windows:
```bash
kmap.exe
```

---

## How to Use the Program — Step by Step

### Step 1 — Choose number of variables

```
Enter The No Of Variables In Kmap : (2 (OR) 3 (OR) 4): 3
```

Type `2`, `3`, or `4` and press Enter.

---

### Step 2 — Enter minterm values

The program will list every possible combination of inputs **in Gray Code order** and ask you to type `1` (output is TRUE) or `0` (output is FALSE) for each.

```
Enter The Minterm Values (0 or 1):
Minterm 000 (A'B'C'): 1
Minterm 001 (A'B'C):  0
Minterm 011 (A'BC):   1
Minterm 010 (A'BC'):  1
Minterm 100 (AB'C'): 1
Minterm 101 (AB'C):  0
Minterm 111 (ABC):   1
Minterm 110 (ABC'):  1
```

Type `1` or `0` for each line and press Enter.

---

### Step 3 — Read the result

```
Final Expression:
F(A,B,C) = A'C' + B + A'B
```

That's your minimized Boolean expression — ready to use in circuit design.

---

## Input Format Explained

### Why Gray Code Order?

The program asks minterms in **Gray Code order** (not standard binary order).  
Adjacent entries differ by only **one bit**, which is what makes K-Map grouping valid.

#### 2 Variables — 4 minterms

| Prompt Label | A | B | Minterm # |
|---|---|---|---|
| `00` | 0 | 0 | 0 |
| `01` | 0 | 1 | 1 |
| `10` | 1 | 0 | 2 |
| `11` | 1 | 1 | 3 |

#### 3 Variables — 8 minterms (Gray Code order)

| Prompt Label | A | B | C | Minterm # |
|---|---|---|---|---|
| `000` (A'B'C') | 0 | 0 | 0 | 0 |
| `001` (A'B'C)  | 0 | 0 | 1 | 1 |
| `011` (A'BC)   | 0 | 1 | 1 | 2 |
| `010` (A'BC')  | 0 | 1 | 0 | 3 |
| `100` (AB'C')  | 1 | 0 | 0 | 4 |
| `101` (AB'C)   | 1 | 0 | 1 | 5 |
| `111` (ABC)    | 1 | 1 | 1 | 6 |
| `110` (ABC')   | 1 | 1 | 0 | 7 |

#### 4 Variables — 16 minterms (Gray Code order)

| Prompt Label | A | B | C | D |
|---|---|---|---|---|
| `0000` (A'B'C'D') | 0 | 0 | 0 | 0 |
| `0001` (A'B'C'D)  | 0 | 0 | 0 | 1 |
| `0011` (A'B'CD)   | 0 | 0 | 1 | 1 |
| `0010` (A'B'CD')  | 0 | 0 | 1 | 0 |
| `0100` (A'BC'D')  | 0 | 1 | 0 | 0 |
| `0101` (A'BC'D)   | 0 | 1 | 0 | 1 |
| `0111` (A'BCD)    | 0 | 1 | 1 | 1 |
| `0110` (A'BCD')   | 0 | 1 | 1 | 0 |
| `1100` (ABC'D')   | 1 | 1 | 0 | 0 |
| `1101` (ABC'D)    | 1 | 1 | 0 | 1 |
| `1111` (ABCD)     | 1 | 1 | 1 | 1 |
| `1110` (ABCD')    | 1 | 1 | 1 | 0 |
| `1000` (AB'C'D')  | 1 | 0 | 0 | 0 |
| `1001` (AB'C'D)   | 1 | 0 | 0 | 1 |
| `1011` (AB'CD)    | 1 | 0 | 1 | 1 |
| `1010` (AB'CD')   | 1 | 0 | 1 | 0 |

> ⚠️ **Important:** The 4-variable order is NOT standard binary order. Follow the prompts exactly as shown — the labels on screen tell you the exact combination.

---

## Worked Examples

### Example 1 — 2 Variables

**Problem:** F(A,B) where output is 1 for A=0,B=0 and A=0,B=1

```
Enter The No Of Variables In Kmap : (2 (OR) 3 (OR) 4): 2

Enter The Minterm Values (0 or 1):
Minterm 00 (A=0,B=0): 1
Minterm 01 (A=0,B=1): 1
Minterm 10 (A=1,B=0): 0
Minterm 11 (A=1,B=1): 0

Final expression:
F(A,B) = A'
```

Both 1s share A=0. Grouped as a pair → A is always 0 → result is `A'`.

---

### Example 2 — 3 Variables

**Problem:** F(A,B,C) where minterms are 0,2,3,4,6,7 (positions in Gray Code order)

```
Enter The No Of Variables In Kmap : (2 (OR) 3 (OR) 4): 3

Minterm 000 (A'B'C'): 1
Minterm 001 (A'B'C):  0
Minterm 011 (A'BC):   1
Minterm 010 (A'BC'):  1
Minterm 100 (AB'C'): 1
Minterm 101 (AB'C):  0
Minterm 111 (ABC):   1
Minterm 110 (ABC'):  1

Final Expression:
F(A,B,C) = B' + A'C' + AC'
```

---

### Example 3 — 4 Variables (all 1s)

```
(Enter 1 for all 16 minterms)

Final Expression is :
F(A,B,C,D) = 1
```

When all cells are `1`, the expression is simply `1` — no variables needed.

---

### Example 4 — 4 Variables (no 1s)

```
(Enter 0 for all 16 minterms)

Final Expression is :
F(A,B,C,D) = 0
```

When no cells are `1`, the output is always `0`.

---

## Understanding the Output

### Normal Output
```
F(A,B,C,D) = A'C' + BD + AB'C
```
This means: **(NOT A AND NOT C)  OR  (B AND D)  OR  (A AND NOT B AND C)**

### Special Outputs

| Output | Meaning |
|---|---|
| `F = 1` | All minterms are 1 — output is always TRUE (Double Octet formed) |
| `F = 0` | No minterms are 1 — output is always FALSE |
| `F = A'` | Only one variable determines the output |

---

## How the Program Works Internally

The program uses a **coverage array** to track which minterms have already been included in a group. It checks groupings from largest to smallest:

```
1. Count total 1s
   ├── If all cells = 1 → print "1" and exit
   └── If no cells = 1  → print "0" and exit

2. Check for Octets (8-cell groups)
   └── If found → print single-variable term, mark those cells as covered

3. Check for Quads (4-cell groups)
   └── Only if at least one cell in the group is not yet covered
   └── If found → print 2-variable term, mark cells as covered

4. Check for Pairs (2-cell groups)
   └── Only if at least one cell in the group is not yet covered
   └── If found → print 3-variable term, mark cells as covered

5. Remaining uncovered 1s → print as full single-cell terms
   (4 variables for full expression)
```

The `coverage[]` array prevents any minterm from being printed twice.  
The `x` flag ensures ` + ` is only printed between terms, not before the first one.

---

## Priority Order of Groupings

The program always tries the **largest group first**:

```
Double Octet (16) → Octet (8) → Quad (4) → Pair (2) → Single (1)
```

This guarantees the most simplified output — larger groups = fewer variables in the term.

---

## Reading the Boolean Expression

| Symbol | Meaning | Example |
|---|---|---|
| `A` | Variable A = 1 | Input A is HIGH |
| `A'` | Variable A = 0 (NOT A) | Input A is LOW |
| Two letters together | AND | `AB` = A AND B |
| `+` | OR | `A + B` = A OR B |

So `A'BC + AD'` means:  
**(NOT A) AND B AND C** &nbsp; OR &nbsp; **A AND (NOT D)**

---

## Gray Code and K-Map Cell Order

Standard binary counts: `00 → 01 → 10 → 11`  
Gray Code counts: `00 → 01 → 11 → 10`

The key difference: **every adjacent pair in Gray Code differs by exactly 1 bit**.  
This property is what allows K-Map cells that are next to each other (or wrap around edges) to be grouped — they always represent a logic pattern that can be expressed cleanly.

This program uses Gray Code ordering for all its K-Map cell positions, which is why the minterm input prompts are not in standard `0,1,2,3...` order.

---

## Troubleshooting

**Q: I entered a valid number but got "Please Enter A Valid Choice"**  
→ Only `2`, `3`, or `4` are accepted. Do not enter spaces or letters.

**Q: My output looks longer than expected**  
→ This is correct behavior — if no large groups exist, the program will list all individual minterms in their full form (e.g., `A'B'C'D'`).

**Q: The program compiled but crashes immediately**  
→ Make sure you're entering only `0` or `1` for each minterm. Any other character will cause unexpected behavior in C's `scanf`.

**Q: I don't see any output**  
→ You may have entered all `0`s — the program correctly outputs `F = 0` and exits.

**Q: On Windows, the `.exe` does not appear after compiling**  
→ Check for compiler errors. Make sure GCC is in your system PATH.

---

## License

This project is released under the **MIT License** — free to use, study, and modify.

---

*Built in C — close to the hardware, just like the circuits it helps design.*
