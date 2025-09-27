# Workflow Dispatch Inputs

GitHub Actions allows you to manually trigger workflows using `workflow_dispatch` and accept user inputs through a web interface.

---

## 1️⃣ Basic Syntax

```yaml
on:
  workflow_dispatch:
    inputs:
      input_name:
        description: 'Description shown in UI'
        required: true/false
        default: 'default_value'
        type: string/boolean/choice/environment
```

---

## 2️⃣ Input Types

### A. String Input
```yaml
message:
  description: 'Custom message to display'
  required: false
  default: 'Hello from workflow dispatch!'
  type: string
```

### B. Boolean Input
```yaml
enable_logging:
  description: 'Enable verbose logging'
  required: false
  default: false
  type: boolean
```

### C. Choice Input
```yaml
environment:
  description: 'Environment to deploy to'
  required: true
  default: 'staging'
  type: choice
  options:
    - staging
    - production
    - development
```

### D. Environment Input
```yaml
target_env:
  description: 'Target environment'
  required: true
  type: environment
```

**What is Environment Input?**
- The `environment` type is a special input that provides a dropdown of **GitHub Environments** configured in your repository
- It automatically populates with environments you've set up in Settings → Environments
- Useful for deployment workflows where you want to select which environment to deploy to
- Provides built-in integration with GitHub's environment protection rules and secrets

**Key Features:**
- **Auto-populated**: Shows only environments that exist in your repo
- **Environment Protection**: Respects approval requirements and deployment restrictions
- **Environment Secrets**: Automatically uses secrets scoped to the selected environment
- **Deployment History**: GitHub tracks deployments to the selected environment

**Example Usage:**
```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    environment: ${{ inputs.target_env }}  # Uses the selected environment
    steps:
      - name: Deploy to environment
        run: echo "Deploying to ${{ inputs.target_env }}"
        env:
          API_KEY: ${{ secrets.API_KEY }}  # Uses environment-specific secret
```

---

## 3️⃣ Accessing Inputs

Use the `inputs` context to access input values in your workflow:

```yaml
steps:
  - name: Display inputs
    run: |
      echo "Message: ${{ inputs.message }}"
      echo "Environment: ${{ inputs.environment }}"
      echo "Logging enabled: ${{ inputs.enable_logging }}"
```

---

## 4️⃣ Conditional Logic

You can use inputs for conditional execution:

```yaml
steps:
  - name: Conditional step
    if: inputs.enable_logging == true
    run: echo "Logging is enabled!"
    
  - name: Environment-specific step
    if: inputs.environment == 'production'
    run: echo "Running in production!"
```

---

## 5️⃣ Key Points

- **Manual Trigger**: Only works with `workflow_dispatch` event
- **UI Integration**: Creates a form in GitHub Actions tab
- **Type Safety**: GitHub validates input types
- **Default Values**: Provide sensible defaults for better UX
- **Required Fields**: Mark critical inputs as required

---

## 6️⃣ Use Cases

- **Environment Selection**: Choose deployment target
- **Feature Flags**: Enable/disable certain steps
- **Custom Parameters**: Pass configuration values
- **Debug Mode**: Enable verbose logging
- **Version Selection**: Specify which version to deploy

---