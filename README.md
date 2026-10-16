# Repository Guide – Working with Submodules

---

## 📦 Overview
This repository uses **Git submodules** to include external projects as part of the codebase. Submodules allow us to keep dependencies in their own Git repositories while still version‑controlling them alongside the main repo.

---

## 🛠️ Clone the Repository
You have two straightforward options:

```bash
# 1️⃣ Clone with submodules in one step
git clone --recursive https://github.com/dan-tran-van/ai-builder-cup.git

# 2️⃣ Clone first, then initialize submodules
git clone https://github.com/dan-tran-van/ai-builder-cup.git
cd ai-builder-cup
git submodule update --init --recursive
```

Both commands result in a fully working checkout where each submodule is checked out at the commit recorded in the parent repo.

---

## 🔄 Keeping Submodules Up‑to‑Date
When the upstream of a submodule changes, bring those updates into your local copy:

```bash
# Pull the latest changes for the super‑project
git pull

# Update all submodules to the commits referenced by the super‑project
git submodule update --remote --merge
```

If you want to update a **single** submodule:

```bash
cd path/to/submodule
git fetch origin
git checkout <desired‑branch-or‑commit>
cd -   # back to the root repo
git add path/to/submodule   # record the new commit reference
git commit -m "Update submodule <name> to <commit>"
```

---

## ✍️ Making Changes Inside a Submodule
1. **Enter the submodule directory**.
2. Create a feature branch, commit, and push *within the submodule*.
3. Return to the parent repo, stage the submodule path, and commit the new submodule SHA.

```bash
cd path/to/submodule
git checkout -b feature/my‑new‑thing
# … make changes …
git commit -am "Add awesome feature"
git push origin feature/my‑new‑thing

# Back to the top‑level repo
cd -
git add path/to/submodule
git commit -m "Update submodule to include new feature"
```

When you push the parent repo, reviewers will see the updated submodule reference and can pull the submodule changes automatically.

---

## 🧹 Removing or Adding Submodules
### Add a new submodule
```bash
git submodule add https://github.com/owner/new‑library.git external/new‑library
git commit -m "Add new‑library submodule"
```

### Remove an existing submodule
```bash
# 1️⃣ Remove the entry from .gitmodules
git config -f .gitmodules --remove-section submodule.path/to/submodule

# 2️⃣ Unstage the submodule directory
git rm -r path/to/submodule

# 3️⃣ Delete the leftover git data
rm -rf .git/modules/path/to/submodule

git commit -m "Remove submodule path/to/submodule"
```

---

## 📚 Helpful One‑Liners
| Action | Command |
|--------|---------|
| List submodules and their current commits | `git submodule status` |
| Show submodule URLs | `git config --file .gitmodules --get-regexp url` |
| Re‑initialize all submodules (useful after a fresh clone) | `git submodule update --init --recursive` |
| Pull latest changes for all submodules (tracking their remote branches) | `git submodule foreach 'git pull'` |

---

## ❓ FAQ
**Q:** *Do I need to commit submodule changes separately?*  
**A:** Yes. Changes inside a submodule are its own Git history. After committing there, you must also commit the *updated SHA* in the parent repo.

**Q:** *Why does `git status` sometimes show “modified content” in a submodule?*  
**A:** That indicates the submodule pointer (the commit SHA) differs from what the parent repo expects. Stage the submodule path to record the new reference.

---

💡 **Tip:** Keep submodule branches in sync with the parent repo’s workflow (e.g., use `main` or a dedicated `stable` branch) to avoid surprising detaches.

---

*Happy coding!*