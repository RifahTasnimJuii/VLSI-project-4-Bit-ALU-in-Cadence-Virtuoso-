# 4-Bit ALU in Cadence Virtuoso (gpdk045)

Transistor-level design, symbol creation and transient verification of a 4-bit Arithmetic Logic Unit (ALU) in Cadence Virtuoso, built on the gpdk045 (45 nm) generic PDK at VDD = 1.8 V.

**Course:** CSE-450 Design and Testing of VLSI Lab, University of Asia Pacific
**Group:** 8

## Operations

| OP1 | OP0 | Operation | Result | FLAG |
|---|---|---|---|---|
| 0 | 0 | ADD | A + B | Carry out |
| 0 | 1 | SUB | A − B | Borrow |
| 1 | 0 | AND | A · B | 0 |
| 1 | 1 | OR | A + B | 0 |

## Hierarchy

```
ALU4
├── ADDSUB4   4 × XOR + 4 × full adder (ripple carry)
├── AND4      4 × AND2
├── OR4       4 × OR2
├── OUTSEL4   8 × 2:1 MUX (two stages per bit)
└── FLAG      XOR + NOT + AND2
```

Basic cells: inverter, NAND2, NOR2, XOR2, AND2, OR2, 2:1 MUX, 1-bit full adder.

## Results

- Functional verification: 20 / 20 test cases passed (400 ns transient, see proposal Section 8.3)
- Worst-case propagation delay (B0 → FLAG, full carry ripple): 238.33 ps
- Average power (400 ns, 20 test cases): 14.48 µW
- Transistor count: ≈ 422

## Repository structure

| Folder | Contents |
|---|---|
| `2x1MUX/` | 2:1 MUX schematic and symbol |
| `2x1MUX_Circuit/` | 2:1 MUX testbench (ADE Explorer) |
| `inverter_test/`, `nand_test/`, ... | Individual cell schematics and testbenches |
| `ALU4/` | Top-level ALU schematic and symbol |
| `ALU4_Circuit/` | Top-level 20-case testbench |
| `ALU4_delay_tb/` | Propagation delay testbench |

Folder names may differ slightly; see the Cadence library for the full list.

## How to reproduce

1. Open Virtuoso with the gpdk045 PDK loaded in `cds.lib`.
2. Open a testbench (for example `ALU4_Circuit`) and launch ADE Explorer.
3. Input stimulus is provided by `vpwlf` sources reading the PWL files from `alu4_pwl/` (one file per input bit). Generate them with `make_alu4_pwl.sh`.
4. Run transient analysis (`tran`, stop time 400 ns for the 20-case test, 20 ns for the delay test).

## Note on the PDK

The gpdk045 PDK and its model files are proprietary Cadence material. They are **not** included in this repository (see `.gitignore`). To run the design, use the PDK available through your own university Cadence license.
