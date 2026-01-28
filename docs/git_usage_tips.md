# Git 사용 가이드

Git 초보자를 위한 필수 명령어와 워크플로우 가이드입니다.

---

## 📚 목차
1. [커밋 메시지 작성법](#커밋-메시지-작성법)
2. [브랜치와 Merge](#브랜치와-merge)
3. [Git Stash 사용법](#git-stash-사용법)
4. [자주 사용하는 명령어 모음](#자주-사용하는-명령어-모음)

---

### 기본 워크플로우
```bash
# 1. 저장소 초기화 또는 클론
git init                    # 새 저장소 생성
git clone <URL>            # 원격 저장소 복사

# 2. 변경사항 확인
git status                 # 현재 상태 확인
git diff                   # 변경 내용 확인

# 3. 파일 추가 및 커밋
git add <파일명>            # 특정 파일 스테이징
git add .                  # 모든 변경사항 스테이징
git commit -m "메시지"     # 커밋 생성

# 4. 원격 저장소와 동기화
git pull origin main       # 원격 변경사항 가져오기
git push origin main       # 로컬 변경사항 업로드
```

---

## 커밋 메시지 작성법

좋은 커밋 메시지는 프로젝트 히스토리를 이해하기 쉽게 만듭니다.

### 커밋 타입

| 타입           | 의미    | 언제 쓰는가       |
| ------------ | ----- | ------------ |
| **feat**     | 기능 추가 | 새로운 기능       |
| **fix**      | 버그 수정 | 기존 동작 오류     |
| **refactor** | 리팩터링  | 동작 변화 없음     |
| **docs**     | 문서 수정 | README, 주석   |
| **style**    | 스타일   | 공백, 포맷, 세미콜론 |
| **test**     | 테스트   | 테스트 코드       |
| **chore**    | 기타 작업 | 빌드, 설정, 패키지  |

### 작성 예시

```
feat(api): add user registration
fix(api): handle empty request body
refactor(service): split auth logic
docs(readme): update install guide
chore(deps): bump mariadb to 10.11
```

---

## 브랜치와 Merge

브랜치를 사용하면 메인 코드를 건드리지 않고 새로운 기능을 개발할 수 있습니다.

### 브랜치 기본 명령어

```bash
# 브랜치 조회
git branch                 # 로컬 브랜치 목록
git branch -r              # 원격 브랜치 목록
git branch -a              # 모든 브랜치 목록

# 브랜치 생성 및 이동
git branch <브랜치명>       # 새 브랜치 생성
git checkout <브랜치명>     # 브랜치 이동
git checkout -b <브랜치명>  # 생성과 동시에 이동

# 최신 명령어 (Git 2.23+)
git switch <브랜치명>       # 브랜치 이동
git switch -c <브랜치명>    # 생성과 동시에 이동

# 브랜치 삭제
git branch -d <브랜치명>    # 병합된 브랜치 삭제
git branch -D <브랜치명>    # 강제 삭제
```

### Merge (병합) 사용법

```bash
# 1. 병합할 브랜치로 이동 (보통 main)
git checkout main

# 2. 다른 브랜치의 변경사항 병합
git merge <브랜치명>

# 3. 충돌 발생 시
# - 충돌 파일을 수동으로 수정
# - git add <파일명>
# - git commit
```

### 실전 워크플로우 예시

```bash
# 1. 새 기능 개발을 위한 브랜치 생성
git checkout -b feature/login

# 2. 작업 후 커밋
git add .
git commit -m "feat: implement login feature"

# 3. main 브랜치로 이동
git checkout main

# 4. 최신 변경사항 가져오기
git pull origin main

# 5. feature 브랜치 병합
git merge feature/login

# 6. 원격 저장소에 푸시
git push origin main

# 7. feature 브랜치 삭제 (선택)
git branch -d feature/login
```

---

## Git Stash 사용법

작업 중인 변경사항을 임시로 저장하고 싶을 때 사용합니다.

### 기본 사용법

```bash
# 현재 작업 임시 저장
git stash                  # 변경사항 스태시에 저장
git stash save "메시지"    # 메시지와 함께 저장

# 스태시 목록 확인
git stash list             # 저장된 스태시 목록
# 출력 예시:
# stash@{0}: WIP on main: abc1234 최근 커밋 메시지
# stash@{1}: On feature: xyz5678 이전 작업

# 스태시 적용
git stash apply            # 가장 최근 스태시 적용 (스태시 유지)
git stash apply stash@{1}  # 특정 스태시 적용
git stash pop              # 가장 최근 스태시 적용 및 삭제

# 스태시 삭제
git stash drop             # 가장 최근 스태시 삭제
git stash drop stash@{1}   # 특정 스태시 삭제
git stash clear            # 모든 스태시 삭제
```

### 활용 시나리오

**시나리오 1: 급한 버그 수정**
```bash
# 1. 기능 개발 중...
# 2. 급한 버그 수정 요청!
git stash save "작업 중인 로그인 기능"

# 3. main 브랜치로 이동하여 버그 수정
git checkout main
# 버그 수정 및 커밋

# 4. 다시 작업 중이던 브랜치로 돌아가기
git checkout feature/login
git stash pop
```

**시나리오 2: 브랜치 변경 전 임시 저장**
```bash
# 잘못된 브랜치에서 작업했을 때
git stash
git checkout <올바른-브랜치>
git stash pop
```

---

## 자주 사용하는 명령어 모음

### 히스토리 조회
```bash
git log                    # 커밋 히스토리
git log --oneline          # 간단하게 보기
git log --graph            # 그래프로 보기
git log -n 5               # 최근 5개만 보기
```

### 변경사항 되돌리기
```bash
# 작업 디렉토리 변경사항 취소
git checkout -- <파일명>

# 스테이징 취소
git reset HEAD <파일명>

# 마지막 커밋 수정
git commit --amend
```

### 원격 저장소 관리
```bash
git remote -v              # 원격 저장소 목록
git remote add origin <URL> # 원격 저장소 추가
git fetch origin           # 원격 변경사항 가져오기 (병합 안함)
git pull origin main       # 가져오기 + 병합
```

### 유용한 팁
```bash
# 특정 커밋으로 이동
git checkout <커밋해시>

# 파일 삭제 추적
git rm <파일명>

# 파일 이름 변경
git mv <이전이름> <새이름>

# .gitignore에 추가한 파일 캐시 삭제
git rm -r --cached .
git add .
git commit -m "chore: update gitignore"
```

---

## 문제 해결

### 자주 발생하는 문제

**충돌(Conflict) 해결**
1. `git status`로 충돌 파일 확인
2. 파일을 열어 `<<<<<<<`, `=======`, `>>>>>>>` 표시 찾기
3. 원하는 코드만 남기고 표시 삭제
4. `git add <파일명>`
5. `git commit`

**실수로 커밋했을 때**
```bash
# 커밋 취소하고 변경사항 유지
git reset --soft HEAD~1

# 커밋과 변경사항 모두 취소
git reset --hard HEAD~1
```

---

더 자세한 정보는 [Git 공식 문서](https://git-scm.com/doc)를 참고하세요.