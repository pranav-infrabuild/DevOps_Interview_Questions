## 🐳 Multi-Stage Dockerfile for Python App

### 📄 Dockerfile

```dockerfile id="msd123"
# -------- Stage 1: Builder --------
FROM python:3.11-slim AS builder
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir --prefix=/install -r requirements.txt

# -------- Stage 2: Final Image --------
FROM python:3.11-slim
WORKDIR /app
COPY --from=builder /install /usr/local
COPY . .
EXPOSE 8000
ENTRYPOINT ["python"]
CMD ["app.py"]
```

---

## 🔍 What is Multi-Stage Build?

A **multi-stage build** uses multiple `FROM` statements to:

* Separate build environment from runtime environment
* Reduce final image size
* Improve security by excluding unnecessary tools

---

## 🧱 Stage-by-Stage Explanation

### 🏗️ Stage 1: Builder

```dockerfile id="bld001"
FROM python:3.11-slim AS builder
```

* Creates a temporary build environment
* Named as `builder` for reference

```dockerfile id="bld002"
COPY requirements.txt .
RUN pip install --prefix=/install -r requirements.txt
```

* Installs dependencies into `/install` directory
* Keeps them isolated from system Python

---

### 🚀 Stage 2: Final Image

```dockerfile id="fin001"
FROM python:3.11-slim
```

* Fresh, minimal runtime image
* Does NOT include build tools

```dockerfile id="fin002"
COPY --from=builder /install /usr/local
```

* Copies only installed dependencies from builder stage
* Avoids reinstalling packages

```dockerfile id="fin003"
COPY . .
```

* Adds application source code

---

### 🌐 Port Exposure

```dockerfile id="fin004"
EXPOSE 8000
```

* Documents the app runs on port 8000

---

### ▶️ Run Command

```dockerfile id="fin005"
ENTRYPOINT ["python"]
CMD ["app.py"]
```

* Final command:

  ```bash
  python app.py
  ```

---

## ⚙️ How It Works

1. Builder stage installs dependencies
2. Final stage copies only required artifacts
3. Results in smaller, cleaner image

---

## 🚀 Build & Run

### Build

```bash id="run001"
docker build -t my-python-app .
```

### Run

```bash id="run002"
docker run -p 8000:8000 my-python-app
```

---

## 💡 Key Interview Points

* ✅ Reduces image size (no build tools in final image)
* ✅ Improves security (less attack surface)
* ✅ Better layer caching
* ✅ Clean separation of concerns (build vs runtime)

---

## ⚖️ Single-stage vs Multi-stage

| Feature          | Single-stage | Multi-stage      |
| ---------------- | ------------ | ---------------- |
| Image size       | Larger       | Smaller          |
| Security         | Lower        | Higher           |
| Build complexity | Simple       | Slightly complex |
| Best for         | Small apps   | Production       |

---

