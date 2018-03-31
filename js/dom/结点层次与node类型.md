# DOM1（结点层次与node类型）

> DOM是针对HTML和XML文档的一个API。DOM描述了一个层次化的结点树，允许开发人员添加，移除和修改页面中的部分。

## 节点层次
每段标记都可以用树中的一个结点进行表示,各个节点都是不一样的类型，共有12种类型，都继承于Node类型，所以都共享着相同的属性和方法。
 
## Node类型
NODE接口，该接口在JavaScript中是由Node类型实现的；IE中不能访问到该类型。  
所有的节点都继承与Node类型，可以通过节点的nodeType来访问其结点的类型，其对照到的类型可以从Node类型的常量中进行对比验证。

    //对照表
    Node.ELEMENT_NODE(1);
    Node.ATTRIBUTE_NODE(2);
    Node.TEXT_NODE(3);
    Node.CDATA_SECTION_NODE(4);
    Node.ENTITY_REFERENCE_NODE(5);
    Node.ENTITY_NODE(6);
    Node.PROCESSING_INSTRUCTION_NODE(7);
    Node.COMMENT_NODE(8);
    Node.DOCUMENT_NODE(9);
    Node.DOCUMENT_TYPE_NODE(10);
    Node.DOCUMENT_FRAGMENT_NODE(11);
    Node.NOTATION_NODE(12);

验证示例：
    
    if(someNode.nodeType == Node.ELEMENT_NODE){
         alert("Node is an element");
    }
    //ps.ie不支持Node类型构造函数，为了兼容性进行改写
       
    if(someNode.nodeType == 1){
         alert("Node is an element");
    }
### Node类型的 nodeName和nodeValue属性
所有结点都有这两个属性，这两个属性的值取决于上面描述的**节点的类型**。  
对于元素结点（如div），其nodeName始终为元素的标签名，nodeValue始终为null。

### 节点关系
> 节点之间的关系可以通过家族关系来直观的进行描述：父元素，子元素以及同胞元素等等。

#### 1.childNodes属性
该属性保存着一个类数组对象：NodeList对象，可以通过下标来**访问子元素**，也可以通过item()方法访问其子元素。它也有length属性来得到长度，但是它不是数组。NodeList是一个“动态对象”，当DOM的结构变化时，它能实时的反映这种变化。
上代码:

     var firstChild = someNode.childNodes[0];
     var secondChild = someNode.childNodes.item(1);
     var count =someNode.childNodes.length;

将NodeList对象转换为数组,考虑兼容性（IE8之前只能通过手动枚举）

    function convertToArray(nodes){
       var array = null;
       try{
          array = Array.prototype.slice.call(nodes,0);
        }
       catch(error){
           array=new Array();
           for(varr i=0,len=nodes.length;i<len;i++)
           {
               array.push(node[i])
           }
        }
        return array;
    }

#### 2.parentNode属性/previousSibling属性/nextSibling属性/firstChild属性/lastChild属性
parentNode属性指向文档树中的父节点。  
previousSibling属性/nextSibling属性：指向同胞节点中的前一个/后一个结点。  
firstChild属性/lastChild属性：父节点指向它的第一个子节点和最后一个子节点。

####3.hasChildNodes()方法
可以用来判断是否有子节点，如果有返回true，否则false。

### 操作结点
#### 1.appendChild()
> 向nodeChildNodes列表的末尾增加一个新的节点。

接收的参数就是要插入的节点，如果该结点已经存在在文档树中，就移动到目标位置,其返回值为插入的新节点。

    someNode.appendChild(newNode);


#### 2.insertBefore()
>将节点插入到指定位置。

接收两个参数，第一个参数是构造的等待插入的节点，第二个参数是相对位置P。调用函数之后节点插入到p位置。返回值是新的节点的值。
    
     someNode.insertBefore(newNode,null);  
     //传空就执行appenChild（）操作
     someNode.insertBefore(newNode,someNode.firstChild);
     //插入成为了第一个元素，原来的第一个元素变为第二个

#### 3.replaceChild()
> 取代某个元素

接收两个参数，第一个参数是构造的新的节点，第二个参数是要取代的参数。返回值为新的节点。

       someNode.replaceChild(newNode,someNode.lastChild);
       //取代了最后一个元素 
#### 4.removeChild()
> 移除某个元素

接收一个参数，参数是要移除的节点，返回值为该节点。

       someNode.removeChild(someNode.lastChild);
       //移除了最后一个元素 

----    
![图片](./img/shuoming.PNG)  


###  其他方法
#### 1.cloneNode()
> 拷贝一个与节点完全相同的副本

接收一个bool值（true/false），true执行深拷贝：复制节点本身以及整个子节点树。false的时候执行浅拷贝:只复制该结点（成为孤儿）。
![说明](./img/cloneNode.PNG)

#### 2.normalize()
> 处理文档树的文本节点

（1）.处理文本节点不包含文本的情况（删除）  
（2).处理两个紧接的文本文档(合并)








