---
layout: post
title: "博客搭建记录"
category: others
---
* 目录/Catalog   
{:toc #markdown-toc}

## 如何使代码块高亮
  - 进入Ruby安装文件夹，安装rouge\
  `$ gem install rouge`
  - 安装完成后查看自带样式\
  `$ rougify help style`
  - 生成syntax.css文件，以下指令生成的是github风格，也可选择上一步中显示的其他风格，注意路径\
  `$ rougify style github > assets/css/syntax.css`
  - 引入css文件，在 **/_includes/head.html** 中的<head>内添加以下代码：
  `<link rel="stylesheet" href="{{ "/assets/css/syntax.css" | prepend: site.baseurl }}">`
  - 参考资料：[Jekyll代码高亮](https://blog.walterlv.com/post/available-themes-of-rouge-style.html)

## 如何在主页增加图片
  主页一般默认为index.html(index.md)，因此直接修改index.md即可，在index.md中加入以下代码：
  ```html
  <div style="position: relative;">
    <img src="pot.png" alt="Image" style="position: absolute; bottom: 610px; left: 430px; width: 70px; height: auto;">
</div>
```
## 如何实现搜索功能
  逻辑：在主页增加一个搜索框元素，点击搜索框触发搜索事件，所需文件：
  - index.md：主页文件
  - search.js：实现搜索功能
  - search.css：定义搜索框外观样式
  - search.html：点击搜索框跳转搜索页面

**1.search.html(位于根目录下)**\
   这段代码定义了一个搜索框，用户可以在其中输入文本进行搜索。同时，搜索框旁边有两个图标，一个是用于开始搜索的图标，另一个是用于清除搜索框内容的图标。

```html
<link rel="stylesheet" type="text/css" href="/assets/css/search.css">
<script async src="/assets/js/search.js"></script>
<div class="search">
    <i class="material-icons search-icon search-start">search</i>
    <input type="text" class="search-input" placeholder="Searching..." />
    <i class="material-icons search-icon search-clear">clear</i>
    <div class="search-results z-depth-4"></div>
</div>
```


**2.search.css文件(位于/assets/css/中)**
```css
.search {
    position: fixed;
    top: 93px;
    right: 145px;
    color: rgb(0, 0, 0);
    height: 350px;
    text-align: right;
    line-height: 30px;
    padding-right: 10px;
}

.search .search-icon {
    float: right;
    /* height: 100%;
    margin: 0 10px;
    line-height: 30px; */
    cursor: pointer;
    user-select: none;
}

.search .search-input {
    float: right;
    width: 60%;
    height: 30px;
    color: rgb(0, 0, 0);
    background-color: white; /* 修改为白色背景 */
    line-height: 30px;
    margin: 0;
    border: 2px solid #ddd;
    border-radius: 10px;
    box-sizing: border-box;
}

.search .search-results {
    display: block;
    z-index: 1000;
    position: absolute;
    top: 30px;
    right: 50px;
    width: 150%;
    max-height: 400px;
    overflow: auto;
    text-align: left;
    border-radius: 5px;
    box-shadow: 0 .3rem .5rem #333;
    background-color: white; /* 修改为白色背景 */
}

.card {
    max-width: 60rem;
}

.search .search-results .result-item {
    color: #000;
    margin: 5px;
    padding: 3px;
    color: rgb(2, 2, 2);
    border-radius: 3px;
    cursor: pointer;
}
```



**3.search.js文件(位于/assets/js/中)**
```js
// 获取搜索框、搜索按钮、清空搜索、结果输出对应的元素
var elSearchBox = document.querySelector(".search"),
    elSearchBtn = document.querySelector(".search-start"),
    elSearchClear = document.querySelector(".search-clear"),
    elSearchInput = document.querySelector(".search-input"),
    elSearchResults = document.querySelector(".search-results");

// 声明保存文章的标题、链接、内容的数组变量
var searchValue = "",
    arrItems = [],
    arrContents = [],
    arrLinks = [],
    arrTitles = [],
    arrResults = [],
    indexItem = [],
    itemLength = 0;
var tmpDiv = document.createElement("div"),
    tmpAnchor = document.createElement("a");
var isSearchFocused = false;

// ajax 的兼容写法
var xhr = new XMLHttpRequest() || new ActiveXObject("Microsoft.XMLHTTP");

// 获取根目录下 feed.xml 文件内的数据
xhr.onreadystatechange = function () {
    if (xhr.readyState == 4 && xhr.status == 200) {
        var xml = xhr.responseXML;
        if (!xml) return;

        arrItems = xml.getElementsByTagName("item");
        itemLength = arrItems.length;

        // 遍历并保存所有文章对应的标题、链接、内容到对应的数组中
        // 同时过滤掉 HTML 标签
        for (i = 0; i < itemLength; i++) {
            arrContents[i] = arrItems[i]
                .getElementsByTagName("description")[0]
                .childNodes[0].nodeValue.replace(/<.*?>/g, "");
            arrLinks[i] = arrItems[i]
                .getElementsByTagName("link")[0]
                .childNodes[0].nodeValue.replace(/<.*?>/g, "");
            arrTitles[i] = arrItems[i]
                .getElementsByTagName("title")[0]
                .childNodes[0].nodeValue.replace(/<.*?>/g, "");
        }

        // 内容加载完毕后显示搜索框
        elSearchBox.style.display = "block";
    }
};
xhr.open("get", "/feed.xml", true);
xhr.send();

// 绑定按钮事件
elSearchBtn.onclick = searchConfirm;
elSearchClear.onclick = searchClear;

// 输入框内容变化后就开始匹配，可以不用点按钮
// 经测试，onkeydown, onchange 等方法效果不太理想，
// 存在输入延迟等问题，最后发现触发 input 事件最理想，
// 并且可以处理中文输入法拼写的变化
elSearchInput.oninput = function () {
    setTimeout(searchConfirm, 0);
};
elSearchInput.onfocus = function() {
    isSearchFocused = true;
}
elSearchInput.onblur = function() {
    isSearchFocused = false;
}

/** 搜索确认 */
function searchConfirm() {
    if (elSearchInput.value == "") {
        searchClear();
    } else if (elSearchInput.value.search(/^\s+$/) >= 0) {
        // 检测输入值全是空白的情况
        searchInit();
        var itemDiv = tmpDiv.cloneNode(true);
        itemDiv.innerText = "Please enter valid content...";
        elSearchResults.appendChild(itemDiv);
    } else {
        // 合法输入值的情况
        searchInit();
        searchValue = elSearchInput.value;
        // 在标题、内容中匹配搜索值
        searchMatching(arrTitles, arrContents, searchValue);
    }
}

/** 搜索清空 */
function searchClear() {
    elSearchInput.value = "";
    elSearchClear.style.display = "none";
    elSearchResults.style.display = "none";
    elSearchResults.classList.remove("result-item");
}

/** 每次搜索完成后的初始化 */
function searchInit() {
    arrResults = [];
    indexItem = [];
    elSearchResults.innerHTML = "";
    elSearchClear.style.display = "block";
    elSearchResults.style.display = "block";
    elSearchResults.classList.add("result-item");
}

/**
 * 匹配搜索内容
 * @param {string[]} arrTitles   - 所有文章标题
 * @param {string[]} arrContents - 所有文件内容
 * @param {string}   input       - 搜索内容
 */
function searchMatching(arrTitles, arrContents, input) {
    var inputReg;

    try {
        // 转换为正则表达式，忽略输入大小写
        inputReg = new RegExp(input, "i");
    } catch (_) {
        var errorInputDiv = tmpDiv.cloneNode(true);

        errorInputDiv.innerHTML =
            '正则表达式语法错误，特殊符号前考虑加上转义符："&Backslash;"';
        errorInputDiv.className = "pink-text result-item";
        elSearchResults.appendChild(errorInputDiv);
        return;
    }

    // 在所有文章标题、内容中匹配搜索值
    for (i = 0; i < itemLength; i++) {
        var titleIndex = arrTitles[i].search(inputReg);
        var contentIndex = arrContents[i].search(inputReg);
        var resultIndex, resultArr;

        if (titleIndex !== -1 || contentIndex !== -1) {
            // 优先搜索标题
            if (titleIndex !== -1) {
                resultIndex = titleIndex;
                resultArr = arrTitles;
            } else {
                resultIndex = contentIndex;
                resultArr = arrContents;
            }

            // 保存匹配值的索引
            indexItem.push(i);

            var len = resultArr[i].match(inputReg)[0].length;
            var step = 10;

            // 将匹配到内容的地方进行黄色标记，并包括周围一定数量的文本
            arrResults.push(
                resultArr[i].slice(resultIndex - step, resultIndex) +
                    "<mark>" +
                    resultArr[i].slice(resultIndex, resultIndex + len) +
                    "</mark>" +
                    resultArr[i].slice(
                        resultIndex + len,
                        resultIndex + len + step
                    )
            );
        }
    }

    // 输出总共匹配到的数目
    var totalDiv = tmpDiv.cloneNode(true);

    totalDiv.className = "result-title";
    totalDiv.innerHTML = "A total of <b>" + indexItem.length + " items were matched";
    elSearchResults.appendChild(totalDiv);

    // 未匹配到内容的情况
    if (indexItem.length == 0) {
        var noneItemDiv = tmpDiv.cloneNode(true);

        noneItemDiv.innerText = "No content was matched...";
        noneItemDiv.className = "teal-text text-darken-3 result-item";
        elSearchResults.appendChild(noneItemDiv);
    }

    // 将所有匹配内容进行组合
    for (i = 0; i < arrResults.length; i++) {
        var itemDiv = tmpDiv.cloneNode(true);
        var itemTitleDiv = tmpDiv.cloneNode(true);
        var itemDetailDiv = tmpDiv.cloneNode(true);
        var itemDetailDivAnchor = tmpAnchor.cloneNode(true);

        itemDiv.className = "card hoverable result-item";
        itemTitleDiv.className = "card-content result-item-title";
        itemDetailDiv.className = "card-action result-item-detail";
        itemDetailDivAnchor.className = "blue-text";

        itemTitleDiv.innerText = arrTitles[indexItem[i]];
        itemDetailDivAnchor.innerHTML = arrResults[i];
        itemDetailDivAnchor.href = arrLinks[indexItem[i]];

        itemDiv.appendChild(itemTitleDiv);
        itemDetailDiv.appendChild(itemDetailDivAnchor);
        itemDiv.appendChild(itemDetailDiv);

        elSearchResults.appendChild(itemDiv);
    }
}

window.addEventListener("load", searchClear);


// 搜索快捷键
document.addEventListener('keydown', function(evt) {
    if (isSearchFocused) return;
    if (evt.key === '/') {
        evt.preventDefault();
        elSearchInput.focus();
        window.isSearchFocused = true;
    }
})
```
**4.最后，在index.md中引入search.html, 并添加以下代码：**
```html
<script async src="/assets/js/search.js"></script>
```
- 参考资料：
- [Jekyll实现搜索功能](https://zorchp.github.io/frontend/Jekyll%E5%8D%9A%E5%AE%A2%E4%B8%AD%E6%B7%BB%E5%8A%A0%E5%85%A8%E6%96%87%E6%9C%AC%E6%90%9C%E7%B4%A2%E7%9A%84%E6%9C%80%E4%BD%B3%E6%96%B9%E6%A1%88%E4%B8%8E%E8%B8%A9%E5%9D%91%E8%AE%B0%E5%BD%95/)

- [个人网站添加搜索功能](https://knightyun.github.io/2019/03/04/articles-search)

## 如何在博客页面底端增加上一条/下一条并显示标题
在post.html中添加以下代码：
```html
<nav class="post_navigation">
  {% if page.previous.url %}
    <a href="{{ page.previous.url }}">&laquo; {{ page.previous.title }}</a>
  {% endif %}
  {% if page.next.url %}
    <a href="{{ page.next.url }}">{{ page.next.title }} &raquo;</a>
  {% endif %}
</nav>
```

## 在博客页面添加返回顶部按钮（主页没有）
**1.添加back_top_button.html**
```html
<div id="back-top" style="position: fixed; bottom: 70px; right: 200px;">
    <a href="#top" title="Back to top">
      Back to top ↑
    </a>
</div>

<script type="text/javascript">
    $("#back-top").hide();
    $(document).ready(function () {
        $(window).scroll(function () {
            if ($(this).scrollTop() > 100) {
                $('#back-top').fadeIn();
            } else {
                $('#back-top').fadeOut();
            }
        });
        $('#back-top a').click(function () {
            $('body,html').animate({
                scrollTop: 0
            }, 500);
            return false;
        });
    });
</script>
```
**2.在post.html中引入back_top_button.html**

## 如何将博客从其他站点导入
## 如何生成侧边栏目录
## 如何增加博客评论功能
## 根目录下的文件如何分类
## 代码折叠（同时高亮）与复制功能



## 学习资料
- [51 tutorials to help you with Jekyll and CloudCannon](https://learn.cloudcannon.com/jekyll-blogging/#list)
- [Jekyll中文文档](https://jekyllcn.com/)
- [zoharandroid 主页](https://github.com/ZoharAndroid/zoharandroid.github.io/tree/master)