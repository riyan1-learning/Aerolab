# AeroLab — Setup, Build & Git Guide

A complete guide for building, running, modifying, and syncing the AeroLab project.

---

# 1. Project Overview

AeroLab is a C++ project for experimenting with aerodynamic simulation and graphics.

Current project structure:

```text
AeroLab/
│
├── .git/
├── .gitignore
├── AeroLab_SETUP.md
├── CMakeLists.txt
│
├── src/
│   └── main.cpp
│
└── build/
    └── (generated automatically by CMake)
```

## Important

The `build/` directory contains generated CMake/Visual Studio files.

It is intentionally ignored by Git.

Do NOT commit the `build/` directory.

---

# 2. Requirements

You need:

- Windows 11
- Git
- CMake
- Visual Studio / MSVC C++ compiler
- VS Code
- CMake project
- GLFW
- GLM

CMake downloads/builds the required dependencies according to the project's `CMakeLists.txt`.

---

# 3. Open the Project

Project location:

```text
D:\AeroLab
```

Using Git Bash:

```bash
cd /d/AeroLab
```

Using PowerShell:

```powershell
cd D:\AeroLab
```

---

# 4. Configure the Project

From the project root:

```bash
cmake -S . -B build
```

This tells CMake:

```text
-S .       = source directory is the current directory
-B build   = put generated build files in build/
```

You normally do NOT need to manually create the build directory.

---

# 5. Build the Project

After configuring:

```bash
cmake --build build --config Debug
```

This compiles AeroLab.

If everything succeeds, the executable will be generated inside the build directory.

---

# 6. Run AeroLab

The exact executable location depends on the CMake generator.

For the Visual Studio generator, it is normally:

```text
build/Debug/AeroLab.exe
```

From Git Bash:

```bash
./build/Debug/AeroLab.exe
```

From PowerShell:

```powershell
.\build\Debug\AeroLab.exe
```

---

# 7. The Normal Development Cycle

Most of the time, you only need to modify:

```text
src/main.cpp
```

Then:

```bash
cmake --build build --config Debug
```

Then run:

```bash
./build/Debug/AeroLab.exe
```

So the normal cycle is:

```text
Edit main.cpp
      ↓
Build
      ↓
Run
      ↓
Test
      ↓
Edit again
```

You usually do NOT need to run CMake configuration every time you only change `main.cpp`.

---

# 8. When Should I Run CMake Again?

Run:

```bash
cmake -S . -B build
```

when you change something that affects the build configuration.

Examples:

- `CMakeLists.txt` changed
- Adding a new dependency
- Changing CMake options
- Changing the project structure
- Deleting/recreating the build directory

For normal C++ source changes:

```text
src/main.cpp
```

you generally only need:

```bash
cmake --build build --config Debug
```

---

# 9. Clean Build

If the build becomes broken or confusing, you can delete the generated build directory.

From the project root:

```bash
rm -rf build
```

Then recreate it:

```bash
cmake -S . -B build
cmake --build build --config Debug
```

On PowerShell:

```powershell
Remove-Item -Recurse -Force build
```

Then:

```powershell
cmake -S . -B build
cmake --build build --config Debug
```

Do NOT delete:

```text
src/
CMakeLists.txt
.git/
.gitignore
```

---

# 10. Git — What Is Git?

Git tracks changes to the source code.

The repository is located at:

```text
D:\AeroLab\.git
```

GitHub stores a remote copy of the repository.

Current GitHub repository:

```text
https://github.com/riyan1-learning/Aerolab.git
```

The local repository is connected to GitHub through:

```text
origin
```

---

# 11. Check Git Status

At any time:

```bash
git status
```

This tells you:

- what files changed
- what files are untracked
- whether you have commits waiting to be pushed
- whether your branch is synchronized

Example:

```text
On branch main
Your branch is up to date with 'origin/main'.
```

means your local repository and GitHub are synchronized.

---

# 12. Check the Remote

Run:

```bash
git remote -v
```

You should see:

```text
origin  https://github.com/riyan1-learning/Aerolab.git (fetch)
origin  https://github.com/riyan1-learning/Aerolab.git (push)
```

