# SPHEREx TimeLens — Development Setup Guide

This guide details the planned local development workflow, environment prerequisites, dependency management, testing strategy, and collaboration procedures for **SPHEREx TimeLens** on Windows (compatible with 16 GB RAM laptops) and Unix-like environments.

---

## 1. Prerequisites

Before setting up the repository, ensure the following core tools are installed on your workstation:

* **Operating System**: Windows 10/11 (PowerShell or Bash), macOS, or Linux.
* **Git**: Version 2.30 or newer (`git --version`).
* **Python**: Version 3.10, 3.11, or 3.12 (`python --version`).
* **Node.js**: Version 18 LTS or 20 LTS with npm (`node --version`, `npm --version`).

---

## 2. Repository Setup & Environment Configuration

### Step 1: Clone the Repository
```powershell
git clone https://github.com/karthikeyan58583-glitch/SPHEREx.git
cd SPHEREx
```

### Step 2: Initialize Configuration
Copy the documented `.env.example` template to `.env`. Never commit `.env` to Git.
```powershell
copy .env.example .env
```

---

## 3. Planned Python Environment Workflow (Backend & ML)

*Note: The commands below represent the planned workflow for Phases 2–5. Dependencies will be installed when Phase 2 begins.*

### Step 1: Create a Project-Specific Virtual Environment
To comply with the rule against global dependency installation, always use an isolated virtual environment:
```powershell
# Create a local virtual environment in the workspace
python -m venv .venv

# Activate the virtual environment (Windows PowerShell)
.\.venv\Scripts\Activate.ps1
```

### Step 2: Proposed Dependency Workflow
When Phase 2 introduces backend requirements, dependencies will be pinned and installed via:
```powershell
# Upgrade pip
python -m pip install --upgrade pip

# Install dependencies from pinned requirements/lockfile (Planned for Phase 2)
# pip install -r backend/requirements.txt
```

---

## 4. Planned Frontend Setup (Phase 6)

*Note: The frontend application will be initialized during Phase 6 using Vite, React, and TypeScript.*

### Proposed npm Workflow
```powershell
# Navigate to the frontend directory
cd frontend

# Install pinned dependencies (Planned for Phase 6)
# npm install

# Start local development server (Planned for Phase 6)
# npm run dev
```

---

## 5. Testing Strategy & Commands

All development phases must maintain test suites. Code cannot be considered complete without passing tests.

### Backend & Scientific Processing Tests (Pytest)
```powershell
# Planned test execution command
# pytest backend/tests/ tests/integration/ -v
```

### Frontend Component Tests (Vitest)
```powershell
# Planned frontend test execution command
# cd frontend && npm run test
```

---

## 6. Git Workflow & Collaboration Rules

To maintain repository hygiene and avoid merge conflicts across team members:

1. **Main Branch Protection**:
   * Do not commit directly to `main` for regular feature development.
   * All feature work must occur on dedicated feature branches (e.g., `feature/phase2-backend-skeleton`, `feature/phase3-irsa-client`).
2. **Commit Hygiene**:
   * Use focused, descriptive commit messages.
   * Verify that no raw FITS files, `.sqlite` databases, or model weights are staged (`git status`) before committing.
3. **Synchronization**:
   * Do not force-push (`git push --force`) or rewrite shared commit history.
   * Coordinate shared API contracts and Pydantic schemas across team members before altering route signatures.
4. **Agent Constraint**:
   * Coding assistants must never commit or push unless explicitly requested by the user.
