# DOM1（Document类型）

> DOM是针对HTML和XML文档的一个API。DOM描述了一个层次化的结点树，允许开发人员添加，移除和修改页面中的部分。

## Document类型
> Javascript通过Document类型来表示文档。document对象是HTMLDocument（继承自Document类型）的一个实例，表示整个html页面。

![](./img/document.PNG)

### 1.特征

- nodeType的值为9;

- nodeName的值为"#document";

- nodeValue的值为null;

- parentNode的值为null;

- owenerDocument为null;

- 其子节点可能是一个DocumentType(最多一个),Element(最多一个),ProcessingInstruction或Comment。

### 2.document对象
可以通过这个对象取得页面相关的信息，而且还能操作页面的外观及其底层结构。

###（1） 文档的子节点
##### 访问文档到的子节点的两个方法：
a.通过documentElement属性：该属性值始终指向<html\>元素（Element元素）  
b.通过childNodes列表进行访问

##### body属性
该属性指向<body/>元素。

##### DocumentType结点
通过doctype属性进行访问。

    var doctype = document.doctype;  
    //不同浏览器之间的实现的差异很大

IE8之前的版本：如果存在文档类型的声明会将它错误的理解为Comment节点，所以这个值为null。
IE9+及Firefox:如果存在就是第一个子节点。
Sarify等：是一个DocumentType的几点，将会解析，但不会出现杂大荧幕上。

###（2） 文档信息
document中的一些属性可以描述网页的一些信息。
##### title属性
包含<title\>中的文本信息。修改title属性的值不会改变<title\>的值
##### 关于网页请求的属性汇总

       URL:包的属性含页面完整的url
       domain：包含页面的域名
       //唯一可以允许修改的值，但不能修改为不同的域
       referrer:显示来源页面 

domain可以修改的用途：
处理跨域的安全限制，来自不同子域的页面无法通过js通信。将页面的document.domain属性设置为相同的值就可以解决跨域问题。   
 
domain的限制：
松散的（默认值可以修改，但修改后不允许重复修改）。即：document.domain**有且仅能有一次**修改的机会。

###（3）**查找元素**
DOM应用最广泛的就是取得某个元素然后对它进行操作了。document提供了两个方法来完成这个功能。
getElementById()和getElementsByTagName()

##### a.getElementById()
接收一个参数，就是要取得的元素的id。此时的id执行严格匹配，找到是返回该dom元素，不存在时返回null。


##### b.getElementByTagName()
接收一个参数，即需要获取的元素的标签的名字如果是要获取全部的元素，则设定为*。其返回值为包含零个或者多个的NodeList。在HTML中获取的是**HTMLCollection对象**,与NodeList类似。可以使用方括号或者item()方法来获取。  
上代码进行说明:  

       var images = document.getElementByTagName('img');
       console.log(images.length);
       console.log(images[0].src);
       console.log(images.item(0).src);
#####HTMLCollection对象的特性：  
---
namedItem()方法:使用这个方法可以进一步筛选取得的元素。通过对元素的name特性取得集合中的项。

      html:
       <img src='./my.gif' name="myImage">
      js:
        //筛选出来
       var images = document.getElementByTagName('img');
       var myIamge = images.nameItem("myImage");
根据上面的方法支持按照名称项访问：
       `var myImage = images["myImage"];`  


**HTMLCollection属性才有的方法**  

getElementByName()：返回一个给定一个特定的name属性的所有元素。    


应用: 
取得所有的单选按钮，将它们绑定为同样的name值。

### （4）特殊集合
> 用来访问文档的快捷方式
       
document.anchors:包含文档中所有带name属性的`<a>`元素。  
document.forms:包含文档中所有带有`<form>`标签的元素。  
document.images:包含文档中所有的带有`<img>`属性的元素  
document.links:包含文档中带有href属性的`<a>`元素 

### (5)文档写入
>可以通过以下四种方法把输出流写入到网页中：write(),writeln(),open(),close();
##### a.wrire()/writeln()
都接受一个string的字符串参数，即要写入到输出流中的文本。write()会按原样写入，而writeln（）则会在后面添加一个换行符。  

特别的：     
可以通过这两个方法动态引入外部的资源。上代码：    

         <html>
         <head>
           <title>动态引入代码示例</title>
         </head>
         <body>
           <script>
              document.write("<script type=\"text/javascript\" src=\"file.js\">+
           <\/script>")
       //需要转义，否则和前面一个script标签进行匹配就会导致错误
           </script>
         </body>
         </html>  
