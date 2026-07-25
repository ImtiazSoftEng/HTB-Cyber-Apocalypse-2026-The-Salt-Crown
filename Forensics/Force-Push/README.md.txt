# 🔍 Force Push

> **Category:** Forensics (Git Forensics)  
> **Platform:** Hack The Box – Cyber Apocalypse CTF 2026: The Salt Crown

---

# 📖 Scenario

A production deployment repository was recovered from a leaked backup. According to the investigation, a developer accidentally committed sensitive production credentials into the Git repository while troubleshooting a deployment issue. The mistake was later removed by rewriting the Git history, making the repository appear clean.

The objective was to recover the deleted credentials by investigating Git's internal object database.

---

# 🎯 Objective

Recover the sensitive information that was accidentally committed and later removed from the visible Git history.

---

# 🛠️ Tools Used

- Git
- Git Bash / Command Prompt

---

# 📂 Evidence

The challenge provided a complete Git repository containing:

```text
.git
.github
crownspire/
docs/
examples/
scripts/
tests/
```

The `.git` directory contained the repository history and internal Git objects required for the investigation.

---

# 🔬 Investigation Steps

## Step 1 – Verify the Repository

First, confirm that the downloaded files are a valid Git repository.

```bash
git status
```

This confirmed that the repository was clean and ready for analysis.

---

## Step 2 – Review the Visible History

Display the commit history.

```bash
git log --oneline --graph --all
```

The visible history appeared normal and contained no obvious secrets.

This suggested that the sensitive commit had already been removed.

---

## Step 3 – Search for Hidden Git Objects

Inspect the internal Git database for objects that are no longer part of the visible branch history.

```bash
git fsck --full --no-reflogs --unreachable
```

This command revealed an **unreachable commit**, indicating that an older commit still existed inside Git even though it was no longer referenced by the current branch.

---

## Step 4 – Inspect the Hidden Commit

Open the unreachable commit.

```bash
git show <commit_hash>
```

The hidden commit contained a credentials file that had been committed temporarily during debugging.

The commit message indicated that the file was meant to be removed after testing.

---

## Step 5 – Recover the Deleted File

The credentials file was recovered from the unreachable commit, allowing the investigation to identify the sensitive information that had been exposed.

---

# 🧠 What Happened?

A developer temporarily added a credentials file to troubleshoot a deployment issue.

After debugging, the commit was removed using rewritten Git history (force push). Although the latest repository appeared clean, the deleted commit still existed inside Git's object database.

By examining unreachable Git objects, the deleted credentials were successfully recovered.

---

# 📚 What I Learned

- Git stores commits, trees, and blobs inside the `.git` directory.
- Removing a commit from the current branch does not always delete it permanently.
- Force-pushed or rewritten history can leave unreachable Git objects behind.
- `git fsck` is useful for discovering hidden commits and orphaned objects.
- `git show` can recover deleted files from old commits.
- Sensitive credentials should never be committed to source control.
- If credentials are exposed, they should be revoked and replaced immediately.

---

# 💻 Commands Used

```bash
git status
git log --oneline --graph --all
git fsck --full --no-reflogs --unreachable
git show <commit_hash>
```

---

# 📷 Screenshots

Add your investigation screenshots here:

- Git commit history
- Unreachable commit discovery (`git fsck`)
- Hidden commit analysis (`git show`)

---

# ⚠️ Disclaimer

This repository documents my personal learning process while solving a Hack The Box challenge.

To respect Hack The Box's platform rules, challenge flags, secrets, and complete active solutions are intentionally omitted.