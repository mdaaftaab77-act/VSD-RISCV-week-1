# Day 5 - Optimization in Synthesis
## Subdivision 1: If-Case Constructs

This document explains the first subdivision of Day 5 - Optimization in Synthesis.  
In this section, we studied the use of **If** and **Case** constructs in Verilog, their correct usage, common pitfalls, and best practices.

---

## What I Learned
- If statements implement priority logic.  
- Incomplete if statements may lead to inferred latches, which is considered a poor coding style.  
- Both if and case statements must be written inside an always block and should operate on `reg` variables.  
- Incomplete case statements cause inferred latches → include a **default** branch.  
- Partial assignments in case statements also lead to latches → assign all outputs in every branch.  
- **If** is priority-based, while **case** allows parallel evaluation. Overlapping case conditions can cause ambiguity.  

---

## The If Statement

### Definition
The if statement is used to implement **priority logic**.  
- The first true condition is executed, and the rest are ignored.  
- If the statement is incomplete (missing an else), synthesis may infer latches.  

### Syntax
```verilog
if (condition1)
    statement1;
else if (condition2)
    statement2;
else
    statement3;
```

### Incomplete If Statement
If an `else` is missing, the synthesizer may retain the previous value, which leads to **inferred latches**.  

**Example (Incomplete If):**
```verilog
if (sel == 2'b00)
    y = a;
else if (sel == 2'b01)
    y = b;
// No else condition → causes latch inference
```

⚠️ This is a **poor coding practice** and should be avoided.  

---

## The Case Statement

### Definition
A `case` statement selects one or more branches based on the value of an expression.  
- Must be written inside an `always` block.  
- Works only with **reg** variables.  

### Syntax
```verilog
case (sel)
    2'b00: y = a;
    2'b01: y = b;
    2'b10: y = c;
    default: y = d;   // Recommended to avoid latches
endcase
```

---

## Common Issues with Case Statements

### Incomplete Case
If not all conditions are handled and no default is given, **inferred latches** will occur.  
✅ **Solution:** Always include a **default** branch.  

### Partial Assignment in Case
If not all outputs are assigned in every branch of the case, **latches will be inferred**.  

✅ **Solution:** Assign all outputs in every case branch.  

**Example:**
```verilog
case (sel)
    2'b00: y = a;
    2'b01: y = b;
    default: y = c;  // Covers all conditions (10 & 11 too)
endcase
```

---

## If vs Case

| Aspect       | If Statement             | Case Statement                           |
|--------------|--------------------------|------------------------------------------|
| Nature       | Priority-based logic     | Parallel evaluation (multiple matches)   |
| Execution    | First true branch only   | All matching branches can be executed    |
| Risk         | Missing else → latch     | Missing default/overlap → latch/ambiguity |
| Best Practice| Always provide an else   | Always provide default + full assignments |

---

## Key Takeaways
- Always complete the if-else structure to avoid unintended latches.  
- Always include a **default** branch in case statements.  
- Assign all outputs in every branch of a case statement.  
- Be careful with overlapping cases.  
- Use **if** for priority logic and **case** for decoding signals.  
