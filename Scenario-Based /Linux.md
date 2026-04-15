<p align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&color=0:000000,100:FFD700&height=140&section=header&text=%F0%9F%90%A7%20LINUX%20SCENARIO%20BASED%20QUESTIONS&fontSize=28&fontColor=ffffff" />
</p>



### 1. You are troubleshooting a large log file and need to view the top and bottom portions quickly. What is the use of head and tail commands, and how would you write the syntax for each?
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

### 2. How to Convert Static IP to Dynamic IP

<details>

## 🔍 First Understand the Difference

| Static IP | Dynamic IP |
|----------|-----------|
| Fixed IP — always stays the same | Gets a new IP every time |
| Slightly higher cost | Low or free |
| Used for production servers, DNS records | Used for dev/test environments |

---

## ☁️ In Azure — Via Portal

1. Go to Azure Portal → Public IP Address resource  
2. Click the **Configuration** tab  
3. Change **Assignment** from Static → Dynamic  
4. Click **Save**  

⚠️ Important: You must detach the IP first — dissociate it from the VM or Load Balancer before you can change the allocation method.

---

## ⚡ In Azure — Via CLI (Fastest Way)

### Step 1 — Detach IP from Network Interface

az network nic ip-config update \
  --resource-group myRG \
  --nic-name myNIC \
  --name ipconfig1 \
  --remove publicIpAddress  

---

### Step 2 — Change allocation to Dynamic

az network public-ip update \
  --resource-group myRG \
  --name myPublicIP \
  --allocation-method Dynamic  

---

## 💡 Real Example to Say in Interview

"In our setup, dev and test environments were using Static IPs which were unnecessarily increasing costs — Azure charges for Static IPs even when the VM is stopped. We switched them to Dynamic allocation using Azure CLI. On every VM restart it gets a new IP, which is perfectly fine for non-production. We kept Static IPs only for production where DNS records and firewall rules depend on a fixed IP address."

</detaisl>
