<p align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&color=0:000000,100:00FF00&height=140&section=header&text=%F0%9F%92%BB%20GIT%20SCENARIO%20BASED%20QUESTIONS&fontSize=28&fontColor=ffffff" />
</p>


### Question 1. We have 50 commits in Develop branch and out of which only 5 commits needs to be pushed in Release branch (Which will eventually deployed in production} ?
<details>

To push only 5 specific commits from the Develop branch to the Release branch in Git, you can use the **`git cherry-pick`** command.  

1. **Cherry-Pick the Commits**: Use `git cherry-pick` to apply the selected commits from the Develop branch to the Release branch.
   ```bash
   git cherry-pick <commit-sha1> <commit-sha2> <commit-sha3> <commit-sha4> <commit-sha5>
   ```





</details>

### Question 2. As a DevOps Engineer, how do you log in to a server? Can you explain the different ways depending on the type of server and environment?

<details>

### 1. Linux Servers (SSH)
- Use **SSH (Secure Shell)** to connect.
- Command:
  ```bash
  ssh username@server-ip

- With a private key:
- Command:
  ```bash
  ssh -i /path/to/private-key.pem username@server-i


</details>


### Question 3. How do you create a user with a non-interactive shell in Linux, and why would you use it?

<details>

We can create a user with **no interactive login access** by assigning a **non-interactive shell** such as `/sbin/nologin` or `/bin/false`

### 🔹 Command:
```bash
sudo useradd -m -s /sbin/nologin myuser
```

</details>

### Question 4. You are working on a project where a developer named **kareem** requires access to a server only until **March 28, 2024**.  How would you create a **temporary user account** in Linux that automatically expires on that date?  

<details>
In Linux, you can use the `useradd` command with the `-e` option to set an expiry date for a user account.

```bash
sudo useradd -e 2024-03-28 kareem
```
You can verify expiry details using:
```bash
sudo chage -l kareem
```
</details>

### Question 5. What happened? **You ran:** `git reset --hard`

<details>


**Result:** Lost 48 hours of work

---

### You checked:

```bash
git log
```

**Output:** ❌ Nothing there → **Panic mode**

---

## Good News: Your Code is NOT Gone

### Why the code is NOT lost

🔹 **Git keeps a hidden history**

- `git log` → shows project history
- `git reflog` → shows **your actions history**

👉 `git reflog` tracks **every move of HEAD**:
- Branch switch
- Commit
- Reset
- Rebase
- Everything

**Think of it as:**
> Git's secret undo button

---

## The ONE Command That Saves You 🧯

```bash
git reflog
```

### Example Output:

```text
abc1234 HEAD@{0}: reset: moving to HEAD~1
def5678 HEAD@{1}: commit: Added payment logic
ghi9012 HEAD@{2}: commit: Fixed bug in authentication
jkl3456 HEAD@{3}: checkout: moving from main to feature-branch
```

**Each line is a restore point.**

---

## How to Recover Your Lost Code

### ✅ 2 Steps Only

#### 1️⃣ Find the commit before disaster

```bash
git reflog
```

**Copy the commit hash** (example: `abc1234`)

---

#### 2️⃣ Restore it

```bash
git reset --hard abc1234
```

✅ **Boom — your work is back!**

---

## Why This Works

### Easy Explanation

**Git is like a file system:**

1. Deleting or resetting does **NOT** erase data immediately
2. Git only removes the **label** (branch/HEAD pointer)
3. The data stays as a **dangling commit**
4. Git cleans it up **weeks later**, not immediately

**So you still have time ⏳**

---

## Complete Recovery Example

### Step-by-Step Walkthrough

#### Before Disaster:
```bash
# You have commits
git log --oneline
# abc1234 Added payment logic
# def5678 Fixed authentication bug
# ghi9012 Initial commit
```

#### The Mistake:
```bash
# Accidentally reset to previous commit
git reset --hard HEAD~2

# Check git log
git log --oneline
# ghi9012 Initial commit
# ❌ Your last 2 commits are gone!
```

#### The Recovery:
```bash
# 1. Check reflog
git reflog
# abc1234 HEAD@{0}: reset: moving to HEAD~2
# def5678 HEAD@{1}: commit: Fixed authentication bug
# abc1234 HEAD@{2}: commit: Added payment logic  ← This one!

# 2. Restore to the commit you want
git reset --hard abc1234

# 3. Verify recovery
git log --oneline
# abc1234 Added payment logic  ✅
# def5678 Fixed authentication bug  ✅
# ghi9012 Initial commit  ✅
```

**✅ Everything is back!**

---

</details>
