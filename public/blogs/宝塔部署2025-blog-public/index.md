> 前置：  
> - 安装 宝塔面板 升级到最新  
> - fork 了 `YYsuni/2025-blog-public`   
> - 

---

## 1. 宝塔面板添加Git部署并设置webhook自动更新

1. 打开 **宝塔面板** 左侧标签栏"网站" PHP项目 添加站点  Git部署  
   填写"域名"  复制"SSHkey"![](/blogs/宝塔部署2025-blog-public/9bb691bfc60db2d7.png)
2.  打开 https://github.com/settings/keys  Giehub settings   SSH and GPG keys   new SSH keys  ![](/blogs/宝塔部署2025-blog-public/68beaa789f53d487.png)
3. Title  随意填写   Key  填入宝塔面板Git部署里的 SSHkey ssh-ed25519 AAAAC3XXXXXXXXXXXXXX
 add SSH key 保存![](/blogs/宝塔部署2025-blog-public/387a23041b62e744.png)
4. 回到宝塔部署里 仓库地址填入你fork的地址 https://github.com/your name/2025-blog-public.git
自动获取分支![](/blogs/宝塔部署2025-blog-public/72c8a1a1e85dcdb3.png)
5. 分支  main  确定部署![](/blogs/宝塔部署2025-blog-public/1ddb52ba823cd4e9.png)
---

## 2. 用本地地址部署Next.js

1. 宝塔软件商店搜索 Node.js版本管理器   安装 v20.19.6	稳定版(没有的话右上角更新版本列表)
2. 左侧标签  网站  最上方Node项目  添加项目![](/blogs/宝塔部署2025-blog-public/f98cd5c1fa39e799.png)
项目目录选择刚才添加的PHP项目根目录/www/wwwroot/blog.4321.love
3. 项目名称  自动填写根目录地址
4. 启动选项  自动获取  dev:next dev -- turbopack - p 2025
5. Node版本 选择刚才安装的 v20.19.6  确定.
6. 打开你刚才部署的项目设置![](/blogs/宝塔部署2025-blog-public/8b96a2db69660c30.png)
7. 左侧模块管理  一件安装项目模块![](/blogs/宝塔部署2025-blog-public/120fbf755ab5dfd6.png)
   弹出安装完成后,回到项目 点击 **状态** 运行
--- 

## 3. 添加域名  外网映射  SSL  
