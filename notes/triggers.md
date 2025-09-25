# GitHub Actions Triggers

GitHub Actions **triggers** are events that start workflows automatically. They determine **when and how workflows run**.

---

## 1️⃣ Types of Triggers

### A. Event-Based

* Triggered by GitHub repo events like:

  * `push` → commits pushed
  * `pull_request` → PR opened/updated
  * `release` → release published
* Can filter by **branch, tag, or path**.

**Example:**

```yaml
on:
  push:
    branches: [main]
```

### B. Schedule-Based

* Trigger workflows at specific times using **cron syntax**.

```yaml
on:
  schedule:
    - cron: "0 0 * * *"  # daily at midnight
```

### C. Manual (workflow\_dispatch)

* Trigger manually from GitHub UI; can accept inputs.

```yaml
on:
  workflow_dispatch
```

### D. Repository Dispatch / Webhooks

* Trigger workflows via **external API calls**.

---

## 2️⃣ Key Points

* A workflow can have **multiple triggers**.
* Proper trigger configuration avoids **unnecessary runs**.
* Event triggers = CI/CD automation; manual/scheduled triggers = on-demand tasks.

---


