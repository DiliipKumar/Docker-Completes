## 7. Dockerfile – Writing Custom Images

### Important Dockerfile Keywords

| Keyword      | Purpose                  | Example                               |
| ------------ | ------------------------ | ------------------------------------- |
| `FROM`       | Base image               | `FROM python:3.9`                     |
| `WORKDIR`    | Working directory        | `WORKDIR /app`                        |
| `COPY`       | Copy files               | `COPY . .`                            |
| `ADD`        | Copy + URLs/tar extract  | `ADD app.tar.gz /app/`                |
| `RUN`        | Run command (build time) | `RUN pip install -r requirements.txt` |
| `CMD`        | Default command          | `CMD ["python", "app.py"]`            |
| `ENTRYPOINT` | Fixed command + args     | `ENTRYPOINT ["python"]`               |
| `EXPOSE`     | Ports                    | `EXPOSE 5000`                         |
| `ENV`        | Environment vars         | `ENV DEBUG=true`                      |
| `ARG`        | Build-time vars          | `ARG VERSION=1.0`                     |
| `LABEL`      | Metadata                 | `LABEL maintainer="you@example.com"`  |
| `VOLUME`     | Mount point              | `VOLUME /data`                        |
| `USER`       | User to run              | `USER appuser`                        |

---

### Sample Dockerfile

```dockerfile
FROM python:3.9-slim
LABEL maintainer="student@example.com"
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
EXPOSE 5000
CMD ["python", "app.py"]
```

---

### Writing Optimized Dockerfiles

- ✅ Use **slim/alpine** images (smaller, faster)
- ✅ Chain RUN commands to reduce layers
- ✅ Use `.dockerignore` to avoid copying junk
- ✅ Multi-stage builds for production

---

### ADD vs COPY & CMD vs ENTRYPOINT

- `COPY` = simple copy (preferred)

- `ADD` = copy + remote URLs + tar extract

- `CMD` = default command (overridable)

- `ENTRYPOINT` = fixed command (not easily overridden)

Example:

```dockerfile
ENTRYPOINT ["python"]
CMD ["app.py"]
```

---

### Multi-Stage Dockerfiles

Used to separate **build stage** from **runtime stage** → smaller, secure images.

Example (Go app):

```dockerfile
FROM golang:1.20 AS builder
WORKDIR /app
COPY . .
RUN go build -o main .

FROM alpine:latest
WORKDIR /root/
COPY --from=builder /app/main .
CMD ["./main"]
```

---

## 8. Docker Volumes (Persistent Storage)

- By default, container data = **temporary**.
- Volumes make data **persistent**.

📌 Example:

```bash
docker run -v mydata:/app/data nginx
docker volume ls
```

---

## 9. Docker Networking

Default networks:

- `bridge` (default)
- `host` (share host network)
- `none` (isolated)

📌 Example:

```bash
docker network create mynet
docker run -d --name db --network=mynet mysql
docker run -d --name app --network=mynet myapp
```

Now app → db communication works via `db`.
