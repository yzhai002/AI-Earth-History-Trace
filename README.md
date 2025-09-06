<h1>AI Earth History Trace</h1>

<h3>简介</h3>
<p>AI Earth History Trace - 一个基于AI技术的历史时间线可视化项目，直观地显示各个历史时间段及历史地图。</p>

<!-- <img src="http://gonnavis.com/timeline/preview2.png"> -->
<img src="https://raw.githubusercontent.com/gonnavis/Timeline/master/other/screenshoot.png">
<img src="https://raw.githubusercontent.com/gonnavis/Timeline/master/other/screenshoot_2.jpg">
<img src="https://raw.githubusercontent.com/gonnavis/Timeline/master/other/screenshoot_twha.jpg">

<h3>运行</h3>
<p>
  npm install<br>
  npm run serve<br>
  http://localhost:8080/<br>
</p>

<h3>编译部署</h3>
<p>
  npm run build<br>
  然后提取 dist/ 内文件即可部署 <br>
  由于添加了 Vuetify 和 Tailwind, 编译时间增加了非常多, 请耐心等待.<br>
</p>


<p>
  http://localhost:8080/?is_edit=true<br>
  点击框选边界<br>
  控制台执行:  JSON.stringify({boundary :smap.vec3s_boundary_dot,camera_position: smap.camera.position,})<br>
  将结果复制粘贴入 src/components/data.js 中对应朝代的 map 属性内<br>
</p>

