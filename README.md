```markdown
# 🌍 Wandering Earth Emby Panel

> 基于《流浪地球》视觉风格的 Emby 账号管理面板，支持多服务器切换、多线路展示、海报墙、求片订阅、卡密续费、播放监控与设备 IP 记录等功能。  
> 单个 `docker-compose.yml` 一键部署，无需额外文件。

[![GitHub stars](https://img.shields.io/github/stars/yourname/wandering-earth-emby-panel?style=social)](https://github.com/yourname/wandering-earth-emby-panel)  
[![Docker Pulls](https://img.shields.io/docker/pulls/yourname/wandering-earth-emby-panel)](https://hub.docker.com/r/yourname/wandering-earth-emby-panel)  
![License](https://img.shields.io/github/license/yourname/wandering-earth-emby-panel)

---

## ✨ 功能特性

- 🎬 **流浪地球主题 UI**：深空星云背景、动态行星发动机光束、全息玻璃拟态卡片、流光按钮  
- 🛰️ **多 Emby 服务器**：支持添加多个 Emby 服务器，用户注册时可选择  
- 🔗 **多线路展示**：每个服务器可配置多条播放线路，用户中心直接显示  
- 📱 **设备屏蔽**：管理员可针对用户禁用指定设备类型（TV、Mobile、Web 等）  
- 🎞️ **海报墙**：公开影视库海报浏览，无需登录即可随机翻阅  
- 🎟️ **卡密续费**：批量生成自定义天数卡密，用户自助使用卡密续期  
- 🎬 **求片系统**：用户提交影片请求，管理员审核批准/拒绝  
- 🤖 **MoviePilot 自动订阅**：批准求片后自动调用 MoviePilot API 添加下载订阅  
- 📡 **播放监控**：实时记录设备名称、IP、播放内容、时长  
- 📊 **可视化图表**：管理端展示设备类型饼图、用户播放时长 Top 柱状图、IP 统计  
- 👥 **独立用户中心**：用户可查看状态、线路、提交请求、使用卡密  
- 🐳 **单文件部署**：全栈集成，一个 `docker-compose.yml` 即开即用  

---

## 📦 快速部署

### 前提条件
- 一台安装了 Docker 和 Docker Compose 的服务器（或 Mac/Windows 本地）
- 至少一个运行中的 Emby 服务器，并已生成 API 密钥

### 1. 下载项目

2. 修改环境变量

编辑 docker-compose.yml，找到 environment 部分，修改以下必填项：

```yaml
environment:
  - EMBY_SERVERS=[{"name":"主服","url":"http://192.168.1.100:8096","api_key":"你的Emby API密钥","lines":"http://line1.com,http://line2.com"}]
  - ADMIN_USER=admin                # 管理后台登录名
  - ADMIN_PASS=your_strong_password # 管理后台密码
  - SECRET_KEY=随机字符串            # Flask session 加密密钥，务必修改
  # 可选：MoviePilot 配置（无需求片自动订阅可删除下面两行）
  - MOVIEPILOT_URL=http://192.168.1.200:3000
  - MOVIEPILOT_API_KEY=你的MoviePilot密钥
```

3. 启动容器

```bash
docker-compose up -d
```

首次启动会自动拉取 python:3.10-slim 镜像并安装依赖，稍等 1~2 分钟。

4. 访问面板

· 海报墙：http://你的IP:8080/posters（公开）
· 用户注册：http://你的IP:8080/register
· 用户登录：http://你的IP:8080/login
· 管理后台：http://你的IP:8080/admin（使用上面设置的管理员账号登录）

---

⚙️ 环境变量说明

变量名 必填 示例 说明
EMBY_SERVERS 是 [{"name":"主服","url":"http://...","api_key":"...","lines":"http://..."}] 初始 Emby 服务器列表（JSON 数组）
ADMIN_USER 是 admin 管理后台登录用户名
ADMIN_PASS 是 your_password 管理后台登录密码
SECRET_KEY 是 random_string Flask session 加密密钥（请务必修改为随机字符）
MOVIEPILOT_URL 否 http://192.168.1.200:3000 MoviePilot 地址（求片自动订阅用）
MOVIEPILOT_API_KEY 否 mp_key MoviePilot API 密钥

多服务器格式：EMBY_SERVERS 是一个 JSON 数组，每个对象包含 name（名称）、url（Emby 地址）、api_key（Emby API 密钥）、lines（播放线路，英文逗号分隔）。如需更多服务器，可在管理后台动态添加，或直接在数组中增加多个对象。

---

🖼️ 界面预览

注册页面 用户中心 海报墙
screenshots/register.png screenshots/dashboard.png screenshots/posters.png

管理后台 播放监控 求片审核
screenshots/admin.png screenshots/monitor.png screenshots/requests.png

你可以将实际截图放置于仓库的 screenshots 文件夹下，然后替换上述路径。

---

🧩 模块详解

多服务器 & 多线路

· 管理员可在 服务器管理 (/admin/servers) 添加/删除 Emby 服务器
· 每个服务器可配置多条播放线路，用户中心直接显示所有可用地址

海报墙

· 公开访问 /posters，随机展示电影和剧集海报
· 自动从 Emby 媒体库获取，无需登录即可浏览

求片 & MoviePilot

· 用户登录后在控制台提交影片信息
· 管理员在 求片审核 (/admin/requests) 中批准或拒绝
· 若配置了 MoviePilot，批准时会自动调用订阅接口

卡密续费

· 管理员在 卡密管理 (/admin/cards) 生成指定天数和数量的卡密
· 用户可在个人中心使用卡密延长账号有效期

播放监控与统计

· 后台线程每 30 秒同步 Emby 活跃会话
· 自动记录设备名称、客户端 IP、播放内容、开始/结束时间、总时长
· 监控面板 (/admin/monitor) 展示实时在线设备、设备类型饼图、用户播放时长柱状图、IP 统计表

设备屏蔽

· 管理员可对单个用户屏蔽特定设备类型
· 屏蔽配置同步写入 Emby 策略，即时生效

---

🔧 常见问题

1. 海报墙无图片显示？

· 检查 EMBY_SERVERS 中的 url 和 api_key 是否正确
· 确保 Emby 服务器允许远程访问，且 API 密钥具有相应权限
· 可以在浏览器直接访问 http://你的Emby地址/emby/Items?api_key=你的key 测试接口是否正常

2. 播放监控没有数据？

· 等待 30 秒以上，系统会自动同步一次所有服务器的活跃会话
· 检查管理后台 → Monitor 页面，或查看容器日志：docker-compose logs -f

3. 如何修改默认端口？

· 编辑 docker-compose.yml，将 ports 中的 8080:5000 改为需要的端口，例如 9090:5000

4. 数据存储在哪里？

· 所有数据（用户、卡密、播放记录）存储在项目目录的 data 文件夹内（SQLite 数据库）
· 即使删除容器，数据也不会丢失

---

📄 开源协议

本项目基于 MIT License 开源，你可以自由使用、修改和分发。

---

🤝 贡献

欢迎提交 Issue 和 Pull Request。在提交代码前请确保风格统一，并测试通过。

---

🌍 让每一个 Emby 用户都感受到流浪地球的浩瀚星空！
如有问题或建议，请在 Issues 中提出。

```