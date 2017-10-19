# DOM操作

DOM就是Document Object Model,文档对象模型

```
document	// 返回整个文档
```

通过ID获取元素
```
document.getElementById('idname');
```
通过类或标签取得元素
```
document.getElementsByClassName('classname');	// 返回HTML集合
var lis = document.getElementsByTagName('li');	// 返回HTML集合
Array.isArray(lis);	// 返回false, 可见lis并不是一个数组

// 转成数组, 之后就可以使用数组方法forEach遍历了
Array.from(lis).forEach(function(item) {
	console.log(item);
});
```

### 查询选择器 Query Selector
```
document.querySelector('#book-list li:nth-child(2) .name');	// 返回查询的一个元素
document.querySelectorAll('#book-list li .name');	// 返回一个NodeList节点列表
```

### 获取元素文本
```
ele.textContent;	// 表示一个节点及其后代的文本内容
```
##### 与innerText的区别
Internet Explorer 引入了 node.innerText。意图类似，但有以下区别：  
* __textContent__ 会获取所有元素的内容，包括 `<script>` 和 `<style>` 元素，然而 __innerText__ 不会。
* __innerText__意识到样式，并且不会返回隐藏元素的文本，而__textContent__会。
* 由于 __innerText__ 受 CSS 样式的影响，它会触发重排（reflow），但__textContent__ 不会。
* 与 __textContent__ 不同的是, 在 Internet Explorer (对于小于等于 IE11 的版本) 中对 __innerText__ 进行修改， 不仅会移除当前元素的子节点，而且还会永久性地破坏所有后代文本节点（所以不可能再次将节点再次插入到任何其他元素或同一元素中）。

##### 与innerHTML的区别
正如其名称，innerHTML 返回 HTML 文本。通常，为了在元素中检索或写入文本，人们使用innerHTML。但是，textContent通常具有更好的性能，因为文本不会被解析为HTML。此外，使用textContent可以防止 XSS 攻击。  

### Element.innerHTML 属性设置或获取描述元素后代的HTML语法。
```
bookList.innerHTML += '<p>This is how you add HTML</p>';
```

### 节点Node
```
const banner = document.querySelector('#page-banner');

console.log('node type is: ', banner.nodeType);	// 数字对应一种类型
console.log('node name is: ', banner.nodeName);	// 节点名字
console.log('has child nodes: ', banner.hasChildNodes());	// 是否有子节点

const clonedBanner = banner.cloneNode(true);	// 参数为false时则只克隆外层节点
console.log(clonedBanner);
```
```
const bookList = document.querySelector('#book-list');

console.log('the parent node is: ', bookList.parentNode);	// 返回父节点
console.log('the parent element is: ', bookList.parentElement); // 返回bookList的父元素

console.log(bookList.childNodes); // 返回nodeList
console.log(bookList.children);	// 返回HTML集合
```
```
const bookList = document.querySelector('#book-list');

console.log('next sibling is: ', bookList.nextSibling);	// 返回下一个兄弟节点
console.log('next element sibling is: ', bookList.nextElementSibling); // 返回下一个兄弟元素

console.log('previous sibling is: ', bookList.previousSibling);
console.log('previous element sibling is: ', bookList.previousElementSibling);

bookList.previousElementSibling.querySelector('p').innerHTML +=
    '<br>Too Cool for everyone else!';
```

### 事件与阻止默认行为
```
var btns = document.querySelectorAll('#book-list .delete');

Array.from(btns).forEach(function(btn) {
    'use strict';
    btn.addEventListener('click', function(e) {
        const li = e.target.parentElement;
        li.parentNode.removeChild(li);
    });
});

const link = document.querySelector('#page-banner a');

link.addEventListener('click', function(e) {
    'use strict';
    e.preventDefault();
    console.log('navigation to ', e.target.textContent, ' was prevented.');
});

```
### 事件冒泡
```
const list = document.querySelector('#book-list ul');

list.addEventListener('click', function(e) {
    'use strict';
    if (e.target.className === 'delete') {
        const li = e.target.parentElement;
        list.removeChild(li);
    }
});
```
### 表单交互
```
const addForm = document.forms['add-book'];

addForm.addEventListener('submit', function(e) {
    'use strict';
    e.preventDefault();
    const value = addForm.querySelector('input[type="text"]').value;
    console.log(value);
});
```
### 建立元素
```
    // create elements
    const li = document.createElement('li');
    const bookName = document.createElement('span');
    const deleteBtn = document.createElement('span');

    // add contents
    bookName.textContent = value;
    deleteBtn.textContent = 'delete';

    // add classes
    bookName.classList.add('name');
    deleteBtn.classList.add('delete');

    // append to document
    li.appendChild(bookName);
    li.appendChild(deleteBtn);
    list.appendChild(li);
```
### 属性
```
ele.getAttribute('class');
ele.setAttribute('class', 'cls-name');
ele.hasAttribute('class');	// 元素是否有这个属性,返回布尔值
ele.removeAttribute('class');	// 移除属性
```
### checkbox的change事件
```
const hideBox = document.querySelector('#hide');

hideBox.addEventListener('change', function(e) {
    'use strict';
    if (e.target.checked) {
        list.style.display = 'none';
    } else {
        list.style.display = 'initial';
    }
});
```
### 搜索框过滤
```
// filter books
const searchBar = document.forms['search-books'].querySelector('input');

searchBar.addEventListener('keyup', function(e) {
    'use strict';
    const term = e.target.value.toLowerCase();
    // const books = list.children;
    const books = list.getElementsByTagName('li');
    Array.from(books).forEach(function(book) {
        const title = book.firstElementChild.textContent.toLowerCase();
        if (title.indexOf(term) !== -1) {
            book.style.display = 'block';
        } else {
            book.style.display = 'none';
        }
    });
});
```
### 标签内容切换
```
// tabbed content
const tabs = document.querySelector('.tabs');
const panels = document.querySelectorAll('.panel');

tabs.addEventListener('click', function(e) {
    'use strict';
    if (e.target.tagName === 'LI') {
        const targetPanel = document.querySelector(e.target.dataset.target);
        panels.forEach(function(panel) {
            if (panel === targetPanel) {
                panel.classList.add('active');
            } else {
                panel.classList.remove('active');
            }
        });
    }
});
```
### DOMContentLoaded Event
```
document.addEventListener('DOMContentLoaded', function() {
	// 有时把js文件放在head标签里,而没有放在body结束标签之前
	// 为了让js正常工作,防止文档元素未加载就绑定事件或获取元素等问题
	// 可以给dom添加一个文档加载监听,确保全部文档加载完成再执行js代码.
});
```