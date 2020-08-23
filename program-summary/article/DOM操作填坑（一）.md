# DOM操作填坑（一）
> 记录我学习过程容易犯的错误。  


## 0.基本概念
> DOM 文档对象模型（Document Object Model，简称DOM），是W3C组织推荐的处理可扩展置标语言的标准编程接口。  
> 它是一种与平台和语言无关的应用程序接口(API),它可以动态地访问程序和脚本,更新其内容、结构和样式。  
> DOM树/节点树：加载HTML页面时浏览器生成的一个树型结构（在DOM中看作是节点组成的节点树）  
 
- DOM中节点种类分成3种：
- 下面以 `<div class = "box">盒子</div>`为例：
  * 1. 元素节点 -- `<div></div>`
  * 2. 属性节点 -- class = "box
  * 3. 文字节点 -- 盒子
  * 4. 注释节点 -- (例子里没有) //  /**/
  * 每个节点都是一个对象，在DOM中提供了属性和方法。

## 1.忽略选取的对象是一个还是一个数组
- document.getElementById()  选取唯一一个
- document.getElementsByClassName() 选取类名相同的一组
- 留意getElement还是getElements，除了id是getElement其他都是getElements；有s时表示一组节点，对节点进行操作时记得选中节点，例如document.getElementsByClassName()[0]选中的是第一个。
- 不要从你写代码的角度认为它只有一个，应该从DOM语法本质上去想怎么选择中想要的。
- 其他补充
![](DOM操作（填坑）_files/1.jpg)

### 扩展
- 把几种选择器封装起来？
- 兼容问题

## 2.带参数的事件函数怎么写，要不要写在页面加载后
> 首先操作DOM有三步：选中对象，绑定对象与函数，实现函数。
> 
> 其次，总共有4种操作方式（3，2+1，1+2，1+1+1）
### 四种方式
- 方式一(无参数)：3，适合小量代码和逻辑
  * 标签内书写 onclick="console.log('你点击了1,并将其改成data-name的值');console.log('访问event和this',event,this); this.innerText=this.getAttribute('data-name');" data-name="a"
- 方式二：2+1，必须与页面同时加载，否则报错（但位置可不定），函数绑定时得加括号
  * 标签内书写 onclick="click2()" 或者 onclick="click22(this,99)" data-name="b"
- 方式三(无参数)：1+2，可页面加载后加载
  * 逻辑代码 btn33.onclick = function(){}
- 方式四：1+1+1，完全分离，可页面加载后加载
  * 逻辑代码 btn33.onclick = click4;
  * 逻辑代码 btn44.onclick = click44(25); // click44函数需要是个闭包函数
  * 逻辑代码 btn444.onclick = function(){ click444(this,26);} 

### 问题1：要不要写在页面加载之后？这由绑定对象与函数的结合方式决定的。
  * 如果是HTML代码上绑定的函数，那么函数代码需要和HTML一起加载（也就是不能在页面元素加载后加载，也就是不能写在window.onload里）至于书写的位置，并没有严格要求。(体现在下面的方式二)
  * 后期再选对象操作的，可以在页面加载后再进行，写在window.onload = function() {}里。(体现在下面的方式三和四)

### 问题2：带参数的事件函数该怎么写？
- 根据传入其他数据的角度分析，可分为直接带参数和间接带参数两种方式。
  * 直接带参数--方式二和方式四，他们都是单独写函数的，同时需要注意this的绑定问题。
  * 间接带参数--通用方式：把参数值赋值给自定义属性。需要使用时再用 节点.getAttribute('自定义属性名') 取出来。
  
- 节点的获取常常和this对应起来，但注意下面两种情况（事件函数和使用对象是分离的情况下）需要绑定this。【该知识点涉及了this的指向】
  + 2+1的方式二，HTML上直接写，如 onclick="click22(this,99)"
  + 1+1+1的方式四，完全分离的写法。(当函数使用了闭包的时候不需要传入this)
    + 函数内部需要this，那么可在外面套一个函数，把this传入。例如：btn444.onclick = function(){click444(this,26)};
  
### 问题3：这四种方式合适什么场景使用？

