# Assignment 3 Solutions: Modal Logic and Capabilities

## Question 1: Hennessy-Milner Logic for a System

### Problem Statement
A system has three states: S1, S2, S3. From S1, action *a* leads to S2 or S3. At S2, property P holds.
- **a)** Express the system's behavior using Hennessy-Milner Logic (HML).
- **b)** Determine if [a]P holds at S1. Justify your answer.

### Solution

#### Part a: Express the System's Behavior Using HML
Hennessy-Milner Logic describes the behavior of labeled transition systems (LTS) using modalities ⟨a⟩φ (possibility: there exists a transition labeled *a* to a state satisfying φ) and [a]φ (necessity: all transitions labeled *a* lead to states satisfying φ).

**System Description**:
- States: {S1, S2, S3}.
- Transitions: S1 --a--> S2, S1 --a--> S3.
- Labeling: P holds at S2, i.e., L(S2) = {P}, L(S1) = L(S3) = ∅.

In HML, we describe what each state can or must satisfy:
- **S1**: Can perform action *a* to reach S2 (where P holds) or S3 (where P does not hold).
  - ⟨a⟩P: There exists an *a*-transition to a state where P holds (S1 --a--> S2).
  - ⟨a⟩¬P: There exists an *a*-transition to a state where P does not hold (S1 --a--> S3).
- **S2**: Satisfies P, no outgoing transitions specified.
  - P: The atomic proposition P holds.
- **S3**: No properties or transitions specified.
  - True (or no specific formula, as no properties hold).

**HML Formulas**:
- At S1: ⟨a⟩P ∧ ⟨a⟩¬P.
- At S2: P.
- At S3: True.

#### Part b: Does [a]P Hold at S1?
The formula [a]P holds at a state *s* if *all* *a*-transitions from *s* lead to states where P holds.

- From S1, the *a*-transitions are:
  - S1 --a--> S2, where P holds (L(S2) = {P}).
  - S1 --a--> S3, where P does not hold (L(S3) = ∅).
- Since there exists an *a*-transition (to S3) where P does not hold, [a]P does not hold at S1.

**Justification**:
- [a]P requires ∀s' . (s --a--> s' ⇒ s' ⊨ P).
- At S1, S3 is a counterexample because S1 --a--> S3 and S3 ⊭ P.

### Final Answer
- **a)** The system's behavior in HML:
  - S1: ⟨a⟩P ∧ ⟨a⟩¬P.
  - S2: P.
  - S3: True.
- **b)** [a]P does not hold at S1 because there is an *a*-transition to S3, where P does not hold.

---

## Question 2: Invariant for Banking System Balance

### Problem Statement
A banking system has a balance *B* that must never be negative. Transactions are +100 or -50 at any step. Prove that the invariant *B ≥ 0* holds for all reachable states, assuming an initial balance of 200.

### Solution
We model the system as a state machine where the state is the balance *B*, and transitions represent transactions.

**System Model**:
- **State**: Balance *B* (integer).
- **Initial State**: *B* = 200.
- **Transitions**:
  - Deposit: *B' = B + 100*.
  - Withdraw: *B' = B - 50*.
- **Invariant**: *B ≥ 0* for all reachable states.

**Proof by Induction**:
We prove that *B ≥ 0* holds for all reachable states using mathematical induction on the number of transactions.

- **Base Case** (n = 0):
  - Initial state: *B* = 200.
  - 200 ≥ 0, so the invariant holds.

- **Inductive Hypothesis**:
  - Assume that after *n* transactions, the balance *B_n* ≥ 0.

- **Inductive Step**:
  - From state *B_n* ≥ 0, consider the possible transactions:
    - **Deposit**: *B_{n+1} = B_n + 100*.
      - Since *B_n* ≥ 0, *B_n + 100* ≥ 100 > 0.
      - Thus, *B_{n+1}* ≥ 0.
    - **Withdrawal**: *B_{n+1} = B_n - 50*.
      - We need *B_n - 50* ≥ 0, i.e., *B_n* ≥ 50.
      - We must show that all reachable states have *B_n* ≥ 50 to ensure withdrawals are safe.

