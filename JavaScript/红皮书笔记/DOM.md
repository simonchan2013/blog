** 针对HTML和XML文档的一个API，描绘一个层次化的节点树，允许开发人员添加、移除和修改页面的某一部分 **

### 12种Node类型（nodeType）
1. Node.ELEMETNT_NODE(1)
2. Node.ATTRIBUTE_NODE(2)
3. Node.TEXT_NODE(3)
4. Node.CDATA_SECTION_NODE(4)
5. Node.ENTITY_REFERENCE_NODE(5)
6. Node.ENTITY_NODE(6)
7. Node.PROCESSING_INSTRUCTION_NODE(7)
8. Node.COMMENT_NODE(8)
9. Node.DOCUMENT_NODE(9)
10. Node.DOCUMENT_TYPE_NODE(10)
11. Node.DOCUMENT_FRAGMENT_NODE(11)
12. Node.NOTATION_NODE(12)

### 节点关系
- childNodes属性

  每一个节点都有一个childNodes属性，其中保存一个NodeList对象（类数组对象，可通过位置访问）。
  NodeList对象是基于DOM结构动态执行查询的结果，因此DOM结构的变化能够自动反映在NodeList对象中。

- parentNode属性：每一个节点都有，指向文档树中的父节点
- previousSibling属性 及 nextSibling属性：访问前后同胞节点。首尾的两个节点各有一个属性为null。

  ````js
  if (someNode.nextSibling === null) {
    alert('It\'s the last node!');
  } else if (someNode.previousSibling === null) {
    alert('It\'s the first node!')
  }
  ````

- firstChild属性 及 lastChild：分别指向childNodes列表的第一个和最后一个节点
- ownerDocument属性：所有节点都有，指向表示整个文档的文档节点（每个文档的根节点，只有一个子节点，即文档元素）。
- method
  - appendChild(newNode)：向childNodes末尾增加一个节点，并返回该节点
  - insertBefore(newNode, theNode)：在theNode前面插入一个节点，并返回该新节点。theNode为null时等价于appendChild(newNode)
  - replaceChild(newNode, oldNode)：用newNode替换oldNode，并返回该新节点。替换后，oldNode还为文档所有，但文档已经没有其位置
  - removeChild(theNode)：移除一个节点。移除后，theNode还为文档所有，但文档已经没有其位置



### Document类型
- JavaScript通过Document类型表示文档
- 浏览器中，document对象是HTMLDocument（继承自Document类型）的一个实例，表示整个HTML页面，且document对象是window对象的一个属性
- Document节点（文档节点）的特征

  1. nodeType == 9
  2. nodeName == '#document'
  3. nodeValue == null
  4. parentNode == null
  5. ownerDocument == null
  6. 子节点可能是一个DocumentType（最多一个）、Element（最多一个）、ProcessingInstruction 或 Comment



### Element类型
- 提供对元素标签名、子节点及特性的访问
- 具有以下特征

  1. nodeType == 1
  2. nodeName的值为元素的标签名
  3. nodeValue == null
  4. parentNode可能是Document或Element
  5. 其子节点可能是Element、Text、Comment、ProcessingInstruction、CDATASection 或 EntityReference

- method
  - getAttribute(attr)：获取指定属性值
  - setAttribute(attr, value)：设置属性值
  - removeAttribute(attr)：删除属性



### Text类型
- 特征

  1. nodeType == 3
  2. nodeName == '#text'
  3. nodeValue的值为节点所包含的文本
  4. parentNode是一个Element
  5. 不支持（没有）子节点
- method
  - appendData(text)：将text添加到节点末尾
  - deleteData(offset, count)：从offset指定的位置开始删除count个字符
  - insertData(offset, text)：在offset指定的位置插入text
  - replaceData(offset, count, text)：用text替换从offset指定的位置开始到offset+count为止处的文本
  - splitText(offset)：从offset指定的位置将当前文本节点分成两个文本节点
  - substringData(offset, count)：提取从offset指定的位置到offset+count为止处的字符串





## DOM扩展

### Selector API
- method
  - querySelector(model)：返回与该模式匹配的第一个元素

    ````js
    var body = document.querySelector('body');          // 获取body元素
    var myDiv = document.querySelector('#mydiv');       // 获取id="mydiv"的元素
    var selected = document.querySelector('.selected'); // 获取class="selected"的元素
    ````

  - querySelectorAll(model)：返回与该模式匹配的元素所组成的一个NodeList的实例。
  - matchesSelector(model)：a.matchesSelector(model)，如果a与模式匹配则返回true，否则返回false



### 与类相关的扩充
- getElementsByClassName(classNameStr)：根据由className组成的字符串返回对应元素

  ````js
  // 取得所有类中同时包含 username 和 current 的元素，类名的先后顺序无所谓
  var tarElement = document.getElementsByClassName('username current');
  ````
