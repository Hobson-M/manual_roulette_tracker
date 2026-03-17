IMPLEMENTATION PLAN:
Phase 1: Create Unified File Structure
Single Python file: autonomous_bot_unified.py
History Monitor class (mother)
Trigger Bot class (integrated child)
Heartbeat Monitor class (shared utility)
Single UI with Option 2 layout
Phase 2: Core Integration
History vision loop (existing logic)
Trigger scanning loop (1s interval)
Heartbeat verification (Step 1 + Step 2)
Progressive checking (1-10s for T2/3, 1-15s for T4/5)
Phase 3: Recovery System
F5 → Lobby → T7 → Get In Session sequence
Retry counter (max 5 attempts)
Alert + pause on failure
Phase 4: Logging & UI
Console + UI + File logging
Real-time status display
Configurable update intervals
