# 1. 模板语法
## 1.1 插值
### 1.1.1 文本插值
在 Vue 中，使用`{{}}`来对文本进行插值。  
使用形式如：
```html
<p>{{message}}</p>
```
上面这行代码会将Vue实例中的`message`属性的值解析到`p`标签上。

### 1.1.2 原始HTML插值
直接使用文本插值的方法插入HTML片段，Vue会直接将该片段以纯文本的形式解析出来。
如：假设 data.rawHtml = <span>我是html片段</span>
```html
<p>{{}}</p>
```
