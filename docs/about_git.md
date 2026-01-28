# Git 완전 정복 가이드

> 초보 Git 사용자(나)를 위한 가이드 문서 with Claude Code

Git을 처음 시작하는 분들을 위한 종합 가이드입니다. 아래 문서들을 통해 Git의 기본부터 GitHub Actions까지 모든 것을 배울 수 있습니다.

---

## 📖 학습 로드맵

![Git 명령어 치트시트](../images/git_guide_img.jpg)

---

## 🚀 가이드 문서 목록

### 1. [Git 사용 가이드](./git_usage_tips.md)

Git 초보자를 위한 필수 명령어와 워크플로우를 다룹니다.

**주요 내용:**
- Git 기본 명령어 및 워크플로우
- 커밋 메시지 작성법 (feat, fix, docs 등)
- 브랜치(Branch) 생성, 이동, 삭제
- Merge (병합) 사용법 및 충돌 해결
- Git Stash로 작업 임시 저장하기
- 자주 사용하는 명령어 모음
- 문제 해결 가이드

**이런 분들에게 추천합니다:**
- Git을 처음 접하는 초보자
- 기본 명령어를 익히고 싶은 분
- 브랜치와 병합 개념을 배우고 싶은 분

👉 [Git 사용 가이드 바로가기](./git_usage_tips.md)

---

### 2. [.gitignore 작성 가이드](./gitignore_guide.md)

Git에서 추적하지 않을 파일을 지정하는 .gitignore 파일 작성법을 다룹니다.

**주요 내용:**
- .gitignore란 무엇이고 왜 필요한가?
- 기본 문법 및 패턴 규칙 (`*`, `**`, `!` 등)
- 언어/프레임워크별 템플릿 (Node.js, Python, Java, React 등)
- 자주 무시하는 파일들 (OS, IDE, 민감 정보)
- 전역 .gitignore 설정 방법
- 이미 추적 중인 파일 무시하는 방법
- gitignore.io 템플릿 생성기 활용

**이런 분들에게 추천합니다:**
- 민감한 정보(API 키, 환경 변수)를 보호하고 싶은 분
- 프로젝트에 맞는 .gitignore를 작성하고 싶은 분
- 불필요한 파일(빌드 결과, 로그 등)을 제외하고 싶은 분

👉 [.gitignore 작성 가이드 바로가기](./gitignore_guide.md)

---

### 3. [GitHub Actions 작성 가이드](./github_actions_guide.md)

GitHub에서 제공하는 CI/CD 자동화 도구인 GitHub Actions 사용법을 다룹니다.

**주요 내용:**
- GitHub Actions 기본 개념 (Workflow, Job, Step, Action)
- Workflow 파일 구조 및 기본 문법
- 이벤트 트리거 (push, pull_request, schedule 등)
- 환경 변수와 Secrets 관리
- 실전 예제 (Node.js CI, Python CI/CD, Docker 빌드, 자동 배포)
- 자주 사용하는 Actions (checkout, setup-node, cache 등)
- Matrix 전략, 캐싱, Artifact 관리
- 문제 해결 가이드

**이런 분들에게 추천합니다:**
- 자동화된 테스트와 배포 환경을 구축하고 싶은 분
- CI/CD 파이프라인을 처음 만들어보는 분
- GitHub Actions로 생산성을 높이고 싶은 분

👉 [GitHub Actions 작성 가이드 바로가기](./github_actions_guide.md)

---

## 📚 학습 순서 추천

Git을 처음 배우는 분들을 위한 단계별 학습 순서입니다:

### Step 1: Git 기초 다지기
먼저 [Git 사용 가이드](./git_usage_tips.md)를 읽고 기본 명령어를 익히세요.
- `git init`, `git add`, `git commit`, `git push`
- 브랜치 생성 및 병합
- Git Stash 활용

### Step 2: 프로젝트 관리
[.gitignore 작성 가이드](./gitignore_guide.md)를 참고하여 불필요한 파일을 관리하세요.
- 민감한 정보 보호
- 언어/프레임워크에 맞는 템플릿 작성
- 전역 설정으로 편의성 향상

### Step 3: 자동화 구축
[GitHub Actions 가이드](./github_actions_guide.md)로 CI/CD 파이프라인을 만들어보세요.
- 자동 테스트 실행
- 코드 품질 검사
- 자동 배포 설정

---

## 💡 빠른 참조

### 자주 사용하는 Git 명령어

```bash
# 저장소 초기화 및 클론
git init
git clone <URL>

# 변경사항 확인 및 커밋
git status
git add .
git commit -m "메시지"
git push origin main

# 브랜치 관리
git branch                    # 브랜치 목록
git checkout -b <브랜치명>    # 브랜치 생성 및 이동
git merge <브랜치명>          # 브랜치 병합

# 작업 임시 저장
git stash
git stash pop
```

### 기본 .gitignore 템플릿

```gitignore
# 환경 변수
.env
.env.*

# 의존성
node_modules/
vendor/

# 빌드 결과
build/
dist/

# IDE
.vscode/
.idea/

# OS
.DS_Store
Thumbs.db
```

### 기본 GitHub Actions Workflow

```yaml
name: CI

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
      - run: npm ci
      - run: npm test
```

---

## 🔗 유용한 링크

### 공식 문서
- [Git 공식 문서](https://git-scm.com/doc)
- [GitHub Docs](https://docs.github.com)
- [GitHub Actions 문서](https://docs.github.com/en/actions)

### 도구 및 리소스
- [gitignore.io](https://www.gitignore.io) - .gitignore 템플릿 생성기
- [GitHub Marketplace](https://github.com/marketplace?type=actions) - Actions 검색
- [Git 치트시트](https://education.github.com/git-cheat-sheet-education.pdf)

### 학습 자료
- [Pro Git Book (무료)](https://git-scm.com/book/ko/v2)
- [GitHub Learning Lab](https://lab.github.com)
- [Visualizing Git](https://git-school.github.io/visualizing-git/)

---

## 📝 문서 개선에 참여하기

이 가이드는 Git 초보자를 위해 작성되었습니다. 
가이드를 작성한 당사자도 초보인지라 잘못된 내용이나 개선할 부분이 있다면 언제든지 피드백을 주세요!

---

<div align="center">

**Happy Coding with Git! 🚀**

*Made with ❤️ by Claude Code*

</div>
