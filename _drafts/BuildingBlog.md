1. 如何使代码块高亮
   [需安装rouge](https://blog.walterlv.com/post/available-themes-of-rouge-style.html)
2. 如何将博客从其他站点导入
3. 如何在主页增加图片
    修改index.md
4. 如何在主页增加搜索框: index.md, search.js, search.css, search.html
5. 如何生成侧边栏目录
6. 如何增加博客评论功能
7.  根目录下的文件如何分类
8.  添加返回顶部按钮 :back_to_top.html, post.html
9.  如何在博客页面底端增加上一条/下一条并显示标题: post.html
在post.html中添加
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


学习资料
- [51 tutorials to help you with Jekyll and CloudCannon](https://learn.cloudcannon.com/jekyll-blogging/#list)
- [Jekyll中文文档](https://jekyllcn.com/)
- [zoharandroid 主页](https://github.com/ZoharAndroid/zoharandroid.github.io/tree/master)