# build-naive-caddy

with GitHub Actions

## Naïve Proxy

关于 Naïve Proxy 的介绍, 可以查看 [Naïve Proxy 的 GitHub 仓库][naiveproxy].

[naiveproxy]: https://github.com/klzgrad/naiveproxy

下面分服务器端和客户端介绍 Naïve Proxy 的配置.

## 服务器端

### 域名购买

首先需要一个域名, 推荐在 [Namesilo][namesilo] 或者 [Cloudflare][cf-registrar] 购买.

[namesilo]: https://www.namesilo.com/
[cf-registrar]: https://www.cloudflare.com/products/registrar/

### 构建带有 Naïve Proxy 代理功能的 Caddy

用户可以手动构建自己的 Caddy, 比如修改代码, 或者使用一些自选插件.

Naïve Proxy 对 Caddy 原本 `forwardproxy` 模块的代码进行了修改, 使其支持 Naïve Proxy. 代码托管在 [github.com/klzgrad/forwardproxy][naive-forwardproxy].

[naive-forwardproxy]: https://github.com/klzgrad/forwardproxy

于是, 在构建 Caddy 时, 使用这份修改的 `forwardproxy` 替换原本的模块, 最后得到的 Caddy 就能够提供 Naïve Proxy 的代理功能了.

Caddy 的自选插件构建构建一般使用 Caddy 方提供的 `xcaddy` 进行. 构建需要 Go 环境, 可以在本地进行, 不过建议使用 GitHub Action, 这样可以省去配置环境的事情. 目前本仓库的 [xcaddy-build.yml](.github/workflows/xcaddy-build.yml) 即为构建适用于 x64 Linux 的 GitHub Action 流程配置文件. 可以 fork 或者使用本仓库的配置文件自行构建, 也可以直接使用本仓库中不定期更新的 [Releases](./releases/latest/) 中的构建产物.

关于 `xcaddy` 的使用, 这里不再展开, 可以参考:

- 关于 "从源码构建 Caddy" 的文档: [Build from source - Caddy Documentation](https://caddyserver.com/docs/build)
- `xcaddy` 的 GitHub 仓库: [caddyserver/xcaddy - github.com](https://github.com/caddyserver/xcaddy)

可以从源码构建 `xcaddy`, 也可以从 Cloudsmith 获取构建好的版本. 这里介绍后一种方式:

```bash
sudo apt update
sudo apt install -y debian-keyring debian-archive-keyring apt-transport-https
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/xcaddy/gpg.key' | sudo gpg --dearmor -o /usr/share/keyrings/caddy-xcaddy-archive-keyring.gpg
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/xcaddy/debian.deb.txt' | sudo tee /etc/apt/sources.list.d/caddy-xcaddy.list
sudo apt update
sudo apt install xcaddy
```

然后带选项运行 `xcaddy`, 构建 Naïve Proxy 的 `forwardproxy`:

```bash
xcaddy build \
  --with github.com/caddyserver/forwardproxy@caddy2=github.com/klzgrad/forwardproxy@naive
```

执行成功后, 构建出的 `caddy` 可执行程序文件应当出现在当前目录.

### 在服务器上配置 Caddy

(待补充)

## 客户端使用

(待补充)
