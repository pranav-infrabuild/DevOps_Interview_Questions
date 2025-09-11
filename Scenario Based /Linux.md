<p align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&color=0:000000,100:FFD700&height=140&section=header&text=%F0%9F%90%A7%20LINUX%20SCENARIO%20BASED%20QUESTIONS&fontSize=28&fontColor=ffffff" />
</p>



### 1) You are troubleshooting a large log file and need to view the top and bottom portions quickly. What is the use of head and tail commands, and how would you write the syntax for each?
<details>

The `head` and `tail` commands are super useful when you're working with large log files and just need a quick look at the beginning or end of the file without opening the whole thing.

---

### ✅ **Purpose of `head` and `tail`:**

- **`head`**: Displays the **first** few lines of a file (default is 10).
- **`tail`**: Displays the **last** few lines of a file (default is 10).

---

### 🧾 **Syntax:**

#### ▶ `head` Command:
```bash
head filename.log
```
- Shows the **first 10 lines** of `filename.log`.

To specify a number of lines:
```bash
head -n 20 filename.log
```
- Shows the **first 20 lines** of the file.

---

#### ▶ `tail` Command:
```bash
tail filename.log
```
- Shows the **last 10 lines** of `filename.log`.

To specify a number of lines:
```bash
tail -n 20 filename.log
```
- Shows the **last 20 lines**.

To continuously monitor the log in real-time (great for logs):
```bash
tail -f filename.log
```

---


</details>
