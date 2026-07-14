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