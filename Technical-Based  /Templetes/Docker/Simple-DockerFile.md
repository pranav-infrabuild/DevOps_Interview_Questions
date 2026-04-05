## 🐳 Python Dockerfile Explanation

### 📄 Dockerfile

```dockerfile
FROM python:3.11-slim
WORKDIR /app
COPY . .
RUN pip install --no-cache-dir -r requirements.txt
EXPOSE 8000
ENTRYPOINT ["python"]
CMD ["app.py"]
```

---

## 🔍 Line-by-Line Explanation

### 1. Base Image

```dockerfile
FROM python:3.11-slim
```

* Uses an official lightweight Python image
* `slim` reduces image size → faster builds & deployments
* Includes Python 3.11 runtime

---

### 2. Set Working Directory

```dockerfile
WORKDIR /app
```

* Sets `/app` as the working directory inside the container
* All subsequent commands run from this directory
* Automatically creates the folder if it doesn't exist

---

### 3. Copy Application Code

```dockerfile
COPY . .
```

* Copies all files from local directory → container `/app`
* Includes source code, dependencies file, etc.

---

### 4. Install Dependencies

```dockerfile
RUN pip install --no-cache-dir -r requirements.txt
```

* Installs Python dependencies from `requirements.txt`
* `--no-cache-dir` reduces image size by avoiding cache storage

---

### 5. Expose Port

```dockerfile
EXPOSE 8000
```

* Documents that the container listens on port `8000`
* Common for web apps (FastAPI, Django, etc.)
* Does **not actually publish the port**, just informs Docker/users

---

### 6. Set ENTRYPOINT

```dockerfile
ENTRYPOINT ["python"]
```

* Defines the main executable (`python`)
* Ensures the container always runs Python

---

### 7. Default Command

```dockerfile
CMD ["app.py"]
```

* Default argument passed to ENTRYPOINT
* Final executed command becomes:

  ```bash
  python app.py
  ```
* Can be overridden at runtime:

  ```bash
  docker run my-app other_script.py
  ```

---

## ⚙️ How It Works Together

| Instruction  | Purpose           |
| ------------ | ----------------- |
| `ENTRYPOINT` | Fixed executable  |
| `CMD`        | Default arguments |

👉 Combined:

```bash
python app.py
```

---

## 🚀 Build & Run

### Build Image

```bash
docker build -t my-python-app .
```

### Run Container

```bash
docker run -p 8000:8000 my-python-app
```


