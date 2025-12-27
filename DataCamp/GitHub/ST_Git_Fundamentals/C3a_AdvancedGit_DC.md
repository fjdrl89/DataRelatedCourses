# Advanced Git

## Description

This course dives deep into Git's advanced features and is geared toward data engineering and data science workflows. You'll master complex merging strategies, learn to manipulate repository history and optimize Git for large-scale data projects. Key topics include advanced rebasing, git reflog for disaster recovery, efficient debugging with git bisect, and managing large files with Git LFS. You'll also explore parallel development using worktrees and how to modularize project structures with submodules. By the end of this course, you'll have the skills to handle complex version control scenarios and issues in data pipeline development and collaborative data projects.

## 1. Advanced Merging Strategies

In this chapter, you will learn practical Git merging techniques for managing complex data engineering projects. You'll explore different merge strategies and understand how to integrate code changes while maintaining a clean project history. By the end, you'll know how to handle various merge scenarios and reorganize commit histories.

### 1.1 Understanding Merge Types

#### Overview

Merging is one of the most fundamental operations in Git, enabling you to integrate work from different lines of development. While basic merging might seem straightforward, understanding the different merge types and strategies Git employs is essential for maintaining clean project history, collaborating effectively with teams, and making informed decisions about how to integrate changes. This section explores the two primary merge types you'll encounter in daily Git work: fast-forward merges and recursive merges.

**What You'll Learn:**

- The fundamental concept of Git merging and merge strategies
- Fast-forward merges: when they work, how they work, and when to use them
- Recursive merges: handling divergent histories and preserving branch context
- How to control merge behavior with specific flags
- Best practices for choosing the right merge strategy
- Real-world scenarios from data engineering and data science workflows

---

#### 1.1.1 What is Git Merge?

##### Technical Definition

**Git merge** is a command that integrates changes from one branch into another by combining their commit histories. Git examines the commit histories of both branches, identifies a common ancestor commit (the base), and creates a unified history that incorporates changes from both branches.

##### Intuitive Explanation

Think of merging like combining two streams of water. At some point in the past, a single stream (your main branch) split into two separate streams (when you created a feature branch). Each stream may have picked up different materials along the way (different commits representing different changes). Merging is the process of bringing these streams back together into a single unified flow.

**The Restaurant Kitchen Analogy:**

Imagine a restaurant kitchen where:

- **Main branch** = The main serving line where completed dishes go out
- **Feature branch** = A prep station where you're working on a new menu item
- **Merging** = Bringing your perfected new dish from the prep station to the main serving line

Just as there are different ways to introduce a new dish to the menu (replace an old item vs. add as a special alongside existing items), there are different ways to merge branches depending on what has happened in the kitchen while you were working.

##### The Anatomy of a Merge

Every merge operation involves three key components:

**1. Source Branch (FROM)**: The branch containing changes you want to integrate **2. Destination Branch (INTO)**: The branch that will receive the changes  
**3. Common Base Commit**: The point where the branches diverged

**Visual Representation:**

```
Common Base (C)
      |
      +------ Source Branch (new commits) -----> [Your changes]
      |
      +------ Destination Branch ----------> [Target location]
                                                     |
                                                     v
                                            [After merge: integrated changes]
```

##### Real-World Scenario - Data Science Workflow

Imagine you're a data scientist working on a machine learning project:

**The Setup:**

```
main branch:     A---B---C (production-ready model)
                      \
feature/new-model:     C---D---E (experimental model improvements)
```

- **Commit C**: Your baseline model with 85% accuracy
- **Commit D**: Added feature engineering (polynomial features)
- **Commit E**: Tuned hyperparameters (improved to 91% accuracy)

**The Goal:** Integrate your improved model (commits D and E) back into the main branch so the production system can use it.

**The Process:** This integration is accomplished through merging, but HOW Git performs this merge depends on what happened to the main branch while you were working on your feature branch.

---

#### 1.1.2 Understanding Merge Strategies

##### What is a Merge Strategy?

**Technical Definition**: A merge strategy is an algorithm Git uses to determine how to combine the commit histories of two branches and create the resulting integrated state.

**Intuitive Explanation**: A merge strategy is like a recipe for combining ingredients. Just as you might use different cooking techniques depending on what ingredients you're working with (mixing, folding, layering), Git uses different strategies depending on the state of the branches you're merging.

##### How Git Selects a Strategy

When you run `git merge`, Git follows this decision-making process:

**Step 1: Analysis**

```
Git examines:
- Has the destination branch changed since the source branch was created?
- Are there any conflicting changes?
- How many commits are involved?
- What is the relationship between the two branch tips?
```

**Step 2: Strategy Selection**

```
IF destination branch hasn't moved:
    → Use fast-forward merge (simplest, no merge commit)
ELSE IF both branches have new commits:
    → Use recursive merge (creates merge commit)
ELSE IF conflict requires manual resolution:
    → Pause merge, request human intervention
```

**Step 3: Execution**

```
Git applies the selected strategy and updates:
- Branch pointers
- Working directory
- Commit history
- Index (staging area)
```

##### Automatic vs. Manual Strategy Selection

**Automatic (Default Behavior):**

```bash
$ git merge feature-branch
# Git analyzes and chooses the best strategy automatically
```

Git will select fast-forward if possible, otherwise it falls back to recursive merge (also called "ort" in Git 2.33+).

**Manual (Explicit Strategy):**

```bash
$ git merge feature-branch --ff-only
# Force fast-forward, fail if not possible

$ git merge feature-branch --no-ff
# Force recursive merge, even if fast-forward is possible
```

##### The Two Primary Merge Types

For daily Git work, you'll primarily encounter two merge types:

**1. Fast-Forward Merge**

- Occurs when destination branch is unchanged
- Simply moves the branch pointer forward
- No merge commit created
- Linear history maintained
- **Analogy**: Like updating your bookmark to a later page in the same book

**2. Recursive Merge** (also called Three-Way Merge)

- Occurs when both branches have new commits
- Creates a new merge commit with two parents
- Preserves branching history
- Shows when and how branches diverged
- **Analogy**: Like weaving two threads together, creating a new pattern that shows both original threads

**Comparison Table:**

