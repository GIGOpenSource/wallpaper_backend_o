# 部署说明

本仓库不含生产密钥、TLS 证书和真实环境 IP。请在服务器本地自行准备。

## 1. 环境变量

```bash
cp .env.example .env
```

至少填写：

- `DJANGO_SECRET_KEY`
- `POSTGRES_*`
- `REDIS_*`
- `AES_SECRET_KEY`（16 字节）

可选：COS、微信、翻译、PageSpeed 等。

## 2. 证书

将站点证书放到 `page/cert/`（该目录不入库）：

- `server.crt`（或完整链）
- `server.key`

并按实际域名修改 `nginx.conf` 里的 `server_name` 与证书路径。

## 3. 本机 / 虚拟环境部署

```bash
python -m venv .venv
source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -r req.txt
python manage.py migrate models
python manage.py runserver 0.0.0.0:8000
```

前面用 nginx 反代 `/api/` 到 8000，静态站用 `page/markwallpapers`。

## 4. 容器（可选）

生产编排不公开。本地可参考 `docker-compose.example.yml`：

```bash
cp docker-compose.example.yml docker-compose.yml
cp .env.example .env
# 填写 POSTGRES_PASSWORD、REDIS_PASSWORD
docker compose up -d --build
```

数据库、Redis 密码只写在 `.env`，不要写进 compose 文件后提交。

## 5. 迁移

```bash
python manage.py makemigrations models
python manage.py migrate models
```

## 6. 注意

- 不要提交 `.env`、`*.key`、`*.pem`、`page/cert/`
- `DEBUG` 生产环境设为 `false`，并收紧 `ALLOWED_HOSTS`
