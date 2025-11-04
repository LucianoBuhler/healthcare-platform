# 🛠 Submodules Workflow — Development Branches

This document explains **how to work with submodules in the Healthcare Platform monorepo** using a `development` branch workflow.  
It covers creating branches, making commits, and updating the monorepo to point to the latest submodule commits.

---

## 📁 Monorepo Structure Example

```

healthcare-platform/
├── backend/        → Backend service (Python)
├── middleware/     → Middleware / API Gateway
├── frontend/       → Frontend application (Vue 3)
├── db-layer/       → Database Layer (models, migrations, DB access)
└── ingestion/      → Data Ingestion Layer (ETL, pipelines)

````

Each folder is a **Git submodule** pointing to its own repository.

---

## 🔹 1. Switch to the development branch in a submodule

```bash
cd backend              # Enter the submodule (example)
git fetch origin        # Fetch latest changes from remote
git checkout development
git pull origin development
````

* If the `development` branch does not exist yet:

```bash
git checkout -b development
git push -u origin development
```

> Repeat for:
> `middleware`, `frontend`, `db-layer`, and `ingestion`.

---

## 🔹 2. Work normally in the submodule

Make your changes, test, and commit as usual:

```bash
git status              # Check modified files
git add .
git commit -m "Implement feature X"
git push origin development
```

> ✅ All commits should be made **inside the submodule**, not in the monorepo root.

---

## 🔹 3. Update the monorepo to point to the latest submodule commit

After pushing commits in the submodule, return to the monorepo root:

```bash
cd ..                   # Back to monorepo
git status
```

You should see something like:

```
modified: backend (new commits)
```

Now update the reference:

```bash
git add backend
git commit -m "Update backend submodule to latest development commit"
git push origin main    # or 'development', depending on your branch strategy
```

> This records the **exact commit hash** of each submodule inside the monorepo.

---

## 🔹 4. Recommended Workflow Summary

1. Enter submodule:
   `cd <submodule>`
2. Switch or create `development` branch:
   `git checkout development`
3. Make changes and commit:
   `git add . && git commit -m "message"`
4. Push to remote submodule:
   `git push origin development`
5. Go back to monorepo root:
   `cd ..`
6. Update submodule reference:
   `git add <submodule> && git commit -m "Update <submodule> reference"`
7. Push monorepo changes:
   `git push origin main` (or `development`)

---

## 🔹 5. Keep All Submodules Updated

To fetch and pull **all submodules** on their `development` branches:

```bash
git submodule foreach git fetch origin
git submodule foreach git checkout development
git submodule foreach git pull origin development
```

> Ensures all five submodules are up to date and synchronized.

---

## 🔹 6. Optional: Development Branch in Monorepo

You can create a `development` branch in the **monorepo** to track ongoing development across all submodules:

```bash
git checkout -b development
git add backend middleware frontend db-layer ingestion
git commit -m "Initialize development branch with latest submodule commits"
git push -u origin development
```

> This branch allows you to integrate and test new features across all layers before merging into `main`.

---

## ⚡ Tips

* Always **commit changes in the submodule first**, then update the monorepo reference.
* Each submodule can maintain its own branches (`main`, `development`, `feature/*`).
* When cloning the monorepo for the first time:

```bash
git clone --recurse-submodules <monorepo-url>
```

* If already cloned without submodules:

```bash
git submodule update --init --recursive
```

* To update all submodules to their latest commit on tracked branches:

```bash
git submodule update --remote --merge
```

---

This ensures a **clean, modular, and reproducible workflow** for multi-repository development within the Healthcare Platform monorepo.
