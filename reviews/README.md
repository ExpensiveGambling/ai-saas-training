Review Gate records live here

CheckPoint 1 Claude Evaluation  
I can confirm your repo is live and matches what you described — 4 commits, `app/`, `public/`, `reviews/`, `SETUP.md`, and a working deployment at `ai-saas-training.vercel.app`. I couldn't pull the exact commit hashes (GitHub blocks that page from automated fetches), so one quick note before the review:

**On "Relevant commits / diff":** this just means point the reviewer at *which* commits contain the work being reviewed, so they don't have to guess. You get commit hashes by running `git log --oneline` in your terminal — it'll list something like `a3f9c1e Add setup notes and reviews folder`. You'd paste 2-3 of those short hashes (the `a3f9c1e` part) into that field. Since your repo is public, you could also just paste the commits link: `https://github.com/ExpensiveGambling/ai-saas-training/commits/master`

---

**Review — Checkpoint 1, Wave 1**

**Correct + why:**  
Your mental model of the loop is accurate: local edit → `git add` (stage) → `git commit` (snapshot) → `git push` (send to GitHub) → Vercel detects the push and redeploys → live on the internet. That's genuinely the core loop the rest of the program builds on, and you can explain *why* each step exists, not just the command syntax.

**Issues by severity:**

- *Minor:* You didn't include commit hashes/diff, likely because you weren't sure how to get them (addressed above — not a real gap, just an unfamiliar step).
- *Minor:* No mention of *why* Vercel is watching GitHub specifically (a webhook GitHub sends to Vercel on push) — not required understanding yet, but worth knowing exists.

**Misunderstandings in your explanation:**  
None, actually — your explanation is correct. The one thing to flag isn't a misunderstanding but a gap you already flagged yourself: syncing this repo to a second machine (Mac) isn't magic, it's `git clone` on the new machine (downloads the whole repo + history), then the same add/commit/push loop from there. Since it's on GitHub, any machine with git and your credentials can pull it down.

**1-2 questions to answer:**

1. If you made an edit on a *second* machine (say, Mac) before pushing, and your Windows machine also had local uncommitted changes — what command would you need to run on Windows to get the Mac's changes, and what might happen if both machines edited the same line? (wasn't sure)
2. What does Vercel actually do differently for a "preview" deploy vs. what you've been doing (pushing straight to `master`)?

**Verdict: Pass**

Solid grasp of the core loop for a first checkpoint. The multi-machine sync question is exactly the kind of thing to let yourself get confused by when you hit it, as you already planned.  
  
  
  
Review — Checkpoint 2, Wave 1

**Repo:** [https://github.com/ExpensiveGambling/ai-saas-training](https://github.com/ExpensiveGambling/ai-saas-training)

**Prompt covered:** what a branch is, what a diff shows, the difference between `restore` and `revert`, and what was checked in Claude Code's diff before accepting it.

---

## Correct + why

- **Branch explanation** — accurate and well-illustrated. The `c1 → c2` (branch) vs. `c1 → c3` (master) example correctly shows that a branch does *not* inherit changes made to `main` after the branch point.
- **Claude Code diff review** — correct and shows real review happened, not a skim: confirmed `git diff --staged` showed only one new file (`app/about/page.tsx`) and nothing else touched.

## Issues by severity

**Major —** `restore` **and** `revert` **were swapped in the original explanation.**


|                       | What it does                                                                            | Creates a commit?                                                          | Original history?                                                                                                             |
| --------------------- | --------------------------------------------------------------------------------------- | -------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| `git restore <file>`  | Reverts an **uncommitted** change in the working directory back to the last commit      | **No** — nothing enters history, since nothing was committed to begin with | N/A — there was never a bad commit                                                                                            |
| `git revert <commit>` | Undoes an **already-committed** change by adding a new commit with the opposite changes | **Yes** — one new commit                                                   | The original bad commit is **not deleted** — it stays visible in `git log` forever; the revert commit just cancels its effect |


**Corrected example:** if `c2` was already committed and you want to undo it while keeping history honest, that's `revert` (adds a new undoing commit; `c2` still shows in `git log`). If the bad edit was made but never committed, that's `restore` (discards the edit, nothing ever shows in history because nothing was ever committed).

## Misunderstandings

Just the one above — the *mechanism* of each command was inverted. The instinct that they're "two different undo tools for two different situations" was correct; it was specifically *which does what* that was swapped.

## Follow-up Q&A (from corrections)

**Q1: If you commit a bad change and then** `git revert` **it, how many new commits show in** `git log`**?** → **1 new commit**, not 2. The bad commit (`c2`) already existed *before* the revert — it's not new. `revert` adds exactly one additional commit on top (e.g. `Revert "added file"`) that cancels `c2`'s changes. `git log --oneline` will show both `c2` (untouched, original) and the new revert commit — but only the revert commit is newly *created* by the revert action.

**Q2: What happens if you try** `git restore` **on a file whose bad change is already committed?** → **Correct as answered: nothing happens.** `restore` only reaches back to the last *commit* — if the bad change is already the last commit, there's nothing for `restore` to "restore back to" that's different from current state. `revert` is the right tool once something's committed.

## Clarification: why use `restore` instead of just `git pull` for a bad local edit?

`git pull` *can* sometimes overwrite a bad local file as a side effect, but it's the wrong tool for this — `restore` is the scalpel, `pull` is a full repo sync with baggage:

1. `pull` **requires the good version to already be pushed to GitHub.** If the bad edit happened before ever pushing the good version, GitHub may have nothing clean to pull back to.
2. `pull` **affects the whole branch, not one file.** Other legitimate uncommitted changes to *different* files can cause conflicts or get blocked. `restore <file>` touches only the one file specified.
3. `pull` **needs internet;** `restore` **doesn't.** `restore` works entirely from local `.git` history, instantly.
4. `pull` **can bring in unwanted changes.** If anything new was pushed elsewhere (another machine, a teammate), `pull` grabs that too — scope creep you didn't ask for.

**Mental model:** `restore` = surgical, one file, no network, no side effects. `pull` = full two-repo sync; fixing a bad file is incidental to its real purpose.

---

## Verdict: **Pass-with-fixes**

Branch understanding and Claude Code review discipline were both solid — the harder parts of this checkpoint. The `restore`/`revert` swap has now been corrected and re-explained; the follow-up Q&A confirms the corrected model holds up under further questioning.