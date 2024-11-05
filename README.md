# 通过 Cloudflare Workers 加速 Docker 镜像下载

![Deploy to Cloudflare Workers](https://deploy.workers.cloudflare.com/button)

基于 Cloudflare Workers 的 Docker 镜像代理工具。它能够中转对 Docker 官方镜像仓库的请求，解决一些访问限制和加速访问的问题。

## 部署方式

- **Workers** 部署：复制 [_worker.js](https://github.com/cmliu/CF-Workers-docker.io/blob/main/_worker.js) 代码，`保存并部署`
  即可
- **Pages** 部署：`Fork` 后 `连接GitHub` 一键部署即可

例如您的Workers项目域名为：`docker.fxxk.io`；

### 1.官方镜像路径前面加域名

```shell
docker pull docker.fxxk.dedyn.io/stilleshan/frpc:latest
```

```shell
docker pull docker.fxxk.dedyn.io/library/nginx:stable-alpine3.19-perl
```

### 2.一键设置镜像加速

修改文件 `/etc/docker/daemon.json`（如果不存在则创建）

```shell
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://docker.fxxk.dedyn.io"]  # 请替换为您自己的Worker自定义域名
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

## 变量说明

| 变量名    | 示例                  | 必填 | 备注                           | 
|--------|---------------------|----|------------------------------|
| URL302 | https://fk.xxx.com  | ❌  | 主页302跳转                      |
| URL    | https://fk.xxx.com/ | ❌  | 主页伪装(设为`nginx`则伪装为nginx默认页面) |
| UA     | netcraft            | ❌  | 支持多元素, 元素之间使用空格或换行作间隔        |
