```markdown
# Custom Git Hooks Setup

This document outlines the setup of custom Git hooks for validating commit messages in your repository. Specifically, we will configure a `commit-msg` hook to ensure that commit messages follow a specified format.

## Directory Structure

Your repository should have the following structure:

your-repo/
│
├── hooks/               # Custom hooks directory
│   ├── commit-msg       # Commit message validation hook script
│
└── setup.sh             # Setup script to automate the configuration

### Step 1: Create the Hooks Directory

In the root of your repository, create a directory named `hooks`:

```bash
mkdir hooks
```

### Step 2: Create the `commit-msg` Hook Script

Create the `commit-msg` hook script in the `hooks` directory:

```bash
touch hooks/commit-msg
```

### Hook Script Content

Add the following script to `hooks/commit-msg`:

```bash
#!/bin/bash

# Retrieve the commit message from the file specified as the first argument
commit_msg=$(cat "$1")
echo "Commit message: '$commit_msg'"

# Define a regular expression for the commit message format
commit_regex="^(feat|fix|chore|refactor|docs|test|style|ci|perf|build)\([A-Z]+-[0-9]+\): .+"

# Check if the commit message matches the required format
if [[ ! "$commit_msg" =~ $commit_regex ]]; then
    echo "Error: Invalid commit message format."
    echo "Your commit message must follow the format: TYPE(PROJECT-123): description."
    echo "Valid types: feat, fix, chore, refactor, docs, test, style, ci, perf, build"
    echo "Example: feat(PROJECT-123): add authentication module"
    exit 1
fi

# If the commit message matches, allow the commit
exit 0
```

### Step 3: Make the Hook Script Executable

Make the `commit-msg` script executable by running:

```bash
chmod +x hooks/commit-msg
```

### Step 4: Create a Setup Script (Optional)

You can create a setup script to automate the configuration of the hooks directory. Create `setup.sh` in the `hooks` directory:

```bash
touch setup.sh
```

Add the following content to `hooks/setup.sh`:

```bash
#!/bin/bash

# Configure Git to use the custom hooks directory
git config core.hooksPath ./hooks
echo "Git configured to use custom hooks directory."
```

### Step 5: Make the Setup Script Executable

Make the `setup.sh` script executable:

```bash
chmod +x setup.sh
```

### Step 6: Run the Setup Script

To configure Git to use your custom hooks directory, run the setup script:

```bash
./setup.sh
```

### Verification

To verify that your custom `commit-msg` hook is working:

1. Try to commit with an invalid message:

   ```bash
   git commit -m "Invalid message"
   ```

   You should see an error message indicating the invalid format.

2. Try a valid commit message:

   ```bash
   git commit -m "feat(PROJECT-123): added new feature"
   ```

   This should succeed if the message format is correct.

## Conclusion

By following these steps, you have successfully set up custom Git hooks to validate commit message formats in your repository. This helps maintain a consistent commit history and ensures that contributors follow the specified format.