---

# 13. Git Pull

`git pull` downloads changes from GitHub and integrates them into your local project.

Use:

```bash
git pull
```

This is useful when:

- you changed the project on another computer
- someone else pushed changes
- GitHub contains commits that your computer doesn't have

Normal workflow:

```bash
git pull
```

Then work on the project.

---

# 14. Git Push

`git push` uploads your local commits to GitHub.

Important:

`git push` does NOT automatically upload every changed file.

First you need to create a commit.

The normal process is:

```bash
git add .
git commit -m "Describe the changes"
git push
```

---

# 15. Complete Git Workflow

Whenever you finish working:

## Step 1 — Check changes

```bash
git status
```

## Step 2 — Add changes

```bash
git add .
```

## Step 3 — Create a commit

```bash
git commit -m "Describe what I changed"
```

## Step 4 — Upload to GitHub

```bash
git push
```

Complete sequence:

```bash
git status
git add .
git commit -m "Describe what I changed"
git push
```

---

# 16. Recommended Daily Workflow

Before starting work:

```bash
cd /d/AeroLab
git pull
```

Then edit:

```text
src/main.cpp
```

Build:

```bash
cmake --build build --config Debug
```

Run:

```bash
./build/Debug/AeroLab.exe
```

When finished:

```bash
git status
git add .
git commit -m "Update aerodynamic simulation"
git push
```

---

# 17. Example Development Session

Start:

```bash
cd /d/AeroLab
git pull
```

Edit:

```text
src/main.cpp
```

Build:

```bash
cmake --build build --config Debug
```

Run:

```bash
./build/Debug/AeroLab.exe
```

Make more changes.

Build again:

```bash
cmake --build build --config Debug
```

When finished:

```bash
git add .
git commit -m "Improve simulation rendering"
git push
```

---

# 18. If Git Says "Your Branch Is Behind"

You may see:

```text
Your branch is behind 'origin/main'
```

Run:

```bash
git pull
```

Then:

```bash
git push
```

---

# 19. If Git Says "Fetch First"

You may see:

```text
rejected
main -> main (fetch first)
```

This means GitHub has changes that your computer doesn't have.

Normally:

```bash
git pull
```

Then:

```bash
git push
```

If Git reports merge conflicts, STOP and inspect the conflict before continuing.

---

# 20. If Git Opens Vim

Sometimes Git opens an editor asking for a merge commit message.

You may see:

```text
Merge branch 'main' ...
```

If the default message is fine:

```text
Esc
:wq
Enter
```

Meaning:

```text
Esc     = leave editing mode
:wq     = write/save and quit
Enter   = execute
```

You will return to the terminal.

---

# 21. If There Is a Merge Conflict

Do NOT blindly run commands.

First:

```bash
git status
```

Git will tell you which files have conflicts.

A conflicted file may contain:

```text
<<<<<<< HEAD

your version

=======

GitHub version

>>>>>>> origin/main
```

The conflict must be resolved manually.

After fixing the file:

```bash
git add .
git commit
git push
```

If you are unsure what to do, stop and inspect the conflict before making further changes.

---

# 22. Git Log

See previous commits:

```bash
git log --oneline
```

Example:

```text
c2b395b Merge branch 'main'
b59ae4e Initial commit
```

---

# 23. See What Changed

Before committing:

```bash
git diff
```

This shows changes that have not yet been staged.

After:

```bash
git add .
```

you can inspect staged changes with:

```bash
git diff --cached
```

---

# 24. Undo Uncommitted Changes

Be careful with this command:

```bash
git restore .
```

It discards uncommitted changes to tracked files.

Only use it when you are certain you don't need the changes.

---

# 25. Git Branch

The main development branch is:

```text
main
```

Check the current branch:

```bash
git branch
```

Example:

```text
* main
```

The `*` means you are currently on that branch.

---

# 26. Creating a Development Branch

For experimental work:

```bash
git switch -c experiment
```

Work normally.

When finished:

