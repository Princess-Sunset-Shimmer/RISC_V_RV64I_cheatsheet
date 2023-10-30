# RISC-V RV64I cheatsheet
I write C style pseudo codes to explain what does every single RV64I instruction do(not include privileged instructions).
rm register for medium operations.

# ALU:
  #### 1. arithmatic
> **add**  rd, rs1, rs2
```c
  rd = rs1 + rs2;
  pc = pc + 4;
```
> **addw**  rd, rs1, rs2
```c
  rd = rs1 + rs2;
  rd = extend_signed_32bit(rd);
  pc = pc + 4;
```
> **addi**  rd, rs1, IMMEDIATE
```c
  rd = extend_signed_12bit(IMMEDIATE);
  rd = rs1 + rd;
  pc = pc + 4;
```
> **addiw**  rd, rs1, IMMEDIATE
```c
  rd = extend_signed_12bit(IMMEDIATE);
  rd = rs1 + rd;
  rd = extend_signed_32bit(rd);
  pc = pc + 4;
```
- - - -
> **auipc**  rd, IMMEDIATE_UPPER
```c
  rd = IMMEDIATE_UPPER;
  rd = rd << 20;
  rd = extend_signed_32bit(rd);
  rd = pc + rd;
  pc = pc + 4;
```
- - - -
> **sub**  rd, rs1, rs2
```c
  rd = rs1 - rs2;
  pc = pc + 4;
```
> **subw**  rd, rs1, rs2
```c
  rd = rs1 - rs2;
  rd = extend_signed_32bit(rd);
  pc = pc + 4;
```

  #### 2. logical shift
> **srl**  rd, rs1, rs2
```c
  rd = rs2 & 63;
  rd = (uint64_t)rs1 >> rd;
  pc = oc + 4;
```
> **srlw**  rd, rs1, rs2
```c
  rd = rs2 & 31;
  rd = (uint64_t)rs1 >> rd;
  rd = extend_signed_32bit(rd);
  pc = pc + 4;
```
> **srli**  rd, rs1, IMMEDIATE_SHIFTAMOUNT63
```c
  rd = extend_unsigned_6bit(IMMEDIATE_SHIFTAMOUNT63);
  rd = (uint64_t)rs1 >> rd;
  pc = pc + 4;
```
> **srliw**  rd, rs1, IMMEDIATE_SHIFTAMOUNT31
```c
  rd = extend_unsigned_5bit(IMMEDIATE_SHIFTAMOUNT31);
  rd = (uint64_t)rs1 >> rd;
  rd = extend_signed_32bit(rd);
  pc = pc + 4;
```
- - - -
> **sll**  rd, rs1, rs2
```c
  rd = rs2 & 63;
  rd = rs1 << rd;
  pc = oc + 4;
```
> **sllw**  rd, rs1, rs2
```c
  rd = rs2 & 31;
  rd = rs1 << rd;
  rd = extend_signed_32bit(rd);
  pc = pc + 4;
```
> **slli**  rd, rs1, IMMEDIATE_SHIFTAMOUNT63
```c
  rd = extend_unsigned_6bit(IMMEDIATE_SHIFTAMOUNT63);
  rd = rs1 << rd;
  pc = pc + 4;
```
> **slliw**  rd, rs1, IMMEDIATE_SHIFTAMOUNT31
```c
  rd = extend_unsigned_5bit(IMMEDIATE_SHIFTAMOUNT31);
  rd = rs1 << rd;
  rd = extend_signed_32bit(rd);
  pc = pc + 4;
```

  #### 3. arithmatic shift
> **sra**  rd, rs1, rs2
```c
  rd = rs2 & 63;
  rd = rs1 >> rd;
  pc = pc + 4;
```
> **sraw**  rd, rs1, rs2
```c
  rd = rs2 & 31;
  rd = rs1 >> rd;
  rd = extend_signed_32bit(rd);
  pc = pc + 4;
```
> **srai**  rd, rs1, IMMEDIATE_SHIFTAMOUNT63
```c
  rd = extend_unsigned_6bit(IMMEDIATE_SHIFTAMOUNT63);
  rd = rs1 >> rd;
  pc = pc + 4;
```
> **sraiw**  rd, rs1, IMMEDIATE_SHIFTAMOUNT31
```c
  rd = extend_unsigned_5bit(IMMEDIATE_SHIFTAMOUNT31);
  rd = rs1 >> rd;
  rd = extend_signed_32bit(rd);
  pc = pc + 4;
```

  #### 4. boolean bitwise
