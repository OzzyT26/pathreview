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

## Week 9 — Solution building & PR submission

### Check-in 1 (mid-week)

**Current progress:**
I have researched the differences between TypeScript and JavaScript in order to better understand their distinguishing deatures. I have compared the issue description from GitHub with the existing failing tests in order to understand which tests are elevant, and to better understand the scope of the full issue. I traced inputs through skill_extractor.py in order to identify why detections were failing. I have implemented logic changes that achieve the following:

- The SkillDetector can now recognize .js, .ts, and .tsx filenames in input text.
- The SkillDetector can now recognize the literal word TypeScript in input text.
-The SkillDetector now recognizes common JavaScript and TypeScript syntax.

I also implemented a test for each logic change that I made in skill_extractor.py.  The tests are as follows:
- test_javascript_detection_from_filename_in_text verifies that JavaScript is detected when a .js filename is mentioned directly in the input text.
- test_typescript_detection_from_ts_filename_in_text verifies that TypeScript is detected when a .ts filename is mentioned directly in the input text.
- test_typescript_detection_from_tsx_filename_in_text verifies that a .tsx filename is recognized as TypeScript evidence, while preserving the existing React detection.
- test_typescript_detection_from_language_name_in_text verifies that TypeScript is detected when the literal language name appears in the input text.

I also verified that the existing test_javascript_detection and test_text_with_typescript_files tests now pass. The complete test_skill_extractor.py file currently has 19 passing tests and three pre-existing failures related to database, Docker, and Docker Compose detection.

**Next steps:**
Now that my implementation is complete, my next step is to open my pull request, request peer feedback, and submit for review. I'll also update JOURNAL.md.

**Blockers:**