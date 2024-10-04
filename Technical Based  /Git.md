# Git Interview Questions 

### Question 1: What is the difference between git merge and git rebase ?
<details>

- Git Merge :-
1. git merge integrates changes from another branch into your current branch.
2. It creates a new commit that combines the histories of both branches.

- Git Rebase :-
1. Rebase takes the commits from one branch and applies them on top of another branch.
2. It rewrites the commit history.
3. It's useful for keeping a clean and linear history.


</details>


### Some Basic Commands 
<details>

Here are some essential Git commands that are commonly asked in a DevOps interview, along with simple explanations:

### 1. **git init**
   - **Explanation**: This command initializes a new Git repository in your current directory. It sets up all the necessary files and folders for Git to start tracking your project.

   **Example**: 
   ```bash
   git init
   ```

### 2. **git clone**
   - **Explanation**: This command is used to copy an existing Git repository from a remote location (like GitHub) to your local machine.

   **Example**:
   ```bash
   git clone https://github.com/username/repository.git
   ```

### 3. **git status**
   - **Explanation**: This command shows the current status of your repository. It lets you know which files have been modified, which are staged (ready to be committed), and which are untracked (not yet added to version control).

   **Example**:
   ```bash
   git status
   ```

### 4. **git add**
   - **Explanation**: This command adds changes (like modified or new files) to the staging area, so they can be committed. Think of it like preparing files for a snapshot.

   **Example**:
   ```bash
   git add filename.txt
   # or to add all files
   git add .
   ```

### 5. **git commit -m "message"**
   - **Explanation**: This command saves the changes from the staging area with a descriptive message, creating a "snapshot" in the project’s history.

   **Example**:
   ```bash
   git commit -m "Added feature X"
   ```

### 6. **git push**
   - **Explanation**: This command sends your local commits to a remote repository, like GitHub. It's like publishing your changes so others can see and access them.

   **Example**:
   ```bash
   git push origin main
   ```

### 7. **git pull**
   - **Explanation**: This command updates your local repository with the latest changes from a remote repository. It’s like downloading updates made by others.

   **Example**:
   ```bash
   git pull origin main
   ```

### 8. **git branch**
   - **Explanation**: This command shows all the branches in your repository. Branches are different versions of your project where you can work on separate features or fixes.

   **Example**:
   ```bash
   git branch
   ```

### 9. **git checkout -b branch_name**
   - **Explanation**: This creates a new branch and switches to it. It’s useful when you want to work on a new feature or bug fix without affecting the main project.

   **Example**:
   ```bash
   git checkout -b new-feature
   ```

### 10. **git merge**
   - **Explanation**: This command combines the changes from one branch into another. It’s typically used when you want to integrate the new features or fixes from your feature branch back into the main branch.

   **Example**:
   ```bash
   git merge new-feature
   ```

### 11. **git log**
   - **Explanation**: This command shows the commit history of the repository. It lists all the commits made, including who made them and when.

   **Example**:
   ```bash
   git log
   ```

### 12. **git reset**
   - **Explanation**: This command undoes changes. It can be used to unstage changes or even to undo commits.

   **Example**:
   ```bash
   git reset HEAD filename.txt
   ```

### 13. **git stash**
   - **Explanation**: This command temporarily saves changes that you aren’t ready to commit yet. It’s like putting your changes in a “safe place” while you work on something else.

   **Example**:
   ```bash
   git stash
   ```

### 14. **git fetch**
   - **Explanation**: This command retrieves the latest changes from the remote repository but does not merge them into your working directory.

   **Example**:
   ```bash
   git fetch origin
   ```

### 15. **git diff**
   - **Explanation**: This command shows the differences between your working directory and the latest commit or between commits. It helps you review changes before staging or committing them.

   **Example**:
   ```bash
   git diff
   ```

These commands are essential for managing code versions, collaborating with others, and ensuring smooth project development. Having a strong understanding of Git will help you in a DevOps role!
</details>


### Question 3. What is difference in git , GitHub and gitlab ?
<details>
 
- Git is the tool you use to track changes in your code locally.
- GitHub is an online platform for sharing Git repositories and collaborating with others.
- GitLab is a more advanced Git hosting platform with built-in DevOps tools and can be self-hosted.


</details>

### Question 4. How do you revert a commit in Git ?
<details>
git revert <commit_hash>: Creates a new commit to undo a previous commit (preserves history).

</details>

### Question 5. Explain the difference between git clone and git fork ?
<details>

- What it is: git clone is used to copy an existing Git repository from a remote source (like GitHub or GitLab) to your local computer.
- What it is: Forking is used on platforms like GitHub or GitLab to create a copy of someone else’s repository into your own account.

</details>