**Analyze Reachable Balances**:
The balance changes by +100 or -50, so all reachable balances are of the form:
- *B = 200 + 100k - 50m*, where *k, m* are non-negative integers (number of deposits and withdrawals).
- Rewrite: *B = 200 + 100k - 50m = 200 + 50(2k - m)*.
- Let *z = 2k - m*. Then *B = 200 + 50z*, where *z* is an integer (since *k, m* ≥ 0, *z* can be positive, negative, or zero).

To find the minimum possible balance:
- Minimize *B = 200 + 50z* subject to *z = 2k - m*, *k, m* ≥ 0.
- Since *z* can be any integer (e.g., *k = 0, m = 2* gives *z = -2*; *k = 1, m = 0* gives *z = 2*), the smallest *z* depends on the number of withdrawals.
- The balance decreases most with maximum withdrawals (*k = 0*):
  - *B = 200 - 50m*.
  - After *m = 4* withdrawals: *B = 200 - 50*4 = 200 - 200 = 0*.
  - After *m = 5*: *B = 200 - 50*5 = -50*, which violates *B ≥ 0*.

**Assumption Clarification**:
The problem does not specify a guard condition (e.g., withdrawals only allowed if *B ≥ 50*). In real banking systems, a withdrawal is only allowed if the resulting balance is non-negative. Let’s assume:
- **Guard**: A withdrawal (*B' = B - 50*) is only allowed if *B - 50* ≥ 0, i.e., *B* ≥ 50.

**Reprove with Guard**:
- **Base Case**: *B* = 200 ≥ 0.
- **Inductive Step**:
  - From *B_n* ≥ 0:
    - **Deposit**: *B_{n+1} = B_n + 100* ≥ 100 > 0.
    - **Withdrawal**: Only allowed if *B_n* ≥ 50*. Then *B_{n+1} = B_n - 50* ≥ 50 - 50 = 0*.
  - Thus, *B_{n+1}* ≥ 0.

** reachable Balances**:
- With the guard, *B = 200 + 50(2k - m)*, but *m* is limited by the condition that *B ≥ 50* before each Withdrawal.
- Minimum balance occurs with maximum withdrawals:
  - Start: *B = 200*.
  - Withdraw 4 times: *200 - 50*4 = 0* (allowed, as *50, 100, 150, 200* ≥ 50 before each withdrawal).
  - Further withdrawal not allowed (*0 < 50*).
- Thus, *B* ≥ 0 always holds.

### Final Answer
Assuming a guard that withdrawals are only allowed if *B* ≥ 50*, the invariant *B ≥ 0* holds for all reachable states. The proof uses induction, showing that deposits increase the balance and guarded withdrawals ensure non-negativity. Without the guard, the invariant could be violated (e.g., *B = -50* after 5 withdrawals).

---

## Question 3: Liveness and Fairness for CPU Scheduler

### Problem Statement
A CPU scheduler assigns tasks in a round-robin fashion for three processes P1, P2, P3. Formulate liveness and fairness properties using temporal logic (e.g., G ◇ P1 for fairness). Explain your reasoning.

### Solution
**System Description**:
- Round-robin scheduling: Processes P1, P2, P3 are executed in a cyclic order (e.g., P1 → P2 → P3 → P1 → ...).
- Each process gets a time slice before the scheduler moves to the next.

**Temporal Logic**:
We use Linear Temporal Logic (LTL) to express properties over infinite execution traces. Key operators:
- **G φ**: φ holds globally (always).
- **◇ φ**: φ holds eventually.
- **φ U ψ**: φ holds until ψ holds.

**Liveness Property**:
Liveness ensures that each process gets to execute eventually.
- For P1: ◇ P1 (P1 is scheduled eventually).
- Generalize: For each process Pi (i = 1, 2, 3):
  - **◇ Pi**: Process Pi is scheduled at some point.
- Combined: **◇ P1 ∧ ◇ P2 ∧ ◇ P3**.

In round-robin scheduling, the cycle ensures each process is scheduled in turn, so ◇ Pi holds for each Pi.

**Fairness Property**:
Fairness ensures that each process is scheduled infinitely often (strong fairness).
- For P1: **G ◇ P1** (always, P1 will eventually be scheduled again).
- Generalize: For each process Pi:
  - **G ◇ Pi**: Process Pi is scheduled infinitely often.
- Combined: **G ◇ P1 ∧ G ◇ P2 ∧ G ◇ P3**.

**Reasoning**:
- **Liveness**: In round-robin, the scheduler cycles through P1, P2, P3. After at most two time slices, any process Pi is scheduled, satisfying ◇ Pi.
- **Fairness**: The cyclic nature ensures that no process is starved. After each execution of Pi, the scheduler will return to Pi after scheduling Pj and Pk, satisfying G ◇ Pi.
- The combined formulas ensure all processes are treated equitably over infinite executions.

### Final Answer
- **Liveness**: ◇ P1 ∧ ◇ P2 ∧ ◇ P3 (each process is scheduled eventually).
- **Fairness**: G ◇ P1 ∧ G ◇ P2 ∧ G ◇ P3 (each process is scheduled infinitely often).
- **Reasoning**: Round-robin scheduling guarantees that each process is executed in a fixed cycle, ensuring eventual and repeated scheduling.

---

## Question 4: LTL for Traffic Light System

### Problem Statement
A traffic light system transitions from Green → Yellow → Red → Green. A car must eventually see a green light after waiting at red. Express these conditions using LTL operators.

### Solution
**System Description**:
- States: Green, Yellow, Red.
- Transitions: Green → Yellow → Red → Green (cyclic).
- Requirement: From Red, Green is eventually reached.

**LTL Formulas**:
1. **Transition Sequence**:
   - The system follows the cycle Green → Yellow → Red → Green.
   - Express that each state is followed by the next:
     - **G (Green → X Yellow)**: If Green, the next state is Yellow.
     - **G (Yellow → X Red)**: If Yellow, the next state is Red.
     - **G (Red → X Green)**: If Red, the next state is Green.
   - Combined: **G (Green → X Yellow) ∧ G (Yellow → X Red) ∧ G (Red → X Green)**.

2. **Car Sees Green After Red**:
   - A car waiting at Red must eventually see Green.
   - **G (Red → ◇ Green)**: If the light is Red, Green will eventually occur.

**Verification**:
- The transition Red → Green ensures that from Red, the next state is Green.
- Thus, G (Red → X Green) is stronger than G (Red → ◇ Green), as X Green implies ◇ Green.

### Final Answer
- **Transition Sequence**: G (Green → X Yellow) ∧ G (Yellow → X Red) ∧ G (Red → X Green).
- **Car Requirement**: G (Red → ◇ Green).
- The formulas capture the cyclic behavior and ensure that a car at Red will see Green in the next state.

---

## Question 5: Modal Mu-Calculus for Infinite Execution

### Problem Statement
Define a recursive formula in modal mu-calculus that ensures a process continues execution infinitely while allowing interruptions (e.g., a server that must eventually restart). Explain how the formula ensures termination or infinite execution.

### Solution
**System Description**:
- A server process executes (action *run*), may be interrupted (action *interrupt*), and must eventually restart (action *restart*).
- Goal: The process runs infinitely often, with possible interruptions, but always restarts.

**Modal Mu-Calculus**:
Mu-calculus uses fixed points to express recursive properties:
- **μX.φ**: Least fixed point (finite paths).
- **νX.φ**: Greatest fixed point (infinite paths).
- Modalities: ⟨a⟩φ (exists a transition), [a]φ (all transitions).

**Formula**:
To ensure infinite execution with interruptions and restarts:
- The process can always *run* or be *interrupted*, and after an *interrupt*, it must eventually *restart*.
- Use a greatest fixed point (ν) for infinite behavior:
  - **νX. (⟨run⟩True ∧ [interrupt]⟨restart⟩X)**.
- Explanation:
  - **⟨run⟩True**: There exists a *run* transition (process can execute).
  - **[interrupt]⟨restart⟩X**: All *interrupt* transitions lead to a state where there exists a *restart* transition to a state satisfying X.
  - **νX**: The property holds for infinite paths (greatest fixed point).

**How It Ensures Infinite Execution**:
- The greatest fixed point ensures there is an infinite path where the process can always *run* or handle *interrupt* followed by *restart*.
- **⟨run⟩True** guarantees execution capability at each step.
- **[interrupt]⟨restart⟩X** ensures that interruptions lead to restarts, returning to a state where the process can continue.
- The formula does not enforce termination, as νX allows infinite recursion.

**Termination**:
- The formula allows finite paths (e.g., stopping after a *run* without *interrupt*), but the greatest fixed point emphasizes infinite paths where the process continues or restarts after interruptions.

### Final Answer
- **Formula**: νX. (⟨run⟩True ∧ [interrupt]⟨restart⟩X).
- **Explanation**: The greatest fixed point ensures infinite execution by requiring *run* capability and handling *interrupt* with eventual *restart*. It allows finite paths but prioritizes infinite continuation.

---

## Question 6: Modal Logic for a System

### Problem Statement
A system has three states:
- S1 --a--> S2.
- S2 --b--> S3.
- S3 satisfies property P.
Express the system behavior using modal logic and determine:
- **a)** Whether ⟨P⟩ holds at S1.
- **b)** Whether [P] holds at S1.

