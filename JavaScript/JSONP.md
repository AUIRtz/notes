## JSONP 和 JSON 的关系
事实上，JSONP 和 JSON 没有关系。若强行要有关系，也只能说 JSONP 这个技术使用了 JSON 这种数据格式。  
__JSON 是一种数据交换格式。__  
__JSONP 是一种非官方跨域数据交互协议，是一种技术。__  

## 跨域是什么？
上面说了 JSON 是一种跨域协议。那么，什么是跨域？
#### 同源策略
为了保证用户信息安全，浏览器使用同源策略。  

同源策略：
- 协议相同
- 域名相同
- 端口相同

非同源的网站会受到下列限制：
- Cookie、LocalStorage 和 IndexDB 无法读取。
- DOM 无法获得
- AJAX 请求不能发送。(或者是请求发送之后，浏览器识别出不同源，将响应拦截了)


因为有同源策略的限制，当需要请求外部网站的数据时，就需要跨域。
#### 跨域
跨域指的是绕过同源政策，对外部网站请求数据。  
跨域的其中一种技术，就是使用 JSONP 。

## JSONP 跨域原理
开发人员发现，带有 `src` 属性的标签都能对服务器发起请求，且不受同源策略的影响。  
这时，就有某些天才的程序员想出利用`<script>`标签来绕过同源策略，进行跨域请求数据。
1. 事先在请求方页面中写定一个 callback 函数，用来获取响应方响应的数据。
2. 在请求方页面中动态创建`<script>`标签，通过设置`<script>`标签的 `src`属性对 `src`指向的网站发起请求。
3. 需要将 1 中的 callback 函数名以查询参数的形式写入 2 中`<script>`标签的 `src` 中，告知后端要执行的回调函数 
4. 响应方获得请求，返回一段JS代码（该段JS代码执行事先写好的 callback 函数，并将__要返回的数据作为 callback 函数的参数带入请求方__）  
5. 这时候，请求方就通过 callback 函数获得了请求的数据。

在实际使用中，通过两个约定来规范 JSONP 的使用：
- 使用查询参数指明 callback 的函数名，形如 `？callback=函数名`
- 查询参数中的函数名一般随机生成，使用完毕即删除，保证不污染全局变量。

一个JSOPN跨域请求例子:
```javascript
function JSONP(path){
        // 创建一个 script 标签
        let script = document.createElement("script") 
        
        // 随机生成回调函数名
        let functionName = 'callback' + parseInt(Math.random()*100000,10)
        // 定义回调函数，处理获得的数据
        window[functionName] = function(data){
            console.log('获取的数据是：')
            console.log(data) 
            alert('请求成功。')
        }
        // 设置JSONP请求路径
        script.src = path + "?callback=" + functionName
        // 将 script 标签插入到页面中
        document.body.appendChild(script)
        // 请求完成之后，删除回调函数,节约内存
        delete window[functionName]
    }
```