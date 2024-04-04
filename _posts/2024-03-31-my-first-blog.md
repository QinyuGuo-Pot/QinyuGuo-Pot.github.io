---
layout: post
title: "How to build Blog based on Github pages and Jekyll"
category: others
---

* 目录
{:toc #markdown-toc}

### Step 1. Create a Github account


### Step 2. Select a favorite theme on [Jekyll Themes](http://jekyllthemes.org/) and fork it to your account


### Step 3. Rename the repository to: **username**.github.io 


### Step 4. Create a local repository

- Suggest creating a new folder (ex. mygit) on another disk (such as D:/ , not C:/)
- Right click on "Git Bash Here" in D:/mygit
- Initialize git repository\
`$ git init`
- git clone username.github.io to D:/mygit\
`$ git clone https://github.com/username/username.github.io.git`

### Step 5. Install Jekyll

- Install [Ruby+Devkit](https://rubyinstaller.org/downloads/) (**not recommended** to install it on the C:/)\
![alt text](https://raw.githubusercontent.com/QinyuGuo-Pot/blog-img/main/image.png)
- Right click on "Git Bash Here" in the installation folder (such as D:/software/Ruby)
- Install Jekyll and Bundler\
`$ gem install jekyll bundler`
- Check the version number to verify if the installation was successful\
`jekyll -v`

### Step 6. Create a new Jekyll site 

- Right click on "Git Bash Here" in D:/mygit
- Create a new folder by jekyll\
`$ jekyll new myblog`
- cd in myblog and Run jekyll\
`$ jekyll s`

- The command line will prompt that the service address is: http://127.0.0.1:4000

### Step 7. Transfer
 
- **Except** for the `.git` file, copy all other files from `/myblog` to `/username.github.io`, **do not** replace files with duplicate names
- Delete files with the same prefix but a `.md`suffix
- Open http://127.0.0.1:4000 in browser

### Step 8. Update all modifications in the local repository to the remote repository

```
$ git status 
$ git add .
$ git commit -m "Change Description"
$ git push origin master
```
- Open https://username/username.github.io in browser

### Step 9. Build picture bed
- Download PicGo
- Create a new repository named blog-img
- Picture bed configuration 
- MY Token: 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


### Tips
1. git push failed, unable to access...Timed out
    ```
    git config --global https.proxy http://127.0.0.1:1080

    git config --global https.proxy https://127.0.0.1:1080

    git config --global --unset http.proxy

    git config --global --unset https.proxy
    ```
2. [Message 'src refspec master does not match any' when pushing commits in Git](https://stackoverflow.com/questions/4181861/message-src-refspec-master-does-not-match-any-when-pushing-commits-in-git)

### References
1. [Install Jekyll](https://nxjniexiao.github.io/2018/08/17/jkeyll-install/)
2. [Jekyll Official document (English)](https://jekyllrb.com/), [(Chinese)](https://jekyllcn.com/docs/home/)
3. [How to quickly set up your own github blog](https://keysaim.github.io/post/blog/2017-08-15-how-to-setup-your-github-io-blog/)
4. [Blog building tutorial](https://github.com/qiubaiying/qiubaiying.github.io/wiki/%E5%8D%9A%E5%AE%A2%E6%90%AD%E5%BB%BA%E8%AF%A6%E7%BB%86%E6%95%99%E7%A8%8B)
5. [My jekyll theme](http://jekyllthemes.org/themes/no-style-please/)
6. [Picture bed configuration](https://zhuanlan.zhihu.com/p/353775844)