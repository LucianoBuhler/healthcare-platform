# 🏥 Healthcare Platform — Monorepo

This is a monorepo for a healthcare data platform, containing **backend**, **middleware (API Gateway)**, and **frontend** as separate submodules.  
Each service is versioned independently and can have its own CI/CD pipeline.  

---

## 📁 Project Structure

```

healthcare-platform/
├── backend/        → Backend service (Python, data processing, EDA)
├── middleware/     → Middleware / API Gateway service
├── frontend/       → Frontend application (Vue 3)
└── README.md       → This file

````

> ⚠️ Each folder is a **Git submodule**, pointing to its own repository.

---

## ⚙️ Initial Setup

### 1. Clone the monorepo including submodules

```bash
git clone --recurse-submodules https://github.com/<user>/healthcare-platform.git
cd healthcare-platform
````

If you already cloned without `--recurse-submodules`:

```bash
git submodule update --init --recursive
```

---

### 2. Initialize and update submodules

```bash
# Pull latest commits from each submodule
git submodule update --remote --merge
```

> This ensures all submodules are on their latest commits and properly initialized.

---

### 3. Enter a submodule to work on a service

```bash
cd backend
# Work normally inside this repository
git status
git pull origin main
```

Repeat for `middleware` and `frontend` as needed.

---

## 🚀 Running the backend service (example)

```bash
cd backend
python -m venv venv
source venv/bin/activate    # macOS/Linux
venv\Scripts\activate       # Windows
pip install -r requirements.txt

# Generate dataset subset
python -m src.generate_subset

# Run analysis and generate EDA report
python -m src.analyse
```

> Reports and outputs will be saved inside the backend's `reports/` folder. Each report gets a unique timestamped filename.

---

## 🔧 Updating submodules

To pull the latest changes from all submodules:

```bash
git submodule foreach git pull origin main
```

---

## 🧩 Adding a new submodule

```bash
git submodule add <repository-url> <path>
git add .gitmodules <path>
git commit -m "Add <service> submodule"
git push
```

---

## ⚠️ Common Issues

| Issue                             | Cause                                    | Solution                                               |
| --------------------------------- | ---------------------------------------- | ------------------------------------------------------ |
| `No url found for submodule path` | Submodule not properly added             | Re-add submodule with `git submodule add <url> <path>` |
| Submodule folder is empty         | No commits in submodule remote           | Make sure the submodule repo has at least one commit   |
| Reports not generated             | Backend environment not set up correctly | Activate Python virtualenv and install dependencies    |

---

## 📜 License

MIT License — free for personal and commercial use.

---

## 🧠 Notes

* This monorepo allows **independent development** of each service.
* Each submodule can have its own dependencies, environment variables, and CI/CD workflow.
* Recommended workflow: make changes inside the submodule, commit there, then update the main monorepo reference.
