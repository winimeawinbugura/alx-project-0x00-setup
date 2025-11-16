# alx-project-0x00-setup
ProDev Frontend Development using Next.js


#!/usr/bin/env bash
# alx-project-0x00-setup.sh
# Executable script to scaffold the Next.js project as requested.
# Usage: save as setup_alx_project.sh, make executable (chmod +x setup_alx_project.sh), then run ./setup_alx_project.sh

set -euo pipefail
IFS=$'\n\t'

PROJECT_NAME="alx-project-0x00"
REPO_NAME="alx-project-0x00-setup"
PORT=3000

echo "\n== ALX Next.js Project Scaffolding Script ==\n"

# Check Node.js version (require >=16)
if command -v node >/dev/null 2>&1; then
  NODE_VERSION=$(node -v | sed 's/v//')
  MAJOR=${NODE_VERSION%%.*}
  if [ "$MAJOR" -lt 16 ]; then
    echo "ERROR: Node.js v16 or later is required. Detected: v$NODE_VERSION"
    exit 1
  fi
else
  echo "ERROR: Node.js is not installed or not in PATH. Please install Node.js v16+ and retry."
  exit 1
fi

# Create project directory if doesn't exist (optional)
if [ -d "$PROJECT_NAME" ]; then
  echo "Directory '$PROJECT_NAME' already exists. Skipping create-next-app step."
else
  echo "Running create-next-app..."

  # Preferred non-interactive flags. create-next-app supports these common flags, but behavior
  # can change between versions. We'll try the flags, and if they fail we fall back to interactive mode.

  set +e
  npx create-next-app@latest "$PROJECT_NAME" \
    --typescript \
    --eslint \
    --tailwind \
    --import-alias "@/*" \
    --src-dir false \
    --no-app \
    --use-npm
  STATUS=$?
  set -e

  if [ $STATUS -ne 0 ]; then
    echo "\nThe non-interactive create-next-app command failed (this can happen if your installed create-next-app version doesn't support one or more flags)."
    echo "Falling back to interactive create-next-app. You will see prompts; please select the following options when prompted:" 
    echo "  • Project name: $PROJECT_NAME"
    echo "  • TypeScript: Yes"
    echo "  • ESLint: Yes"
    echo "  • Tailwind CSS: Yes"
    echo "  • Import alias: Yes (use @/*)"
    echo "  • src/ directory: No"
    echo "  • App Router: No (use Pages Router)"
    echo "\nRunning interactive npx create-next-app@latest..."
    npx create-next-app@latest
  fi
fi

# Move into project
cd "$PROJECT_NAME"

# Initialize git if not already
if [ ! -d .git ]; then
  git init
  git add -A
  git commit -m "chore: scaffold Next.js project (${PROJECT_NAME})"
fi

# Create README.md per spec (overwrite if exists)
cat > README.md <<'README'
# alx-project-0x00

Scaffolding project — Objective: Understand how to create a Next.js project via `npx create-next-app`.

## Quick start (what this script performs)
1. Creates a Next.js project named `alx-project-0x00`.
2. Uses TypeScript, ESLint, Tailwind CSS, and import alias `@/*`.
3. Uses the Pages Router (no App Router) and does not use a `/src` directory.
4. Starts the dev server on port 3000.

## To run locally
```bash
# from repository root (this directory)
cd alx-project-0x00
npm install
npm run dev -- -p 3000
```

## Notes / Troubleshooting
- This script attempts to run `npx create-next-app@latest` with flags; if your local npx/create-next-app version does not support a flag, the script will fall back to the interactive flow and you should choose the options documented above.
- You must have Node.js v16+ and npm installed.
- To open the project in VS Code from the script, the `code` command must be available on your PATH (see VS Code: Command Palette -> "Shell Command: Install 'code' command in PATH").

## Repo
GitHub repository: `alx-project-0x00-setup`
Directory: `alx-project-0x00`
File: `README.md`

README

# Try to open VS Code (optional)
if command -v code >/dev/null 2>&1; then
  echo "Opening VS Code in the project folder..."
  # open in new window
  code . || true
else
  echo "VS Code 'code' command not found. If you want to open VS Code from terminal, enable the 'code' command in PATH."
fi

# Install dependencies (safe to run even if already installed)
if [ -f package.json ]; then
  echo "Installing dependencies (npm install)..."
  npm install
fi

# Start dev server on port 3000
echo "\nStarting dev server on port $PORT..."
npm run dev -- -p $PORT

# End of script



