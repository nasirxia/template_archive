# Create a New GitHub Repository from an Existing Template Repository

This procedure creates a **new, independent Git repository** from an existing GitHub repository.

The new repository starts with the files from the template but **does not retain the template repository's Git history or remote connection**.

## 1. Go to a Directory Outside Any Existing Git Repository

For example:

```bash
cd ~/projects
```

It is preferable not to clone the repository inside another Git repository.

## 2. Clone the Template Repository

Clone the existing repository while giving the local directory the name of the new project:

```bash
gh repo clone nasirxia/template_archive delivery
```

This creates:

```text
~/projects/delivery/
```

Enter the new project:

```bash
cd delivery
```

At this point, `delivery` is still a clone of `template_archive`. It contains the original repository's `.git` directory, Git history, and remote configuration.

## 3. Remove the Existing Git Repository Information

Delete the `.git` directory:

```bash
rm -rf .git
```

**Warning:** `rm -rf` permanently deletes the specified directory without confirmation. Make sure you are inside the correct project directory before running it.

You can check your location first:

```bash
pwd
```

Removing `.git` does **not** delete the project files. It removes the Git history, branches, remote configuration, and other Git metadata inherited from the template.

The directory is now just a collection of ordinary project files.

## 4. Initialize a Fresh Git Repository

```bash
git init
```

This creates a new `.git` directory and starts a completely new Git repository.

You may see:

```text
Initialized empty Git repository in .../delivery/.git/
```

The `.git/description` file may contain:

```text
Unnamed repository; edit this file 'description' to name the repository.
```

There is normally **no need to edit this file**. GitHub does not use `.git/description` to determine the repository's name.

## 5. Stage the Project Files

```bash
git add .
```

The `.` means the current directory.

This stages the files in the current directory and its subdirectories for the next commit. Files excluded by `.gitignore` will normally not be staged.

Check what has been staged:

```bash
git status
```

You should see the project files under:

```text
Changes to be committed:
```

## 6. Create the Initial Commit

```bash
git commit -m "Initial commit"
```

This creates the first commit in the new repository.

You can check the repository afterward:

```bash
git status
```

## 7. Check the Current Remote

```bash
git remote -v
```

Because the original `.git` directory was removed and a fresh repository was initialized, this command should normally produce **no output**.

No output means the new local repository is not currently connected to any GitHub repository.

This is expected.

## 8. Create the New GitHub Repository and Push

From inside the new project directory, run:

```bash
gh repo create delivery --private --source=. --remote=origin --push
```

Use `--public` instead if the repository should be public:

```bash
gh repo create delivery --public --source=. --remote=origin --push
```

This command performs several operations:

1. Creates a new GitHub repository named `delivery`.
2. Uses the current directory (`.`) as the source.
3. Adds the new GitHub repository as the Git remote named `origin`.
4. Pushes the local repository to GitHub.

## 9. Verify the Remote

```bash
git remote -v
```

You should now see something similar to:

```text
origin  https://github.com/nasirxia/delivery.git (fetch)
origin  https://github.com/nasirxia/delivery.git (push)
```

The new project is now connected to its own GitHub repository.

## Complete Procedure

For a new project called `delivery` based on `nasirxia/template_archive`:

```bash
# Go somewhere outside an existing Git repository
cd ~/projects

# Clone the template into the new project's directory
gh repo clone nasirxia/template_archive delivery

# Enter the new project
cd delivery

# Remove the template repository's Git history and configuration
rm -rf .git

# Initialize a fresh Git repository
git init

# Stage the project files
git add .

# Inspect what will be committed
git status

# Create the new repository's first commit
git commit -m "Initial commit"

# Confirm that no old remote remains
git remote -v

# Create the new GitHub repository, add it as origin, and push
gh repo create delivery --private --source=. --remote=origin --push

# Verify the new remote
git remote -v
```

## Result

Before:

```text
GitHub
└── nasirxia/template_archive
        ↓ clone
Local
└── delivery
```

After removing `.git`, initializing Git, and creating the new GitHub repository:

```text
GitHub
├── nasirxia/template_archive    # Original template
└── nasirxia/delivery            # New independent project
```

The two repositories are independent. `delivery` contains the template's files at the time it was cloned, but it has its **own Git history and its own GitHub remote**.