|Aspect|Fast-Forward Merge|Recursive Merge|
|---|---|---|
|**Trigger Condition**|Destination unchanged since branch creation|Both branches have new commits|
|**Commits Created**|None (pointer moves)|One merge commit|
|**History Type**|Linear|Branched|
|**Branch Context**|Lost (can't tell work was on branch)|Preserved (clear branch history)|
|**Visual Complexity**|Simple, straight line|Shows branching and merging|
|**Rollback Difficulty**|Easy (just move pointer back)|Moderate (revert merge commit)|
|**Best For**|Quick fixes, simple features|Complex features, team development|
|**Common In**|Solo development|Collaborative projects|

---

#### 1.1.3 Fast-Forward Merge: The Simple Path

##### What is a Fast-Forward Merge?

**Technical Definition**: A fast-forward merge occurs when the destination branch's HEAD is a direct ancestor of the source branch's HEAD, allowing Git to simply move the destination branch pointer forward to the source branch's position without creating a new commit.

**Intuitive Explanation**: Imagine you're reading a book together with someone. You put down your bookmark at chapter 5, they keep reading to chapter 8, and then you pick up the book again. Moving your bookmark from chapter 5 to chapter 8 is a "fast-forward" operation—there's no need to create any new content; you're just catching up to where they are.

**The Timeline Analogy:**

Think of your Git history as a timeline of events:

```
Past                                Future
|----------------------------------------->
A---B---C (main is here)
         \
          C---D---E (feature is here)
```

If main is still at C and feature has progressed to E, main can simply "fast-forward" to E. No time travel or parallel universes involved—just moving forward along the same timeline.

##### Conditions Required for Fast-Forward

A fast-forward merge is **only possible** when **ALL** of the following conditions are met:

**Condition 1: Linear History**

- There must be a direct path from the destination branch to the source branch
- No branching or diverging has occurred

**Condition 2: Destination Branch is Unchanged**

- The destination branch has not received any new commits since the source branch was created
- The destination branch pointer hasn't moved

**Condition 3: No Conflicts**

- Since the destination hasn't changed, conflicts are impossible
- This makes fast-forward merges the safest merge type

**Visual Representation - Valid Fast-Forward:**

```
Before merge:
main:         A---B---C
                       \
feature:                C---D---E---F

After merge:
main:         A---B---C---D---E---F
                                  ↑
                            (main moved here)
feature:                C---D---E---F
                                  ↑
                          (feature stays here)
```

**Visual Representation - CANNOT Fast-Forward:**

```
Cannot fast-forward because main has advanced:

main:         A---B---C---G---H
                       \
feature:                C---D---E---F

Branches diverged at C. Three-way merge required.
```

##### How Fast-Forward Merges Work

**Step-by-Step Mechanics:**

**Step 1: Verification**

```bash
$ git merge feature-branch

# Git checks:
# - Is main's HEAD an ancestor of feature-branch's HEAD?
# - Has main advanced since feature-branch was created?
```

**Step 2: Pointer Movement**

```
IF verification passes:
    1. Git moves the destination branch pointer forward
    2. No new commits are created
    3. Working directory updates to match source branch
    4. History remains linear
```

**Step 3: Confirmation**

```bash
# Git outputs:
Updating 7a8b9c0..3f4e5a6
Fast-forward
 src/data_pipeline.py     | 47 +++++++++++++++++++++++++++
 src/feature_engineering.py | 23 +++++++++++--
 2 files changed, 68 insertions(+), 2 deletions(-)
```

##### Fast-Forward Merge Commands

###### Command 1: Default Merge (Allow Fast-Forward)

**What it does**: Attempts to merge using fast-forward if possible; falls back to recursive merge if not.

**Syntax:**

```bash
git merge <source-branch>
```

**Full Example with Context:**

```bash
# Check current branch
$ git branch
* main
  feature/improve-accuracy

# Verify main hasn't changed since branching
$ git log --oneline --graph --all
* 9e8f7d6 (feature/improve-accuracy) Optimize hyperparameters
* 2a1b3c4 Add cross-validation
* 5d4e3f2 (HEAD -> main) Initial model implementation

# Perform merge (will fast-forward automatically)
$ git merge feature/improve-accuracy
Updating 5d4e3f2..9e8f7d6
Fast-forward
 model_training.py | 67 +++++++++++++++++++++++++++++++++++++++++++++++
 config.yaml       | 12 +++++++++
 2 files changed, 79 insertions(+)
```

**Expected Output:**

```
Updating 5d4e3f2..9e8f7d6
Fast-forward
 model_training.py | 67 +++++++++++++++++++++++++++++++++++++++++++++++
 config.yaml       | 12 +++++++++
 2 files changed, 79 insertions(+)
```

**Output Explained:**

- `Updating 5d4e3f2..9e8f7d6`: Moving main pointer from commit 5d4e3f2 to 9e8f7d6
- `Fast-forward`: Confirms the merge strategy used
- File changes: Shows what was modified
- Line counts: `67` lines added, `12` lines added

**When to Use:**

- Daily workflow when you want Git to choose the best approach
- When you're unsure if fast-forward is possible
- When you don't have strong preferences about history shape

###### Command 2: Force Fast-Forward Only

**What it does**: Attempts fast-forward merge; **fails** if fast-forward is not possible instead of falling back to recursive merge.

**Syntax:**

```bash
git merge --ff-only <source-branch>
```

**Full Example - Success Case:**

```bash
# Ensure we're on main branch
$ git checkout main
Already on 'main'

# Attempt fast-forward only merge
$ git merge --ff-only feature/data-cleaning
Updating c4d5e6f..a2b3c4d
Fast-forward
 data_cleaning.py | 89 +++++++++++++++++++++++++++++++++++++++++++++++++
 utils.py         | 15 +++++++++
 2 files changed, 104 insertions(+)
```

**Full Example - Failure Case:**

```bash
# Try to merge when main has advanced
$ git merge --ff-only feature/visualization
fatal: Not possible to fast-forward, aborting.

# Git stops instead of creating a merge commit
# This protects you from unexpected recursive merges
```

**When to Use:**

- When you specifically want to maintain linear history
- When you want to ensure your feature branch is up-to-date before merging
- In automated scripts where you want predictable behavior
- When team policy requires linear history

**Pro Tip - Pre-Merge Validation:**

Use `--ff-only` as a check before performing the actual merge:

```bash
# Check if fast-forward is possible without making changes
$ git merge --ff-only --no-commit feature-branch

# If successful, you know fast-forward will work
# If it fails, you can update your feature branch first
```

###### Command 3: Prevent Fast-Forward (Force Merge Commit)

**What it does**: Creates a merge commit even when fast-forward is possible. This explicitly preserves branch history.

**Syntax:**

```bash
git merge --no-ff <source-branch>
```

**Full Example:**

```bash
# Current state allows fast-forward
$ git log --oneline --graph --all
* 3f4e5a6 (feature/user-auth) Complete authentication system
* 7a8b9c0 Add login validation
* c4d5e6f (HEAD -> main) Update dependencies
* a2b3c4d Initial commit

# Force merge commit creation
$ git merge --no-ff feature/user-auth

# Git opens editor for merge commit message
# Default message: "Merge branch 'feature/user-auth'"
# Save and close editor

Merge made by the 'ort' strategy.
 auth.py          | 142 +++++++++++++++++++++++++++++++++++++++++++
 auth_utils.py    | 67 ++++++++++++++++++++
 tests/test_auth.py | 89 +++++++++++++++++++++++++
 3 files changed, 298 insertions(+)
 create mode 100644 auth.py
 create mode 100644 auth_utils.py
 create mode 100644 tests/test_auth.py
```

**Expected Output After Merge:**

```bash
$ git log --oneline --graph --all -5
*   9d8e7f6 (HEAD -> main) Merge branch 'feature/user-auth'
|\
| * 3f4e5a6 (feature/user-auth) Complete authentication system
| * 7a8b9c0 Add login validation
|/
* c4d5e6f Update dependencies
* a2b3c4d Initial commit
```

**Notice the Difference:**

**WITH --no-ff (merge commit created):**

```
*   9d8e7f6 Merge branch 'feature/user-auth'  ← New merge commit
|\
| * 3f4e5a6 Complete authentication system
| * 7a8b9c0 Add login validation
|/
* c4d5e6f Update dependencies
```

**WITHOUT --no-ff (fast-forward):**

```
* 3f4e5a6 (HEAD -> main, feature/user-auth) Complete authentication system
* 7a8b9c0 Add login validation
* c4d5e6f Update dependencies
```

The `--no-ff` version preserves the information that commits 7a8b9c0 and 3f4e5a6 were developed on a feature branch.

**When to Use:**

- When you want to preserve feature branch context in history
- For significant features that deserve documentation of when they were integrated
- When team policy requires explicit merge commits
- To make it easier to revert entire features later

**Pro Tip - Team-Wide Policy:**

Some teams configure Git to always use `--no-ff` by default:

```bash
# Set as repository default
$ git config merge.ff false

# Or globally for all repositories
$ git config --global merge.ff false
```

##### Real-World Scenario: Solo Data Science Development

**Scenario:** You're a data scientist developing a new feature engineering technique for customer churn prediction.

**Timeline:**

**Day 1 - Morning:**

```bash
# Start from clean main branch
$ git checkout main
$ git pull  # Ensure you're up to date

# Create feature branch
$ git checkout -b feature/rfm-analysis
Switched to a new branch 'feature/rfm-analysis'

# Main and feature are at the same commit
$ git log --oneline -1
c7d8e9f (HEAD -> feature/rfm-analysis, main) Update model baseline
```

**Day 1 - Throughout the Day:**

```bash
# Work on RFM (Recency, Frequency, Monetary) analysis
$ # ... edit feature_engineering.py ...
$ git add feature_engineering.py
$ git commit -m "Add RFM score calculation"
[feature/rfm-analysis 1a2b3c4] Add RFM score calculation

$ # ... add tests ...
$ git add tests/test_rfm.py
$ git commit -m "Add RFM analysis tests"
[feature/rfm-analysis 5d6e7f8] Add RFM analysis tests

$ # ... optimize implementation ...
$ git add feature_engineering.py
$ git commit -m "Optimize RFM calculation for large datasets"
[feature/rfm-analysis 9g8h7i6] Optimize RFM calculation for large datasets
```

**Day 2 - Ready to Merge:**

```bash
# Check current state
$ git log --oneline --graph --all
* 9g8h7i6 (HEAD -> feature/rfm-analysis) Optimize RFM calculation
* 5d6e7f8 Add RFM analysis tests
* 1a2b3c4 Add RFM score calculation
* c7d8e9f (main) Update model baseline  ← main hasn't moved!

# Since main hasn't changed, we can fast-forward
$ git checkout main
$ git merge feature/rfm-analysis
Updating c7d8e9f..9g8h7i6
Fast-forward
 feature_engineering.py | 156 +++++++++++++++++++++++++++++++++++++++++
 tests/test_rfm.py      | 89 +++++++++++++++++++++
 2 files changed, 245 insertions(+)
 create mode 100644 tests/test_rfm.py
```

**Result - Clean Linear History:**

```bash
$ git log --oneline --graph -5
* 9g8h7i6 (HEAD -> main, feature/rfm-analysis) Optimize RFM calculation
* 5d6e7f8 Add RFM analysis tests
* 1a2b3c4 Add RFM score calculation
* c7d8e9f Update model baseline
* a1b2c3d Initial churn model
```

**Key Observation:** The history is perfectly linear. Someone reading the history later cannot tell that commits 1a2b3c4, 5d6e7f8, and 9g8h7i6 were developed on a separate branch. This is ideal for solo development with simple features.

##### Real-World Scenario: Team Development Preventing Fast-Forward

**Scenario:** You're a data engineer working on a feature while teammates also work on main.

**Your Timeline:**

**Monday Morning:**

```bash
# You branch off main
$ git checkout -b feature/kafka-integration main
Switched to a new branch 'feature/kafka-integration'

# Start point: commit abc1234
$ git log --oneline -1
abc1234 (HEAD -> feature/kafka-integration, main) Add data validation
```

**Monday-Tuesday (Your Work):**

```bash
# You make 3 commits over two days
$ git log --oneline feature/kafka-integration -3
def5678 (HEAD -> feature/kafka-integration) Complete Kafka consumer
9gh0123 Add Kafka producer configuration  
4jk5678 Set up Kafka connection manager
```

**Teammate's Work (Parallel, on main):**

```bash
# Meanwhile, a teammate merged their feature into main
# Main is now at a different commit

$ git log --oneline main -1
xyz9876 (main) Optimize database queries  ← This is NEW (not in your branch)
```

**Current State:**

```
main:              abc1234---xyz9876
                          \
feature/kafka:             \
                            abc1234---4jk5678---9gh0123---def5678
```

**Wednesday - Attempt to Merge:**

```bash
$ git checkout main
$ git merge feature/kafka-integration

# Fast-forward NOT possible because main has advanced
# Git will use recursive merge (covered in next section)

Merge made by the 'ort' strategy.
 kafka_consumer.py | 234 +++++++++++++++++++++++++++++++++++++
 kafka_producer.py | 187 ++++++++++++++++++++++++++++++
 kafka_config.py   | 56 ++++++++++
 3 files changed, 477 insertions(+)
```

**Pro Tip - Keeping Fast-Forward Possible:**

If you want to maintain the ability to fast-forward even in team environments:

```bash
# Regularly update your feature branch with main
$ git checkout feature/kafka-integration

# Pull latest main changes into your feature branch
$ git merge main
# Or: git rebase main (covered in section 1.3)

# Now when you're ready to merge back:
$ git checkout main
$ git merge feature/kafka-integration
# More likely to fast-forward since your branch includes main's changes
```

However, this creates its own trade-offs, which we'll discuss in Section 1.3 on rebasing.

##### When to Use Fast-Forward Merges

**Ideal Use Cases:**

**1. Quick Bug Fixes**

```bash
# Hot fix for production bug
$ git checkout -b hotfix/critical-null-check main
$ # ... fix bug ...
$ git commit -am "Fix null pointer in data processor"
$ git checkout main
$ git merge hotfix/critical-null-check
# Fast-forward keeps history clean for small fixes
```

**2. Solo Development**

- You're the only one working on main
- No risk of main advancing while you work
- Simple, linear history is preferred

**3. Short-Lived Feature Branches**

- Feature takes < 1 day to complete
- Low chance of conflicts
- Simple changes that don't need complex history tracking

**4. Sequential Work**

- You finish one feature before starting another
- One feature builds directly on the previous
- Natural linear progression

**When to AVOID Fast-Forward Merges:**

**1. Complex Features Needing Context**

```bash
# This feature deserves to be documented as a distinct unit
$ git merge --no-ff feature/machine-learning-pipeline
# The merge commit preserves when this feature was integrated
```

**2. Team Environments**

- Multiple people committing to main
- Main likely to advance while you work
- Fast-forward probably impossible anyway

**3. Features You Might Need to Revert**

```bash
# If you might need to remove this feature later
$ git merge --no-ff feature/experimental-algorithm
# Easier to revert a single merge commit than multiple individual commits
```

**4. Audit/Compliance Requirements**

- Need clear history of when features were integrated
- Need to show which commits were part of which feature
- Regulatory requirements for change tracking

##### Fast-Forward Merge Best Practices

**Pro Tip #1: Check Before Merging**

Always verify the current state before merging:

```bash
# Check if fast-forward is possible
$ git log --oneline main..feature-branch  # Commits in feature not in main
$ git log --oneline feature-branch..main  # Commits in main not in feature

# If the second command shows commits, fast-forward impossible
```

**Pro Tip #2: Visualize First**

Use Git's visualization tools to understand what will happen:

```bash
$ git log --oneline --graph --all
* 3f4e5a6 (feature/new-feature) Add feature X
* 7a8b9c0 (HEAD -> main) Last main commit

# Clear linear path → fast-forward will work
```

**Pro Tip #3: Test Before Merging**

```bash
# Make sure everything works before merging
$ git checkout feature-branch
$ pytest tests/
$ # All tests pass

# Now safe to merge
$ git checkout main
$ git merge feature-branch
```

**Pro Tip #4: Clean Up After Merging**

```bash
# After successful merge, delete the feature branch
$ git branch -d feature/new-feature
Deleted branch feature/new-feature (was 3f4e5a6).

# This keeps your branch list clean
# The commits are still in main's history
```

##### Common Issues with Fast-Forward Merges

**Issue #1: "Not possible to fast-forward"**

**Cause:** Main branch has advanced since you created your feature branch.

**Solution Options:**

**Option A: Accept Recursive Merge**

```bash
$ git merge feature-branch
# Will create merge commit instead
```

**Option B: Rebase Then Fast-Forward** (Advanced - covered in 1.3)

```bash
$ git checkout feature-branch
$ git rebase main  # Replay your commits on top of main
$ git checkout main
$ git merge feature-branch  # Now fast-forward works
```

**Option C: Update Main, Start Over**

```bash
$ git branch -D feature-branch  # Delete feature branch
$ git pull  # Update main
$ git checkout -b feature-branch  # Recreate branch
$ # Redo your work (only if work is minimal)
```

**Issue #2: "Already up to date"**

**Cause:** You're trying to merge a branch into itself, or the branches are at the same commit.

**Solution:**

```bash
# Check your current branch
$ git branch
* main  ← You're on main

# Make sure you're merging the right branches
$ git log --oneline --graph --all
# Verify the branches are actually different
```

**Issue #3: Fast-Forward Loses Context**

**Problem:** After fast-forward, you can't tell which commits were part of which feature.

**Prevention:**

```bash
# Use --no-ff for features you want to track
$ git merge --no-ff feature/important-feature

# Set as default for repository
$ git config merge.ff false
```

---

#### 1.1.4 Recursive Merge: Preserving Branch History

##### What is a Recursive Merge?

**Technical Definition**: A recursive merge (also called a three-way merge or ort merge in Git 2.33+) occurs when both the source and destination branches have new commits since their common ancestor. Git creates a new merge commit that has two parent commits, preserving the full history and branching structure.

**Intuitive Explanation**: Imagine two authors working on different chapters of a book simultaneously. One author wrote chapters 6-7, while another wrote chapters 8-9. When you need to combine their work into a complete manuscript, you're not just moving a bookmark forward—you're weaving together two distinct lines of work into a unified whole. The resulting book needs to acknowledge both authors' contributions.

**The River Confluence Analogy:**

Think of your Git history as rivers:

```
        Common Source (Mountain spring)
                |
        +-------+-------+
        |               |
    River A         River B
    (main)       (feature branch)
  flows south    flows southeast
        |               |
    New commits    New commits
        |               |
        +-------+-------+
                |
          Confluence Point
       (merge commit with
        two parent commits)
                |
                v
        Combined River
```

Just as a river confluence creates a new combined flow while still showing where each tributary came from, a recursive merge creates a new commit that combines both branches while preserving the history of both.

##### Why "Recursive" and "Three-Way"?

**Three-Way Explained:**

The term "three-way" refers to the three commits Git examines:

**Point 1: Common Ancestor (Base)**

- The last commit that existed before the branches diverged
- The "baseline" for comparison

**Point 2: Your Branch Tip (Ours)**

- The latest commit on the destination branch
- What "we" have

**Point 3: Their Branch Tip (Theirs)**

- The latest commit on the source branch
- What "they" have

**Visual Representation:**

```
         [1] Common Ancestor
               (Commit C)
                   |
        +----------+----------+
        |                     |
        v                     v
     [2] Ours              [3] Theirs
    (Commit F)            (Commit E)
     Main Branch          Feature Branch
        |                     |
        +----------+----------+
                   |
                   v
              Merge Commit
           (Parents: F and E)
```

**Why "Recursive"?**

The name "recursive" comes from how Git handles complex merging scenarios with multiple common ancestors. The algorithm recursively merges the common ancestors into a virtual base before performing the final three-way merge. Don't worry about the complex details—just know that "recursive" means Git is smart enough to handle complicated branching patterns.

**Note on "Ort" Strategy:** As of Git 2.33, the default merge strategy is "ort" (Ostensibly Recursive's Twin), which is an improved implementation of the recursive strategy. For practical purposes, you can think of ort as "recursive 2.0"—faster and more accurate, but conceptually the same.

##### Conditions That Trigger Recursive Merge

A recursive merge occurs when **ANY** of the following conditions exist:

**Condition 1: Both Branches Have New Commits**

```
main:        A---B---C---F---G (advanced since branch creation)
                      \
feature:               C---D---E (also has new commits)
```

Both branches have moved forward from commit C.

**Condition 2: Explicit Request**

```bash
$ git merge --no-ff feature-branch
# Forces recursive merge even if fast-forward is possible
```

**Condition 3: Complex Merge Scenarios**

- Multiple common ancestors (criss-cross merges)
- Branches that have been merged multiple times
- Git determines recursive strategy is needed for safety

##### How Recursive Merges Work

**Step-by-Step Mechanics:**

**Step 1: Find Common Ancestor**

```bash
# Git identifies where branches diverged
$ git merge-base main feature-branch
c7d8e9f  # This is the common ancestor commit
```

**Step 2: Three-Way Comparison**

```
Git compares:
- What changed in main since the ancestor?
- What changed in feature since the ancestor?
- Are any changes conflicting?
```

**Step 3: Automatic Merge**

```
For each file:
    IF only main changed it:
        → Use main's version
    ELIF only feature changed it:
        → Use feature's version
    ELIF both changed different parts:
        → Merge both changes
    ELSE (both changed same part):
        → Flag as CONFLICT, need human resolution
```

**Step 4: Create Merge Commit**

```
Git creates a new commit that:
- Has TWO parent commits (main's tip and feature's tip)
- Contains the merged result
- Preserves the complete history
- Shows when and how branches came together
```

##### Recursive Merge Commands

###### Command: Create Recursive Merge

**Syntax:**

```bash
git merge <source-branch>
# Will use recursive merge if fast-forward isn't possible

# OR explicitly force recursive merge:
git merge --no-ff <source-branch>
```

**Full Example - Automatic Recursive Merge:**

**Setup:**

```bash
# Check current state
$ git log --oneline --graph --all
* h8i9j0k (feature/add-validation) Complete validation rules
* g7h8i9j Add input sanitization
| * f6g7h8i (HEAD -> main) Update dependencies
| * e5f6g7h Fix typo in README
|/
* d4e5f6g (Common ancestor) Add data processing module
```

Notice how both branches have new commits since d4e5f6g—main has 2 new commits (e5f6g7h, f6g7h8i), and feature has 2 new commits (g7h8i9j, h8i9j0k).

**Execute Merge:**

```bash
$ git checkout main
Already on 'main'

$ git merge feature/add-validation
# Git determines recursive merge is needed
# Opens text editor for merge commit message

# Default message shown in editor:
Merge branch 'feature/add-validation'

# Please enter a commit message to explain why this merge is necessary,
# especially if it merges an updated upstream into a topic branch.
#
# Lines starting with '#' will be ignored, and an empty message aborts
# the commit.
```

**Save the Merge Message:**

```
In nano: Ctrl+O, Enter, Ctrl+X
In vim: :wq
In VS Code: Save and close the file
```

**Expected Output:**

```
Merge made by the 'ort' strategy.
 validation.py       | 145 ++++++++++++++++++++++++++++++++++++++++++
 sanitization.py     | 78 ++++++++++++++++++++++
 tests/test_validation.py | 92 ++++++++++++++++++++++++
 3 files changed, 315 insertions(+)
 create mode 100644 validation.py
 create mode 100644 sanitization.py
 create mode 100644 tests/test_validation.py
```

**Output Explained:**

- `Merge made by the 'ort' strategy`: Confirms recursive merge was used
- File statistics: Shows total changes from the feature branch
- `create mode 100644`: These are new files added by the feature branch

**View Result:**

```bash
$ git log --oneline --graph --all -6
*   k1l2m3n (HEAD -> main) Merge branch 'feature/add-validation'
|\
| * h8i9j0k (feature/add-validation) Complete validation rules
| * g7h8i9j Add input sanitization
* | f6g7h8i Update dependencies
* | e5f6g7h Fix typo in README
|/
* d4e5f6g Add data processing module
```

**Key Observations:**

1. **Merge commit** k1l2m3n has **two parents** (h8i9j0k and f6g7h8i)
2. **Branch lines** clearly show where the work diverged and converged
3. **History preserved**: You can see exactly which commits came from which branch

**Full Example - Explicit Recursive Merge (--no-ff):**

**Setup (Fast-Forward is Possible):**

```bash
$ git log --oneline --graph --all
* 3f4e5a6 (feature/optimize-queries) Optimize database queries
* 7a8b9c0 Add query caching
* c4d5e6f (HEAD -> main) Initial database setup

# Main hasn't changed, so fast-forward is possible
# But we want to explicitly preserve the feature branch history
```

**Execute Merge:**

```bash
$ git merge --no-ff feature/optimize-queries

# Editor opens for merge commit message
# Add context about why this feature is important:
Merge branch 'feature/optimize-queries'

Improved query performance by 300% through caching and optimization.
This resolves performance issues reported in issue #234.
```

**Expected Output:**

```
Merge made by the 'ort' strategy.
 database/queries.py       | 234 ++++++++++++++++++++++++++++++++++++
 database/cache.py         | 145 +++++++++++++++++++++++
 tests/test_queries.py     | 89 ++++++++++++++
 3 files changed, 468 insertions(+)
```

**View Result:**

```bash
$ git log --oneline --graph --all -5
*   9d8e7f6 (HEAD -> main) Merge branch 'feature/optimize-queries'
|\
| * 3f4e5a6 (feature/optimize-queries) Optimize database queries
| * 7a8b9c0 Add query caching
|/
* c4d5e6f Initial database setup
* a2b3c4d Project initialization
```

**Why This is Better:** Even though fast-forward was possible, the merge commit 9d8e7f6 serves as documentation that commits 7a8b9c0 and 3f4e5a6 were developed together as a feature. Six months from now, this context will be valuable.

##### Anatomy of a Merge Commit

A merge commit is special because it has **two parent commits**. Let's examine what this means:

**Viewing Merge Commit Details:**

```bash
$ git show HEAD  # Assuming HEAD is at the merge commit

commit 9d8e7f6a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5d6e
Merge: f6g7h8i h8i9j0k  ← TWO parents!
Author: Jane Developer <jane@company.com>
Date:   Wed Nov 6 14:32:01 2025 -0800

    Merge branch 'feature/add-validation'
    
    Added comprehensive input validation and sanitization to improve
    security and data quality. All validation rules include tests.

(Followed by a diff showing combined changes)
```

**Understanding the Parents Line:**

```
Merge: f6g7h8i h8i9j0k
       ↑        ↑
       |        |
       |        +-- Parent 2: Tip of feature branch (what was merged IN)
       |
       +-- Parent 1: Tip of main branch (what we merged INTO)
```

**Visual Representation:**

```
                    f6g7h8i (Parent 1 - main)
                        ↑
                        |
Merge Commit ------+----+
(9d8e7f6)          |
                   |
                   +-----> h8i9j0k (Parent 2 - feature)
```

**Why Two Parents Matter:**

**1. History Preservation**

```bash
# Follow first parent (main's history)
$ git log --first-parent --oneline
9d8e7f6 Merge branch 'feature/add-validation'
f6g7h8i Update dependencies
e5f6g7h Fix typo in README
d4e5f6g Add data processing module

# Follow feature's history  
$ git log feature/add-validation --oneline
h8i9j0k Complete validation rules
g7h8i9j Add input sanitization
d4e5f6g Add data processing module
```

**2. Reversibility**

```bash
# Can revert the entire merge in one command
$ git revert 9d8e7f6 -m 1
# Removes all changes from the feature branch
# But keeps the history that the merge happened
```

**3. Audit Trail**

- Shows exactly what was integrated and when
- Identifies who performed the integration
- Provides context in the commit message

##### Real-World Scenario: Data Engineering Pipeline Development

**Scenario:** You're a data engineer adding a new data source to your ETL pipeline while your teammate works on improving the existing transformations.

**Week 1 - Monday:**

**You:**

```bash
# Start new feature
$ git checkout -b feature/add-kafka-source main
Switched to a new branch 'feature/add-kafka-source'

# Initial state
$ git log --oneline -1
abc1234 (HEAD -> feature/add-kafka-source, main) Baseline ETL pipeline
```

**Your Teammate:**

```bash
# They work on main
$ git checkout -b feature/optimize-transforms main

# They complete their work quickly and merge Wednesday
$ git checkout main
$ git merge feature/optimize-transforms
# Fast-forward (they finished before main advanced)
```

**Week 1 - Wednesday:**

**Current State:**

```bash
# Check what happened while you were working
$ git log --oneline --graph --all
* xyz9876 (main) Merge branch 'feature/optimize-transforms'
* def5678 Refactor transformation logic
* 9gh0123 Add performance monitoring
| * 4jk5678 (HEAD -> feature/add-kafka-source) Complete Kafka integration
| * 7mn8901 Add Kafka consumer
| * 2pq3456 Set up Kafka connection
|/
* abc1234 Baseline ETL pipeline
```

**Your Branch:**

- abc1234 (start point)
- 2pq3456 Set up Kafka connection
- 7mn8901 Add Kafka consumer
- 4jk5678 Complete Kafka integration

**Main Branch:**

- abc1234 (start point - same as yours)
- 9gh0123 Add performance monitoring (teammate's work)
- def5678 Refactor transformation logic (teammate's work)
- xyz9876 Merge commit (teammate's work)

**The Branches Have Diverged!**

**Week 1 - Thursday (Ready to Merge):**

```bash
# Switch to main
$ git checkout main
Switched to branch 'main'

# Attempt merge
$ git merge feature/add-kafka-source

# Git opens editor with default message
Merge branch 'feature/add-kafka-source'

# You add context:
Merge branch 'feature/add-kafka-source'

Added Kafka as a new data source for the ETL pipeline. This enables
real-time ingestion of user activity events from the mobile app.

Includes:
- Kafka consumer configuration
- Schema validation for Kafka messages  
- Integration tests with test Kafka broker
- Documentation in docs/kafka-integration.md

Closes #456
```

**Merge Output:**

```
Merge made by the 'ort' strategy.
 kafka_consumer.py          | 267 ++++++++++++++++++++++++++++++++
 kafka_config.py            | 89 +++++++++++
 schemas/kafka_schema.py    | 45 ++++++
 tests/test_kafka.py        | 178 +++++++++++++++++++++
 docs/kafka-integration.md  | 134 ++++++++++++++++
 5 files changed, 713 insertions(+)
 create mode 100644 kafka_consumer.py
 create mode 100644 kafka_config.py
 create mode 100644 schemas/kafka_schema.py
 create mode 100644 tests/test_kafka.py
 create mode 100644 docs/kafka-integration.md
```

**Final History:**

```bash
$ git log --oneline --graph --all -8
*   rst4567 (HEAD -> main) Merge branch 'feature/add-kafka-source'
|\
| * 4jk5678 (feature/add-kafka-source) Complete Kafka integration
| * 7mn8901 Add Kafka consumer
| * 2pq3456 Set up Kafka connection
* | xyz9876 Merge branch 'feature/optimize-transforms'
* | def5678 Refactor transformation logic
* | 9gh0123 Add performance monitoring
|/
* abc1234 Baseline ETL pipeline
```

**Key Insights from This History:**

**1. Parallel Development Visible**

- Clear that two features were developed simultaneously
- Can trace each feature's commits separately
- Understand the timeline of integrations

**2. Merge Commits Document Integration Points**

- rst4567: When Kafka source was integrated
- xyz9876: When optimization was integrated
- Both serve as milestones in project history

**3. Branching Shows Collaboration**

- Team members can work independently
- History shows who did what and when
- Easy to identify which feature introduced which changes

##### When to Use Recursive Merges

**Ideal Use Cases:**

**1. Long-Running Feature Branches**

```bash
# Feature that takes multiple days/weeks
$ git checkout -b feature/machine-learning-model main
# ... work for 2 weeks ...
# Main definitely advanced during this time
$ git checkout main
$ git merge feature/machine-learning-model
# Recursive merge preserves the 2 weeks of parallel development
```

**2. Team Development**

- Multiple developers committing to main
- Features being integrated frequently
- Need to see who worked on what
- Audit requirements for change tracking

**3. Features You Might Need to Revert**

```bash
# Easy to revert a single merge commit
$ git revert <merge-commit> -m 1
# This removes all feature changes while keeping history
```

**4. Complex Features Requiring Context**

```bash
$ git merge feature/refactor-database-layer --no-ff

# The merge commit message documents:
# - What the feature was
# - Why it was needed
# - What issues it solves
# - Any special considerations
```

**5. Preserving Development History**

```bash
# Want to see the chronological order of development
$ git log --graph --oneline
# Shows parallel development paths and when they merged
```

**When to AVOID Recursive Merges (Use Fast-Forward Instead):**

**1. Quick Hot Fixes**

```bash
# Simple bug fix that takes 5 minutes
$ git checkout -b hotfix/null-check main
$ # ... fix bug ...
$ git checkout main
$ git merge hotfix/null-check  
# Fast-forward keeps history clean for trivial changes
```

**2. Solo Development with Sequential Work**

- You finish one feature completely before starting another
- Main never advances while you work
- Prefer simple linear history

**3. Tiny Changes Not Worth Documenting**

```bash
# Fixing a typo, updating a comment, etc.
# Fast-forward avoids cluttering history with merge commits
```

##### Recursive Merge Best Practices

**Pro Tip #1: Write Excellent Merge Commit Messages**

The merge commit message is documentation for future developers. Make it count:

**Bad Merge Message:**

```
Merge branch 'feature/new-stuff'
```

**Good Merge Message:**

```
Merge branch 'feature/anomaly-detection'

Implement real-time anomaly detection for transaction monitoring.

This feature adds a statistical model that identifies unusual patterns
in transaction data and flags them for review. The model uses a
sliding window approach with configurable thresholds.

Key changes:
- Added AnomalyDetector class with Z-score and IQR methods
- Integrated with existing transaction pipeline
- Added alerting system for detected anomalies
- Comprehensive test suite with edge cases

Performance: Processes 10,000 transactions/sec with <50ms latency
Testing: 95% coverage, all integration tests passing

Closes #789
Related: #654, #723
```

**What Makes This Good:**

- **Summary line**: Clear one-line description
- **Context**: Explains what and why
- **Technical details**: Implementation approach
- **Key changes**: Major components added
- **Metrics**: Performance and testing data
- **Traceability**: Links to issues

**Pro Tip #2: Test Before Merging**

```bash
# Before merging, verify everything works
$ git checkout feature/new-feature

# Run full test suite
$ pytest tests/
$ # All tests pass ✓

# Check for integration issues
$ make integration-test
$ # Integration tests pass ✓

# Now merge
$ git checkout main
$ git merge feature/new-feature
```

**Pro Tip #3: Visualize Before and After**

```bash
# Before merging - see what will happen
$ git log --oneline --graph --all

# Perform merge
$ git merge feature-branch

# After merging - verify result
$ git log --oneline --graph --all -10
```

**Pro Tip #4: Keep Merge Commits Clean**

```bash
# Don't make changes during merge (unless resolving conflicts)
# The merge commit should ONLY represent the integration

# If you need to make adjustments:
$ git merge feature-branch
$ # Merge completes
$ # Make your adjustments
$ git commit --amend
# This keeps the merge commit clean
```

**Pro Tip #5: Use Merge Commits as Documentation**

```bash
# Configure Git to always open editor for merge messages
$ git config --global merge.ff false
$ git config --global core.editor "code --wait"  # Or your preferred editor

# This forces you to write meaningful merge commit messages
```

##### Common Issues with Recursive Merges

**Issue #1: Merge Conflicts**

**Scenario:**

```bash
$ git merge feature/update-schema
Auto-merging schema.py
CONFLICT (content): Merge conflict in schema.py
Automatic merge failed; fix conflicts and then commit the result.
```

**What Happened:** Both branches modified the same lines in schema.py differently.

**Solution:**

```bash
# Check what's in conflict
$ git status
On branch main
You have unmerged paths.

Unmerged paths:
  both modified:   schema.py

# Open schema.py and look for conflict markers:
<<<<<<< HEAD (Current Change - Main)
def process_data(input_data):
    return validate_schema_v2(input_data)
=======
def process_data(input_data):
    return validate_with_strict_mode(input_data)
>>>>>>> feature/update-schema (Incoming Change)

# Decide which version to keep (or combine them)
def process_data(input_data):
    # Use both: validate with v2 schema in strict mode
    validated = validate_schema_v2(input_data)
    return validate_with_strict_mode(validated)

# Mark as resolved and complete merge
$ git add schema.py
$ git commit -m "Merge branch 'feature/update-schema'

Resolved conflict in schema.py by combining both validation approaches."
```

**Note:** Section 1.2 will cover merge conflicts in depth with advanced resolution strategies.

**Issue #2: Accidentally Created Fast-Forward When Wanted Merge Commit**

**Problem:**

```bash
$ git merge feature-branch
Updating abc1234..def5678
Fast-forward  ← Didn't want this!
```

**Prevention:**

```bash
# Use --no-ff to force merge commit
$ git merge --no-ff feature-branch

# Or set as repository default
$ git config merge.ff false
```

**Fix After the Fact:**

```bash
# Undo the fast-forward
$ git reset --hard HEAD~1  # Move main back one commit

# Redo with --no-ff
$ git merge --no-ff feature-branch
```

**Issue #3: Forgot to Write Good Merge Commit Message**

**Problem:** You accepted the default "Merge branch 'feature-branch'" message without adding context.

**Fix:**

```bash
# Immediately after merge, amend the message
$ git commit --amend

# Editor opens, write better message:
Merge branch 'feature/add-caching'

Added Redis caching layer to improve API response times.
Reduces average response time from 450ms to 80ms.
...
```

**Issue #4: Want to Undo a Merge**

**Scenario:** You merged but realized the feature isn't ready.

**Solution - Before Pushing:**

```bash
# Reset to before the merge
$ git reset --hard HEAD~1
# This removes the merge commit completely
# Your local main is back to pre-merge state
```

**Solution - After Pushing:**

```bash
# Create a revert commit
$ git revert -m 1 HEAD
# This creates a new commit that undoes the merge
# Preserves history that merge happened and was reverted
```

The `-m 1` flag tells Git to revert to the first parent (main branch) state before the merge.

---

#### 1.1.5 Fast-Forward vs. Recursive: Side-by-Side Comparison

##### Visual Comparison

**Fast-Forward Merge:**

```
Before:
main:        A---B---C
                      \
feature:               C---D---E

After:
main:        A---B---C---D---E
feature:               C---D---E (can be deleted)

History: A---B---C---D---E (linear)
```

**Recursive Merge:**

```
Before:
main:        A---B---C---F
                      \
feature:               C---D---E

After:
main:        A---B---C---F---M
                      \       /
feature:               C---D---E (can be deleted)

History: Shows branching and merge point M
```

##### Comprehensive Comparison Table

|Aspect|Fast-Forward Merge|Recursive Merge|
|---|---|---|
|**Trigger Condition**|Destination branch unchanged|Both branches have new commits|
|**Command**|`git merge <branch>` (automatic)<br>`git merge --ff-only <branch>` (forced)|`git merge <branch>` (when needed)<br>`git merge --no-ff <branch>` (forced)|
|**Commits Created**|0 (just moves pointer)|1 (merge commit)|
|**Commit Parents**|N/A (no new commit)|2 (both branch tips)|
|**History Shape**|Linear (straight line)|Branched (shows divergence)|
|**Branch Context**|Lost (can't tell work was on branch)|Preserved (clear branch history)|
|**Visual Complexity**|Simple|Shows branching pattern|
|**Revert Method**|`git reset` to previous commit|`git revert -m 1 <merge-commit>`|
|**Revert Difficulty**|Easy|Moderate|
|**Best For**|Quick fixes, simple features, solo work|Complex features, team collaboration|
|**Common In**|Solo development, linear workflows|Team environments, parallel development|
|**Documentation**|Commit messages on individual commits|Merge commit provides integration context|
|**Audit Trail**|Individual commits only|Shows when feature was integrated|
|**Use Case Example**|Hotfix for typo|Adding new ML model pipeline|
|**Team Scalability**|Breaks down with multiple developers|Scales well to large teams|
|**History Reading**|Easy to follow chronologically|Requires graph view to understand|
|**Merge Commit Message**|N/A|Opportunity to document feature|
|**Default Behavior**|Git prefers when possible|Git uses when necessary|

##### Decision Matrix: Which Merge Type to Use?

Use this flowchart-style decision guide:

```
START: Ready to merge feature into main?
    |
    v
Is main unchanged since you branched?
    |
    +--- YES --> Is this a quick/simple fix?
    |               |
    |               +--- YES --> Use Fast-Forward
    |               |           `git merge feature`
    |               |
    |               +--- NO --> Do you want to preserve branch context?
    |                           |
    |                           +--- YES --> Use Recursive (--no-ff)
    |                           |           `git merge --no-ff feature`
    |                           |
    |                           +--- NO --> Use Fast-Forward
    |                                       `git merge feature`
    |
    +--- NO --> Recursive merge required
                (main has advanced)
                `git merge feature`
```

##### Example Scenarios with Recommendations

**Scenario 1: Typo Fix**

```
Situation: Found typo in README, fixed in 1 commit
Branch age: 10 minutes
Main status: Unchanged
Team size: Solo developer

Recommendation: Fast-Forward
Reasoning: Trivial change doesn't need branching context
Command: git merge --ff-only fix/readme-typo
```

**Scenario 2: New Feature (Solo)**

```
Situation: Added data validation module
Branch age: 2 days
Main status: Unchanged (you're solo developer)
Commits: 5 well-structured commits

Recommendation: Recursive (--no-ff)
Reasoning: Document this feature as a cohesive unit
Command: git merge --no-ff feature/data-validation
```

**Scenario 3: New Feature (Team)**

```
Situation: Added ML model training pipeline
Branch age: 1 week
Main status: Advanced (teammates merged their work)
Team size: 5 developers

Recommendation: Recursive (required)
Reasoning: Branches diverged, must use recursive
Command: git merge feature/ml-training
```

**Scenario 4: Critical Hotfix**

```
Situation: Production bug causing data loss
Branch age: 30 minutes
Main status: Unchanged
Urgency: Critical

Recommendation: Fast-Forward
Reasoning: Need clean deployment, no time for merge commit ceremony
Command: git merge --ff-only hotfix/data-loss-bug
```

**Scenario 5: Experimental Feature**

```
Situation: Testing new algorithm approach
Branch age: 3 weeks
Main status: Advanced significantly
Might revert: Yes, if performance not better

Recommendation: Recursive (--no-ff even if ff possible)
Reasoning: Easy to revert merge commit if experiment fails
Command: git merge --no-ff feature/experimental-algorithm
```

---

#### 1.1.6 Merge Strategies: Philosophy and Team Practices

##### The Git Philosophy on Merging

Git's design philosophy around merging reflects several core principles:

**Principle 1: Distributed Collaboration**

- Multiple developers can work independently
- No single point of failure
- Branches are lightweight and encourage experimentation

**Principle 2: History Preservation**

- All history is preserved by default
- Can always trace back to understand decisions
- Supports accountability and auditing

**Principle 3: Flexibility**

- Multiple strategies available for different situations
- Teams can adopt workflows that match their needs
- No "one size fits all" approach

##### Common Team Merge Policies

Different teams adopt different policies based on their needs. Here are common approaches:

###### Policy 1: Always Preserve History (GitHub Flow Style)

**Rule:** Always use `--no-ff` for feature merges

**Configuration:**

```bash
$ git config --global merge.ff false
# Forces merge commits for all merges
```

**Pros:**

- Clear feature boundaries in history
- Easy to see what was developed when
- Simple to revert entire features
- Excellent audit trail

**Cons:**

- More merge commits
- History graph more complex
- Takes slightly more time

**Best For:**

- Teams with compliance requirements
- Projects needing detailed audit trails
- When features might need rollback

**Example Workflow:**

```bash
# Create feature
$ git checkout -b feature/user-auth main

# Work and commit
$ git commit -am "Add authentication"

# Ready to merge
$ git checkout main
$ git merge --no-ff feature/user-auth
# Always creates merge commit, even if ff possible
```

###### Policy 2: Clean Linear History (Rebase Workflow)

**Rule:** Rebase before merging to keep history linear

**Workflow:**

```bash
# Update feature with latest main
$ git checkout feature/new-feature
$ git rebase main  # Replay commits on top of main

# Merge with fast-forward
$ git checkout main
$ git merge feature/new-feature  # Now ff is possible
```

**Pros:**

- Perfectly linear history
- Easy to understand chronologically
- Clean `git log` output

**Cons:**

- Rewrites history (can be confusing)
- More complex workflow
- Loses parallel development context

**Best For:**

- Solo developers
- Small teams with good communication
- Projects preferring simplicity over detail

**Note:** Rebasing is covered in detail in Section 1.3.

###### Policy 3: Hybrid Approach

**Rule:** Fast-forward for trivial changes, recursive for features

**Decision Criteria:**

```bash
# Trivial changes (typos, small fixes)
$ git merge hotfix/typo  # Allow ff

# Features and significant changes  
$ git merge --no-ff feature/major-change  # Force merge commit
```

**Pros:**

- Balance between clarity and simplicity
- Documents important changes
- Keeps history reasonably clean

**Cons:**

- Requires judgment calls
- Can be inconsistent if team doesn't agree

**Best For:**

- Most teams
- Projects with mixed change types
- Balancing history clarity with simplicity

##### Setting Team-Wide Merge Policies

**Repository Configuration:**

Create a `.gitconfig` file in your repository:

```bash
# In repository root
$ cat > .gitconfig << EOF
[merge]
    # Prevent fast-forward for feature branches
    ff = false
    
[branch "main"]
    # Protect main branch settings
    mergeoptions = --no-ff
EOF
```

**Git Hooks for Enforcement:**

Create a pre-merge hook to enforce policies:

```bash
# .git/hooks/pre-merge-commit
#!/bin/bash

# Get the branch being merged
branch=$(git symbolic-ref --short HEAD)

# Check if merging into main
if [ "$branch" = "main" ]; then
    # Ensure --no-ff was used by checking for merge commit
    if ! git log -1 --format=%P | grep -q " "; then
        echo "Error: Merges to main must use --no-ff"
        exit 1
    fi
fi

exit 0
```

**Documentation:**

Document your team's merge policy in `CONTRIBUTING.md`:

````markdown
## Merge Policy

### Feature Branches
- Always use `--no-ff` when merging to main
- Write descriptive merge commit messages
- Reference issue numbers in merge commits

### Hotfixes  
- Fast-forward merges acceptable
- Must include "Hotfix:" in commit message

### Example
```bash
# Feature merge
$ git merge --no-ff feature/new-analytics

# Hotfix merge
$ git merge --ff-only hotfix/critical-bug
````

````

---

### 1.1.7 Practical Tips for Data Scientists and Data Engineers

#### Data Science Workflows

**Scenario 1: Experimenting with Models**

**Challenge:** You're trying different ML models, and you want to keep experiments isolated.

**Solution:**
```bash
# Create branch for each experiment
$ git checkout -b experiment/random-forest main
$ # ... train random forest ...
$ git commit -am "Random Forest: 85% accuracy"

$ git checkout -b experiment/xgboost main  
$ # ... train XGBoost ...
$ git commit -am "XGBoost: 91% accuracy"

# Merge the winner
$ git checkout main
$ git merge --no-ff experiment/xgboost

# Keep merge commit message detailed:
Merge experiment/xgboost

XGBoost achieved 91% accuracy vs 85% for Random Forest.
Selected XGBoost for production due to:
- 6% accuracy improvement
- Better recall on minority class (0.87 vs 0.79)
- Acceptable training time (12 min vs 8 min)

Model parameters:
- max_depth: 7
- learning_rate: 0.1
- n_estimators: 200

Training data: 2024-10-01 to 2025-10-31
````

**Scenario 2: Notebook Development**

**Challenge:** Jupyter notebooks create large diffs, making merges complex.

**Best Practice:**

```bash
# Create feature branch for analysis
$ git checkout -b analysis/customer-segmentation main

# Work in notebook
$ # ... develop analysis ...

# Before committing, clear output
$ jupyter nbconvert --clear-output --inplace analysis.ipynb

# Commit only the code
$ git add analysis.ipynb
$ git commit -m "Add customer segmentation analysis"

# When merging back
$ git checkout main
$ git merge --no-ff analysis/customer-segmentation
```

**Pro Tip - Notebook Merging:**

```bash
# Install nbdime for better notebook diffs/merges
$ pip install nbdime
$ nbdime config-git --enable --global

# Now Git understands notebook structure better
```

**Scenario 3: Data Pipeline Development**

**Challenge:** Pipeline changes might affect multiple stages, need to test integration.

**Best Practice:**

```bash
# Branch for pipeline changes
$ git checkout -b feature/add-data-quality-checks main

# Make changes to pipeline stages
$ git commit -am "Add schema validation to ingestion"
$ git commit -am "Add data quality metrics to transformation"
$ git commit -am "Add quality gates to loading"

# Test full pipeline before merging
$ make test-pipeline

# Merge with detailed documentation
$ git checkout main
$ git merge --no-ff feature/add-data-quality-checks

# Merge commit message:
Merge feature/add-data-quality-checks

Added comprehensive data quality framework to the ETL pipeline.

Quality checks implemented:
- Schema validation at ingestion (rejects invalid records)
- Completeness metrics in transformation (logs missing data %)
- Consistency checks before loading (validates referential integrity)

Impact:
- Reduced bad data in warehouse by 94%
- Pipeline execution time +8% (acceptable for quality gain)
- Detected and prevented 3 data issues in staging during testing

Configuration: config/data_quality_rules.yaml
```

##### Data Engineering Workflows

**Scenario 4: Infrastructure Changes**

**Challenge:** Database schema changes or infrastructure updates need careful tracking.

**Best Practice:**

```bash
# Branch for schema migration
$ git checkout -b migration/add-user-preferences-table main

# Create migration files
$ git add migrations/0012_add_user_preferences.sql
$ git commit -m "Add user preferences table migration"

# Add rollback script
$ git add migrations/0012_rollback.sql
$ git commit -m "Add rollback for user preferences migration"

# Test migration
$ make test-migration

# Merge with infrastructure details
$ git checkout main
$ git merge --no-ff migration/add-user-preferences-table

# Detailed merge commit:
Merge migration/add-user-preferences-table

Added user preferences storage for personalization features.

Database changes:
- New table: user_preferences (indexed on user_id)
- FK constraint to users table
- Default values for all preference columns

Migration strategy:
- Zero-downtime deployment using shadow table approach
- Estimated migration time: 3 minutes for 10M users
- Rollback script tested in staging

Deployment notes:
1. Deploy code first (backward compatible)
2. Run migration during low-traffic window
3. Monitor for 24h before removing rollback capability

Related tickets: INFRA-456, FEAT-789
```

**Scenario 5: Performance Optimizations**

**Challenge:** Optimizations need clear before/after documentation.

**Best Practice:**

```bash
# Branch for optimization
$ git checkout -b perf/optimize-aggregation-query main

# Benchmark before
$ make benchmark > benchmarks/before.txt

# Make optimization changes
$ git commit -am "Replace nested loops with vectorized operations"

# Benchmark after
$ make benchmark > benchmarks/after.txt

# Commit benchmark results
$ git add benchmarks/
$ git commit -m "Add benchmark results for optimization"

# Merge with performance data
$ git checkout main
$ git merge --no-ff perf/optimize-aggregation-query

# Merge commit with metrics:
Merge perf/optimize-aggregation-query

Optimized aggregation query performance by 15x.

Performance improvements:
- Query execution: 45s → 3s (15x faster)
- Memory usage: 8GB → 2GB (75% reduction)
- CPU utilization: 95% → 40% (better headroom)

Approach:
- Replaced Python loops with NumPy vectorized operations
- Pre-computed commonly used aggregates
- Added result caching for repeated queries

Benchmarks included in benchmarks/ directory.
Tested with production data volume (50M records).

Impact: Dashboard load time reduced from 60s to 5s.
```

---

#### 1.1.8 Troubleshooting Common Merge Issues

##### Issue: "Already up to date" When You Expected Changes

**Symptom:**

```bash
$ git merge feature-branch
Already up to date.
```

**Causes:**

**1. You're Already Merged**

```bash
# Check if feature-branch commits are in main
$ git log --oneline main..feature-branch
# If empty, feature-branch is already merged
```

**2. You're on the Wrong Branch**

```bash
# Check current branch
$ git branch
  main
* feature-branch  ← You're on the feature branch!

# You tried to merge into yourself
```

**3. Branches Point to Same Commit**

```bash
$ git log --oneline main feature-branch -1
abc1234 (HEAD -> main, feature-branch) Latest commit
# Both at same commit
```

**Solutions:**

```bash
# Make sure you're on the destination branch
$ git checkout main
$ git merge feature-branch

# Verify branches are actually different
$ git log --oneline --graph main feature-branch
```

##### Issue: "Merge Conflicts" on First Merge Attempt

**Symptom:**

```bash
$ git merge feature-branch
Auto-merging data_processor.py
CONFLICT (content): Merge conflict in data_processor.py
Automatic merge failed; fix conflicts and then commit the result.
```

**Understanding:**

- This is normal when both branches modified the same code
- Git paused the merge for you to resolve conflicts
- Your working directory now contains conflict markers

**Resolution Steps:**

**Step 1: Identify Conflicted Files**

```bash
$ git status
On branch main
You have unmerged paths.

Unmerged paths:
  both modified:   data_processor.py
  both modified:   config.yaml
```

**Step 2: Open and Resolve Conflicts**

```bash
$ cat data_processor.py
def process_data(df):
<<<<<<< HEAD (Current Change - Main)
    # Version from main branch
    return df.dropna().reset_index(drop=True)
=======
    # Version from feature branch
    return df.fillna(method='ffill').reset_index(drop=True)
>>>>>>> feature-branch (Incoming Change)
```

**Step 3: Choose Resolution**

```python
# Option 1: Keep only main's version
def process_data(df):
    return df.dropna().reset_index(drop=True)

# Option 2: Keep only feature's version  
def process_data(df):
    return df.fillna(method='ffill').reset_index(drop=True)

# Option 3: Combine both (best for this case)
def process_data(df):
    # Fill forward first, then drop remaining NaNs
    return df.fillna(method='ffill').dropna().reset_index(drop=True)
```

**Step 4: Mark as Resolved**

```bash
$ git add data_processor.py config.yaml
$ git status
On branch main
All conflicts fixed but you are still merging.
```

**Step 5: Complete the Merge**

```bash
$ git commit -m "Merge feature-branch

Resolved conflicts in data_processor.py by combining both approaches:
- Use forward fill from feature branch
- Then apply dropna from main branch
This gives us the benefits of both strategies."
```

**Note:** Conflict resolution is covered extensively in Section 1.2.

##### Issue: Wanted Fast-Forward but Got Recursive

**Symptom:**

```bash
$ git merge feature-branch
Merge made by the 'ort' strategy.  ← Didn't want merge commit
```

**Cause:** Main branch advanced while you were working on feature branch.

**Prevention:**

```bash
# Use --ff-only to fail instead of creating merge commit
$ git merge --ff-only feature-branch
fatal: Not possible to fast-forward, aborting.

# Now you can decide how to proceed:

# Option 1: Accept recursive merge
$ git merge feature-branch

# Option 2: Rebase to make ff possible (Section 1.3)
$ git checkout feature-branch
$ git rebase main
$ git checkout main
$ git merge feature-branch  # Now ff works
```

##### Issue: Accidentally Merged Wrong Branch

**Symptom:**

```bash
$ git merge feature-wrong-one
# Oh no! I meant to merge feature-right-one!
```

**Solution - Before Pushing:**

```bash
# Undo the merge
$ git reset --hard HEAD~1
# This removes the merge commit
# Now you can merge the correct branch

$ git merge feature-right-one
```

**Solution - After Pushing:**

```bash
# Create a revert commit
$ git revert -m 1 HEAD
# This creates a new commit undoing the wrong merge

# Now merge the correct branch
$ git merge feature-right-one
```

##### Issue: Merge Happened but Files Don't Look Right

**Symptom:** After merge, expected files are missing or wrong version.

**Diagnosis:**

```bash
# Check what was merged
$ git log --oneline -1
abc1234 (HEAD -> main) Merge branch 'feature-branch'

# Check what files changed in the merge
$ git show HEAD --name-only

# Check diff of merge
$ git show HEAD
```

**Common Causes:**

**1. Merged into Wrong Branch**

```bash
$ git branch
* feature-other  ← You're not on main!
  main

# You merged INTO feature-other instead of merging feature-other INTO main
```

**2. Conflict Resolution Was Wrong**

```bash
# Review the merge commit
$ git show HEAD

# If resolution was wrong, amend it
$ git checkout HEAD -- problematic_file.py  # Reset to original
$ # Fix the file properly
$ git add problematic_file.py
$ git commit --amend
```

**3. Expected Files Were in Different Branch**

```bash
# Verify what branch has the changes
$ git log --all --oneline -- missing_file.py
# This shows which branches touched that file
```

---

#### 1.1.9 Summary and Key Takeaways

##### Core Concepts Mastered

**1. Git Merge Fundamentals**

- Merging integrates changes from one branch into another
- Git uses different strategies based on branch state
- Two primary types: fast-forward and recursive

**2. Fast-Forward Merges**

- Occurs when destination branch is unchanged
- Simply moves branch pointer forward
- No merge commit created
- Maintains linear history
- Best for simple changes and solo development

**3. Recursive Merges**

- Occurs when both branches have new commits
- Creates merge commit with two parents
- Preserves branching history
- Shows parallel development
- Best for complex features and team collaboration

##### Decision Framework

**Use Fast-Forward When:**

- ✓ Quick fixes and small changes
- ✓ Solo development
- ✓ Main hasn't changed since branching
- ✓ Prefer simple, linear history

**Use Recursive When:**

- ✓ Complex features
- ✓ Team collaboration
- ✓ Want to preserve context
- ✓ Might need to revert feature
- ✓ Main has advanced (required)

##### Essential Commands Reference

```bash
# Check current state
$ git status
$ git log --oneline --graph --all

# Fast-forward merge (default)
$ git merge <branch>

# Force fast-forward (fail if not possible)
$ git merge --ff-only <branch>

# Force merge commit (even if ff possible)
$ git merge --no-ff <branch>

# Undo merge (before push)
$ git reset --hard HEAD~1

# Revert merge (after push)
$ git revert -m 1 HEAD

# Clean up merged branch
$ git branch -d <branch>
```

##### Best Practices Checklist

**Before Every Merge:**

- [ ] Clean working directory (`git status`)
- [ ] On correct destination branch
- [ ] Reviewed changes (`git log`, `git diff`)
- [ ] Tests passing
- [ ] Feature complete and tested

**During Merge:**

- [ ] Write meaningful merge commit messages
- [ ] Document important context
- [ ] Reference issue numbers
- [ ] Include performance metrics if relevant

**After Merge:**

- [ ] Verify result (`git log --graph`)
- [ ] Run tests
- [ ] Delete merged feature branch
- [ ] Push changes to remote
- [ ] Notify team if relevant

##### Common Pitfalls to Avoid

**❌ Don't:**

- Merge without checking working directory status
- Accept default merge messages without adding context
- Forget to delete merged branches
- Merge into the wrong branch
- Skip testing after merge
- Use fast-forward for complex features
- Use recursive for trivial changes

**✅ Do:**

- Check status before every merge
- Write detailed merge commit messages
- Clean up branches after merging
- Verify you're on the correct branch
- Test thoroughly after merging
- Choose strategy based on change complexity
- Match strategy to team policy

##### What's Next?

This section covered the fundamentals of merge types. The next sections build on this foundation:

**Section 1.2: Complex Merge Scenarios**

- Advanced conflict resolution
- Three-way merge details
- Merge conflict strategies
- Handling binary files
- Team merge workflows

**Section 1.3: Advanced Branch Integration - Git Rebasing**

- Alternative to merging
- Rewriting history safely
- Interactive rebasing
- When to rebase vs. merge
- Golden rules of rebasing

##### Practice Exercises

To solidify your understanding, practice these scenarios:

**Exercise 1: Fast-Forward Merge**

```bash
$ git checkout -b practice/fast-forward main
$ echo "test" > file.txt
$ git add file.txt
$ git commit -m "Add test file"
$ git checkout main
$ git merge practice/fast-forward
# Verify it was fast-forward
$ git log --oneline --graph -3
```

**Exercise 2: Forced Merge Commit**

```bash
$ git checkout -b practice/merge-commit main
$ echo "test2" > file2.txt
$ git add file2.txt
$ git commit -m "Add test file 2"
$ git checkout main
$ git merge --no-ff practice/merge-commit
# Verify merge commit was created
$ git log --oneline --graph -3
```

**Exercise 3: Recursive Merge (Diverged Branches)**

```bash
# Create feature branch
$ git checkout -b practice/diverged main
$ echo "feature" > feature.txt
$ git add feature.txt
$ git commit -m "Feature work"

# Advance main
$ git checkout main
$ echo "main" > main.txt
$ git add main.txt
$ git commit -m "Main work"

# Merge (will be recursive)
$ git merge practice/diverged
$ git log --oneline --graph -5
```

---

#### 1.1.10 Quick Reference Guide

##### Visual Decision Tree

```
Ready to merge?
    |
    +---> Is main unchanged since branch creation?
    |         |
    |         YES ---> Use: git merge <branch>
    |         |        Result: Fast-forward
    |         |
    |         NO ----> Use: git merge <branch>
    |                  Result: Recursive (merge commit)
    |
    +---> Want to force merge commit even if FF possible?
    |         |
    |         YES ---> Use: git merge --no-ff <branch>
    |                  Result: Merge commit
    |
    +---> Want to ensure FF (fail if not possible)?
              |
              YES ---> Use: git merge --ff-only <branch>
                       Result: FF or error
```

##### Command Quick Reference

|Goal|Command|Result|
|---|---|---|
|Merge (auto-strategy)|`git merge feature`|FF if possible, else recursive|
|Ensure fast-forward|`git merge --ff-only feature`|FF or fail|
|Force merge commit|`git merge --no-ff feature`|Always creates merge commit|
|Check if FF possible|`git merge --ff-only --no-commit feature`|Test without committing|
|See what would merge|`git diff main..feature`|Preview changes|
|Undo last merge|`git reset --hard HEAD~1`|Before push only|
|Revert merged changes|`git revert -m 1 HEAD`|After push|
|Delete merged branch|`git branch -d feature`|Clean up|

##### Mental Models Summary

**Fast-Forward = Moving Your Bookmark**

- Main is at page 10
- Feature progressed to page 15
- Just move main's bookmark to page 15
- Same book, same story, just catching up

**Recursive = Weaving Two Threads**

- Main thread went one direction
- Feature thread went another direction
- Merge weaves them together
- New knot shows where they joined

##### Key Terms Glossary

- **Merge**: Combining changes from one branch into another
- **Fast-Forward**: Moving branch pointer forward without creating new commit
- **Recursive Merge**: Creating new merge commit to combine diverged branches
- **Three-Way Merge**: Same as recursive; uses three points (base, ours, theirs)
- **Merge Commit**: Commit with two parents, documenting integration point
- **Merge Strategy**: Algorithm Git uses to perform the merge
- **Common Ancestor**: Last commit before branches diverged
- **Source Branch**: Branch being merged FROM
- **Destination Branch**: Branch being merged INTO
- **--ff-only**: Flag to force fast-forward or fail
- **--no-ff**: Flag to force merge commit creation
- **Ort Strategy**: Modern implementation of recursive strategy (Git 2.33+)

---

### Looking Ahead

You now have a solid understanding of the two primary merge types in Git. Section 1.1 provided the foundation for understanding how Git combines work from different branches.

In **Section 1.2: Complex Merge Scenarios**, we'll dive deeper into:

- Handling merge conflicts like a pro
- Advanced three-way merge techniques
- Strategies for minimizing conflicts
- Tools and workflows for conflict resolution
- Team collaboration patterns

In **Section 1.3: Advanced Branch Integration - Git Rebasing**, we'll explore an alternative to merging:

- What rebasing is and how it differs from merging
- When to use rebase vs. merge
- Interactive rebasing for history cleanup
- The golden rules of rebasing
- Avoiding common rebasing pitfalls

These skills build on your merge type knowledge to make you proficient in advanced Git workflows used in professional data science and data engineering environments.

---

### 1.2 Complex Merge Scenarios



### 1.3 Advanced Branch Integration: Git Rebasing



## 2. Git History and Exploration

In this chapter, you will develop skills for investigating and managing your project's Git history. You'll learn techniques for selectively applying changes, identifying and fixing bugs, and managing sensitive information in your repository. These tools will help you maintain clean, traceable code in data engineering workflows.

### 2.1 Cherry-Picking


### 2.2 Bisect


### 2.3 Git Filter Repo



### 2.4 Git reflog



## 3. Advanced Repository Management

In this chapter, you will explore advanced Git techniques for managing complex software projects. You'll learn how to work on multiple features simultaneously, organize code dependencies, handle large files, and implement efficient development workflows. These skills are essential for managing modern data engineering and software development projects.

### 3.1 Git Worktrees


### 3.2 Git Submodules


### 3.3 Git Large File Storage


### 3.4 Trunk Based Development


