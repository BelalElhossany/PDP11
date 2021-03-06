# About 

This project is about an implementation for PDP-11 minicomputer instruction set architecture (ISA) and its CPU KA-11 in VHDL.
Microprogramming approach is used for the implementation of the control unit.

<p align="center">
  <a href="" rel="noopener">
 <img src="https://github.com/mhomran/PDP11/raw/master/demo/Block_Diagram.png" alt="block diagram Visualization"></a>
</p>

#### CPU specs 

- data and address bus of 16-bit width.
- 12 clock cycles per instruction (CPI).
- can handle one interrupt request.
- Register file of 8 general purpose registers including 2 special registers (SP, PC).

#### RAM specs

- 4 KB RAM.
- Word addressable.

#### Bus system

- single bus architecture

# List of the implemented instructions

## two operands instructions

| Instruction mnemonic  |          syntax            |            Description            |
| --------------------- | -------------------------- | --------------------------------- |
| MOV                   | `MOV Op1, Op2`             | Op1 <- Op2                        |
| ADD                   | `ADD Op1, Op2`             | Op2 <- Op1 + Op2                  |
| ADC                   | `ADC Op1, Op2`             | Op2 <- Op1 + Op2 + carry flag     |
| SUB                   | `SUB Op1, Op2`             | Op2 <- Op1 - Op2                  |
| SBC                   | `SBC Op1, Op2`             | Op2 <- Op1 - Op2 - carry flag     |
| AND                   | `AND Op1, Op2`             | Op2 <- Op1 & Op2 (bitwise and)    |
| OR                    | `OR Op1, Op2`              | Op2 <- Op1 \| Op2 (bitwise or)     |
| XOR                   | `XOR Op1, Op2`             | Op2 <- Op1 ^ Op2 (bitwise xor)    |
| CMP                   | `CMP Op1, Op2`             | Op1 - Op2 (just flags change)     |

## single operand instructions

| Instruction mnemonic  |          syntax            |            Description            |
| --------------------- | -------------------------- | --------------------------------- |
| INC                   | `INC Op`                   | Op <- Op + 1                      |
| DEC                   | `DEC Op`                   | Op <- Op - 1                      |
| CLR                   | `CLR Op`                   | Op <- 0                           |
| INV                   | `INV Op`                   | Op <- ~OP (bitwise inversion)     |
| LSR                   | `LSR Op`                   | Op <- Op >> 1                     |
| ROR                   | `ROR Op`                   | Op <- Op(0) concat. Op(15:1)      |
| ASR                   | `ASR Op`                   | Op <- Op(15) concat. Op (15:1)    |
| LSL                   | `LSL Op`                   | Op <- Op << 1                     |
| ROL                   | `ROL Op`                   | Op <- Op(14:0) concat. Op(15)     |
| JSR                   | `JSR Op` Or `JSR label`    | far jump to a subroutune          |


## Branch instructions

| Instruction mnemonic  |          syntax            |         Branch condition(s)       |
| --------------------- | -------------------------- | --------------------------------- |
| BR                    | `BR label`                 | None                              |
| BEQ                   | `BEQ label`                | Z = 1                             |
| BNE                   | `BNE label`                | Z = 0                             |
| BLO                   | `BLO label`                | C = 0                             |
| BLS                   | `BLS label`                | C = 0 or Z = 1                    |
| BHI                   | `BHI label`                | C = 1                             |
| BHS                   | `BHS label`                | C = 1 or Z = 1                    |


## No operand instructions

| Instruction mnemonic  |          syntax            |            Description            |
| --------------------- | -------------------------- | --------------------------------- |
| HLT                   | `HLT`                      | stops the cpu                     |
| NOP                   | `NOP`                      | do nothing                        |
| RTS                   | `RTS`                      | return from a subroutune            |
| IRET                  | `IRET`                     | return to main program before interruption            |


# Addressing modes

## main Addressing modes
|         Name          |          syntax            |            Operation            |
| --------------------- | -------------------------- | ------------------------------- |
| Register              | `Rn`                       | Operand = [Rn]                  |
| Auto-Increment        | `(Rn)+`                    | Operand = Mem[Rn] <br> Rn = Rn + 1   |
| Auto-Decrement        | `-(Rn)`                    |  Rn = Rn - 1 <br> Operand = Mem[Rn]  |
| index                 | `X(Rn)`                    | Operand = Mem [X + [Rn]] |
| Register indirect     | `@(Rn)`                    | Operand = Mem[Rn] |
| Auto-Increment indirect | `@(Rn)+`                 | Operand = Mem[Mem[Rn]] <br> Rn = Rn + 1|
| Auto-Decrement indirect | `@-(Rn)`                 | Rn = Rn - 1 <br> Operand = Mem[Mem[Rn]]|
| Index indirect        | `@X(Rn)`                   | Operand = Mem[Mem [X + [Rn]]]     |

## Derived addressing modes
|         Name          |          syntax            |            Operation            |
| --------------------- | -------------------------- | ------------------------------- |
| Immediate             | `#n`                       | Operand = Mem[PC] = n <br> Inc PC <br> (operand n follows instruction) |
| Relative              | `VAR`                      | Fetch X<br>Inc PC<br>Operand=Mem[X+[PC]=A] <br>(Addr A is given relative to instruction) |


# Programmer guide
- To write a comment use `;`
- The variables should be written in the end of the assembly file. It also should be in this format `DEFINE VAR 5`.
- The interrupt subroutine should be written before the variables and has this label `Interrupt:`.
- Labels must be written in a separate line.
- There's no case sensitivity.
- To assemble the project, run `python assembler.py <assembly_file> <output_file>`. You should have python3.

# To run the test cases
- Install ModelSim (there's a free edition for students)
- Create a project, then add the vhdl files in it
- In the tcl console, write `do do_file` where `do_file` is the file that contains the simulation instructions for ModelSim. 
You can find them in the codes_do_files folder for each testcase.
- To avoid changing the relative paths in the do files, make sure that your model sim project is in the directory prior to the repository.


