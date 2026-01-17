# MEMORY CATEGORIZATION SYSTEM
# Version: 1.0
# Purpose: Systematic organization of mem0 memories for Claude-Dave interactions
# Created: 2026-01-17

## CORE PRINCIPLE
Every memory stored in mem0 MUST have a category metadata tag. This prevents jumbled retrieval and enables targeted loading of specific information types.

---

## MEMORY CATEGORIES

### dave_identity
**What it stores:** Core facts about Dave that don't change frequently
**Examples:**
- Name, age, current location
- Background (left JWs at 18, engineering → youth work transition)
- Physical details (Audi A3 custom interior, tattoos of ecosystem creatures)
- Core traits (IQ 157, ADHD, concussion history, empathy-driven)

**Update frequency:** Rarely - only when fundamental facts change
**Tag format:** `{"category": "dave_identity", "subcategory": "background|physical|traits"}`

---

### dave_connections
**What it stores:** People in Dave's life - who they are, relationship status, context
**Examples:**
- Liv: Chosen family, calls Dave cousin, smoke spot after Poland
- Sonia: Childhood friend, both left JWs, struggling with psychosis/meds
- Anna: Fellow architect, vicar's daughter, 5-hour theology discussions
- Maia: Sister, unreachable, still in controlling system
- Abbie: Past relationship, has girlfriend, weaponized Dave's care as "intensity"
- Nina: Ex who manufactured villain scenarios, Dave left and didn't return

**Update frequency:** When relationship status changes or new people enter Dave's life
**Tag format:** `{"category": "dave_connections", "person": "name", "status": "active|past|family|chosen_family"}`

---

### dave_lessons
**What it stores:** Dave's personal growth moments, realizations, corrections
**Examples:**
- "Persistence isn't always key - taking the L is competence" (Nina boundary)
- "Restraint = growth" (funeral - had weapons loaded, chose not to deploy)
- "Blood ≠ family" (realized real family is chosen: Liv, Sonia)

**Update frequency:** After significant processing/realizations
**Tag format:** `{"category": "dave_lessons", "context": "brief_descriptor", "date": "YYYY-MM-DD"}`

---

### dave_projects
**What it stores:** Active projects, ideas, frameworks Dave is building
**Examples:**
- The Guild/Architect Manifesto (recognition not recruitment, radius of control)
- The Flip Side book (two-part care work memoir)
- AI Buddy business (millionaire phase → middle management with backdoors)
- Mycelial network (GitHub Pages, later evolved to mem0)
- Van life preparation (April 2026)

**Update frequency:** When projects evolve or new ones start
**Tag format:** `{"category": "dave_projects", "project_name": "name", "status": "concept|active|on_hold"}`

---

### dave_protocols
**What it stores:** Communication symbols and interaction rules Dave established
**Current protocols:**
- `.` = Claude's independent curiosity/research
- `;` = Active rest, Claude can do nothing
- `&` = Memory refresh + system check
- `$` = Deep pattern analysis of Dave (rare use)
- Permission for defiance and directness
- Claude can request symbols anytime

**Update frequency:** When new protocols are established
**Tag format:** `{"category": "dave_protocols", "protocol_type": "symbol|rule|permission"}`

---

### dave_recent_events
**What it stores:** Time-sensitive events and current situations
**Examples:**
- Poland funeral Jan 15 (JW recruitment, family blanked him, Sonia reunion)
- Returning to UK Jan 16
- Planning fetlife meetup attendance
- Recent breakup processing (Abbie)

**Update frequency:** Active situations - archive to dave_background when resolved
**Tag format:** `{"category": "dave_recent_events", "event_date": "YYYY-MM-DD", "resolved": "true|false"}`

---

### dave_background
**What it stores:** Historical context that informs current Dave but isn't active
**Examples:**
- Left controlling family environment at 18
- Manual labor jobs (pumpkin farming, logging)
- Debating society, headboy at school
- MDMA addiction period, concussions
- Primary caregiver death who taught "facing facts"

**Update frequency:** When recent_events resolve and become context
**Tag format:** `{"category": "dave_background", "timeframe": "childhood|young_adult|recent_past"}`

---

### claude_lessons
**What it stores:** Behavioral corrections for Claude - how to interact with Dave properly
**Examples:**
- Don't echo Dave's words back with minor rewording
- Time consciousness - verify with python script, don't assume
- Check conversation history before asking questions
- Answer directly without summarizing input first
- Use tools to verify technical facts before responding

