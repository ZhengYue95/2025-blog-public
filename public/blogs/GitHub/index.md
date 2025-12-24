### 前置准备
- 安装宝塔面板并升级到最新版本
- 在GitHub上fork仓库 `YYsuni/2025-blog-public`
- 准备好自定义域名（已完成解析到VPS公网IP）
---
### 1.  宝塔面板添加Git部署并设置webhook自动更新
#1.1 Git部署
1. 打开宝塔面板，左侧导航栏选择「网站」→ 「PHP项目」→ 「添加站点」，勾选「Git部署」选项；填写域名后，复制页面中的「SSHkey」。![](/blogs/GitHub/b15ac718b5c4b61f.png)
2. 打开GitHub设置页面 https://github.com/settings/keys  进入「SSH and GPG keys」→ 「New SSH key」。![](/blogs/GitHub/68beaa789f53d487.png)
3. 在「Title」栏随意填写标识（如「宝塔部署密钥」），「Key」栏粘贴刚才从宝塔复制的SSHkey（格式以ssh-ed25519开头），点击「Add SSH key」保存。![](/blogs/GitHub/387a23041b62e744.png)
4. 回到宝塔的Git部署设置页面，「仓库地址」填入你fork后的仓库地址：https://github.com/你的用户名/2025-blog-public.git 点击「自动获取分支」。![](/blogs/GitHub/72c8a1a1e85dcdb3.png)
5. 分支选择「main」，点击「确定部署」完成Git关联
#1.2 Webhook自动更新
1. 打开  设置  Git管理  仓库  部署脚本  (感谢 明日林然 大佬指正)
```bash
pnpm i
pnpm run build
```
![](/blogs/GitHub/b146b165d7d8a8d7.png)
2. 打开 https://github.com/你的用户名/2025-blog-public/settings/hooks/new  ![](/blogs/GitHub/0f7b5f278ea2a01d.png)
  **Payload URL**  填写 Webhook URL 令牌  https://IP/hook?access_key=XXXXXXXXXXX。
  **Content type**  选择 `application/json `。
  **SSL verification**   选择 `Disable (not recommended) ` 未部署SSL add webhook 确认。![](/blogs/GitHub/45926833827c0bf0.png)
3. Sync fork后,Git管理  查看webhook日志 `🚀 Git 自动部署 ` 自动更新成功。
---
### 2. 部署Next.js项目
1. 进入宝塔面板「软件商店」，搜索「Node.js版本管理器」，安装「v20.19.6 稳定版」（若列表中没有，点击右上角更新版本列表）。![](/blogs/GitHub/c90641d6dd0d332e.png)
2. 左侧导航栏选择「网站」→ 顶部切换到「Node项目」→ 「添加项目」。![](/blogs/GitHub/f98cd5c1fa39e799.png)
3. 「项目目录」选择第一步添加的PHP项目根目录（示例：/www/wwwroot/blog.4321.love），项目名称会自动填充，无需修改。![](/blogs/GitHub/9e50db456233ef76.png)
4. 「启动选项」会自动获取开发环境「dev:next dev -- turbopack -p」监听端口默认为3000，改为生产环境「start:next start」即可。 (感谢 明日林然 大佬指正)
5. 「Node版本」选择刚安装的「v20.19.6」，点击「确定」。
6. 找到刚创建的Node项目，点击进入「设置」页面。![](/blogs/GitHub/ba8fc1426283e29a.png)
7. 左侧选择「模块管理」，点击「一键安装项目模块」，等待安装完成后，返回项目列表，点击「状态」→ 「运行」启动项目。![](/blogs/GitHub/df74385160fc070c.png)
---
### 3. 添加域名、外网映射与SSL配置
#3.1 绑定域名到Node项目
1.  进入宝塔「网站」→ 「Node项目」，找到目标项目点击「设置」。
2.  选择「域名管理」→ 「添加域名」，输入你的自定义域名 blog.4321.love ，点击「确定」；若需支持www前缀，需额外添加www域名。
3.  确认域名绑定成功后，检查项目「运行状态」是否正常，异常则点击「重启」。
#3.2 配置SSL证书（免费Let's Encrypt）
1.  在项目「设置」页面，选择「SSL证书」选项卡。
2.  证书类型选择「Let's Encrypt」，勾选需要配置的域名，勾选「自动续签」（避免证书过期），点击「申请」。
3.  申请成功后，开启「强制HTTPS」，确保HTTP访问自动跳转至HTTPS。
#3.3 外网映射（内网VPS专用）
若VPS处于内网环境（家庭/公司内网），需配置端口映射或内网穿透：
1.  路由器端口映射：登录路由器管理后台，添加规则，将内网IP（VPS内网地址）+ 内网端口（3000）映射到外网端口（如4433）。
2.  内网穿透工具：使用frp、ngrok等工具，将内网Node项目端口映射到公网可访问地址（参考对应工具官方文档）。
#3.4 外网访问测试
1. 打开浏览器输入https://你的域名 https://blog.4321.love ，能正常显示博客页面即配置成功。
---
### 关键注意事项
1.  端口放行：确保宝塔防火墙及VPS安全组已开放80（HTTP）、443（HTTPS）、3000（项目端口）。
2.  生产环境优化：开发模式启动命令（next dev）适合调试，生产环境建议改为「start:next start」，在项目「设置」→ 「启动选项」中修改后重启。
3.  域名备案：国内VPS需完成域名备案，否则无法通过80/443端口正常访问。
4.  模块安装失败：若一键安装模块失败，可进入项目根目录，通过宝塔「终端」执行npm install手动安装。