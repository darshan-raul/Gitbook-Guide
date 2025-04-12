# cherry picking

**What is Git cherry-pick?**

* **Selective Commit Transfer:** `git cherry-pick` allows you to pick a specific commit (or multiple commits) from one branch and apply it onto another branch. This creates a new commit on the target branch with the same changes as the cherry-picked commit.

**How it Works (Under the Hood)**

1. **Find the Commit:** You identify the commit (usually by its SHA-1 hash) that you want to cherry-pick.
2. **Calculate Diff:** Git calculates the changes (diff) introduced by the chosen commit.
3. **Apply Changes:** Git tries to apply that diff onto the current branch (the target branch).
4. **New Commit:** If applying the changes is successful, a new commit is created on the target branch. This new commit has the same changes and commit message as the original commit, but a different parent and SHA-1 hash.

**When to Use Cherry-Pick**

1. **Isolating Changes:** When you need only a few specific commits from another branch, rather than merging the entire branch's history.
2. **Bug Fixes on Release Branches:** Let's say you find a critical bug fix on the `develop` branch, and you need to urgently apply it to an older release branch. Cherry-picking the fix is ideal.
3. **Undoing Work in Progress:** Realize some commits on a feature branch aren't going in the right direction? Cherry-pick the good ones onto a new branch and abandon the old one.
4. **"Undoing" Mistakes:** Accidentally committed to the wrong branch? Cherry-pick the commit to the correct branch and then remove it from the original branch.

**Cautions**

* **Potential Conflicts:** If the cherry-picked changes overlap with existing modifications on the target branch, you might have to resolve merge conflicts.
* **Duplicate Commits:** Be aware that cherry-picking creates new, almost-duplicate commits. This can make the commit history a bit harder to follow if used excessively.

**Example**

You have a `feature-A` branch and you want one commit from it on your `main` branch:

1. `git checkout main` (Switch to the target branch)
2. `git cherry-pick <commit-hash>` (Replace `<commit-hash>` with the actual commit identifier)

**Tips**

* **Cherry-pick a Range:** You can cherry-pick a range of commits: `git cherry-pick <oldest-commit>..<newest-commit>`
* **Inspect Before Committing:** Add the `-n` flag to see the changes _before_ committing: `git cherry-pick -n <commit-hash>`

***

## practical scenario



**The Scenario:**

* You're working on a new feature in a branch called `new-widget`.
* While developing, you realize a small change you made two commits ago would be immensely useful on your `main` branch to fix a minor bug that users are reporting.
* You don't want to merge the whole `new-widget` branch into `main` yet, as the feature isn't complete.

**Solution using `git cherry-pick`**

1. **Identify the Commit:**
   * Use `git log --oneline new-widget` to list the commits on your feature branch.
   * Find the specific commit containing the fix you need and note its hash (e.g., `abcdef12`).
2. **Switch to the 'main' Branch:**
   * `git checkout main`
3. **Cherry-Pick the Commit:**
   * `git cherry-pick abcdef12`
4. **Resolve Conflicts (if necessary):**
   * If the changes from the cherry-picked commit conflict with current `main` branch changes, you'll need to resolve those conflicts manually.
5. **Verify and Commit:**
   * If everything looks good, proceed as with a normal commit, providing a meaningful commit message if the cherry-picked one isn't clear enough.

**Outcome**

* The critical fix is now on your `main` branch, likely deployed to address the user-reported bug.
* Your `new-widget` feature development can continue uninterrupted.
* You avoided a premature merge of your incomplete feature into `main`.

**Let's Add a Twist: Undoing a Mistake**

Imagine you later realize that a commit on your `new-widget` branch introduced an accidental problem. You need to get rid of it:

1. **Cherry-pick the Revert Commit:** Find the commit where you _fixed_ the accidental change, and cherry-pick that onto the `new-widget` branch. This neatly 'cancels out' the bad change.

**Why Not Just Merge?**

Merging the entire `new-widget` branch would bring in all its changes, not just the one needed fix. This could break the `main` branch or introduce unwanted side effects. `cherry-pick` provides surgical precision.

Let me know if you'd like to walk through another scenario, or explore more complex cherry-picking with multiple commits or conflict resolution!