- 1.从是否可复用及与其他代码联系来看
  * 方式一与方式三：代码不可复用，和其他逻辑没有关联的，一对一，不用考虑this指向，直接可用this来获取节点。（若是代码量很少的考虑用方式一）
  * 方式二与方式四：抽离出逻辑函数，函数可复用，一对一或一对多，需要考虑this指向，若用到this需要绑定好this。

- 2.书写位置上区别
  * 方式一和方式二都是写在HTML上的，他们的代码需要和HTML内容一同加载，不建议代码量大的时候采用这两种方式。（注意，方式二上写函数需要带括号）
  * 方式三和方式四都是页面加载后再加载的，大部分情况下也是使用这种方式书写。（建议写在window.onload=function(){}里）

### 问题4：想给同一个事件添加多个响应函数怎么办？
> 上述事件中都是给元素添加onclick事件，但实际上这种绑定是一对一的绑定，后面绑定的会覆盖掉前面的绑定，为了绑定多个事件函数，需要使用到侦听事件。  
> div.addEventListener("click",clickHandler,false)  
> 其中 click为事件类型，clickHandler为事件函数，最后的false表示不冒泡

### 小结
**1. 四种方式中采用哪种方式看两个标准：可复用性+代码量**
- 逻辑代码的使用者是多个？（也就是复用的问题）
  * 多者使用则采用方式二和方式四，同时注意this指向。
    + 代码量大的优先采用方式四，因为他可以选择在页面加载后加载，减少首次渲染时间。
  * 若只有唯一使用者，可采用方式一和方式三。
    + 代码量大的优先采用方式三，因为他可以选择在页面加载后加载，减少首次渲染时间。  
    
**2. 函数名后面要不要写括号？**
- 标签内要加，如 `<button onclick="click22(this,99)">点击</button>`
- 逻辑代码中大多数不加，如`btn33.onclick = click4`，不加的原因是：加了括号表示调用，它会立刻执行而不是等到点击的时候执行。若真想加括号，那么该函数应该写成**闭包函数**（返回一个函数）才可以。

## 3.节点的增删改查
> 这里主要讲 元素节点 属性节点 文字节点
### （1）元素节点 
#### 增加
- 创建新节点 document.createElement("li")
- 克隆节点  节点.cloneNode()  节点.cloneNode(true)
- 插入节点 
  * 父节点.appendChild(新节点) 默认在最后插入
  * 父节点.insertBefore(新节点，位置节点) 在位置节点前插入
  * 父节点.insertAfter(新子节点，位置节点) 在位置节点后插入

#### 删除
- 删除子节点  父节点.removeChild(要删除的子节点), 会返回删除的节点
- 删除本身    节点.remove()

#### 修改 + 查找/访问
- 替换节点  replaceChild(newnode, oldnode)
- 获取节点  
  * 事件中的this(详细看前面第二大点关于事件参数的讲解)
  * 除了getElementById("") 与 querySelector("") 选取的是单个元素之外，其他的需要用[index] 指定是第几个。（详细看第一大点）
- 相邻节点（兄弟父子）
  * 单个元素节点 firstElementChild/lastElementChild
  * 所有子节点 children--随时更新 / childNodes--包括空白的文本节点
    + childNodes指的是返回当前元素子节点的所有**元素节点**和**文本节点**，其中连空格和换行符都会默认文本节点
    + childern指的是返回当前元素的所有元素节点
  * 父级节点  parentElement-父元素 / parentNode-父节点 
    + 大部分情况通用，父元素一直找到最后document得到的节点名是null，而父节点最后得到的节点名是#document
  * 兄弟节点  nextSibling-后一个 previousSibling-前一个
- 访问属性
  * 常见属性  id style class title length
  * 标签属性 tagName nodeName nodeType--当该节点是元素节点的时候返回1
  
  
### （2）属性节点
> 

#### a.点操作
- [增/改] obj.id = "box" // 仅支持标签默认属性，自定义属性不行。
- [删] obj.id = ""  // 不支持删除，仅支持设为空字符串 
- [查] console.log(obj.title)
- 点操作访问和修改class属性时，用**obj.className**

