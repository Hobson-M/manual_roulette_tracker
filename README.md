SUMMARY OF UNDERSTANDING - CLOSED LOOP 24/7 SYSTEM
CRITICAL CONCEPT: HEARTBEAT
Heartbeat = Step 1 (numbers flowing with NO errors) + Step 2 (Trigger 1 visible and NOT stuck)

If heartbeat is lost at ANY point → Execute recovery sequence until heartbeat restored.

STEP 1: History Monitor (Numbers Must Flow - No Errors)
Normal Operation:
history.py reads 4 boxes continuously
All 4 boxes must show valid numbers (no errors)
When numbers flowing cleanly → Proceed to Step 2
Error Handling (Step 1 Failures):
Minor errors detected:

Wait for self-correction (history.py has built-in healing)
Monitor for recovery
Persistent errors (self-correction fails):

Press F5 (refresh page)
Lands in lobby
Execute Trigger 7 (Select Table)
Execute Get in Session (Fullscreen)
Return to Step 1 (verify numbers flowing)
Repeat until heartbeat achieved
STEP 2: Trigger 1 Verification (Heartbeat Check)
Normal Operation:
Check if Trigger 1 (Wait For Next Game) is visible
Trigger 1 should appear and disappear normally (betting window closing/opening)
If Step 1 ✅ + Step 2 ✅ → HEARTBEAT CONFIRMED → Proceed to Step 3
Error Handling (Step 2 Failures):
Case A: Trigger 1 NOT detected at all
Could mean:
Not in session
Wrong screen state
Technical issue
Action:
Press F5 (refresh)
Lobby → Trigger 7 → Get in Session
Return to Step 1
Repeat until heartbeat
Case B: Trigger 1 STUCK on screen (prolonged period)
Stuck = Visible for >X seconds without changing (dealer issue, ball stuck, technical glitch)
Action:
Press F5 (refresh)
Lobby → Trigger 7 → Get in Session
Return to Step 1
Repeat until heartbeat
STEP 3: Active Scanning Loop (In Session - Heartbeat Alive)
Concurrent Scanning (Every 1 second):
Scan Triggers 1-5 simultaneously:

TRIGGER 1: Heartbeat Monitor (Always Active)
Purpose: Continuous session verification

Normal State:

Appears during ball spin (betting closed)
Disappears when betting opens
Cycling = Healthy heartbeat
Abnormal States:

Not visible when expected → Lost session → Recovery
Stuck visible (>X seconds) → Frozen state → Recovery
Recovery Action:

F5 → Lobby → Trigger 7 → Get in Session
Return to Step 1
Repeat until heartbeat
TRIGGER 2: Inactivity Pause
If detected:

Click center of ROI
Wait 1 second
Check heartbeat (Step 1 + Step 2)
✅ Heartbeat OK → Resume scanning
❌ No heartbeat → F5 → Lobby → Trigger 7 → Get in Session → Step 1 → Repeat until heartbeat
TRIGGER 3: Isolated OK Button
If detected:

Click OK button
Wait 1 second
Check heartbeat (Step 1 + Step 2)
✅ Heartbeat OK → Resume scanning
❌ No heartbeat → F5 → Lobby → Trigger 7 → Get in Session → Step 1 → Repeat until heartbeat
TRIGGER 4: Dead-end Session (F5 Refresh)
If detected:

Press F5 (page refresh)
Wait 5 seconds (lands in lobby)
Execute Trigger 7 (Select Table)
Wait 5 seconds
Execute Get in Session (Fullscreen)
Wait 5 seconds
Check heartbeat (Step 1 + Step 2)
✅ Heartbeat OK → Resume scanning
❌ No heartbeat → Repeat entire Trigger 4 sequence until heartbeat
TRIGGER 5: Session Crash with OK Button
If detected:

Click OK button
Wait 5 seconds (lands in lobby)
Execute Trigger 6 (Find Provider - infinity scroll)
Wait 5 seconds
Execute Trigger 7 (Select Table - infinity scroll)
Wait 5 seconds
Execute Get in Session (Fullscreen - infinity scroll)
Wait 5 seconds
Check heartbeat (Step 1 + Step 2)
✅ Heartbeat OK → Resume scanning
❌ No heartbeat → Repeat entire Trigger 5 sequence until heartbeat
ADDITIONAL LOGIC I'M ADDING:
1. Heartbeat Verification Function
python