### Solution
**System Description**:
- States: {S1, S2, S3}.
- Transitions: S1 --a--> S2, S2 --b--> S3.
- Labeling: L(S3) = {P}, L(S1) = L(S2) = ∅.

**Modal Logic**:
We use HML:
- **S1**: Can perform *a* to S2, which can perform *b* to S3 (where P holds).
  - ⟨a⟩⟨b⟩P.
- **S2**: Can perform *b* to S3.
  - ⟨b⟩P.
- **S3**: Satisfies P.
  - P.

**Part a: Does ⟨P⟩ Hold at S1?**
- In HML, ⟨P⟩ is not a standard notation. Assume it means ⟨a⟩P or "eventually P" (reachability).
- Interpret as: Is there a path from S1 to a state where P holds?
- Path: S1 --a--> S2 --b--> S3, where P holds.
- In mu-calculus, "eventually P" is μX. (P ∨ ⟨-⟩X), where ⟨-⟩ is any transition.
- Since S1 can reach S3 via *a, b*, ⟨P⟩ (reachability) holds at S1.

**Part b: Does [P] Hold at S1?**
- [P] is also non-standard. Assume it means [a]P (all *a*-transitions lead to P).
- At S1:
  - S1 --a--> S2, where P does not hold (L(S2) = ∅).
  - Since S2 ⊭ P, [a]P does not hold at S1.

