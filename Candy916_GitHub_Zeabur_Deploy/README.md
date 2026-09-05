# Candy916 翻译服务部署说明

本包采用你提供的 HK8 压缩包的 public/translations 目录约定，语言内容全部来自新制作的 Candy916 六语言包。没有导入 HK8 译文。
六包均为 716 个原文键。没有 Loader、数据库、密钥或后台 API。
这是待部署文件，不代表 GitHub 仓库或 Zeabur 服务已经创建。

## 1. GitHub 新建仓库

登录 GitHub → New repository。
名称建议 candy916-translation-service，可见性建议 Private。
创建后选择 uploading an existing file，或 Add file → Upload files。
将解压目录内的 public 文件夹和 README.md 拖入上传区域，提交到 main。

注意：要上传解压后的文件，不能只上传 ZIP；仓库第一层要直接看到 public。
正确路径为 public/translations/en.json，不能多一层 candy916-zeabur/public。
不需要上传之前的 Node 项目、server.mjs、package.json 或旧 Loader。

## 2. Zeabur 新建项目和服务

Create Project → 命名 Candy916 Translation → 选择要使用的服务器。
如新服务器已在账号里，直接选该服务器；如尚未配置，可按需要购买新服务器或绑定自有服务器。
仅需要独立服务时，也能选现有服务器；新项目与新物理服务器是不同概念。

Add Service → GitHub → 选择 candy916-translation-service。
如果找不到仓库，Configure GitHub → 授权 Zeabur App 访问这个新仓库。
服务名建议 candy916-translation。

Settings → Root Directory 设置 public。
如果必须先部署才能看到 Settings，先创建服务，再把 Root Directory 改为 public 并重新部署。
按静态服务部署，保持自动构建设置；无需填写 npm install、npm start 或运行端口。
不需要新建数据库或配置 Access ID/Token。
确认部署对应 main 分支与最新提交。

## 3. 域名

打开服务的 Networking/Domain 页面，选择 Generate Domain。
可以尝试 candy916-translation-service 前缀，但能否使用以页面提示为准。
记下实际生成的 HTTPS 域名；本包没有预先绑定任何域名。

## 4. 验证

下面 DOMAIN 均替换成实际域名。
https://DOMAIN/health.txt 应显示 OK。
https://DOMAIN/version.json 应显示 version 1.0.0。
https://DOMAIN/translations/en.json
https://DOMAIN/translations/ms.json
https://DOMAIN/translations/zh.json
https://DOMAIN/translations/vi.json
https://DOMAIN/translations/th.json
https://DOMAIN/translations/bn.json
六个链接均应 HTTP 200，返回 JSON 原文键值内容。

URL 不应含 /public/。出现 404 时先确认仓库目录与 Root Directory。
这是文件服务，没有首页文件；请用 health.txt 验证，不要只判断根地址是否显示网页。
在浏览器 Network 检查 JSON Content-Type、Access-Control-Allow-Origin: * 和 Cache-Control。
_headers 用于静态服务器跨域及缓存配置；需以实际响应确认配置生效。

## 5. 网站接入

部署成功只意味着语言文件可访问，网站还需要加载器读取这些地址。
接入时使用实际服务域名和 /translations/ 路径，并正确绑定 Candy916 语言选择器。
不要直接使用前面未经联调的草稿 Loader。
把实际 Zeabur 域名及 Candy916 现有语言切换代码提供后，再完成独立 Loader。
此部署包没有修改 HK8 网站或其服务器。

## 6. 后续更新

修改 public/translations/ 下对应语言文件，保持原文键不变。
同时将 version.json 版本递增，例如 1.0.0 → 1.0.1，并填入本次更新时间。
提交到 Zeabur 绑定的分支；确认自动部署已启用并检查新部署成功。
语言 JSON 采用 no-cache, must-revalidate，允许缓存保存但使用前必须验证新鲜度。
version.json 也采用同样规则。仅修改 version.json 不会让没有版本检测逻辑的 Loader 自动更新。
若网站还有 localStorage 或内存缓存，必须由 Loader 的更新逻辑一并处理。

翻译文件已通过 JSON、键集合、数字和金额检查，尚未经过母语审校与线上网页联调。
原文分段文章需在实际 DOM 检查语序；Turnover 等业务定义仍需按你的规则确认。

## 官方参考

- [GitHub 上传文件](https://docs.github.com/en/repositories/working-with-files/managing-files/adding-a-file-to-a-repository)
- [Zeabur 创建项目](https://zeabur.com/docs/en-US/deploy/create/create-project)
- [GitHub 集成](https://zeabur.com/docs/en-US/deploy/methods/github-integration)
- [Root Directory](https://zeabur.com/docs/en-US/deploy/config/root-directory)
- [静态服务](https://zeabur.com/docs/en-US/guides/static)
- [公网域名](https://zeabur.com/docs/en-US/deploy/networking/public-networking)
