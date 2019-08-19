#简单得不能再简单的 Node 爬虫

这里的爬虫，就是爬取页面上的数据并进行整合为自己所用。
第一步，先要有 node 环境。
第二步，请求页面获取页面数据。
第三步，爬~。

安装好 node 环境后，我们需要通过工具请求到需要爬取的页面，这里用到的是`superagent`，一个轻量的 http 请求工具，也可以使用其他的，比如：`request`，`http.request`，`axios`，`Got`...
开始请求页面，这里用的栗子是阮老师的http://www.ruanyifeng.com/blog/javascript/
![图片](https://github.com/MarioLuLu7/Notes-Share/blob/master/images/node_1_1.png)
要爬取的就是这个列表。
首先，先请求页面：

```
var superagent = require('superagent')
let url = 'http://www.ruanyifeng.com/blog/javascript/'
superagent.get(url).end((err, res) => {
  if (err) {
    console.log(err)
  } else {
    console.log(res.text)
  }
})
```

然后，需要分析页面，提取出需要的数据。这里用到的是 `cheerio`，使用方式类似于 JQ。
分析页面的结构，找到关键 dom(`#alpha-inner .module-list li a`):
![图片](https://github.com/MarioLuLu7/Notes-Share/blob/master/images/node_1_2.png)

```
let $ = cheerio.load(res.text)
let data = []
$('#alpha-inner .module-list li a').each((index, el) => {
  data.push({
    name: $(ele).text(), // 获取标题
    src: $(ele).attr('href') // 获取链接
  })
})
console.log(data)
```

控制台执行：

```
node index.js
```

这样就完成了爬取一个页面。对于简单有规律的页面，`node` 爬取是非常迅速，操作也十分方便，但处理复杂、数据量大而且整合麻烦的页面，还是用`python`吧。
