# 云原生 CI/CD 容器化 Demo

极简示例：Go HTTP 服务 + Docker + docker-compose + GitHub Actions CI + K8s 清单。

## 本地运行

```bash
go run .
# 访问 http://localhost:8080
```

## Docker 运行

```bash
docker build -t exam-app .
docker run -p 8080:8080 exam-app
```

## docker-compose 运行

```bash
docker-compose up -d
```

## CI/CD 自动部署

推送到 `main`/`master` 分支后，GitHub Actions 会自动：

1. 编译 Go 程序
2. 构建 Docker 镜像
3. 推送到 Docker Hub

镜像地址：`<你的DockerHub用户名>/exam-app:latest`

**配置 Secrets**：仓库 Settings → Secrets and variables → Actions，添加：
- `DOCKERHUB_USERNAME`：Docker Hub 用户名
- `DOCKERHUB_TOKEN`：Docker Hub Access Token（推荐用 [Access Token](https://hub.docker.com/settings/security) 代替密码）

## K8s 部署

```bash
# 需先将镜像 push 到集群可访问的 registry
kubectl apply -f k8s/
```

## 端点

- `GET /` - Hello
- `GET /health` - 健康检查