### Final Answer
- **System Behavior**:
  - S1: ⟨a⟩⟨b⟩P.
  - S2: ⟨b⟩P.
  - S3: P.
- **a)** ⟨P⟩ (as reachability) holds at S1, as S1 can reach S3 where P holds.
- **b)** [P] (as [a]P) does not hold at S1, as S1 --a--> S2, where P does not hold.

---

## Question 7: Safety Property for Traffic Light Controller

### Problem Statement
A traffic light controller cycles through Green → Yellow → Red → Green. Prove that the safety property "never transition from Green to Red directly" holds using state transition invariants.

### Solution
**System Description**:
- States: {Green, Yellow, Red}.
- Transitions: Green → Yellow, Yellow → Red, Red → Green.
- Safety Property: No transition Green → Red.

**State Transition Invariant**:
An invariant is a property that holds in all reachable states and transitions.
- Define the invariant on transitions:
  - **I**: For all states *s*, if *s = Green*, then the next state *s'* is Yellow.
  - In LTL: G (Green → X Yellow).

**Proof**:
- **Initial State**: Assume Green (or any state in the cycle).
- **Transitions**:
  - From Green, the only transition is to Yellow (Green → Yellow).
  - No transition exists from Green to Red.
- **Induction on Transitions**:
  - **Base Case**: In Green, the next state is Yellow (satisfies I).
  - **Inductive Step**: For each state:
    - Green → Yellow: Next state is Yellow, not Red.
    - Yellow → Red: Not Green, so I is vacuously true.
    - Red → Green: Not Green, so I is vacuously true.
  - The cycle ensures all reachable states follow the sequence.
- **Conclusion**: The transition Green → Red is not possible, so the safety property holds.

### Final Answer
The safety property "no direct transition from Green to Red" holds. The invariant G (Green → X Yellow) ensures that from Green, the next state is always Yellow, proven by the fixed transition sequence.

---

## Question 8: Printer Queue Liveness and Waiting Time

