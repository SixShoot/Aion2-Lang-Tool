# Contributing & Developer Guide

Thanks for helping finish the i18n work. This guide explains exactly what to do locally in VS Code and how to finish, test, and push the i18n-english-ready branch changes.

Repository: https://github.com/SixShoot/Aion2-Lang-Tool/tree/i18n-english-ready

> Summary
- Branch: i18n-english-ready — work and final commits should go to this branch.
- Goal: finish moving/organizing English localization assets into `en/`, update scripts, test UI, and open a PR to merge into `main`.

---

## Prerequisites (local)
- Git (2.0+)
- GitHub account with push permissions to the repo (or be a collaborator)
- (Recommended) GitHub CLI (`gh`) for PR creation
- PowerShell (Windows) to run the GUI scripts
- (Optional) Git LFS if you plan to track large binaries
- VS Code with these extensions (recommended):
  - PowerShell (ms-vscode.powershell)
  - GitLens
  - GitHub Pull Requests and Issues

---

## Quick local setup (VS Code)
1. Clone repository and checkout the branch:

   git clone https://github.com/SixShoot/Aion2-Lang-Tool.git
   cd Aion2-Lang-Tool
   git fetch origin
   git checkout i18n-english-ready

2. Open the folder in VS Code:

   code .

3. Install recommended extensions if prompted (PowerShell, GitHub PRs, GitLens).

---

## Files & directory layout (what we expect)
- en/ — English localization assets (L10NString.dat, L10NString.json, pakchunk502000-Windows_0_P.pak).
- zh/ and zh-TW/ — Chinese localization sources (existing).
- App.ps1 — GUI PowerShell app (now prefers en/ first, falls back to root or zh/).
- Aion2CNTool.ps1 — CLI helper (language resolution logic updated).

If files are still at repository root (L10NString.dat, pakchunk502000-Windows_0_P.pak), move them into `en/` or upload copies into `en/` instead of deleting root files until the PR is merged.

---

## Recommended workflow (detailed)
1. Make a working branch off `i18n-english-ready` (optional but safe):

   git checkout -b i18n-english-ready/my-edits

2. Make edits in VS Code:
   - Update App.ps1 or Aion2CNTool.ps1 if needed.
   - Add or move English assets into `en/`.
   - Add or update `en/README.txt` and `pl/README.txt` if needed.

3. If you move files that already existed in the repo root, prefer using git mv so history is preserved:

   git mv L10NString.dat en/L10NString.dat
   git mv pakchunk502000-Windows_0_P.pak en/pakchunk502000-Windows_0_P.pak
   git mv L10NString.json en/L10NString.json || true

   If moving fails because the file isn't tracked, just add the files to en/ and remove or ignore the root files later.

4. Stage and commit your changes with clear messages:

   git add .
   git commit -m "Organize English localization into en/ and update App.ps1 i18n fallback"

5. Push your branch to origin:

   git push -u origin i18n-english-ready/my-edits

6. Create a Pull Request:
   - Web: Go to GitHub, switch to your branch and click "Compare & pull request".
   - CLI (gh):

     gh pr create --base main --head SixShoot:i18n-english-ready/my-edits --title "English GUI translation and en/ localization structure" --body-file PR_BODY.md

   Use the PR body template in PR_BODY.md (we prepared a draft in the branch) or copy the suggested PR body from the repo.

---

## Testing the GUI (App.ps1)
1. Ensure the AION 2 game and launcher are closed.
2. Open Windows PowerShell in the repo root (where App.ps1 lives).
3. Run the app with the bypass execution policy:

   powershell -ExecutionPolicy Bypass -File .\App.ps1

4. Actions to test:
   - Click `Rescan` to detect local AION 2 installs.
   - Or `Pick path` to manually choose the game root (the folder that contains the `Aion2` directory).
   - Select a client and accept the disclaimer checkbox to enable the Apply buttons.
   - Click `Apply [Simplified Chinese]` (or Traditional), then verify:
     - Backup folder `Aion2_English_Backup_Safe` is created in the game root.
     - The target path `Aion2\Content\Paks\L10N\Text\en-US` contains the localization files copied from `en/`.
   - Click `Restore [Default English]` and verify restoration from the backup.

5. If you encounter permission or lock errors:
   - Ensure the game process (Aion2, launcher) is not running.
   - Run PowerShell as Administrator (only if needed).

---

## Git LFS notes (binaries)
- The en/ assets are currently present in the branch. If you plan to add larger or more binary files, use Git LFS to avoid bloating the repo.

Install and track types:

   git lfs install
   git lfs track "*.dat" "*.pak" "*.json"
   git add .gitattributes
   git commit -m "Track localisation assets with Git LFS"

If files were already committed without LFS and need to be converted, use the steps below (simple re-add method):

   git rm --cached en/L10NString.dat en/pakchunk502000-Windows_0_P.pak en/L10NString.json
   git add en/L10NString.dat en/pakchunk502000-Windows_0_P.pak en/L10NString.json
   git commit -m "Move localization assets to Git LFS"
   git push origin i18n-english-ready

If you need history rewrite to purge big files from previous commits, ask me and I will provide the recommended sequence (it will rewrite commit history and you will need to force-push).

---

## PR checklist (before requesting review)
- [ ] All required English assets are in `en/` (dat/json/pak) or tracked via LFS.
- [ ] App.ps1 and Aion2CNTool.ps1 correctly prefer `en/` and fall back to repo root/`zh/`.
- [ ] No accidental large files left at repo root unless intended.
- [ ] README.md updated to document the language layout and usage.
- [ ] Added tests or manual test steps documented (this file covers basic tests).
- [ ] Commit history is clean and commit messages descriptive.

---

## Helpful VS Code tips
- Use the PowerShell extension to run and debug scripts inside VS Code (integrated console).
- Use the Git panel to stage/commit and open Pull Requests (GitHub PR extension helps).
- Use the terminal inside VS Code to run `git` and `gh` commands without leaving the editor.

---

If you want, I can:
- Create a PR_BODY.md file in this branch with the full PR description (done if you need it saved in the branch),
- Give the exact `gh pr create` command to run locally (I prepared that too), or
- Walk you through any of the steps live while you run them in VS Code and paste errors here.

Tell me what you want next (I can stay with you while you run the checks).