> **and**  rd, rs1, rs2
```c
  rd = rs1 & rs2;
  pc = pc + 4;
```
> **andi**  rd, rs1, IMMEDIATE
```c
  rd = extend_signed_12bit(IMMEDIATE);
  rd = rs1 & rd;
  pc = pc + 4;
```
- - - -
> **or**  rd, rs1, rs2
```c
  rd = rs1 | rs2;
  pc = pc + 4;
```
> **ori**  rd, rs1, IMMEDIATE
```c
  rd = extend_signed_12bit(IMMEDIATE);
  rd = rs1 | rd;
  pc = pc + 4;
```
- - - -
> **xor**  rd, rs1, rs2
```c
  rd = rs1 ^ rs2;
  pc = pc + 4;
```
> **xori**  rd, rs1, IMMEDIATE
```c
  rd = extend_signed_12bit(IMMEDIATE);
  rd = rs1 ^ rd;
  pc = pc + 4;
```

  #### 5. comparison
> **slt**  rd, rs1, rs2
```c
  rd = rs1 < rs2;
  pc = pc + 4;
```
> **sltu**  rd, rs1, rs2
```c
  rd = (uint64_t)rs1 < (uint64_t)rs2;
  pc = pc + 4;
```
> **slti**  rd, rs1, IMMEDIATE
```c
  rd = extend_signed_12bit(IMMEDIATE);
  rd = rs1 < rd;
  pc = pc + 4;
```
> **sltiu**  rd, rs1, IMMEDIATE
```c
  rd = extend_signed_12bit(IMMEDIATE);
  rd = (uint64_t)rs1 < (uint64_t)rd;
  pc = pc + 4;
```

# transfer control:
  #### 1. conditional
> **beq**  rs1, rs2, IMMEDIATE_BRANCH
```c
  rm1 = rs1 == rs2;
  rm1 = -rm1;
  rm2 = IMMEDIATE_BRANCH_OFFSET;
  rm2 = rm2 << 1;
  rm2 = extend_signed_13bit(rm2);
  rm2 = rm2 - 4;
  rm2 = rm2 & rm1;
  rm2 = rm2 + 4;
  pc = pc + rm2;
```
> **bne**  rs1, rs2, IMMEDIATE_BRANCH
```c
  rm1 = rs1 != rs2;
  rm1 = -rm1;
  rm2 = IMMEDIATE_BRANCH_OFFSET;
  rm2 = rm2 << 1;
  rm2 = extend_signed_13bit(rm2);
  rm2 = rm2 - 4;
  rm2 = rm2 & rm1;
  rm2 = rm2 + 4;
  pc = pc + rm2;
```
> **blt**  rs1, rs2, IMMEDIATE_BRANCH
```c
  rm1 = rs1 < rs2;
  rm1 = -rm1;
  rm2 = IMMEDIATE_BRANCH_OFFSET;
  rm2 = rm2 << 1;
  rm2 = extend_signed_13bit(rm2);
  rm2 = rm2 - 4;
  rm2 = rm2 & rm1;
  rm2 = rm2 + 4;
  pc = pc + rm2;
```
> **bltu**  rs1, rs2, IMMEDIATE_BRANCH
```c
  rm1 = (uint64_t)rs1 < (uint64_t)rs2;
  rm1 = -rm1;
  rm2 = IMMEDIATE_BRANCH_OFFSET;
  rm2 = rm2 << 1;
  rm2 = extend_signed_13bit(rm2);
  rm2 = rm2 - 4;
  rm2 = rm2 & rm1;
  rm2 = rm2 + 4;
  pc = pc + rm2;
```
> **bge**  rs1, rs2, IMMEDIATE_BRANCH
```c
  rm1 = rs1 >= rs2;
  rm1 = -rm1;
  rm2 = IMMEDIATE_BRANCH_OFFSET;
  rm2 = rm2 << 1;
  rm2 = extend_signed_13bit(rm2);
  rm2 = rm2 - 4;
  rm2 = rm2 & rm1;
  rm2 = rm2 + 4;
  pc = pc + rm2;
```
> **bgeu**  rs1, rs2, IMMEDIATE_BRANCH
```c
  rm1 = (uint64_t)rs1 >= (uint64_t)rs2;
  rm1 = -rm1;
  rm2 = IMMEDIATE_BRANCH_OFFSET;
  rm2 = rm2 << 1;
  rm2 = extend_signed_13bit(rm2);
  rm2 = rm2 - 4;
  rm2 = rm2 & rm1;
  rm2 = rm2 + 4;
  pc = pc + rm2;
```

  #### 2. unconditional
