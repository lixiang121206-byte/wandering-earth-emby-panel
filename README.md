# 🌍 Wandering Earth Emby Panel

基于《流浪地球》主题的 Emby 账号管理面板，支持注册、多服务器、多线路、设备屏蔽、求片系统、MoviePilot 自动订阅、卡密续费等功能。

## ✨ 特性

- 🎬 电影感 UI：深空星云、发动机光束、玻璃拟态卡片
- 🛰️ 多 Emby 服务器管理，用户注册时可选择
- 🔗 多线路展示
- 📱 屏蔽播放设备（TV / Mobile / Web 等）
- 🎞️ 求片系统，管理员审核后可自动推送到 MoviePilot 订阅
- 🎟️ 卡密生成与用户自助续费
- 🧑‍💻 独立用户中心，查看状态、线路、提交请求

## 📦 快速部署

- 1.将 docker-compose.yml的内容保存
2.修改environment 中的必填项:  
（1）EMBY_SERVERS:初始 Emby 服务器配置 (JSON 数组，可之后在管理页面继续添加)
（2）MOVIEPILOT_URL / MOVIEPILOT _API_KEY (如需自动订阅则填写，可不填) 
（3）ADMIN_USER / ADMIN_ _PASS 
3.启动: bash 复制 docker-compose up -d 
4.访问： 注册端：http://你的IP:8080/register 。用户登录：http://你的IP:8080/login(注 册后使用相同账号密码) 管理端：http://你的IP:8080/admin(需用 管理员账号登录)