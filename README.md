# openGym (LazyCat)

自托管健身与体重追踪器，打包为 LazyCat LPK v2。上游项目：[DuarteSantos8/openGym](https://github.com/DuarteSantos8/openGym)。

## 部署信息

- **包名**：`cloud.lazycat.app.opengym`
- **版本**：跟随上游镜像 SemVer tag 自动更新（当前 1.3.10；每日 UTC 03:17 检查，**不使用 latest**）
- **交付模式**：mirror（镜像引用 + digest 校验）
- **min_os_version**：1.5.0

## 服务组成

| 服务 | 镜像 | 说明 |
|------|------|------|
| `web` | ghcr.io/duartesantos8/opengym-web（自动更新，版本源） | React 前端 + nginx（代理 /api，媒体服务） |
| `api` | ghcr.io/duartesantos8/opengym-api（自动更新） | Node 后端（passkey 认证、用户数据） |
| `media` | alpine/git（静态） | 首次启动下载训练图片/GIF（~140MB，来自上游数据集，不随包重新分发） |

## 关键配置

- **passkey 登录**：`RP_ID=${LAZYCAT_SUBDOMAIN}.${LAZYCAT_BOX_DOMAIN}`、`ORIGIN=https://${LAZYCAT_SUBDOMAIN}.${LAZYCAT_BOX_DOMAIN}`（WebAuthn 绑定访问域名）
- 数据持久化：`/lzcapp/var/data:/data`（用户、passkey、上传）；媒体：`/lzcapp/var/media/{img,gif}`
- 媒体版权：训练图片/动画 © Gym visual，来自 [hasaneyldrm/exercises-dataset](https://github.com/hasaneyldrm/exercises-dataset)，由实例自行下载（体现上游 NOTICE.md 约束）

## 文件结构

```
package.yml              # 包元数据
lzc-manifest.yml         # 运行结构（3 服务 + upstream + 运行时变量）
lzc-build.yml            # 构建配置
icon.png                 # 应用图标（上游 PWA icon-512）
.github/lazycat-action.yml      # Action 配置（web/api 双镜像 semver / mirror / 喵喵商店）
.github/workflows/lazycat.yml   # 发布工作流
```
