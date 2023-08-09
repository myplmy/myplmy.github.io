---
layout  : post
title   : jekyll wiki 마이그레이션
summary : jekyll wiki skeleton 가져와서 수정하기
date    : 2023-08-09 22:28:00 +0900
updated : 2023-08-10 00:04:00 +0900
tag     : jekyll-wiki
resource: 68/A3B727-C89E-4EEF-8576-8A05AA36BFCB
toc     : true
comment : true
public  : true
---
* TOC
{:toc}

github.io를 활용한 블로그-wiki 만들기를 시작했다. [^1]

생각보다 수정할 부분이 많아서 기록을 남겨둔다.

필자는 windows 환경에서 작업중이고, vim을 사용하지 않고 notepad++ 을 사용하는 바... vimwiki는 아니게 되었다.

이거 notepad++로 쓰려니까 죽겠다. wiki 편집이 용이하도록 하려면 어떻게 해야 좋을 지 긴급하게 고민을 좀 해봐야겠다.


## fork
github의 skeleton[^2]을 포크해왔다.

## _config.yml
jekyll 설정파일로 보인다.
사이트 전반에 영향을 미치므로 오타나지 않게 잘 수정한다.

{{ "{{% site.edit" }} %}} 과 같이 변수마냥 활용할 수 있다. [^3]


### My information

```
email: 
author: 
profile: 
twitter_username: 
github_username: 
comment-support: 
```
 * email, twitter_username, github_username 은 작성시 index.html 에 연결되는 링크 버튼을 만들어준다.
 * author, profile, comment-support 는 뭐에 쓰는지 아직 확인되지 않았다.


### Comment Service

```
giscus:
  repo: 
  repo_id: 
  category: 
  category_id: 
```

giscus[^4]라고 하는 github app으로 댓글기능을 만들어준다.

이건 giscus 앱을 설치[^5]해야한다. 설치라고는 하지만 github repository에 연동하는 식이므로 부담갖지 않아도 된다.

giscus 페이지의 설정 항목에 가서 하나하나씩 값을 잘 설정하고 giscus 사용에 가서 코드를 확인한다.

repo_id와 category, category_id를 가져다가 붙여넣으면 된다.


실제로 적용시에는 어느 파일에서 적용하는 지 잘 모르겠다. footer쯤 될 것 같은데 추후 확인해봐야 한다.


### 그 외
```
google_analytics:
  ua: SAMPLE-VALUE
  encrypted_ua: THIS-IS-SAMPLE-VALUE

google_adsense:
  client: ???

blame: "https://github.com/github_id/github_id.github.io/blame/master/_wiki"
edit: "https://github.com/github_id/github_id.github.io/edit/master/_wiki"
issue: "https://github.com/github_id/github_id.github.io/issues/new"
```

 * google_analytics 와 google_adsense 는 무슨 값인지 잘 모르겠다. 추후 도입할 때 쯤 알아보도록 하자.
 * blame, edit, issue 는 github 기능의 blame 보기, 편집하기, 의견(issue) 남기기 기능을 활용한 것으로, 위키나 포스트 우측 상단에 표시되는 하이퍼텍스트 링크에 활용된다.


## google custom search engine

GCSE(google custom search engine,구글 맞춤 검색 엔진, 프로그래밍 검색 엔진)을 활용하여 웹페이지에서 검색이 가능하다.

 1. 구글 프로그래밍 검색 엔진 페이지에 가서 새 검색 엔진을 만든다 [^6]
 1. 디자인의 레이아웃을 '전체 너비'로 바꿔준다.
 1. search.html 가서 var cx 값을 바꿔준다.

```
     <script>
         (function() {
             var cx = '여기';
             var gcse = document.createElement('script');
             gcse.type = 'text/javascript';
             gcse.async = true;
             gcse.src = 'https://cse.google.com/cse.js?cx=' + cx;
             var s = document.getElementsByTagName('script')[0];
             s.parentNode.insertBefore(gcse, s);
         })();
     </script>
```

저 cx 값은 GCSE에서 만들면 알려준다. 남의 거 그대로 쓰면 남의 검색엔진 그대로 사용되니까 쓸모가 없다.

## google-site-verification

구글 서치콘솔에 사이트를 등록하려면 이 값을 수정해줘야 한다.

_includes/header.html 파일의 9번째 행쯤에 있는데, 이 값을 수정하자.

```
<meta name="google-site-verification" content="요거 수정" />
```

## post의 FrontMatter 중 resource

UUID 생성해서 넣어주면 된다.

```
명령프롬프트에서 실행
> powershell -Command "[guid]::NewGuid().ToString()"
```
출력된 값을 "앞에 두글자 / 나머지 뒷글자" 와 같이 나눠서 입력해주면 된다.
왜 나누는 지는 모르겠다. UUID 구조와는 관계가 없는 것 같은데..?
파워쉘로 예쁘게 잘라서 출력하는 방법 없나?


## 다음 이시간에 계속





[^1]: [https://github.com/johngrib/johngrib-jekyll-skeleton](https://github.com/johngrib/johngrib-jekyll-skeleton)

[^2]: [https://johngrib.github.io/wiki/my-wiki/](johngrib님 블로그 글 "Vimwiki + Jekyll + Github.io로 나만의 위키를 만들자")

[^3]: 이건 진짜 escape계의 레전드다. 엘레강스하게 미치는 방법 중 하나인 것 같다. <br> [https://github.com/scottkf/tesoriere.com/blob/master/_posts/2010-08-25-liquid-code-in-a-liquid-template-with-jekyll.markdown](liquid code in a liquid template with jekyll markdown)

[^4]: [https://giscus.app/ko](https://giscus.app/ko)

[^5]: [https://github.com/apps/giscus](https://github.com/apps/giscus) : giscus 설치페이지

[^6]: [https://programmablesearchengine.google.com/about/](https://programmablesearchengine.google.com/about/)
