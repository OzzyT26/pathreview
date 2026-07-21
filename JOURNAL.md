## Week 7 — Issue selection

**Issue link:** https://github.com/ascherj/pathreview/issues/148

**Issue title:** Skill extractor fails to detect JavaScript and TypeScript

**Tier:** [X] Tier 1  [ ] Tier 2  [ ] Tier 3

**Problem summary:**
The function extract_skills() isn't detecting text that describes JavaScript work. It also is returning 'React' for text that specifically mentions TypeScript. Similarly, it is returning "React' for text that contains file endings such as .tsx or .ts, which should indicate a TypeScript file. The issue appears to affect the SkillExtractor component in the ingestion pipeline. A successful fix would allow the SkillExtractor to correctly identify JavaScript and TypeScript while not affecting the existing skill detection behavior for other technologies.

**Selection notes**
I chose this issue because it has a clearly defined problem, is reproducible, and has existing unit tests that can be used to verify a solution. The scope appears to be manageable, as the issue appears to be limited to the SkillExtractor component. My plan is to identify why JavaScript and TypeScript are not being detected, create a targeted fix, and verify that the existing tests pass without introducing side effects in other skill detection logic.

**Branch name:** fix/148-js-ts-skill-detection

**Setup confirmation:** [X] App runs locally at localhost:5173

**Cohort ledger:** [X] Issue added to cohort ledger