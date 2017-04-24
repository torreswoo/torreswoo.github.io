---
bg: "posts/tools/jekyll/jekyll_bg.png"
layout: post
title:  "Tools - Github page & Jekyll & Google Analytics"
crawlertitle: "Github page & Jekyll & Google Analytics"
summary: "Github page & Jekyll & Google Analytics"
date:   2017-04-24 14:09:47 +0700
categories: posts
tags: ['Tools']
author: torreswoo
---

## contents
* Jekyll
** Jeykll install
** Jekyll 기본 구조 및 설정
** Jekyll post 글 작성하기

* Google Analytics
* Google Search Console 
** google & naver & daum 
** sitemap, RSS feed, robots.txt 생성
* Google Data studio

---
# Jekyll

## Jekyll install
- 01.install - MAC OS X
** ruby / jekyll / git 설치
- 02.Jekyll themes - 사이트에서 git clone
** https://jekyllthemes.io/

{% highlight bash %}
#. jekyll 설치
$ [sudo] gem install jekyll
$ jekyll new 깃허브사용자명.github.com // 깃허브사용자명.github.com 디렉토리를 만들어 기본 디렉토리와 화일을 생성한다.
$ jekyll serve --watch  // localhost:4000
{% endhighlight %}

## Jekyll 기본 구조 및 설정
- 디렉터리 구조 - http://jekyllrb-ko.github.io/docs/structure/
![](/assets/images/posts/tools/jekyll/jekyll_dir_info.png)

- 환경설정 : _config.yml 작성
{% highlight bash %}
# Site settings
title: awesome posts
email: torreswoo@gmail.com
description: > # this means to ignore newlines until "baseurl:"
baseurl: "" # the subpath of your site, e.g. /blog
lang: "en"
url: "http://localhost:4000" # the base hostname & protocol for your site
github_username: torreswoo
linkedin_username: ....
facebook_username: torres.woo
 
# Build settings
markdown: kramdown
relative_permalinks: false
permalink: /:categories/:title/
 
# Posts settings
future: true
safe: false
lsi: false
excerpt_separator: "\n\n"
 
compress_html:
  comments: ["<!-- ", " -->"]
  clippings: all
  endings: all
 
sass:
  sass_dir: assets/_sass
  style: compressed
 
images: '/assets/images'
{% endhighlight %}