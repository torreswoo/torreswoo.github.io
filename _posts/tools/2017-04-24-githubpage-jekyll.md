---
bg: "posts/tools/jekyll/jekyll_bg.png"
layout: post
title:  "Github page / Jekyll / GA - 블로그 시작하기"
crawlertitle: "Github page & Jekyll & Google Analytics"
summary: "Github page & Jekyll & Google Analytics"
date:   2017-04-24 14:09:47 +0700
categories: posts
tags: ['Tools']
author: torreswoo
---

## contents
* Jekyll
- Jeykll install
- Jekyll 기본 구조 및 설정
- Jekyll post - 글 작성하기

* Google Analytics
* Google Search Console 
* Google Data studio

---
# Jekyll

### Jekyll install
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

### Jekyll 기본 구조 및 설정
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

### Jekyll post - 글 작성하기
- _posts 폴더에 모든 블로그 포스트 컨텐츠와 메타데이타가 있다. 지킬은 매우 스마트해서 이 폴더에 Markdown, HTML 과 다른 포맷을 섞어 넣을 수도 있다
- 블로그를 포스트하려면 마크다운 화일을 _posts 폴더에 떨어뜨리면 된다. 그러나, 포스트와 화일의 이름은 다음과 같은 이름 구조를 가져야한다:
- YYYY-MM-DD-[POST SLUG].[FORMAT]
- post slug는 포스트 이름의 URL 친화적인 형식을 말한다. 스페이스나 이상한 문자와 URL에서 보통 허용되지 않는 것을 쓰지말라는 것이다. 표준 관례는 모든 문자는 소문자, 스페이스 대신 -를, 비교적 짧은 slug를 사용하는 것을 포함

# Google Anaytics 설정추가
- 개인 블로그를 얼마나 사람들이 사용하는지 확인하고 싶어 GA를 붙어서 사용 상황을 볼 수 있다. 
- 실시간 탭에서는 정말 실시간 활성 사용자수와 트래픽을 확인 할 수 있다. 제대로된 분석 결과는 하루가 지나서야 확인 할 수 있는데, 어떤 글을 많이 보고 사용자의 행동 패턴등을 알 수 있어 재미있게 활용 할 수 있다
![](/assets/images/posts/tools/jekyll/ga.png)

- Google Analytics : https://analytics.google.com/analytics/web/
- 계정 torres > 속성 및 앱 torreswoo.github.io (추적ID: UA-...) >
- Jekyll에 GA추가 : https://aweekj.github.io/2016-08-08/jekyll-with-google-analytics/
{% highlight bash %}
# _config.yml - 01. GA id생성
google_analytics: "UA-..."
 
# google-analytics.html - 02. GA script파일작성
{% if site.google_analytics %}
<script>
    (function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
            (i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
        m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
    })(window,document,'script','https://www.google-analytics.com/analytics.js','ga');
    ga('create', '{{ site.google_analytics }}', 'auto');
    ga('send', 'pageview');
</script>
{% endif %}
 
# default.html - 03. GA script를 include
<html>
...
  {% include google-analytics.html %}
...
</html>
{% endhighlight %}

# Google Search Console
![](/assets/images/posts/tools/jekyll/ga_search_console.png)

- google & naver & daum에서 각각 블로그의 글들이 검색가능하게 설정을 해주어야한다. 웹마스터, 구글의 경우에서는 search console에서 설정을 해주어야한다. 
- sitemap, RSS feed, robots.txt 생성
- 참고 : http://jinyongjeong.github.io/2017/01/13/blog_make_searched/
- Google Search Console :https://www.google.com/webmasters/tools/
- Naver Web Master : http://webmastertool.naver.com/board/main.naver
- 필요파일작성
- sitemap.xml : https://github.com/torreswoo/torreswoo.github.io/blob/master/sitemap.xml
- feed.xml : https://github.com/torreswoo/torreswoo.github.io/blob/master/feed.xml
- robots.txt : https://github.com/torreswoo/torreswoo.github.io/blob/master/robots.txt

# Google Data studio
![](/assets/images/posts/tools/jekyll/ga_data_studio.png)

- https://datastudio.google.com/
- 데이터 소스가 GA, Google Search Console이 생겼으니 이제 시각화를 해보자.

### 참고사이트
- http://jekyllrb-ko.github.io/
- https://aweekj.github.io/2016-08-08/jekyll-with-google-analytics/
- jekyll intro : https://nolboo.kim/blog/2013/10/15/free-blog-with-github-jekyll/
- http://tech.kakao.com/2016/07/07/tech-blog-story/
- Hexo : http://holaxprogramming.com/2017/04/16/github-page-and-hexo/
- Hugo : https://gohugo.io/
- http://blog.saltfactory.net/upgrade-github-pages-dependency-versions/
- http://thdev.net/644
- http://dogfeet.github.io/articles/2012/github-pages.html
- http://blog.saltfactory.net/create-personal-web-site-using-with-github-pages/
