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

<h3>计划开发功能</h3>
<p>用河流图直观地同时显示时间和面积信息，完善标尺，时间段嵌套，自定义／上传时间段。 </p>

<h3>twha(The World Historical Atras)项目代码</h3>
<p>果然已经有人整理了所有年代的历史地图并做成了网页, 不需要自己再搞一遍了，<a href="http://x768.com/w/twha.ja" target="_blank">官网</a>，奇怪的是并没有放到线上, 因此稍做修改优化了下操作方式放到了线上 http://gonnavis.com/timeline/twha/ 目前只支持pc</p>
<p>代码放在本项目的 twha 文件夹下</p>

<h3>运行twha版</h3>
<p>
  直接打开 twha/index.html 即可
</p>

<h3>部署twha版</h3>
<p>
  直接部署 twha/ 文件夹下所有文件即可
</p>

<h3>备注</h3>
<p>
  目前使用 vue2 开发<br>
  之前的 angular2 版 git SHA-1: b2bbf00d95ac24568e1941da56a91884b95edc8d<br>
  之前的 angular1 版 git SHA-1: b5372193ab1ff344b9bd0d3dabb2e3339b436269
</p>

<h3>地图编辑模式(暂时弃用，因为目前使用图片地图而非路径地图，后续适时继续开发)</h3>
<p>
  http://localhost:8080/?is_edit=true<br>
  点击框选边界<br>
  控制台执行:  JSON.stringify({boundary :smap.vec3s_boundary_dot,camera_position: smap.camera.position,})<br>
  将结果复制粘贴入 src/components/data.js 中对应朝代的 map 属性内<br>
</p>

