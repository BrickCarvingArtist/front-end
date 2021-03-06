# 前端架构设计

## 目标
高负载，高性能，高可用，稳定可控，正确开发

### 高负载
1. 关键渲染路径
	* 关键资源（目标：减少渲染等待。途径：页面数据加载只关注首屏必要资源）
	* 关键路径长度（目标：使用最低传输来回次数。途径：http2同路复用）
	* <span id="criticalBytes">关键字节</span>（目标：减少渲染等待，减少资源占用。途径：编码只引用必要的代码，只加载必要的资源，uglify，minimize，Gzip，CSS3代替图片）
2. 页面渲染精细流程
<table>
	<thead>
		<th colspan="2">页面渲染精细流程</th>
	</thead>
	<tbody>
		<tr>
			<td rowspan="11">键入页面URL（进度条读条）</td>
		</tr>
		<tr>
			<td><span id="promptForUnload">前页卸载</span>（目标：加快卸载。途径：较少的单页业务处理量，最少的节点数）</td>
		</tr>
		<tr>
			<td>域名重定向（由浏览器自动控制，无法控制，无影响）</td>
		</tr>
		<tr>
			<td>网络链路（目标：加快网络响应速度。途径：由运维负责）</td>
		</tr>
		<tr>
			<td>应用缓存（目标：加快处理页面文档对象模型。途径：提示用户使用最新浏览器，进退缓存，由浏览器自动控制）</td>
		</tr>
		<tr>
			<td>DNS寻址（目标：加快寻址速度。途径：由运维负责）</td>
		</tr>
		<tr>
			<td>TCP握手（目标：加快握手速度。途径：由运维负责）</td>
		</tr>
		<tr>
			<td>SSL连接（目标：加快连接速度。途径：由运维负责）</td>
		</tr>
		<tr>
			<td><span id="requestResponse">资源来回</span>（目标：加快请求速度。途径：处理方式同[关键字节](#criticalBytes)）</td>
		</tr>
		<tr>
			<td><span id="domContentLoaded">DOM生成</span>（目标：加快DOM生成。途径：处理方式同[前页卸载](#promptForUnload)）</td>
		</tr>
		<tr>
			<td><span id="domInteractive">DOM处理（目标：加快DOM生成。途径：增量式编写CSS规则，降低CSSOM计算复杂度，尽量不使JS阻塞初次渲染，添加defer属性）</td>
		</tr>
		<tr>
			<td>页面可交互（鼠标、滚轮可操作）</td>
			<td>流媒体加载至完成（目标：尽快使流媒体可见。途径：缓存，压缩，使用webp）</td>
		</tr>
		<tr>
			<td rowspan="5">页面加载完毕（进度条结束读条）</td>
		</tr>
		<tr>
			<td>延迟加载开始（目标：加快请求速度。途径：处理方式同[资源来回](#requestResponse)）</td>
		</tr>
		<tr>
			<td>DOM处理（目标：加快DOM生成。途径：处理方式同[DOM处理](#domInteractive)）</td>
		</tr>
		<tr>
			<td>DOM修改（目标：加快DOM生成。途径：处理方式同[DOM生成](#domContentLoaded)）</td>
		</tr>
	</tbody>
</table>

### 高性能
1. 浏览器

指导用户使用最新浏览器

2. 框架选择

使用具有优化DOM操作，体积小，处理快的框架

3. 编码方式

[前端编码性能指南（2018第一季度草案）.pdf]()

### 高可用
1. 离线可访问
2. 保证较差网络情况可访问性
3. 必要功能降级处理
4. 页面展示与路由共进退

### 稳定可控
1. 开发规约

[前端开发规约（2018第一季度草案）.pdf]()

2. 技术文档
	* 代码文档
	* 接口文档
	* 流程文档
	* 配置文档
	* 新人上手文档
3. 代码审查
	* 能力评级 = (优秀代码 - 垃圾代码) / 合格指标
4. 监控系统
	* 视图监控
		1. 路由视图
		2. 异常筛选
	* 资源监控
		1. 完整请求
		2. 异常筛选
		3. UA分析
	* 脚本监控
		1. 异常堆栈
		2. 异常统计
		3. 链路追责
	* 流程监控
		1. 用户误操作
		2. 服务错误
	* 行为监控
		1. 鼠标轨迹
		2. 聚焦热区
		3. 按钮点击
		4. 跳转链路
	* 性能监控
		1. 白屏时间
		2. 函数执行
		3. 监听器
		4. 资源时间
		5. 内存占用
	* 报警通知
		1. 规则设置，基线设置
		2. `性能指数 = (满意数 + 可容忍数 / 2) / 总样本量`
		3. `JS错误率 = 错误样本量 / 总样本量`
		4. `API成功率 = 接口调用成功的样本量 / 总样本量`
		5. 通知推送
	* 操作重现

## 正确开发

### 工作流
1. 开发工具
2. 开发规约
	* 命名
	* 逻辑
	* 样式表
	* 脚本
	* 资源
3. 代码文档
	* 样式表
	* 业务流程
	* 独立功能
	* 配置项
4. 打包整合
5. 单元测试
6. 视觉还原
7. 持续集成
8. 去服务化

### 组件库
1. 公共类
	* 常量
	* 基本数据处理
	* 校验
	* 统计代码
	* 监控代码
2. PC端
	* 前台
	* 中台
3. 移动端
	* UI库
	* 手势
	* 图标库
	* 字体库
4. 可视化
	* 图表
	* 报表

## 技术选型
基于React的[PWA](#pwa)，无Native

## <span id="pwa">Why PWA？</span>

### 优势
1. 满足原生功能实现（离线访问，消息推送，PRPL等）
2. 自由（不基于应用中心，不存在应用审核）
3. 轻（无需下载应用，网页一键保存到桌面，不基于打开的浏览器。原理需要与移动组调研）
4. 灵活（网页随意更新，客户可见的内容是否更新仅受缓存影响）
5. 未来已来（谷歌极力推动，Twitter，阿里，饿了么等一线互联网公司一年前已落地实践）

### 不足
1. 兼容性要求较高（解决方案：做降级处理，高标准地要求业务编码，良好体验参见[饿了么](https://h5.ele.me)，[https://shop.polymer-project.org/](https://shop.polymer-project.org/)，[我的博客](https://punchy.ikindness.cn)）
2. 新技术需要学习（解决方案：有成熟的文档，以老带新，循序渐进）
