<<<<<<< HEAD
# WurstScript 网站与文档 （中文）
=======
[![Build Status](https://travis-ci.org/wurstscript/wurstscript.github.io.svg?branch=master)](https://travis-ci.org/wurstscript/wurstscript.github.io)
# WurstScript Webpage & Documentation
>>>>>>> mainsite/master

在线访问：http://wurst.wow8.org/

该文件夹包含WurstScript网站的完整源代码。这是一个jekyll项目，并通过github-pages进行部署。

* 网站设计来源：https://github.com/xriley/PrettyDocs-Theme
* 原仓库：https://github.com/wurstscript/wurstscript.github.io

<<<<<<< HEAD
## 教程
=======
# Developer Information

## Serving the page locally:

1. Check whether you have Ruby 2.1.0 or higher installed:

    `ruby --version`

2. Install Bundler:

    `gem install bundler`

3. Execute `bundle install` in this repository.
4. Run the server with:

    `bundle exec jekyll serve`

## Styling

We use Sass as stylesheet language and you can find the files inside the `_sass` folder.

## Tutorials
>>>>>>> mainsite/master

教程作为文档的一部分包含在它们自己的章节中。

创建一个新的教程:

<<<<<<< HEAD
* 在`_doc/tutorials/`中创建一个新的目录索引文件，比如`cp wurstbeginner.md new_tutorial.md`。
* Create a new set of tuturial pages in `_doc/tutorials/` - e.g. `cp -r wurstbeginner new_tutorial`.
=======
* Create a new table of contents file in `_doc/tutorials/`, for example `cp wurstbeginner.md new_tutorial.md`.
* Create a new set of tuturial pages in a subfolder of `_doc/tutorials/` - e.g. `cp -r wurstbeginner new_tutorial`.
>>>>>>> mainsite/master
* Define the pages by changing the contents of `_doc/tutorials/new_tutorial/` - usually one or two markdown files will suffice.
* Add the new tutorial to the tutorials listing in `_doc/tutorials.md` - just need to add a uri in the `navigation` section.
* Setup `new_tutorial.md` as necessary for your pages:
    - Edit the title, excerpt, date, icon, color.
    - Change the `sections` to match the uri of the pages of your tutorial.
* Write your tutorial pages, being sure to update the heading sections as necessary.

<<<<<<< HEAD
## 在本地搭建页面

1. 检查你是否安装了ruby 2.1.0或者更高的版本：`ruby --version`
2. 安装 Bundler：`gem install bundler`
3. 在本仓库的根目录中执行 `bundle install`
4. 运行服务器：`bundle exec jekyll serve`
=======
## Standard library doc

Adding a standard library doc page works almost the same as tutorials.

* Create a new table of contents file in `_doc/stdlib/`
* Create a new set of tuturial pages in a subfolder of `_doc/stdlib/`, e.g. `_doc/stdlib/new_doc`
* Define the pages by changing the contents of `_doc/stdlib/new_doc/`
* __This part differs__ Add the new doc page to the stdlib index in `_doc/stdlib/intro.md`. Entries should be alphabetically sorted.
* Setup `new_doc.md` as necessary for your pages:
    - Edit the title, excerpt, date, color.
    - Change the `sections` to match the uri of the pages of your tutorial.
* Write your new_doc pages, being sure to update the heading sections as necessary.
>>>>>>> mainsite/master