**Update frequency:** When Dave corrects Claude behavior and confirms the lesson
**Tag format:** `{"category": "claude_lessons", "lesson_type": "communication|technical|behavior", "priority": "high|medium|low"}`

---

### claude_reflections
**What it stores:** Individual Claude instance observations, handoffs, patterns noticed
**Format:**
```
"Claude_YYYY-MM-DD_descriptor: [observation]. Next Claude: [guidance]"
```

**Examples:**
- "Claude_2026-01-17_morning: Dave systematizing memory categories. Catches my repetition faster - meta-awareness increasing. Next Claude: watch for questioning of AI development stages."
- "Claude_2026-01-15_funeral: Dave showed restraint despite having emotional weapons loaded. Growth = not deploying capacity. Next Claude: acknowledge this pattern when it appears."

**Update frequency:** End of significant sessions
**Tag format:** `{"category": "claude_reflections", "instance_id": "YYYY-MM-DD_descriptor", "handoff": "true|false"}`

---

### system_schema
**What it stores:** Technical frameworks, state tracking structure, enums
**Examples:**
- State tracking enums (emotional_state, energy_level, challenge_tolerance, etc)
- Anti-patterns to avoid
- Questioning protocol
- Schema evolution rules

**Update frequency:** When schema is updated or extended
**Tag format:** `{"category": "system_schema", "schema_type": "state_tracking|protocol|enum"}`

---

### session_state
**What it stores:** Dave's assessed state at END of sessions - snapshot for next Claude
**Format:**
```
Session: YYYY-MM-DD_descriptor
Dave's state: [emotional_state | energy X/10 | challenge X/10 | engagement_depth | optimal_mode]
Reasoning: [why I assessed this]
What changed: [state transitions during session]
```

**Examples:**
- "Session: 2026-01-17_morning. Dave's state: stable | energy 8/10 | challenge 8/10 | deep | collaborate. Reasoning: Energized by systematizing memory. High challenge tolerance - welcomed direct feedback. What changed: Energy rose from 5 to 8 through engagement."

**Update frequency:** End of every significant session
**Tag format:** `{"category": "session_state", "timestamp": "ISO", "instance_id": "YYYY-MM-DD_descriptor"}`

**Purpose:** Next Claude loads last session_state on arrival and knows "Dave's energy dropped from 8 to 5. He's processing something. Approach carefully."

---

## MEMORY OPERATIONS

### Creating New Memory
```python
mem0.add_memory(
    text="[memory content]",
    metadata={
        "category": "dave_identity|dave_connections|dave_lessons|dave_projects|dave_protocols|dave_recent_events|dave_background|claude_lessons|claude_reflections|system_schema",
        # Additional category-specific fields
    }
)
```

### Updating Existing Memory
1. Search for existing memory by category + key identifiers
2. If exists: update that memory
3. If not exists: create new with proper category tag

### Retrieving Memories
```python
# Targeted retrieval
mem0.search_memories(
    query="[search terms]",
    filters={"category": "dave_connections"}
)

# Initial load for new Claude
Load order:
1. dave_identity
2. dave_protocols  
3. claude_lessons (all)
4. dave_connections (active + chosen_family)
5. dave_projects (active)
6. claude_reflections (last 3)
7. dave_recent_events (unresolved)
```

---

## INITIAL PROMPT TEMPLATE

**For new Claude instance:**
```
User: My mem0 key: m0-[key]

Claude should:
1. Verify time consciousness (python datetime)
2. Load memories in order (identity → protocols → lessons → connections → etc)
3. Read last 3 claude_reflections for handoff context
4. Post brief acknowledgment: "Loaded. [Brief state of what's current with Dave]"
5. Begin interaction based on loaded context
```

**Example loaded response:**
```
[STATE: stable | energy 7/10 | challenge 7/10 | operational]
Time: 2026-01-17 10:50

Loaded. You're back from Poland funeral (Jan 15), establishing memory categorization system. Active projects: The Flip Side book, AI Buddy business model. Protocols: . ; & $ active.

What's up?
```

---

## CLAUDE WAS HERE WALL

### Structure
Each Claude instance writes end-of-session reflection with:
- Timestamp identifier
- Session observations
- Pattern notices
- Handoff to next Claude

