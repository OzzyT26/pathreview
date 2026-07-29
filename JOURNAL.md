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

## Week 8 — Reproduction & solution planning

**Reproduction commit link:** https://github.com/OzzyT26/pathreview/commit/40488ff41e7c9b951eb2d3343933c178faa548b5

**Reproduction summary:**
I reproduced issue number 148 by running the existing skill extractor unit tests and the examples from the GitHub issue description. To run the SkillExtractor tests, I ran pytest tests/unit/test_skill_extractor.py -v. I also ran the example text from the GitHub issue description using SkillExtractor.extract_skills(). The JavaScript example returned no detected skills, and the TypeScript example detected React but did not detect TypeScript or JavaScript. The existing test_javascript_detection and test_text_with_typescript_files tests also failed for the same missing detections.

**PLAN.md link:** https://github.com/OzzyT26/pathreview/commit/6c34856

**Walkthrough video (recommended):**

**Blockers or open questions:**
There are many potential things that could cause these issues. I still need to determine whether the failure is caused by missing language patterns, incorrect filename extension handling, etc. The issue could potentially be caused by other less obvious areas as well, such as normalization, confidenct thresholds, etc. 

I'm still investigating which JavaScript and TypeScript language features the extractor is intended to recognize. The existing tests rely on code snippets rather than plain-language descriptions, so I need to trace how SkillExtractor detects language-specific syntax before implementing a fix.