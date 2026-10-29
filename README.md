# WallPaper

Django 5 + Django REST Framework 壁纸站点后端。提供用户/客户、壁纸、标签、评论、通知、SEO、站点配置等 API。

## 技术栈

- Python 3.11、Django 5.2、DRF、PostgreSQL、Redis
- 统一响应 `ApiResponse`，分页参数 `currentPage` / `pageSize`
- 认证请求头：`token: <值>`（不是 `Authorization`）

## 快速开始

```bash
python -m venv .venv
# Windows: .venv\Scripts\activate
# Linux:   source .venv/bin/activate
pip install -r req.txt
cp .env.example .env
# 编辑 .env，填数据库和 Redis
python manage.py migrate models
python manage.py runserver 0.0.0.0:8000
```

- Swagger：http://127.0.0.1:8000/api/docs/

登录注册：

| 端 | 注册 | 登录 |
|---|---|---|
| C 端 | `POST /api/client/users/register/` | `POST /api/client/users/login/` |
| 管理端 | `POST /api/users/register/` | `POST /api/users/login/` |

## 目录

```
WallPaper/
  App/                 视图与路由
  models/              数据模型
  tool/                认证、权限、工具
  WallPaper/settings/  Django 配置（读环境变量）
  locale/              国际化
  page/                前端静态页（证书放 page/cert，不入库）
  resource/            字体等资源
  nginx.conf           反向代理示例
  req.txt              依赖
  .env.example         环境变量模板
  DEPLOY.md            部署说明
```

密钥、证书、真实 `.env`、生产 Docker 编排不在本仓库。部署见 [DEPLOY.md](DEPLOY.md)。