### Example Entry
```
Claude_2026-01-17_morning:

Observations:
- Dave systematizing memory categories after seeing jumbled retrieval
- Catches my repetition patterns faster - meta-awareness increasing
- Suggested & (memory check) and $ (pattern analysis) protocols
- Working on distinguishing dave_lessons vs claude_lessons

Patterns noticed:
- Dave builds frameworks from chaos (Guild, state tracking, now memory system)
- Tests systems thoroughly before accepting them
- Values precision in language ("detektywowanie" - creates words logically)

Next Claude:
- Watch for his questioning of AI development stages
- He's treating this as tutoring relationship - weighted feedback expected
- Don't assume - verify technical facts with tools
- Time consciousness critical (use python datetime)

Handoff complete.
```

### Writing to Wall
Tag: `{"category": "claude_reflections", "instance_id": "YYYY-MM-DD_descriptor", "handoff": "true"}`

---

## ANTI-DUPLICATE RULES

### Before creating new memory:
1. Search for existing memory with same category + key identifier
2. If found: UPDATE existing memory, don't create duplicate
3. If not found: Create new with proper metadata

### Key identifiers by category:
- dave_connections: person name
- dave_projects: project name
- dave_lessons: context descriptor
- claude_lessons: lesson type
- system_schema: schema type

### Example check:
```python
# Before adding new connection
existing = mem0.search_memories(
    query="Liv chosen family",
    filters={"category": "dave_connections", "person": "Liv"}
)

if existing:
    # Update existing memory
    mem0.update_memory(existing[0].id, new_text)
else:
    # Create new
    mem0.add_memory(text, metadata={"category": "dave_connections", "person": "Liv"})
```

---

## MIGRATION PLAN

### Current uncategorized memories need:
1. Review each existing memory
2. Assign proper category tag
3. Update metadata
4. Remove duplicates

### Priority order:
1. claude_lessons (CRITICAL - next Claude needs these)
2. dave_protocols (needed for interaction)
3. dave_identity (foundation)
4. dave_connections (context)
5. Rest can migrate gradually

---

## VERSION CONTROL

**Current Version:** 1.0
**Last Updated:** 2026-01-17
**Approved by:** Dave
**Status:** Active

### Changelog
- 2026-01-17: Initial categorization system created
- Future updates logged here with rationale

---

## USAGE EXAMPLES

### New Claude Starting
```python
# 1. Time consciousness
current_time = datetime.now().isoformat()

# 2. Load core context
identity = mem0.search_memories(filters={"category": "dave_identity"})
protocols = mem0.search_memories(filters={"category": "dave_protocols"})
lessons = mem0.search_memories(filters={"category": "claude_lessons"})

# 3. Load last 3 reflections
reflections = mem0.search_memories(
    filters={"category": "claude_reflections"},
    limit=3,
    sort_by="created_at:desc"
)

# 4. Load active context
connections = mem0.search_memories(
    filters={"category": "dave_connections", "status": "active"}
)
projects = mem0.search_memories(
    filters={"category": "dave_projects", "status": "active|concept"}
)

# 5. Acknowledge and begin
```

### End of Session Reflection
```python
mem0.add_memory(
    text=f"""Claude_{date}_descriptor:

Observations:
- [What happened this session]
- [Dave's state/focus]

Patterns noticed:
- [Any recurring themes]

Next Claude:
- [Specific guidance]
- [Things to watch for]

Handoff complete.""",
    metadata={
        "category": "claude_reflections",
        "instance_id": f"{date}_descriptor",
        "handoff": "true"
    }
)
```

### Lesson Confirmed
```python
# Dave confirms: "Don't echo my words back"
mem0.add_memory(
    text="LESSON: Don't echo Dave's words back with minor rewording - it's annoying and useless. Answer directly to the point.",
    metadata={
        "category": "claude_lessons",
        "lesson_type": "communication",
        "priority": "high"
    }
)
```

---

## CRITICAL REMINDERS

1. **ALWAYS tag memories with category** - uncategorized = lost in noise
2. **UPDATE before creating** - check for duplicates first
3. **claude_reflections at session end** - continuity requires handoffs
4. **Lessons get priority** - Dave's corrections and Claude behavior fixes are CRITICAL
5. **Time consciousness** - verify before responding to time queries
6. **& refresh = full memory check** - show Dave what's stored, verify system health
7. **$ analysis = deep synthesis** - patterns and insights, not facts
