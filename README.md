# 通过 Cloudflare Workers 加速 Docker 镜像下载

基于 Cloudflare Workers 的 Docker 镜像代理工具。它能够中转对 Docker 官方镜像仓库的请求，解决一些访问限制和加速访问的问题。

## 准备工作

1. 注册 Cloudflare 账号
2. 准备一个域名
3. 在 Cloudflare 配置域名解析，将域名解析到 Cloudflare

例如项目域名为：`hub.fxxk.com`；

## 部署方式

### Workers 部署

1. 创建一个 Workers 项目
2. 点击编辑代码
3. 复制 [_worker.js](https://github.com/cmliu/CF-Workers-docker.io/blob/main/_worker.js) 代码，修改`workers_url`
   为项目域名,保存并部署
4. 配置自定义域名

### Pages 部署

1. Fork 本项目
2. 连接 GitHub，选择 Fork 的项目，修改`workers_url`为项目域名, 一键部署
3. 配置自定义域名

[//]: # ([![Deploy to Cloudflare Workers]&#40;https://deploy.workers.cloudflare.com/button&#41;]&#40;https://deploy.workers.cloudflare.com/?url=https://github.com/possible318/cf-docker-hub&#41;)

## 使用方式

### 1.官方镜像路径前面加域名

```shell
docker pull hub.fxxk.com/stilleshan/nginx:latest
```

### 2.一键设置镜像加速

修改文件 `/etc/docker/daemon.json`（如果不存在则创建）

```shell
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://hub.fxxk.com"]  # 请替换为您自己的Worker自定义域名
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

## 变量说明

| 变量名    | 示例                  | 必填 | 备注                           | 
|--------|---------------------|----|------------------------------|
| URL302 | https://fk.xxx.com  | x  | 主页302跳转                      |
| URL    | https://fk.xxx.com/ | x  | 主页伪装(设为`nginx`则伪装为nginx默认页面) |
