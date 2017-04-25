---
title: "Hexo를 이용하여 블로그 제작하기 (1)"
catalog: true
date: 2017-04-18 14:45:42
subtitle: "누구나 쉽게 제작 할 수 있는 블로그"
header-img:
tags:
- Hexo
- Blog
- Getting Started
categories:
- 개발
- Hexo
---
티스토리 블로그 초대장이 있었지만 깃헙으로 블로그를 만들면 무료로 호스팅을 쓸 수 있으므로 Hexo를 이용해 블로그를 제작해보습니다.

자바스크립트를 모르더라도 쉽게 만들 수 있다는 장점이 있습니다.

제작에 중점을 두었기 때문에 내용이 부족할수 있습니다.
궁굼한점은 덧글이나 [hexo](https://hexo.io/ko/) 홈페이지를 참조하시기 바랍니다.


# 시작전 준비 사항
---
- [Node.js](https://nodejs.org/ko/)
- [Git](https://git-scm.com/) (해당 포스팅에서는 Git 사용법은 다루지 않습니다.)
- [Github](https://github.com) 계정

위에 3가지가 모두 준비되었으면 npm을 이용하여 Hexo를 설치합니다.
```bash
npm install -g hexo-cli
```

# 블로그 프로젝트 생성
---
먼저 Github repository를 만듭니다.
repository 이름은 (Github username).github.io로 만듭시다.
repository 이름이 블로그 주소가 됩니다.
> 자세한건 [Github Page](https://pages.github.com/) 확인

repository가 만들어지면 터미널에서 해당 repository를 clone 받습니다.
```bash
git clone https://github.com/mirrors89/mirrors89.github.io.git
```

clone 받은 타겟 폴더에서 Hexo를 초기화하기 위해 아래의 명령을 수행합니다.
```bash
hexo init mirrors89.github.io.git
cd mirrors89.github.io.git
npm install
```



# 서버 실행
---
```bash
hexo server
```
로컬 서버 실행 후 https://localhost:4000 접속해봅니다.
