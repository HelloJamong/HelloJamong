# .gitignore 작성 가이드

Git에서 추적하지 않을 파일을 지정하는 .gitignore 파일 작성법을 설명합니다.

---

## 📚 목차
1. [.gitignore란?](#gitignore란)
2. [기본 문법](#기본-문법)
3. [패턴 예시](#패턴-예시)
4. [언어/프레임워크별 템플릿](#언어프레임워크별-템플릿)
5. [자주 무시하는 파일들](#자주-무시하는-파일들)
6. [실전 팁](#실전-팁)

---

## .gitignore란?

`.gitignore` 파일은 Git이 추적하지 않을 파일이나 디렉토리를 지정하는 설정 파일입니다.

### 왜 필요한가?

- **민감한 정보 보호**: API 키, 비밀번호, 환경 변수 파일
- **불필요한 파일 제외**: 빌드 결과물, 로그 파일, 임시 파일
- **개발 환경 차이**: IDE 설정, OS별 시스템 파일
- **저장소 크기 관리**: 용량이 큰 바이너리 파일, 의존성 패키지

---

## 기본 문법

### 주석
```gitignore
# 이것은 주석입니다
# 라인 앞에 #을 붙이면 주석 처리됩니다
```

### 파일과 디렉토리 무시

```gitignore
# 특정 파일 무시
secret.txt

# 특정 확장자 무시
*.log
*.tmp

# 특정 디렉토리 무시
node_modules/
build/
dist/

# 모든 하위 디렉토리의 특정 파일 무시
**/logs
**/temp
```

### 패턴 규칙

| 패턴 | 의미 | 예시 |
|-----|------|------|
| `*` | 0개 이상의 문자 | `*.log` → 모든 .log 파일 |
| `**` | 모든 디렉토리 | `**/foo` → 모든 foo 파일/폴더 |
| `?` | 정확히 1개 문자 | `file?.txt` → file1.txt, fileA.txt |
| `[abc]` | 괄호 안의 문자 중 하나 | `file[12].txt` → file1.txt, file2.txt |
| `!` | 무시 규칙 제외 (예외) | `!important.log` → 무시하지 않음 |
| `/` | 루트 디렉토리 | `/config.js` → 루트의 config.js만 |

### 예외 처리

```gitignore
# 모든 .log 파일 무시
*.log

# 단, important.log는 추적
!important.log

# logs 디렉토리 무시
logs/

# 단, logs/keep.txt는 추적
!logs/keep.txt
```

---

## 패턴 예시

### 1. 특정 파일 무시
```gitignore
# 정확한 파일명
config.json
secret.key
.env
```

### 2. 확장자로 무시
```gitignore
# 로그 파일
*.log
*.logs

# 임시 파일
*.tmp
*.temp
*.swp
*.swo

# 백업 파일
*.bak
*.backup
*~
```

### 3. 디렉토리 무시
```gitignore
# 빌드 결과물
build/
dist/
out/

# 의존성
node_modules/
vendor/
packages/

# 캐시
.cache/
__pycache__/
*.pyc
```

### 4. 복잡한 패턴
```gitignore
# test 디렉토리 아래의 모든 .log 파일
test/**/*.log

# 루트의 TODO 파일만 무시 (하위 디렉토리의 TODO는 추적)
/TODO

# doc 디렉토리를 제외한 모든 .pdf 무시
*.pdf
!doc/*.pdf
```

---

## 언어/프레임워크별 템플릿

### Node.js / JavaScript
```gitignore
# 의존성
node_modules/
npm-debug.log*
yarn-debug.log*
yarn-error.log*
package-lock.json
yarn.lock

# 환경 변수
.env
.env.local
.env.*.local

# 빌드 결과
dist/
build/
.next/
.nuxt/

# 테스트 커버리지
coverage/
.nyc_output/

# IDE
.vscode/
.idea/
*.swp
*.swo
```

### Python
```gitignore
# 바이트 컴파일 파일
__pycache__/
*.py[cod]
*$py.class

# 가상환경
venv/
env/
ENV/
.venv

# 배포
*.egg-info/
dist/
build/
*.egg

# Jupyter Notebook
.ipynb_checkpoints

# 환경 변수
.env
*.env

# 테스트
.pytest_cache/
.coverage
htmlcov/
```

### Java / Spring
```gitignore
# 컴파일 결과
*.class
*.jar
*.war
*.ear
target/
build/

# Gradle
.gradle/
gradle-app.setting
!gradle-wrapper.jar

# Maven
pom.xml.tag
pom.xml.releaseBackup
pom.xml.versionsBackup

# IDE
.idea/
*.iml
*.iws
.vscode/
.settings/
.classpath
.project

# 로그
*.log
logs/
```

### React / Vue
```gitignore
# 의존성
node_modules/

# 빌드
build/
dist/
.next/
out/

# 환경 변수
.env
.env.local
.env.development.local
.env.test.local
.env.production.local

# 로그
npm-debug.log*
yarn-debug.log*
yarn-error.log*

# 기타
.DS_Store
.cache/
```

### Docker
```gitignore
# Docker
.dockerignore
docker-compose.override.yml
.env.docker
```

---

## 자주 무시하는 파일들

### OS별 시스템 파일

```gitignore
# macOS
.DS_Store
.AppleDouble
.LSOverride
._*

# Windows
Thumbs.db
ehthumbs.db
Desktop.ini
$RECYCLE.BIN/

# Linux
*~
.directory
.Trash-*
```

### IDE 설정 파일

```gitignore
# Visual Studio Code
.vscode/
*.code-workspace

# IntelliJ IDEA
.idea/
*.iml
*.iws

# Eclipse
.classpath
.project
.settings/

# Vim
*.swp
*.swo
*~

# Sublime Text
*.sublime-project
*.sublime-workspace
```

### 민감한 정보

```gitignore
# 환경 변수
.env
.env.*
*.env

# API 키 / 인증
secrets.yml
credentials.json
*.key
*.pem
*.cert

# 데이터베이스
*.sql
*.sqlite
*.db
```

### 빌드 및 패키지

```gitignore
# 빌드 결과
build/
dist/
out/
target/
bin/

# 패키지 관리자
node_modules/
vendor/
packages/
bower_components/

# 압축 파일
*.zip
*.tar.gz
*.rar
*.7z
```

---

## 실전 팁

### 1. 전역 .gitignore 설정

모든 프로젝트에 공통으로 적용할 무시 규칙을 설정할 수 있습니다.

```bash
# 전역 gitignore 파일 생성
touch ~/.gitignore_global

# Git 전역 설정에 등록
git config --global core.excludesfile ~/.gitignore_global
```

`~/.gitignore_global` 파일 예시:
```gitignore
# OS
.DS_Store
Thumbs.db

# IDE
.vscode/
.idea/
*.swp
```

### 2. 이미 추적 중인 파일 무시하기

이미 Git이 추적하고 있는 파일을 나중에 무시하려면:

```bash
# 1. .gitignore에 파일 추가
echo "config.json" >> .gitignore

# 2. Git 캐시에서 제거 (파일은 유지)
git rm --cached config.json

# 3. 커밋
git add .gitignore
git commit -m "chore: stop tracking config.json"
```

모든 무시된 파일을 캐시에서 제거:
```bash
git rm -r --cached .
git add .
git commit -m "chore: apply gitignore to all files"
```

### 3. .gitignore 템플릿 생성기 활용

**gitignore.io 사용하기**
```bash
# curl로 템플릿 생성
curl -L https://www.toptal.com/developers/gitignore/api/node,react,vscode > .gitignore

# 또는 웹사이트 방문
# https://www.gitignore.io
```

**GitHub 템플릿 활용**
- https://github.com/github/gitignore
- 다양한 언어/프레임워크별 템플릿 제공

### 4. .gitignore 확인하기

```bash
# 특정 파일이 무시되는지 확인
git check-ignore -v filename.txt

# 무시되는 모든 파일 확인
git status --ignored

# .gitignore에 매칭되는 파일 확인
git ls-files --ignored --exclude-standard
```

### 5. 디렉토리 구조 유지하기

빈 디렉토리는 Git이 추적하지 않으므로, `.gitkeep` 파일을 사용합니다.

```bash
# logs 디렉토리는 유지하되, 내부 파일은 무시
mkdir logs
touch logs/.gitkeep
```

`.gitignore`:
```gitignore
# logs 디렉토리의 모든 파일 무시
logs/*

# 단, .gitkeep은 추적
!logs/.gitkeep
```

### 6. 프로젝트별 추천 구조

```gitignore
# ==========================================
# 환경 변수 및 민감 정보
# ==========================================
.env
.env.*
secrets.yml
*.key

# ==========================================
# 의존성
# ==========================================
node_modules/
vendor/

# ==========================================
# 빌드 결과
# ==========================================
build/
dist/
*.min.js
*.min.css

# ==========================================
# 로그 및 임시 파일
# ==========================================
*.log
logs/
*.tmp
*.temp

# ==========================================
# OS
# ==========================================
.DS_Store
Thumbs.db

# ==========================================
# IDE
# ==========================================
.vscode/
.idea/
*.swp

# ==========================================
# 테스트
# ==========================================
coverage/
.nyc_output/
```

### 7. 주의사항

1. **.gitignore는 추적되지 않은 파일에만 적용됨**
   - 이미 커밋된 파일은 `.gitignore`에 추가해도 무시되지 않음
   - `git rm --cached`로 먼저 제거 필요

2. **패턴 순서 중요**
   - 나중에 나오는 규칙이 우선순위가 높음
   - 예외 처리(`!`)는 무시 규칙 다음에 작성

3. **민감 정보 유출 주의**
   - `.env`, API 키 등은 반드시 무시
   - 이미 커밋된 민감 정보는 히스토리에서 완전 삭제 필요

4. **팀 협업 시**
   - 프로젝트 공통 `.gitignore`: 저장소에 포함
   - 개인 환경 설정: 전역 `.gitignore` 사용

---

## 자주 묻는 질문

**Q: .gitignore 파일 자체를 무시할 수 있나요?**
A: 할 수 있지만 권장하지 않습니다. `.gitignore`는 보통 프로젝트에 포함되어야 합니다.

**Q: 이미 푸시한 민감 정보를 삭제하려면?**
A: `git filter-branch` 또는 `BFG Repo-Cleaner`를 사용하여 히스토리에서 완전히 제거해야 합니다.

**Q: 하위 디렉토리에 별도의 .gitignore를 둘 수 있나요?**
A: 네, 각 디렉토리마다 `.gitignore`를 둘 수 있으며, 해당 디렉토리와 하위에만 적용됩니다.

---

## 유용한 링크

- [gitignore.io](https://www.gitignore.io) - 템플릿 생성기
- [GitHub gitignore 템플릿](https://github.com/github/gitignore)
- [Git 공식 문서](https://git-scm.com/docs/gitignore)

---

더 자세한 정보는 [Git 공식 문서 - gitignore](https://git-scm.com/docs/gitignore)를 참고하세요.