### Problem Statement
A printer queue processes jobs in FIFO order. Job Jn is submitted at *t = 0*. Jobs are printed every 2 minutes.
- **a)** Express the liveness property ensuring Jn is eventually printed.
- **b)** Determine the maximum time Jn will wait if there are 4 jobs ahead.

### Solution
**System Description**:
- FIFO queue: Jobs are processed in the order submitted.
- Processing time: Each job takes 2 minutes.
- Jn submitted at *t = 0*, with 4 jobs ahead (J1, J2, J3, J4).

**Part a: Liveness Property**:
Liveness ensures Jn is eventually printed.
- In LTL: **◇ Printed(Jn)**, where Printed(Jn) means Jn is processed.
- In a FIFO queue, Jn will be printed after all jobs ahead are processed.

**Part b: Maximum Waiting Time**:
- Jobs ahead: 4 (J1, J2, J3, J4).
- Each job takes 2 minutes.
- Processing order: J1, J2, J3, J4, Jn.
- Timeline:
  - J1 finishes at *t = 2* minutes.
  - J2 finishes at *t = 4* minutes.
  - J3 finishes at *t = 6* minutes.
  - J4 finishes at *t = 8* minutes.
  - Jn starts at *t = 8* and finishes at *t = 10* minutes.
- Waiting time for Jn: Time until Jn starts printing = 8 minutes.

### Final Answer
- **a)** Liveness: ◇ Printed(Jn).
- **b)** Maximum waiting time: 8 minutes (4 jobs * 2 minutes each).

---

## Question 9: Hennessy-Milner Logic Verification

### Problem Statement
A system has transitions:
- S1 --a--> S2.
- S2 --b--> S3.
- S3 satisfies P.
Verify in HML:
- **a)** Does ⟨a⟩⟨b⟩P hold at S1?
- **b)** Does [a]P hold at S1?

### Solution
**System Description**:
- States: {S1, S2, S3}.
- Transitions: S1 --a--> S2, S2 --b--> S3.
- Labeling: L(S3) = {P}, L(S1) = L(S2) = ∅.

**Part a: Does ⟨a⟩⟨b⟩P Hold at S1?**
- ⟨a⟩φ holds if there exists an *a*-transition to a state satisfying φ.
- At S1:
  - S1 --a--> S2.
  - Check if S2 ⊨ ⟨b⟩P:
    - S2 --b--> S3, where S3 ⊨ P (L(S3) = {P}).
    - Thus, S2 ⊨ ⟨b⟩P.
- Therefore, S1 ⊨ ⟨a⟩⟨b⟩P.

**Part b: Does [a]P Hold at S1?**
- [a]P holds if all *a*-transitions lead to states where P holds.
- At S1:
  - S1 --a--> S2.
  - S2 ⊭ P (L(S2) = ∅).
- Since S2 does not satisfy P, [a]P does not hold at S1.

### Final Answer
- **a)** ⟨a⟩⟨b⟩P holds at S1, as S1 --a--> S2 --b--> S3, where P holds.
- **b)** [a]P does not hold at S1, as S1 --a--> S2, where P does not hold.

---

## Question 10: LTL for Web Server

### Problem Statement
A web server processes requests:
- Requests are processed within 5 seconds.
- No request remains pending indefinitely.
Express these conditions using LTL.

### Solution
**System Description**:
- Requests are received and must be processed.
- Timing: Processed within 5 seconds.
- Liveness: No request is pending forever.

**LTL Formulas**:
1. **Processed within 5 Seconds**:
   - If a request is received, it is processed within 5 seconds.
   - Assume discrete time steps (e.g., seconds).
   - **G (Request → X Processed ∨ X^2 Processed ∨ X^3 Processed ∨ X^4 Processed ∨ X^5 Processed)**.
   - Simplified: **G (Request → ◇_≤5 Processed)**, where ◇_≤5 φ means φ holds within 5 steps.

2. **No Request Pending Indefinitely**:
   - Every request is eventually processed.
   - **G (Request → ◇ Processed)**.

**Note**:
- The first property (within 5 seconds) implies the second (eventual processing), as ◇_≤5 Processed → ◇ Processed.
- Thus, **G (Request → ◇_≤5 Processed)** is sufficient.

### Final Answer
- **LTL Formula**: G (Request → ◇_≤5 Processed).
- This ensures requests are processed within 5 seconds, implying no request remains pending indefinitely.
