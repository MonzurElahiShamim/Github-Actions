# Environment Variables in GitHub Actions

Environment variables allow you to store and use data throughout your GitHub Actions workflows.

---

## 🎯 Three Levels of Environment Variables

### 1. Workflow Level
```yaml
env:
  GLOBAL_VAR: "Available everywhere"
  BUILD_NUMBER: ${{ github.run_number }}
```
- Available to **all jobs** in the workflow
- Defined at the top level of the workflow file

### 2. Job Level
```yaml
jobs:
  my-job:
    env:
      JOB_VAR: "Available in this job only"
```
- Available to **all steps** within that specific job
- Defined under a specific job

### 3. Step Level
```yaml
steps:
  - name: My step
    env:
      STEP_VAR: "Only available in this step"
    run: echo $STEP_VAR
```
- Available **only within that specific step**
- Defined under a specific step

---

## 🏗️ Built-in GitHub Variables

GitHub automatically provides many useful environment variables:

| Variable | Description |
|----------|-------------|
| `GITHUB_REPOSITORY` | Repository name (owner/repo) |
| `GITHUB_REF` | Branch or tag that triggered the workflow |
| `GITHUB_ACTOR` | Username of the person who triggered the workflow |
| `GITHUB_RUN_NUMBER` | Unique number for each workflow run |
| `GITHUB_WORKSPACE` | Path to the runner's workspace |
| `GITHUB_SHA` | Commit SHA that triggered the workflow |

---

## 📝 Using Environment Variables

### In Shell Commands
```bash
echo "Repository: $GITHUB_REPOSITORY"
echo "My variable: $MY_VAR"
```

### In Workflow Expressions
```yaml
run: echo "Build ${{ env.BUILD_NUMBER }}"
```

### Setting Dynamic Variables
```yaml
- name: Set variable for later steps
  run: echo "MY_VAR=some_value" >> $GITHUB_ENV

- name: Use the variable
  run: echo "Value: $MY_VAR"
```

---

## ⚡ Key Points

- **Precedence**: Step > Job > Workflow (closer scope wins)
- **Built-in variables** start with `GITHUB_`
- Use `$GITHUB_ENV` to set variables dynamically
- Environment variables are **strings** only
- Variables set in one job are **not available** in other jobs

---

## 🔒 Environment Variables vs Secrets

| Environment Variables | Secrets |
|----------------------|---------|
| Visible in logs | Masked in logs |
| Good for non-sensitive data | Good for sensitive data |
| `env:` block | `secrets:` context |
| Example: API URLs | Example: API keys |

---