def check_heartbeat():
    """
    Returns True only if BOTH conditions met:
    - Step 1: history.py showing 4 valid numbers (no errors)
    - Step 2: Trigger 1 visible AND not stuck (healthy cycling)
    """
    step1 = history_monitor.all_boxes_valid()  # No errors in 4 boxes
    step2 = trigger1_healthy()  # Visible but not stuck
    return step1 and step2
2. Trigger 1 "Stuck" Detection
Track last state change timestamp
If Trigger 1 visible for >60 seconds without changing → STUCK
If stuck → Execute recovery
3. Recovery Loop with Retry Limit
Each recovery attempt tracked
After 5 failed recovery attempts → Show alert + pause for user intervention
Prevents infinite F5 spam
4. Scan Priority
Trigger 1 check (heartbeat monitor) - every cycle
Triggers 2-5 scan - execute first match found
After any action → Verify heartbeat
If heartbeat lost → Recovery sequence
5. Bootstrap Sequence (Startup)
code

Program Start
    ↓
Wait for history.py to initialize
    ↓
Check Step 1 (numbers flowing?)
    ↓ No → Wait/Retry
    ↓ Yes
Check Step 2 (Trigger 1 visible & healthy?)
    ↓ No → F5 Recovery
    ↓ Yes
HEARTBEAT CONFIRMED
    ↓
Enter Active Scanning Loop (Step 3)

Show more
FLOW DIAGRAM:
code

START
  ↓
╔═══════════════════════════════════╗
║  STEP 1: History Monitor          ║
║  (4 boxes must be error-free)     ║
╚═══════════════════════════════════╝
  ↓
Numbers flowing? ──No──→ Wait for self-heal
  ↓ Yes                       ↓
  ↓                    Still broken?
  ↓                           ↓ Yes
  ↓                    F5 → Lobby → T7 → Get In Session
  ↓                           ↓
  ↓                    Return to STEP 1 ↑
  ↓
╔═══════════════════════════════════╗
║  STEP 2: Trigger 1 Verification   ║
║  (Must be visible & not stuck)    ║
╚═══════════════════════════════════╝
  ↓
Trigger 1 healthy? ──No (missing/stuck)──→ F5 → Lobby → T7 → Get In Session
  ↓ Yes                                            ↓
  ↓                                         Return to STEP 1 ↑
  ↓
✅ HEARTBEAT CONFIRMED
  ↓
╔═══════════════════════════════════╗
║  STEP 3: Active Scanning Loop     ║
║  (Scan Triggers 1-5 every 1s)     ║
╚═══════════════════════════════════╝
  ↓
Check Trigger 1 (heartbeat)
  ↓
Stuck/Missing? ──Yes──→ F5 Recovery → STEP 1 ↑
  ↓ No (healthy)
Scan Triggers 2-5
  ↓
Trigger found?
  ↓ Yes
Execute Trigger Action
  ↓
Check Heartbeat
  ↓
Lost? ──Yes──→ F5 Recovery → STEP 1 ↑
  ↓ No
Resume Loop ↑

Show more
QUESTIONS BEFORE CODING:
1. Trigger 1 "Stuck" Timeout:
How long should Trigger 1 be visible before considered "stuck"?

Suggestion: 60 seconds (normal betting window ~20-30s)
2. Step 1 Error Detection:
How do we detect "errors" in history.py boxes?

Check for None values?
Check for OCR failures?
Check for mismatches?
Suggestion: Use existing history.py error tracking (mismatch_counters)
3. Self-Correction Wait Time:
When Step 1 errors detected, how long to wait before F5?

Suggestion: 10 seconds (give history.py time to self-heal)
4. Recovery Retry Limit:
After how many failed recovery attempts should we alert user?

Suggestion: 5 attempts (prevents infinite loops)
5. Scan Frequency:
You said 1 second - confirm this is good for all triggers?

Confirmed: 1 second scan interval
6. Wait Times:
Trigger 2/3: 1 second after action ✅
Trigger 4/5/6/7: 5 seconds after each step ✅
7. Architecture:
Should I create:

Option A: Single unified file (all logic in one)
Option B: Modular tabs (history + triggers separate but integrated)
Your preference?