> **jal**  rd, IMMEDIATE_JUMP
```c
  rd = pc + 4;
  rm1 = IMMEDIATE_JUMP_OFFSET;
  rm1 = rm1 << 1;
  rm1 = extend_signed_21bit(rm1);
  pc = pc + rm1;
```
> **jalr**  rd, rs1, IMMEDIATE
```c
  rd = pc + 4;
  rm1 = extend_signed_12bit(IMMEDIATE);
  pc = rs1 + rm1;
  pc = pc & -2;
```

# data transmission:
  #### 1. load
> **lb**  rd, IMMEDIATE(rs1)
```c
  rd = extend_signed_12bit(IMMEDIATE);
  rs1 = rs1 + rd;
  rd = *(int8_t*)rs1;
```
> **lbu**  rd, IMMEDIATE(rs1)
```c
  rd = extend_signed_12bit(IMMEDIATE);
  rs1 = rs1 + rd;
  rd = *(uint8_t*)rs1;
```
> **lh**  rd, IMMEDIATE(rs1)
```c
  rd = extend_signed_12bit(IMMEDIATE);
  rs1 = rs1 + rd;
  rd = *(int16_t*)rs1;
```
> **lhu**  rd, IMMEDIATE(rs1)
```c
  rd = extend_signed_12bit(IMMEDIATE);
  rs1 = rs1 + rd;
  rd = *(uint16_t*)rs1;
```
> **lw**  rd, IMMEDIATE(rs1)
```c
  rd = extend_signed_12bit(IMMEDIATE);
  rs1 = rs1 + rd;
  rd = *(int32_t*)rs1;
```
> **lwu**  rd, IMMEDIATE(rs1)
```c
  rd = extend_signed_12bit(IMMEDIATE);
  rs1 = rs1 + rd;
  rd = *(uint32_t*)rs1;
```
> **ld**  rd, IMMEDIATE(rs1)
```c
  rd = extend_signed_12bit(IMMEDIATE);
  rs1 = rs1 + rd;
  rd = *(int64_t*)rs1;
```
- - - -
> **lui**  rd, IMMEDIATE_UPPER
```c
  rd = IMMEDIATE_UPPER;
  rd = rd << 20;
  rd = extend_signed_32bit(rd);
  pc = pc + 4;
```

  #### 2. store
> **sb**  rs2, IMMEDIATE_STORE(rs1)
```c
  rm1 = extend_signed_12bit(IMMEDIATE_STORE);
  rs1 = rs1 + rm1;
  *(int8_t*)rs1 = rs2;
```
> **sh**  rs2, IMMEDIATE_STORE(rs1)
```c
  rm1 = extend_signed_12bit(IMMEDIATE_STORE);
  rs1 = rs1 + rm1;
  *(int16_t*)rs1 = rs2;
```
> **sw**  rs2, IMMEDIATE_STORE(rs1)
```c
  rm1 = extend_signed_12bit(IMMEDIATE_STORE);
  rs1 = rs1 + rm1;
  *(int32_t*)rs1 = rs2;
```
> **sd**  rs2, IMMEDIATE_STORE(rs1)
```c
  rm1 = extend_signed_12bit(IMMEDIATE_STORE);
  rs1 = rs1 + rm1;
  *(int64_t*)rs1 = rs2;
```
