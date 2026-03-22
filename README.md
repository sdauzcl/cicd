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

### 自动部署到本机（CD）

推送代码后自动在本机拉取镜像并运行，需先配置 **自托管 Runner**：

1. 打开仓库 **Settings** → **Actions** → **Runners** → **New self-hosted runner**
2. 选择 **macOS**，按页面指引在终端执行命令（会下载并配置 runner）
3. 最后执行 `./run.sh` 启动 runner（建议用 `launchd` 或 `nohup` 保持常驻）
4. 确保本机已安装并运行 Docker

配置完成后，每次 push 会先在 GitHub 构建并推送镜像，再在你的 Mac 上自动 `docker pull` 并重启容器。访问 http://localhost:8080 查看服务。

## K8s 部署

```bash
# 需先将镜像 push 到集群可访问的 registry
kubectl apply -f k8s/
```

## 端点

- `GET /` - Hello
- `GET /health` - 健康检查
