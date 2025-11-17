# Git Repository Information

## Repository Details

**GitHub Repository:**
- **URL:** `https://github.com/abhii-01/syllabus-portal.git`
- **Owner:** `abhii-01`
- **Repository Name:** `syllabus-portal`

**Local Path:**
- `/Users/aadarsh/Documents/code/syllabus-portal`

## Git Configuration

**User Details:**
- **Username:** `abhii-01`
- **Email:** `85636902+abhii-01@users.noreply.github.com`

**Remote:**
- **Name:** `origin`
- **Fetch URL:** `https://github.com/abhii-01/syllabus-portal.git`
- **Push URL:** `https://github.com/abhii-01/syllabus-portal.git`

## Branch Structure

**Main Branch:**
- `main` - Production branch, deployed to Vercel

**Current Active Branch:**
- `new-feature` - Currently checked out

**Remote Branches:**
- `origin/main`
- `origin/HEAD` → `origin/main`

## Workflow

### Creating a New Feature Branch
```bash
git checkout main
git pull origin main
git checkout -b feature-name
```

### Committing Changes
```bash
git add .
git commit -m "Descriptive commit message"
git push origin feature-name
```

### Merging to Main
```bash
git checkout main
git merge feature-name
git push origin main
git branch -d feature-name
git push origin --delete feature-name
```

## Deployment

**Platform:** Vercel
- Automatically deploys `main` branch to production
- Creates preview deployments for feature branches
- Environment variables required:
  - `NEXT_PUBLIC_SUPABASE_URL`
  - `NEXT_PUBLIC_SUPABASE_ANON_KEY`
  - `SUPABASE_SERVICE_ROLE_KEY`

## Previous Branches (Merged & Deleted)

1. `feature/new-feature` - Initial attempt tracking feature
2. `filter_fixing` - Fixed server-side filters and added cascading
3. `darkmode_cosmetics1` - Dark mode theme and iPad two-column layout

## Clone Command

To clone this repository in another location:
```bash
git clone https://github.com/abhii-01/syllabus-portal.git
cd syllabus-portal
```

## Important Notes

- Always create feature branches from `main`
- Test features on Vercel preview deployments before merging
- Keep `main` branch stable and deployable
- Delete feature branches after merging to keep repo clean
- Use descriptive commit messages
- Required permissions for commits: `["all"]` (bypasses pre-commit hooks)

---

**Last Updated:** 2025-11-17
**Current Branch:** `new-feature`