```bash
git add .
git commit -m "Experiment with aerodynamic model"
git push -u origin experiment
```

You can then merge the branch into `main` later.

For simple development, however, working directly on `main` is fine.

---

# 27. Important Git Rule

Do NOT commit generated build files.

The project already has:

```text
build/
```

in `.gitignore`.

Therefore:

```text
build/
```

should remain untracked/ignored.

You want Git to track source/configuration files such as:

```text
CMakeLists.txt
src/main.cpp
.gitignore
AeroLab_SETUP.md
```

but not generated files such as:

```text
build/*.obj
build/*.exe
build/*.pdb
build/CMakeCache.txt
build/*.vcxproj
```

---

# 28. If You Accidentally Add build/

Check:

```bash
git status
```

If build files were staged, remove them from Git's staging/index:

```bash
git rm -r --cached build
```

Then:

```bash
git add .gitignore
git commit -m "Ignore build directory"
git push
```

Do not delete the actual build directory unless you intentionally want to clean the build.

---

# 29. First-Time Clone on Another Computer

If you want AeroLab on another computer:

```bash
git clone https://github.com/riyan1-learning/Aerolab.git
```

Then:

```bash
cd Aerolab
```

Configure:

```bash
cmake -S . -B build
```

Build:

```bash
cmake --build build --config Debug
```

Run:

```bash
./build/Debug/AeroLab.exe
```

---

# 30. Clone vs Pull

Use `git clone` when you DON'T have the project locally.

```text
New computer
     ↓
git clone
     ↓
AeroLab appears locally
```

Use `git pull` when you ALREADY have the project.

```text
Existing AeroLab
       ↓
git pull
       ↓
Get newest GitHub changes
```

Never clone the repository again every time you want the latest changes.

---

# 31. Recommended Commands Cheat Sheet

## Open project

```bash
cd /d/AeroLab
```

## Get latest changes

```bash
git pull
```

## Check status

```bash
git status
```

## Configure CMake

```bash
cmake -S . -B build
```

## Build

```bash
cmake --build build --config Debug
```

## Run

```bash
./build/Debug/AeroLab.exe
```

## See changes

```bash
git diff
```

## Stage changes

```bash
git add .
```

## Commit

```bash
git commit -m "Describe the changes"
```

## Push

```bash
git push
```

## See commits

```bash
git log --oneline
```

## See remote

```bash
git remote -v
```

---

# 32. The Most Important Workflow

If you remember only one thing, remember this:

### Start working

```bash
cd /d/AeroLab
git pull
```

### Build

```bash
cmake --build build --config Debug
```

### Run

```bash
./build/Debug/AeroLab.exe
```

### Finish working

```bash
git add .
git commit -m "Describe my changes"
git push
```

That's the basic AeroLab workflow.

---

# 33. Quick Mental Model

Think of the system like this:

```text
             GitHub
               │
             pull
               ↓
        ┌──────────────┐
        │ Local Git    │
        │ Repository   │
        └──────────────┘
               │
             edit
               ↓
        ┌──────────────┐
        │ src/main.cpp │
        └──────────────┘
               │
             build
               ↓
        ┌──────────────┐
        │    CMake     │
        │    build/    │
        └──────────────┘
               │
             run
               ↓
        ┌──────────────┐
        │ AeroLab.exe  │
        └──────────────┘
               │
           git add
               ↓
           git commit
               ↓
            git push
               │
               ↓
             GitHub
```

---

# 34. Golden Rules

1. **Pull before starting work** when working across multiple computers.
2. **Build after changing C++ code.**
3. **Run the program to test your changes.**
4. **Commit meaningful changes.**
5. **Push commits to GitHub.**
6. **Never commit `build/`.**
7. **Don't delete `.git/`.**
8. **Don't run destructive Git commands unless you understand what they do.**
9. **If Git reports a merge conflict, stop and inspect it.**
10. **Use `git status` whenever you are unsure what Git thinks is happening.**

---

# 35. One-Line Emergency Check

If you ever get confused, run:

```bash
git status
```

It is usually the best first command for figuring out what state your repository is in.