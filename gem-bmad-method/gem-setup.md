# BMad Method - Gem setup

## Create it
1. Open gemini.google.com, go to Gems, and choose New Gem.
2. Name: BMad Method
3. Description: Expert guide to the BMad Method: which skill to run, in what order, and what each one produces.
4. Instructions: paste the full contents of `gem-instructions.md`.
5. Knowledge: upload every file listed below.
6. Save, then run the test prompts.

## Knowledge files
| File | Size | Tokens (est.) | Contains |
|---|---|---|---|
| knowledge/bmad-method-knowledge.txt | 1.8 MB (1,821,157 bytes) | 431,934 | English docs, all 30 skill folders (SKILL.md, assets, references, scripts), README; 308 files |

Total: 1 of 10 files, about 432k tokens.

## Source
- Repository: https://github.com/bmad-code-org/BMAD-METHOD
- Branch or commit: `main` at `4f61d4e769e50bc11d0d5d724f48942aac699679`
- Scope: `docs/**`, `skills/**`, `README.md`
- Excluded: translated docs (`docs/cs`, `docs/fr`, `docs/ko-kr`, `docs/vi-vn`, `docs/zh-cn`), tests, `*.html`, `*.excalidraw`; and, by not being included, `docs-site/`, `tools/`, `CHANGELOG.md`
- Bundled on: 2026-10-02

The whole repository is 596 files and about 1.18M tokens, which is over the Gem context window. The scope above brings it to 432k.

## Test prompts
1. "What does BMad mean by a well-defined intent?" (definition; `docs/plan/choose-a-planning-path.md`)
2. "How do I install BMad in my project and check that the install worked?" (how-to; `docs/start/install-bmad.md`)
3. "Draft a BMad spec for adding rate limiting to a public REST API, using the spec template." (artifact; `skills/bmad-spec/assets/spec-template.md`)
4. "I have an epic-sized feature in a codebase I inherited. Which skills do I run, in what order, and where does each output land?" (combines `docs/existing-codebases/start-in-an-existing-codebase.md` and `docs/plan/choose-a-planning-path.md`)
5. "Walk me through the Unity workflow in BMad Game Dev Studio." (outside the knowledge: that module lives in another repository, so the Gem should say so and label anything it adds as general knowledge)

## Refresh
From the root of this repository (the folder that contains `gem-bmad-method/`), run this, re-upload the knowledge file, and update the date and commit in the instructions:

    bunx repomix --remote bmad-code-org/BMAD-METHOD --remote-branch main --include "docs/**,skills/**,README.md" -i "docs/cs/**,docs/fr/**,docs/ko-kr/**,docs/vi-vn/**,docs/zh-cn/**,**/tests/**,**/*.test.*,**/*.html,**/*.excalidraw" --style markdown --token-count-tree 10000 -o gem-bmad-method/knowledge/bmad-method-knowledge.txt

Then record the new commit:

    git ls-remote https://github.com/bmad-code-org/BMAD-METHOD main
