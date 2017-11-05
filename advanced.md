# 进阶概念

1. 分布式(*distributed*)

> 一个业务分拆多个子业务，部署在不同的服务器上

eg.  

**中心式**

业务系统

```js
import {createServer} from "http";
// 对/a业务及对/b业务集中部署在同一个服务器上
const systemA = createServer((req, res) => {
	req.url === "/a" && res.end(1234);
	req.url === "/b" && res.end(2345);
}).listen(8080);
```

**分布式**  

业务系统A

```js
import {createServer} from "http";
// 对/a业务分布式部署在服务器A上
const systemA = createServer((req, res) => {
	req.url === "/a" && res.end(1234);
}).listen(1234);
```

业务系统B

```js
import {createServer} from "http";
// 对/b业务分布式部署在服务器B上
const systemB = createServer((req, res) => {
	req.url === "/b" && res.end(2345);
}).listen(2345);
```

2. 集群(*cluster*)

> 同一个业务，部署在多个服务器上

eg.  

```js
import cluster from "cluster";
import {cpus} from "os";
import {createServer} from "http";
if(cluster.isMaster){
	let {length} = cpus(),
		a = 0;
	while(a++ < length){
		cluster.fork(); // 主管在每一个cpu上部署一个做同一业务得服务器员工
	}
	return;
}
createServer(() => {}).listen(1234); // 每一个做同一业务的服务器员工起一个http服务
```