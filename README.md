<h1>AI Earth History Trace</h1>

<h3>简介</h3>
<p>AI Earth History Trace - 一个基于AI技术的历史时间线可视化项目，直观地显示各个历史时间段及历史地图。</p>



<h3>本地开发预览（Vue2 + Three.js 3D）</h3>


<h3>Git安装与项目克隆</h3>
<p>
  1. 确认 Git 是否安装<br><br>
  
  Ubuntu/Debian 系统<br>
  git --version<br>
  # 如果没有<br>
  sudo apt update && sudo apt install -y git<br><br>
  
  CentOS/RHEL<br>
  sudo yum install -y git<br><br>
  
  Windows Server<br>
  打开 PowerShell：<br>
  git --version<br><br>
  
</p>

<p>
  2. 克隆仓库<br>
  在你要存放项目的目录执行：<br>
  git clone https://github.com/yzhai002/AI-Earth-History-Trace.git<br>
</p>

<p>
  3. 进入项目目录<br>
  cd AI-Earth-History-Trace<br>
</p>



<p>
  0) 系统准备（一次性）<br>
  # 刷新软件索引<br>
  sudo apt update<br><br>
  
  # 安装 Nginx + 基本工具<br>
  sudo apt install -y nginx curl build-essential<br><br>
  
  # 安装 Node.js LTS（推荐 20.x）<br>
  curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -<br>
  sudo apt install -y nodejs<br><br>
  
  #（可选）给 npm 换镜像源，加速依赖安装<br>
  npm config set registry https://registry.npmmirror.com<br>
  （可选）给小机器加 swap（建议 2C4G 的 ECS 加）<br>
  sudo fallocate -l 4G /swapfile<br>
  sudo chmod 600 /swapfile<br>
  sudo mkswap /swapfile<br>
  sudo swapon /swapfile<br>
  echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab<br>
</p>

<p>
  1) 构建 3D 版（Vue 主项目）<br>
  在 AI Earth History Trace 仓库根目录执行：<br>
  # 安装依赖<br>
  npm install<br><br>
  
  # （可选）给 Node 增加一点可用内存，防止构建卡死<br>
  export NODE_OPTIONS=--max_old_space_size=2048<br>
  export NODE_ENV=production<br><br>
  
  # 打包<br>
  npm run build<br>
  完成后会生成 dist/ 目录。<br>
</p>

<p>
  2) 准备 2D（twha）静态目录<br>
  把仓库里的 twha/ 拷贝到一个对外可托管的位置（和 dist 一起放在 /var/www 下最清晰）：<br>
  sudo mkdir -p /var/www/timeline<br>
  sudo cp -r dist /var/www/timeline/dist<br>
  sudo cp -r twha /var/www/timeline/twha<br><br>
  
  # 赋权以便 Nginx 读取<br>
  sudo chown -R www-data:www-data /var/www/timeline<br>
  说明：<br>
  • /var/www/timeline/dist 用来托管 3D 主站。<br>
  • /var/www/timeline/twha 用来托管 2D 版本。<br>
</p>

<p>
  3) 配置 Nginx（同一台机上同时开 3D 和 2D）<br>
  创建一个独立的站点配置，例如 timeline：<br>
  sudo bash -c 'cat > /etc/nginx/sites-available/timeline' << "EOF"<br>
  server {<br>
      listen 80;<br>
      server_name _;  # 没有域名就先用默认<br><br>
  
      # --- 3D 主站（Vue 构建包） ---<br>
      root /var/www/timeline/dist;<br>
      index index.html;<br><br>
  
      # SPA 路由就绪：前端路由找不到的路径回退到 index.html<br>
      location / {<br>
          try_files $uri $uri/ /index.html;<br>
      }<br><br>
  
      # --- 2D 版本挂到 /2d/ 路径 ---<br>
      location /2d/ {<br>
          alias /var/www/timeline/twha/;<br>
          index index.html;<br>
          try_files $uri $uri/ /2d/index.html;<br>
      }<br><br>
  
      # 缓存与压缩优化（静态资源长缓存，HTML 不缓存）<br>
      location ~* \.(?:js|mjs|css|png|jpg|jpeg|gif|svg|ico|woff2?|ttf|otf)$ {<br>
          add_header Cache-Control "public, max-age=31536000, immutable";<br>
          try_files $uri =404;<br>
      }<br>
      location = /index.html {<br>
          add_header Cache-Control "no-cache";<br>
      }<br>
      location = /2d/index.html {<br>
          add_header Cache-Control "no-cache";<br>
      }<br><br>
  
      # gzip 压缩<br>
      gzip on;<br>
      gzip_comp_level 5;<br>
      gzip_types text/plain text/css application/javascript application/json image/svg+xml;<br>
      gzip_min_length 1024;<br><br>
  
      # 基本安全头（可选）<br>
      add_header X-Content-Type-Options nosniff;<br>
  }<br>
  EOF<br><br>
  
  # 启用站点<br>
  sudo ln -sf /etc/nginx/sites-available/timeline /etc/nginx/sites-enabled/timeline<br><br>
  
  # 关闭默认站点（可选）<br>
  sudo rm -f /etc/nginx/sites-enabled/default<br><br>
  
  # 语法检查并重载<br>
  sudo nginx -t && sudo systemctl reload nginx<br><br>
  
  # 放通防火墙（如果启用了 ufw）<br>
  sudo ufw allow "Nginx Full" || true<br>
</p>

<p>
  4) 访问与验证<br>
  • 3D 主站（Vue 版）：http://&lt;你的服务器IP&gt;/<br>
  • 2D 版（twha）：http://&lt;你的服务器IP&gt;/2d/<br>
  验证静态资源是否长缓存、是否压缩：<br>
  curl -I http://&lt;你的服务器IP&gt;/<br>
  curl -I http://&lt;你的服务器IP&gt;/assets/xxx.js<br>
  应看到：<br>
  • index.html 响应头含 Cache-Control: no-cache<br>
  • JS/CSS 等含 Cache-Control: public, max-age=31536000, immutable<br>
  • Content-Encoding: gzip（若浏览器支持）<br>
</p>

<h3>常见坑与排查</h3>
<p>
  Node 版本：若 `npm install` 报错，先用 `node -v` 看版本，尽量用当前 LTS；必要时删除 `node_modules/` 和 `package-lock.json` 重装<br>
  Windows 权限/杀软：若 dev server 启不起来，检查本地防火墙或安全软件是否拦截 8080 端口<br>
  路径与中文目录：尽量将项目放到无空格/纯英文路径，避免奇怪的构建问题。<br>
  网络代理：`npm install` 慢或失败可换源（如 `npm config set registry https://registry.npmmirror.com`）后再装<br><br>
  
  构建卡住/内存不足：确认已加 swap；临时 export NODE_OPTIONS=--max_old_space_size=4096 再 npm run build。<br><br>
  
  访问 2D 路径 404：确认 location /2d/ 用的是 alias（不是 root），且路径末尾包含斜杠；/var/www/timeline/twha/index.html 是否存在。<br><br>
  
  白屏或前端路由 404：3D 站点一定要有 try_files $uri $uri/ /index.html;。<br><br>
  
  Nginx 未生效：sudo nginx -t 看语法；sudo journalctl -u nginx -e 查错误；sudo systemctl reload nginx 或 restart。<br><br>
  
  速度慢：前面已开 gzip 和长缓存；若有域名与 HTTPS，后续可上 CDN 或在 Nginx 开 HTTP/2。<br>
</p>

