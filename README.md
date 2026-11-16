# traffic-light-modN-counter
# Traffic-Light Controller and Mod-6 Counter

Design document, Applied Digital Logic Design (25ECE101)
Name: Dhruvisha  |  USN: 2604637

## 1. Aim
To design, simulate and verify (a) a 3-state traffic-light controller
and (b) a synchronous mod-6 counter, using D flip-flops.

## 2. Tools Used
Logisim Evolution / Falstad (circuit simulation), D flip-flops, basic gates.

---

## PART A: TRAFFIC-LIGHT CONTROLLER

### 3. State Assignment
| State  | Q1 Q0 |
|--------|-------|
| Green  | 0 0   |
| Yellow | 0 1   |
| Red    | 1 0   |
| Unused | 1 1   |

### 4. State Diagram
Green (00) -> Yellow (01) -> Red (10) -> Green (00), repeating on each clock.
Unused state 11 goes to Red (10) on the next clock, so the circuit
recovers on its own.


### 5. State Table
| Present Q1 Q0 | Next Q1 Q0 | D1 | D0 |
|---------------|------------|----|----|
| 0 0           | 0 1        | 0  | 1  |
| 0 1           | 1 0        | 1  | 0  |
| 1 0           | 0 0        | 0  | 0  |
| 1 1 (unused)  | 1 0        | 1  | 0  |

### 6. Simplified Equations (D flip-flop: D = next state)
D1 = Q0
D0 = Q1' . Q0'

### 7. Outputs
Green  = Q1' . Q0'
Yellow = Q1' . Q0
Red    = Q1

### 8. Circuit
Two D flip-flops on a common clock. D1 is connected directly to Q0.
D0 comes from a NOR gate with Q1 and Q0 as inputs. The three output
lights are driven by the output equations above.


### 9. Simulation Result
The outputs follow Green -> Yellow -> Red -> Green on each clock pulse.


---

## PART B: SYNCHRONOUS MOD-6 COUNTER

### 10. Design
The counter counts 0 to 5 and then returns to 0.
It uses 3 D flip-flops (Q2 Q1 Q0) on one common clock.
States 110 and 111 are not used and are treated as don't-cares.

### 11. State Table
| Present Q2 Q1 Q0 | Next Q2 Q1 Q0 | D2 | D1 | D0 |
|------------------|---------------|----|----|----|
| 0 0 0            | 0 0 1         | 0  | 0  | 1  |
| 0 0 1            | 0 1 0         | 0  | 1  | 0  |
| 0 1 0            | 0 1 1         | 0  | 1  | 1  |
| 0 1 1            | 1 0 0         | 1  | 0  | 0  |
| 1 0 0            | 1 0 1         | 1  | 0  | 1  |
| 1 0 1            | 0 0 0         | 0  | 0  | 0  |
| 1 1 0            | X X X         | X  | X  | X  |
| 1 1 1            | X X X         | X  | X  | X  |

### 12. K-Map Simplification


D0 = Q0'
D1 = Q2' . (Q1 XOR Q0)
D2 = Q2' . Q1 . Q0 + Q2 . Q0'

### 13. Self-Starting Check
If the counter powers up in an unused state:
110 -> 101 (valid)
111 -> 000 (valid)
So it always returns to the normal sequence.

### 14. Circuit
Three D flip-flops with a common clock. An XOR gate and an AND gate
form D1. An AND gate, an AND gate and an OR gate form D2.
D0 is simply Q0 inverted.


### 15. Timing Diagram
Clock pulse:  1    2    3    4    5    6    7
Count (Q2Q1Q0): 001  010  011  100  101  000  001


---

## 16. Conclusion
Both circuits were designed using D flip-flops and verified in
simulation. The traffic-light controller cycles through
Green, Yellow and Red. The mod-6 counter counts 0 to 5 and resets,
and both circuits recover from unused states.
