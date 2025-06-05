---
layout: page
title: Git Branch Management Strategy
permalink: /adr-001-branch-management/
up: ../adl/
---

# ADR-001: Git Branch Management Strategy

**Date:** 2025-06-02
**Status:** Proposed
**Decision Makers:** [Development Team]
**Technical Story:** Standardize Git branch management to improve collaboration

## Context and Problem Statement

Our current branch management process is very simple but has limitations:

**Current situation:**
- Single main branch: `master`
- All features are developed from `master`
- No staging/pre-production environment
- Direct merge to `master` without integrated testing phase

**Identified problems:**
- **Regression risk**: no integration testing phase
- **Difficult to prepare releases** with multiple features
- **No separation** between development and stable code
- **Potential conflicts** during simultaneous merges
- **Lack of feature traceability**
- **No possibility for integration testing** before merge

We need a strategy that introduces an intermediate integration level while maintaining simplicity.

## Decision Drivers

- **Simplicity**: Easy-to-understand and follow strategy
- **Stability**: Ensure stable code on the main branch
- **Collaboration**: Facilitate teamwork
- **Flexibility**: Allow parallel feature development
- **Traceability**: Clear change history
- **Smooth migration**: Natural transition from our current setup

## Considered Options

### Option 1: Maintain Status Quo (Master only)
- Single master branch
- Temporary feature branches
- Direct merge to production

### Option 2: Complete Git Flow
- Branches: main, develop, feature, release, hotfix
- Complex process with numerous branches
- Significant overhead for our team

### Option 3: GitHub Flow
- Single main branch (main)
- Feature branches directly from main
- No integration zone
- Requires continuous deployment to be effective

### Option 4: Main + Develop Strategy (Recommended)
- **Migration from master**: Rename master → main
- **Create develop** from main for integration
- **Feature branches** from develop
- **Preparation for future** evolutions

## Decision Outcome: Main + Develop Strategy

### Votes
- [ ] Pascal
- [ ] julien
- [ ] Karim
- [ ] Bilel
- [ ] Mohammed
- [ ] Nathan
- [ ] Arnaud

**Vote deadline:** 2025-06-09
**Consensus criteria:** Simple majority (3/5)

## Branch Structure

### Main + Develop Strategy (Recommended)

```
BEFORE (current situation):
master
├── feature/xyz (if temporary branches)
└── direct commits/merges

AFTER (target):
main (rename of master - stable code)
├── develop (new integration branch)
    ├── feature/US-123-user-authentication
    ├── feature/US-124-dashboard
    ├── bugfix/US-456-navigation-fix
    └── task/US-789-dependency-updates
```

### Branch Types

| Type | Prefix | Base | Merge to | Example                        | Notes |
|------|--------|------|----------|--------------------------------|-------|
| **Feature** | `feature/` | `develop` | `develop` | `feature/US-123-login-form`    | New features |
| **Bugfix** | `bugfix/` | `develop` | `develop` | `bugfix/US-456-mobile-menu`    | Non-critical fixes |
| **Task** | `task/` | `develop` | `develop` | `task/US-789-update-deps`      | Technical tasks |
| **Hotfix** | `hotfix/` | `main` | `main` + `develop` | `hotfix/critical-security-fix` | Critical fixes |

### Naming Convention

**Format**: `{type}/{ticket-id}-{short-description}`

**Valid examples**:
- `feature/US-123-oauth-authentication`
- `bugfix/US-456-responsive-fix`
- `task/US-789-vue3-migration`
- `hotfix/critical-security-fix`

**Rules**:
- Use hyphens (-) to separate words
- English description, short and descriptive
- Include ticket ID
- No spaces, special characters or accents

## Development Workflow

### 1. Feature Development

```bash
# 1. Update develop
git checkout develop
git pull origin develop

# 2. Create feature branch
git checkout -b feature/US-123-user-dashboard

# 3. Create pull request to develop in Draft mode
Go to GitHub, create PR from `feature/US-123-user-dashboard` to `develop`, then choose Create Draft Pull Request.

# 4. Develop with conventional commits
git commit -m "feat: add dashboard component"
git commit -m "test: add dashboard unit tests"

# 5. Push branch
git push origin feature/US-123-user-dashboard

# 6. Mark the PR as ready for review
```

### 2. Review and Merge Process

**Pull Request to `develop`:**
- ✅ Code review by at least 1 peer
- ✅ A11y tests pass
- ✅ No conflicts
- ✅ Conventional commits respected
- ✅ Rebase merge recommended

### 3. Integration to Main

```bash
# Create PR: develop → main
# Final team review
# Merge after validation
```

### 4. Critical Hotfix

```bash
# 1. Create hotfix from main
git checkout main
git pull origin main
git checkout -b hotfix/critical-security-fix

# 2. Make the fix
git commit -m "fix: correct critical security vulnerability"

# 3. PR to main AND develop
# 4. Priority merge after review
```

## Branch Protection Rules

### `main` Branch
- ❌ Direct push forbidden
- ✅ Pull Request required
- ✅ Required review (2 people)
- ✅ CI tests required (if configured)
- ✅ Up-to-date branch required
- ✅ Only admins can override

### `develop` Branch
- ❌ Direct push forbidden
- ✅ Pull Request required
- ✅ Required review (1 person)
- ✅ CI tests required (if configured)
- ✅ Up-to-date branch required

## Migration Plan

### Phase 1: Master to Main + Develop Migration (Week 1)

**Step 1: Rename master → main**
```bash
# On GitHub: Settings → Branches → Rename master to main
# Or via Git:
git branch -m master main
git push -u origin main
git push origin --delete master

# Update team local repos:
git branch -m master main
git fetch origin
git branch -u origin/main main
```

**Step 2: Create develop from main**
```bash
git checkout main
git pull origin main
git checkout -b develop
git push -u origin develop
```

**Step 3: GitHub Configuration**
- Change default branch: `main` → `develop` (for new PRs)
- Configure protection rules
- Update documentation

### For Technical Team
- [ ] **Rename master → main** on GitHub and locally
- [ ] **Create develop branch** from main
- [ ] **Update branch protection rules**
- [ ] **Configure develop as default branch** for PRs

## Immediate Benefits

Even without configured deployment environments, this strategy provides:

1. **Secure integration zone** with develop
2. **Better collaboration** with clear conventions
3. **Conflict reduction** through structured process
4. **Improved feature traceability**
5. **Future preparation** for project evolution
6. **Increased stability** of code on main

## Success Metrics

### Quantitative Metrics
- **Merge conflict reduction**: Target 70% reduction
- **Conflict resolution time**: 50% reduction

### Success Criteria
- [ ] Successful master to main migration
- [ ] Operational develop branch
- [ ] Team trained and comfortable with process
- [ ] First successful merges via develop → main
- [ ] No major incidents during transition

## Future Evolution

This strategy naturally prepares us for:
- **Staging/production environments**
- **Release automation tools**
- **Complete CI/CD pipeline**
- **Automated deployments**
- **Automated monitoring and rollback**

*This ADR will be updated after usage experience and team feedback.*
