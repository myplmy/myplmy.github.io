---
layout  : post
title   : vscode wiki 플러그인 수정(1)
summary : vscode wiki를 jekyll 블로그에 맞게 수정해보기
date    : 2023-10-16 21:10:00 +0900
updated : 2023-10-16 21:10:00 +0900
tag     : jekyll-wiki, typescript, vscode
resource: 63/56F77D-2841-4188-AD1D-5746BAEA59E3
toc     : true
comment : true
public  : true
---
* TOC
{:toc}

wiki 편집을 용이하게 하기 위해 vscode의 플러그인인 vscode wiki[^1]을 수정해보기로 했다.\
typescript로 되어있어서 수정하려면 공부를 좀 해야된다.

## 어떤 기능을 추가할까
### UI 및 신기능
#### 맨처음 위키 생성할 때 인터랙티브하게 초기세팅해주기
위키 폴더, index file 을 다이얼로그를 띄워서 찾도록 하는 정도는 해낸 것 같다. \
신규 작성시 header를 플러그인 옵션에서 설정할 수 있도록 해주면 좋을까?

#### wiki 문서 트리를 한눈에 보여주기
옆쪽에 새 탭 항목을 띄우는 것은 됐는데, 디렉토리와 폴더를 받아오고, 트리구조로 보여주는 것은 쉽지 않을 것 같다.

### 자동화
#### wiki 문서 생성시 jekyll 포맷에 맞춰서 생성해주기
아래와 같은 내용이 자동으로 들어갔으면 좋겠다.
```
---
layout  : post/wiki
title   : 
summary : 
date    : 2023-10-16 21:10:00 +0900
updated : 2023-10-16 21:10:00 +0900
tag     : jekyll-wiki, typescript, vscode
resource: 63/56F77D-2841-4188-AD1D-5746BAEA59E3
toc     : true
comment : true
public  : true
---
* TOC
{:toc}
```

resource는 현재 수동으로 만들고, "앞의 두자리/뒤의 나머지"와 같은 식으로 쓰고있다. \
만드는 방법은 아래와 같다. [이전 블로그 글에도 써놨다.](https://myplmy.github.io/blog/2023/08/09/migration#post의-frontmatter-중-resource)

```
powershell -Command "[guid]::NewGuid().ToString().ToUpper()"
```

가능하면 typescript에서 guid 만드는 방법을 알아내서 그쪽에서 처리해보자.


#### metadata 파일 자동으로 생성해주기
메타데이터는 postname.json 과 같은 형태로 메타데이터 폴더를 지정해서 넣어주면 될 것 같다. \
대신, layout을 읽어서 post면 type:"blog"로 하고, 날짜형태에 맞춰서 폴더도 생성해줘야되는 듯 하다. \
귀찮은데 blog를 죄다 wiki로 옮길까? 
```
{
 "type": "blog",
 "title": "vscode wiki 플러그인 수정(1)",
 "summary": "vscode wiki를 jekyll 블로그에 맞게 수정해보기",
 "url": "/blog/2023/08/09/migration",
 "updated": "2023-10-16 21:10:00 +0900",
 "resource": "63/56F77D-2841-4188-AD1D-5746BAEA59E3",
 "children": []
}
```

## typescript
자바스크립트를 표준어처럼 예쁘게 쓸 수 있게 해준다.\
속을 까보면 자바스크립트인데, 포장을 잘 해둔 것같다.

### 비교연산자
 * ==, != : 자료형 변환하여 일치시키고 비교한다
 * ===, !== : 자료형 변환하지 않고 비교한다. !==는 같은 자료형이 아닌 경우에도 true 를 반환한다.

### 에러처리 방법
 * https://immigration9.github.io/typescript/2022/01/09/error-typescript.html \
   타입스크립트는 무려 try{}catch(error){}에서 error의  type이 unknown이다.
 * https://stackoverflow.com/questions/40141005/property-code-does-not-exist-on-type-error
 * https://immigration9.github.io/typescript/2022/01/09/error-typescript.html

## eslint
perl의 strict;나 warning; 과 같은 역할을 해준다. \
한마디로 그지같이 짜면 컴파일도 안시켜주겠단 이야기다. \
대신 자동으로 척척 고쳐주는 기능도 있고, 문서화가 아주 잘 되어있어서 좋은 코딩 습관을 기를 수 있을 것 같다. 
### rule 사용법

eslint rule 사용법

.eslintrc.js 기준
```
rules: {
 "룰제목" : [ (0,1,2 혹은 "off", "warn", "error"), 옵션 , { "option for an exception" : 값 },
}
```
예시
```
rules:{
    "brace-style":[ "error" ,"allman", {"allowSingleLine": true}],
}
```

## vscode plugin
### 개발환경 설정
 * Node.js 필요
 * npm, chocolatey 를 깔면 편하다.
 * visualstudio2019buldtool이 안깔리면 https://community.chocolatey.org/packages/visualstudio2019buildtools

### 가이드
 * vscode 익스텐션 가이드 \
   한국어 번역이 있다(!)
   * https://github.com/pg-vscode-extn-kr/pg-vscode-extn-kr.github.io/tree/master
   * https://pg-vscode-extn-kr.github.io/

 ※ UX 가이드라인은 번역이 없다....

 * UX 가이드라인 (sidebar의 view를 활용해보자)
   * https://code.visualstudio.com/api/extension-guides/webview (커스터마이즈 가능 but 무겁다.)
   * https://code.visualstudio.com/api/ux-guidelines/views#welcome-views
   * https://code.visualstudio.com/api/references/contribution-points#contributes.viewsWelcome
 * inputbox
   * https://stackoverflow.com/questions/39481386/how-to-create-a-custom-dialog-in-vscode
   * https://code.visualstudio.com/api/references/vscode-api#window.showInputBox


### 참고자료
 * [이상훈님의 Medium 글 "VS Code Extension 개발하기"](https://medium.com/frontend-developers/vs-code-extension-%EA%B0%9C%EB%B0%9C%ED%95%98%EA%B8%B0-ae933343d2b5)
 * [강디너님의 블로그 글 "Vscode Extension (플러그인) 만들기_3"](https://kdinner.tistory.com/8)
 * [올빼밋님의 블로그 글 "[TOY] VScode Extension 만들기(4) - 코딩하는 고양이[test]"](https://olppaemmit.tistory.com/146)


[^1]: [https://github.com/hannut91/vs-code-wiki](https://github.com/hannut91/vs-code-wiki)