# 浏览器跨域方法

https://segmentfault.com/a/1190000006095018#articleHeader11

# 跨域资源共享 CORS 详解

http://www.ruanyifeng.com/blog/2016/04/cors.html

# 异步编程
http://exploringjs.com/es6/ch_promises.html

https://github.com/getify/You-Dont-Know-JS

ES2017 在 6 月最终敲定了，随之而来的是广泛的支持了我最喜欢的最喜欢的JavaScript功能：  
async(异步) 函数。如果你也曾为异步 Javascript 而头疼，那么这个就是为你设计的。
如果你没有的话，那么你有可能是个天才。
Async(异步) 函数或多或少允许你编写顺序的 JavaScript 代码，
而无需将所有逻辑包装在 callbacks(回调)，generators(生成器) 或 promises 中。
考虑一下这个代码：
```
<script type="text/javascript">
	
	function logger(){
		let data = fetch('http://api.douban.com/v2/movie/top250?start=25&count=25');
		console.log(data);
	}
	logger();//返回的一个异步函数Primose
</script>
```

这段代码没有按照你的预期执行。
如果你写过 JS 的话，你可能知道上面的代码为什么不会按预期运行。
但是这个代码确实做了你所期望的。

```
<script type="text/javascript">
	var request = new Request('http://api.douban.com/v2/movie/top250?start=25&count=25', {

	    method: 'POST', 

	    mode: 'no-cors', 

	    redirect: 'follow',

	    headers: new Headers({

	        'Content-Type': 'application/json'

	    })

	});
	async function logger(){
		let data = await fetch(request);
		console.log(data);
	}
	logger();//返回的一个异步函数Primose
</script>
```

ES6 之前的异步 JavaScript
在我们深入学习 async 和 await 之前，有必要先了解一下 promises 。
要弄懂 promises，我们需要再回到简单的回调。
在ES6中引入了 Promises ，并对在 JavaScript 中编写异步代码做了很大的改进。
不再有所谓的 “回调地狱”，让我们对 Promises 产生了一点亲切感。
回调是一个函数，可以将结果传递给函数并在该函数内进行调用，以响应任何事件。 这是JS的基础

```
function readFile('file.txt', (data) => {
  // 回调函数内部
  console.log(data)
}
```

这个函数只是简单的从一个文件记录数据，在文件完成之前进行读取是不可能的。
看起来很简单，但是如果你想按顺序读取和记录五个不同的文件怎么办？
在 Promises 出现之前，为了执行顺序任务，你需要嵌套回调，如下所示：

```
// 这就是标准的回调地狱
function combineFiles(file1, file2, file3, printFileCallBack) {
    let newFileText = ''
    readFile(string1, (text) => {
        newFileText += text
        readFile(string2, (text) => {
            newFileText += text
            readFile(string3, (text) => {
                newFileText += text
                printFileCallBack(newFileText)
            }
        }
    } 
}
```

这时 Promise 就派上用场了。Promise 是对尚未存在的数据进行推理的一种方法。
你所不知道的 JavaScript 系列 的作者 Kyle Simpson 以异步 JavaScript 演讲而闻名。
他对 Promise 的 解释 是：这就像是在快餐店里点餐。

点餐。
付钱并获得取餐小票。
等餐。
当餐准备好了，他们会叫你的单号提醒你取餐。
取餐。
正如他指出的，当你在等餐的时候，你是不可能吃你的午餐，
但是你可以盼它，你可以为你的午餐做好准备。
当你等餐的时候，你可以进行其它事情，即使现在没有拿到菜，
但是这个午餐已经 “promise” 给你了。
这就是所谓的 Promise。一个用于表示终将出现数据的对象。

```
readFile(file1)
  .then((file1-data) => { /* do something */ })
  .then((previous-promise-data) => { /* do the next thing */ })
  .catch( /* handle errors */ )
```

最佳（且最新）方式： Async ／ Await
几年前，async 函数被纳入了 JavaScript 生态系统。
截止上个月，async 函数成为了 JavaScript 语言的官方特性，并得到了广泛支持。
async 和 await 关键字基于 pormise 和 generator 做了简单的封装。本质上，
它允许我们在所需的任意位置使用 await 关键字来“暂停”一个函数。

```
async function logger() {
  // 暂停直到获取到返回数据
  let data = await fetch('http://sampleapi.com/posts')
  console.log(data)
}
```

这段代码会按照你所期望的那样运行。 
它记录了来自 API 调用的数据。如果你连这个都理解困难，那我也不知道咋办了。
这样做的好处是非常直观。
你以大脑思考的方式编写代码，然后告诉代码在所需的位置暂停。
另一个好处就是可以使用 promise 不能使用的 try 和 catch ：

```
async function logger ()  {
    try {
        let user_id = await fetch('/api/users/username')
        let posts = await fetch('/api/`${user_id}`')
        let object = JSON.parse(user.posts.toString())
        console.log(posts)
    } catch (error) {
        console.error('Error:', error) 
    }
}
```
这是一个故意写错的例子，为了证明了一点：
catch 将捕获在该过程中的任何步骤发生的错误。
至少有 3 个地方 try 可能会失败，这是在异步代码中的一种最干净的方式来处理错误。

我们也可以使用 async 函数让循环和条件判断不再令人头疼：
```
async function count() {
    let counter = 1
    for (let i = 0; i < 100; i++) {
        counter += 1
        console.log(counter)
        await sleep(1000)
    }
}
```

一些要注意的细节
现在，你应该已经确信 async 和 await 的美妙之处，接下来我们深入了解一些细节：

async 和 await 基于 promise 的。 使用 async 的函数将会始终返回一个 promise 对象。
这一点很重要，要记住，这可能是你遇到的容易犯错的地。
在使用 await 的时候我们暂停了函数，而非整段代码。
async 和 await 是非阻塞的。
你仍然可以使用 Promise 例如 Promise.all()。正如我们之前的例子：
```
async function logPosts ()  {
    try {
        let user_id = await fetch('/api/users/username')
        let post_ids = await fetch('/api/posts/${user_id}')
        let promises = post_ids.map(post_id => {
            return  fetch('/api/posts/${post_id}')
        }
        let posts = await Promise.all(promises)
        console.log(posts)
    } catch (error) {
        console.error('Error:', error) 
    }
}
```

await 只能用于被声明为 async 的函数。
因此，不能在全局范围内使用 await。

```
// 抛出异常
function logger (callBack) {
    console.log(await callBack)
}
 
// 正常工作
async function logger () {
    console.log(await callBack)
}
```
