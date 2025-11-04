# 🛠 Submodules Workflow — Development Branches

This document explains **how to work with submodules in a monorepo** using a `development` branch workflow. It covers creating branches, making commits, and updating the monorepo to point to the latest submodule commits.

---

## 📁 Monorepo Structure Example

```
healthcare-platform/
├── backend/        → Backend service (Python)
├── middleware/     → Middleware / API Gateway
├── frontend/       → Frontend application (Vue 3)
└── README.md
```

Each folder is a **Git submodule** pointing to its own repository.

---

## 🔹 1. Switch to the development branch in a submodule

```bash
cd backend              # Enter the submodule
git fetch origin        # Fetch latest changes from remote
git checkout development
git pull origin development
```

* If the `development` branch does not exist yet:

```bash
git checkout -b development
git push -u origin development
```

> Repeat for `middleware` and `frontend` as needed.

---

## 🔹 2. Work normally in the submodule

Edit files, add features, or fix bugs:

```bash
# Make changes
git status              # See modified files
git add .
git commit -m "Implement feature X"
git push origin development
```

> All commits should be done **inside the submodule**, not in the monorepo root.

---

## 🔹 3. Update the monorepo to point to the latest submodule commit

After pushing commits in the submodule, go back to the monorepo root:

```bash
cd ..                   # Back to monorepo
git status
```

You will see:

```
modified: backend (new commits)
```

Update the monorepo reference:

```bash
git add backend
git commit -m "Update backend submodule to latest development commit"
git push origin main    # or another branch of the monorepo
```

> This ensures the monorepo tracks the **specific commit** of the submodule you want.

---

## 🔹 4. Recommended workflow summary

1. Enter submodule: `cd <submodule>`
2. Switch or create development branch: `git checkout development`
3. Make changes and commit: `git add . && git commit -m "msg"`
4. Push to remote submodule: `git push origin development`
5. Go back to monorepo root: `cd ..`
6. Update submodule reference: `git add <submodule> && git commit -m "Update <submodule> reference"`
7. Push monorepo changes: `git push origin main` (or development)

---

## 🔹 5. Keep all submodules updated

To fetch and update **all submodules** to the latest `development` branch:

```bash
git submodule foreach git fetch origin
git submodule foreach git checkout development
git submodule foreach git pull origin development
```

---

## 🔹 6. Optional: Development branch in monorepo

You can create a `development` branch in the **monorepo** to reflect the latest development submodules:

```bash
git checkout -b development
git add backend middleware frontend
git commit -m "Initialize development branch with latest submodule commits"
git push -u origin development
```

> This branch allows you to integrate submodule development without affecting the main branch.

---

## ⚡ Tips

* Always **commit changes in the submodule first**, then update the monorepo reference.
* Each submodule can maintain independent branches (`main`, `development`, `feature/*`).
* When cloning the monorepo:

```bash
git clone --recurse-submodules <monorepo-url>
```

* To initialize or update submodules later:

```bash
git submodule update --init --recursive
```

---

This ensures a **clean and reproducible workflow** for monorepos using multiple submodules and development branches.
