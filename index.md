---
layout: home
---

<!---------------------------search bar without css--------------------------------------->

<!-- <script async src="/assets/js/search.js"></script>

<div class="search-container" style="position: absolute; top: 95px; right: 60px; margin: 0px;">
    <i class="material-icons search-icon search-start">search</i>
    <input type="text" class="search-input" placeholder="Searching..." />
    <i class="material-icons search-icon search-clear">clear</i>
    <div class="search-results z-depth-4"></div>
</div> --> 

<!----------------------------search bar with css-------------------------------------->

{% include search.html %}

<script async src="/assets/js/search.js"></script>

<!-----------------------------------LOGO-------------------------------------->

<div style="position: relative;">
    <img src="pot.png" alt="Image" style="position: absolute; bottom: 610px; left: 430px; width: 70px; height: auto;">
</div>

<!-----------------------------图片跳转搜索页面------------------------------------->

<!-- <div style="position: relative;">
    <a href="search.html">
        <img src="beagle.png" alt="Image" style="position: absolute; bottom: 610px; left: 550px; width: 80px; height: auto;">
    </a>
</div> -->

<!-- style = 'fixed' -->