#### b.方法操作
> 方法 setAttribute("属性名","属性值")  
> 方法 getAttribute("属性名")  
> 方法 removeAttribute("属性名")  
> 方法 hasAttribute("属性名")  
> 属性 attributes
- [增/改] obj.setAttribute("id","box"); 
- [删] obj.removeAttribute("data-name");
- [查] 
  * 单个 console.log(obj.getAttribute("data-url"))
  * 全部 console.log(obj2.attributes); // 得到NamedNodeMap对象
  * 存在否 console.log(obj2.hasAttribute("data-name"));

#### c.特别地，class属性
> 上面的都是整体修改，一改就覆盖了。但对于多个类名的操作显然是不合适的。   
> obj.classList属性 -- 得到由类名组成的类数组对象 DOMTokenList  
> 虽然本质上和js中的Set集合对象有点类似，但实际方法是有区别的，不要混淆了。
```js DOMTokenList的属性和方法
var classList = obj2.classList;
console.log(classList[0]); // 可用索引访问
classList.add("boxx"); // 增加一个值
classList.remove("box"); // 删除一个值
classList.replace("boxx","bbb"); // 修改一个值
classList.toggle("bbbox");// 若存在则删除该值，若不存在则添加该值
// classList.toggle("bbbox"，false); // toggle()方法还有一个可选布尔值参数，值为false表示强制删除
console.log(classList.contains("bbb")); // 查找是否有这样的一个值
```

#### d.特别地，style属性
> 理由和class差不多，因为复杂，所以需要更多的方法。  
> 主要分为两种 访问 行内样式 和 当前有效样式。  
> 扩展：常见的修改样式并不是直接修改某个样式，而是增删对应的类名。

```js 
  // 1.获取行内样式
  console.log(oDiv.style.backgroundColor); 
  console.log(oDiv.style["background-color"]); // 这样写也行
  
  // 原因是oDiv.style得到的是一个对象CSSStyleDeclaration,它遵守对象属性的用法 。
  // CSSStyleDeclaration对象的方法	描述
  // getPropertyPriority()	返回指定的 CSS 属性是否设置了 "important!" 属性。
  // getPropertyValue()	返回指定的 CSS 属性值。
  // item()	通过索引方式返回 CSS 声明中的 CSS 属性名。
  // removeProperty()	移除 CSS 声明中的 CSS 属性。
  // setProperty()	在 CSS 声明块中新建或者修改 CSS 属性。
  
  // 2.获取当前有效样式
    function getStyle(element,attr){
      return element.currentStyle ? element.currentStyle[attr] : getComputedStyle(element)[attr];
    }
    console.log(getStyle(oDiv,'color')); 
    // getComputedStyle(element)[attr] --- 适用于火狐、谷歌、Safari 
    // element.currentStyle[attr]  --- 适用于IE 
  }
```


#### 其他
- 默认的属性有哪些？[](https://www.w3school.com.cn/tags/html_ref_standardattributes.asp)

### （3）文字节点
> 没有严格意义下的增删改查，用 innerHTML innerText 实现

- document.createTextNode("233")
- innerHTML 允许标签内容 确保内容是安全的，预防XSS攻击
- innerText 仅文字
  * element.innerText = "文字内容"
  * console.log(element.innerText);
- 防范XSS漏洞原则包括：
  * （1）不信任用户提交的任何内容，对所有用户提交内容进行可靠的输入验证，包括对URL、查询关键字、HTTP头、REFER、POST数据等，仅接受指定长度范围内、采用适当格式、采用所预期的字符的内容提交，对其他的一律过滤。尽量采用POST而非GET提交表单；对“<”，“>”，“；”，“””等字符做过滤；任何内容输出到页面之前都必须加以en-code，避免不小心把htmltag显示出来。
  * （2）实现Session 标记（session tokens）、CAPTCHA（验证码）系统或者HTTP引用头检查，以防功能被第三方网站所执行，对于用户提交信息的中的img等link，检查是否有重定向回本站、不是真的图片等可疑操作。
  * （3）cookie 防盗。避免直接在cookie中泄露用户隐私，例如email、密码，等等；通过使cookie和系统IP绑定来降低cookie泄露后的危险。这样攻击者得到的cookie没有实际价值，很难拿来直接进行重放攻击。
  * （4）确认接收的内容被妥善地规范化，仅包含最小的、安全的Tag（没有JavaScript），去掉任何对远程内容的引用（尤其是样式表和JavaScript），使用HTTPonly的cookie。

