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
