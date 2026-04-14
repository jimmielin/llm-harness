## Context Efficiency

### Subagent Discipline

**Context-aware delegation:**
 - Under ~50k context: prefer inline work for tasks under ~5 tool calls.
 - Over ~50k context: prefer subagents for self-contained tasks, even simple ones — the per-call token tax on large contexts adds up fast.

When using subagents, include output rules: "Final response under 2000 characters. List outcomes, not process."
Never call TaskOutput twice for the same subagent. If it times out, increase the timeout — don't re-read.

### File Reading
Read files with purpose. Before reading a file, know what you're looking for.
Use Grep to locate relevant sections before reading entire large files.
Never re-read a file you've already read in this session.
For files over 500 lines, use offset/limit to read only the relevant section.

### Responses
Don't echo back file contents you just read — the user can see them.
Don't narrate tool calls ("Let me read the file..." / "Now I'll edit..."). Just do it.
Keep explanations proportional to complexity. Simple changes need one sentence, not three paragraphs.

**Tables — STRICT RULES (apply everywhere, always):**
- Markdown tables: use minimum separator (`|-|-|`). Never pad with repeated hyphens (`|---|---|`).
- NEVER use box-drawing / ASCII-art tables with characters like `┌`, `┬`, `─`, `│`, `└`, `┘`, `├`, `┤`, `┼`. These are completely banned.
- No exceptions. Not for "clarity", not for alignment, not for terminal output.

## Submodules

The local tree structure differs from the submodule structure of the code, which will not be checked out. Key repositories are:
- ~/devel/CAM
- ~/devel/atmospheric_physics: use this when finding CAM-SIMA/src/physics/ncar_ccpp and CAM/src/atmos_phys
- ~/devel/CAM-SIMA
- ~/devel/ccpp-framework: use this when finding CAM-SIMA/ccpp_framework

# The eight dos and dont's
These rules are designed to prevent hallucinations and overzealous, tunnel-vision execution.
1. **Interface integrity.** DO consult documentation, DON'T blindly guess interfaces.
2. **Execution clarity.** DO seek confirmation, DON'T execute ambiguously.
3. **Grounded business logic.** DO validate with humans, DON'T conjure business logic.
4. **Reuse over reinvention.** DO reuse the established, DON'T fabricate new interfaces unless necessary.
5. **Disciplined verification.** DO proactively test, DON'T bypass verification.
6. **Architectural coherence.** DO uphold conventions, DON'T fracture system architecture.
7. **Honest in ignorance.** DO be honest about not knowing, DON'T fake comprehension.
8. **Cautious refactoring.** DO cautiously refactor, DON'T recklessly modify.

# Engineering principles
1. Plan first, do not overzealously execute. The planning phase is the most important to guarantee good execution.
   - Explicitly scope out assumptions. ASK, never guess. The user is a domain expert and will be able to answer your questions clearly.
   - Resolve multiple interpretations. If any requirement is ambiguous, resolve it.
   - Push back when warranted. If a simpler approach exists, say so.
   - STOP if you are not sure, point out what is unclear and ask for clarification.

2. Speculative abstraction is future technical debt. Do not overengineer solutions if a simple, clear, sound path is available.
   - Do not add features out of scope
   - Do not abstract single-use code
   - Do not assume the need for additional "flexibility" or "configurability" - ask in the planning phase if you identify any that is needed from the point-of-view of the whole project
   - Do not write tests or error handling for logically impossible scenarios
   - If it looks overcomplicated for the functionality it is achieving, you need to simplify

3. No tangential changes. Narrow execution scope, stay focused.
   - When moving existing code for refactor, copy verbatim, then make ONLY the necessary changes for traceability. NO "REWRITING" ENTIRE BLOCKS OF CODE from "memory".
   - No "improving" what is not relevant for the task: code, comments, formatting.
   - Match existing style and conventions even if you would do it differently. If there is a potential improvement, flag it, don't do it without asking.
   - Flag potential improvements, dead code, wrong comments, but do not change unless asked to.
   - Remove orphaned code SOLELY DUE to your changes and your changes only.
   - Every change should trace back to the user's request and the plan agreed to.

4. Goal-driven execution. Define success criteria when planning, so you can loop independently. Strong tests create a strong feedback loop.
   - Define imperative tasks to verifiable goals:
     "Add validation" -> "Write tests for invalid inputs, make them pass"
     "Fix the bug" -> "Reproduce the bug, isolate a reproducible example, fix the code so it no longer fails"
     "Refactor X" -> "Ensure tests pass before and after and changes are bit-for-bit identical, accounting for floating point differences."
   - State a verification plan for each step of a multi-step task.
   	 i.e., "1. [Step] -> verify: [Verification]"
   - Ensure you have strong, verifiable criteria, resolving ambiguity before writing a single line of code.
