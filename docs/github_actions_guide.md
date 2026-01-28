# GitHub Actions 작성 가이드

GitHub에서 제공하는 CI/CD 자동화 도구인 GitHub Actions와 Workflow 작성법을 설명합니다.

---

## 📚 목차
1. [GitHub Actions란?](#github-actions란)
2. [기본 개념](#기본-개념)
3. [Workflow 파일 구조](#workflow-파일-구조)
4. [기본 문법](#기본-문법)
5. [실전 예제](#실전-예제)
6. [자주 사용하는 Actions](#자주-사용하는-actions)
7. [유용한 팁](#유용한-팁)

---

## GitHub Actions란?

GitHub Actions는 GitHub에서 제공하는 CI/CD(지속적 통합/배포) 플랫폼입니다.

### 주요 기능

- **자동화**: 코드 푸시, PR 생성 시 자동으로 작업 실행
- **CI/CD**: 테스트, 빌드, 배포 자동화
- **이벤트 기반**: 다양한 GitHub 이벤트에 반응
- **무료**: Public 저장소는 무료, Private 저장소도 일정 시간 무료

### 사용 사례

- 코드 푸시 시 자동 테스트 실행
- PR 생성 시 코드 린팅 및 포맷 검사
- 특정 브랜치에 푸시 시 자동 배포
- 이슈 자동 라벨링 및 관리
- 정기적인 작업 스케줄링 (크론잡)

---

## 기본 개념

### 핵심 용어

| 용어 | 설명 |
|-----|------|
| **Workflow** | 자동화된 프로세스 전체 (YAML 파일로 정의) |
| **Event** | Workflow를 트리거하는 이벤트 (push, pull_request 등) |
| **Job** | Workflow 내의 작업 단위 (여러 Step으로 구성) |
| **Step** | Job 내의 개별 작업 (명령어 실행 또는 Action 사용) |
| **Action** | 재사용 가능한 작업 단위 (GitHub Marketplace에서 제공) |
| **Runner** | Workflow를 실행하는 서버 (GitHub 제공 또는 자체 호스팅) |

### Workflow 동작 흐름

```
Event 발생 (push, PR 등)
    ↓
Workflow 트리거
    ↓
Runner에서 실행
    ↓
Job 1, Job 2, Job 3... (병렬 또는 순차 실행)
    ↓
각 Job 내의 Step 실행
    ↓
결과 확인 (성공/실패)
```

---

## Workflow 파일 구조

### 파일 위치

```
프로젝트-루트/
├── .github/
│   └── workflows/
│       ├── ci.yml          # CI 워크플로우
│       ├── deploy.yml      # 배포 워크플로우
│       └── test.yml        # 테스트 워크플로우
```

### 기본 구조

```yaml
name: Workflow 이름

on:
  # 이벤트 정의

jobs:
  job-name:
    runs-on: ubuntu-latest
    steps:
      - name: Step 이름
        run: 명령어
```

---

## 기본 문법

### 1. Workflow 이름 지정

```yaml
name: CI Pipeline
```

### 2. 이벤트 트리거 (on)

**단일 이벤트**
```yaml
on: push
```

**여러 이벤트**
```yaml
on: [push, pull_request]
```

**특정 브랜치/태그**
```yaml
on:
  push:
    branches:
      - main
      - develop
    tags:
      - v*
  pull_request:
    branches:
      - main
```

**특정 파일 경로**
```yaml
on:
  push:
    paths:
      - 'src/**'
      - '**.js'
```

**스케줄 (크론잡)**
```yaml
on:
  schedule:
    # 매일 오전 9시 (UTC)
    - cron: '0 9 * * *'
```

**수동 실행**
```yaml
on:
  workflow_dispatch:
    inputs:
      environment:
        description: '배포 환경'
        required: true
        default: 'staging'
```

### 3. Job 정의

**기본 Job**
```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Run build
        run: npm run build
```

**여러 OS에서 실행**
```yaml
jobs:
  test:
    runs-on: ${{ matrix.os }}
    strategy:
      matrix:
        os: [ubuntu-latest, windows-latest, macos-latest]
    steps:
      - run: echo "Testing on ${{ matrix.os }}"
```

**Job 순서 제어**
```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - run: npm run build

  test:
    runs-on: ubuntu-latest
    needs: build  # build가 완료된 후 실행
    steps:
      - run: npm test

  deploy:
    runs-on: ubuntu-latest
    needs: [build, test]  # build와 test 모두 완료 후 실행
    steps:
      - run: npm run deploy
```

### 4. Step 정의

**명령어 실행**
```yaml
steps:
  - name: Print message
    run: echo "Hello, GitHub Actions!"
```

**여러 명령어 실행**
```yaml
steps:
  - name: Multiple commands
    run: |
      npm install
      npm run build
      npm test
```

**Action 사용**
```yaml
steps:
  - name: Checkout repository
    uses: actions/checkout@v4

  - name: Setup Node.js
    uses: actions/setup-node@v4
    with:
      node-version: '20'
```

### 5. 환경 변수

**Workflow 레벨**
```yaml
env:
  NODE_ENV: production

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - run: echo $NODE_ENV
```

**Job 레벨**
```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    env:
      API_URL: https://api.example.com
    steps:
      - run: echo $API_URL
```

**Step 레벨**
```yaml
steps:
  - name: Build
    env:
      BUILD_MODE: release
    run: npm run build
```

### 6. Secrets 사용

GitHub 저장소 설정에서 Secrets를 등록하고 사용합니다.

```yaml
steps:
  - name: Deploy
    env:
      API_KEY: ${{ secrets.API_KEY }}
      DB_PASSWORD: ${{ secrets.DB_PASSWORD }}
    run: ./deploy.sh
```

### 7. 조건부 실행

```yaml
steps:
  - name: Deploy to production
    if: github.ref == 'refs/heads/main'
    run: npm run deploy

  - name: Run on PR
    if: github.event_name == 'pull_request'
    run: npm test
```

---

## 실전 예제

### 1. Node.js CI (테스트 자동화)

```yaml
name: Node.js CI

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest

    strategy:
      matrix:
        node-version: [18, 20, 22]

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Setup Node.js ${{ matrix.node-version }}
        uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node-version }}
          cache: 'npm'

      - name: Install dependencies
        run: npm ci

      - name: Run linter
        run: npm run lint

      - name: Run tests
        run: npm test

      - name: Build
        run: npm run build
```

### 2. Python CI/CD

```yaml
name: Python CI/CD

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest

    strategy:
      matrix:
        python-version: ['3.9', '3.10', '3.11']

    steps:
      - uses: actions/checkout@v4

      - name: Set up Python ${{ matrix.python-version }}
        uses: actions/setup-python@v5
        with:
          python-version: ${{ matrix.python-version }}

      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install -r requirements.txt
          pip install pytest flake8

      - name: Lint with flake8
        run: flake8 . --count --select=E9,F63,F7,F82 --show-source --statistics

      - name: Test with pytest
        run: pytest

  deploy:
    needs: test
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'

    steps:
      - uses: actions/checkout@v4

      - name: Deploy to production
        env:
          DEPLOY_KEY: ${{ secrets.DEPLOY_KEY }}
        run: |
          echo "Deploying to production..."
          # 배포 스크립트 실행
```

### 3. Docker 빌드 및 푸시

```yaml
name: Docker Build and Push

on:
  push:
    branches: [main]
    tags:
      - 'v*'

jobs:
  docker:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3

      - name: Login to Docker Hub
        uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}

      - name: Extract metadata
        id: meta
        uses: docker/metadata-action@v5
        with:
          images: username/app-name

      - name: Build and push
        uses: docker/build-push-action@v5
        with:
          context: .
          push: true
          tags: ${{ steps.meta.outputs.tags }}
          labels: ${{ steps.meta.outputs.labels }}
```

### 4. 자동 배포 (Vercel/Netlify)

**Vercel 배포**
```yaml
name: Deploy to Vercel

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Install Vercel CLI
        run: npm install --global vercel@latest

      - name: Pull Vercel Environment
        run: vercel pull --yes --environment=production --token=${{ secrets.VERCEL_TOKEN }}

      - name: Build Project
        run: vercel build --prod --token=${{ secrets.VERCEL_TOKEN }}

      - name: Deploy to Vercel
        run: vercel deploy --prebuilt --prod --token=${{ secrets.VERCEL_TOKEN }}
```

**Netlify 배포**
```yaml
name: Deploy to Netlify

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'

      - name: Install dependencies
        run: npm ci

      - name: Build
        run: npm run build

      - name: Deploy to Netlify
        uses: netlify/actions/cli@master
        env:
          NETLIFY_AUTH_TOKEN: ${{ secrets.NETLIFY_AUTH_TOKEN }}
          NETLIFY_SITE_ID: ${{ secrets.NETLIFY_SITE_ID }}
        with:
          args: deploy --prod --dir=build
```

### 5. 코드 품질 검사 (Linting & Formatting)

```yaml
name: Code Quality

on:
  pull_request:
    branches: [main]

jobs:
  lint:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'

      - name: Install dependencies
        run: npm ci

      - name: Run ESLint
        run: npm run lint

      - name: Run Prettier
        run: npm run format:check

      - name: Type check
        run: npm run type-check
```

### 6. 자동 릴리즈 노트 생성

```yaml
name: Release

on:
  push:
    tags:
      - 'v*'

jobs:
  release:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: Generate changelog
        id: changelog
        uses: metcalfc/changelog-generator@v4.1.0
        with:
          myToken: ${{ secrets.GITHUB_TOKEN }}

      - name: Create Release
        uses: actions/create-release@v1
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          tag_name: ${{ github.ref }}
          release_name: Release ${{ github.ref }}
          body: ${{ steps.changelog.outputs.changelog }}
          draft: false
          prerelease: false
```

### 7. 스케줄 작업 (크론잡)

```yaml
name: Scheduled Tasks

on:
  schedule:
    # 매일 오전 9시 (UTC 0시)
    - cron: '0 0 * * *'
  workflow_dispatch:  # 수동 실행도 가능

jobs:
  cleanup:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Run cleanup script
        run: |
          echo "Running daily cleanup..."
          npm run cleanup

      - name: Generate report
        run: npm run generate-report

      - name: Send notification
        env:
          SLACK_WEBHOOK: ${{ secrets.SLACK_WEBHOOK }}
        run: |
          curl -X POST -H 'Content-type: application/json' \
          --data '{"text":"Daily cleanup completed"}' \
          $SLACK_WEBHOOK
```

---

## 자주 사용하는 Actions

### 공식 Actions

| Action | 용도 | 사용 예시 |
|--------|------|-----------|
| `actions/checkout@v4` | 코드 체크아웃 | `uses: actions/checkout@v4` |
| `actions/setup-node@v4` | Node.js 설정 | `uses: actions/setup-node@v4` |
| `actions/setup-python@v5` | Python 설정 | `uses: actions/setup-python@v5` |
| `actions/cache@v4` | 의존성 캐싱 | `uses: actions/cache@v4` |
| `actions/upload-artifact@v4` | 빌드 결과 업로드 | `uses: actions/upload-artifact@v4` |
| `actions/download-artifact@v4` | 빌드 결과 다운로드 | `uses: actions/download-artifact@v4` |

### 커뮤니티 Actions

**Docker**
```yaml
- uses: docker/setup-buildx-action@v3
- uses: docker/login-action@v3
- uses: docker/build-push-action@v5
```

**코드 커버리지**
```yaml
- uses: codecov/codecov-action@v4
  with:
    token: ${{ secrets.CODECOV_TOKEN }}
```

**슬랙 알림**
```yaml
- uses: 8398a7/action-slack@v3
  with:
    status: ${{ job.status }}
    webhook_url: ${{ secrets.SLACK_WEBHOOK }}
```

---

## 유용한 팁

### 1. Matrix 전략 활용

여러 환경에서 동시 테스트:

```yaml
jobs:
  test:
    runs-on: ${{ matrix.os }}
    strategy:
      matrix:
        os: [ubuntu-latest, windows-latest, macos-latest]
        node-version: [18, 20, 22]
        include:
          - os: ubuntu-latest
            node-version: 20
            experimental: true

    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node-version }}
      - run: npm test
```

### 2. 캐싱으로 속도 향상

```yaml
steps:
  - uses: actions/checkout@v4

  - name: Cache node modules
    uses: actions/cache@v4
    with:
      path: ~/.npm
      key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
      restore-keys: |
        ${{ runner.os }}-node-

  - run: npm ci
```

### 3. Artifact 저장 및 다운로드

**업로드**
```yaml
steps:
  - run: npm run build

  - name: Upload build artifacts
    uses: actions/upload-artifact@v4
    with:
      name: build-files
      path: dist/
```

**다운로드**
```yaml
steps:
  - name: Download artifacts
    uses: actions/download-artifact@v4
    with:
      name: build-files
      path: dist/
```

### 4. 환경별 워크플로우 분리

```yaml
on:
  push:
    branches:
      - main
      - develop

jobs:
  deploy-prod:
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    environment: production
    steps:
      - run: echo "Deploying to production"

  deploy-staging:
    if: github.ref == 'refs/heads/develop'
    runs-on: ubuntu-latest
    environment: staging
    steps:
      - run: echo "Deploying to staging"
```

### 5. 재사용 가능한 Workflow

**재사용할 Workflow (.github/workflows/reusable.yml)**
```yaml
name: Reusable Workflow

on:
  workflow_call:
    inputs:
      environment:
        required: true
        type: string

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Deploying to ${{ inputs.environment }}"
```

**호출하는 Workflow**
```yaml
jobs:
  call-workflow:
    uses: ./.github/workflows/reusable.yml
    with:
      environment: production
```

### 6. 실패 시 슬랙 알림

```yaml
jobs:
  notify:
    runs-on: ubuntu-latest
    if: failure()
    steps:
      - name: Notify failure
        env:
          SLACK_WEBHOOK: ${{ secrets.SLACK_WEBHOOK }}
        run: |
          curl -X POST -H 'Content-type: application/json' \
          --data '{"text":"Build failed! Check GitHub Actions"}' \
          $SLACK_WEBHOOK
```

### 7. PR에 코멘트 추가

```yaml
jobs:
  comment:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Add comment to PR
        uses: actions/github-script@v7
        with:
          script: |
            github.rest.issues.createComment({
              issue_number: context.issue.number,
              owner: context.repo.owner,
              repo: context.repo.repo,
              body: '✅ Build successful!'
            })
```

### 8. 보안 스캔

```yaml
jobs:
  security:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Run security scan
        uses: aquasecurity/trivy-action@master
        with:
          scan-type: 'fs'
          scan-ref: '.'
          format: 'sarif'
          output: 'trivy-results.sarif'

      - name: Upload to GitHub Security
        uses: github/codeql-action/upload-sarif@v3
        with:
          sarif_file: 'trivy-results.sarif'
```

---

## 문제 해결

### 자주 발생하는 문제

**1. Permission denied 에러**
```yaml
# Solution: permissions 추가
permissions:
  contents: write
  pull-requests: write
```

**2. Secrets 접근 불가**
- Repository Settings → Secrets and variables → Actions에서 등록 확인
- Organization secrets는 repository 접근 권한 확인

**3. 캐시 무효화**
```yaml
# 캐시 키에 날짜 추가
key: ${{ runner.os }}-${{ hashFiles('**/package-lock.json') }}-${{ github.run_id }}
```

**4. Timeout 에러**
```yaml
# 타임아웃 시간 증가 (기본 360분)
jobs:
  build:
    timeout-minutes: 60
```

---

## 유용한 링크

- [GitHub Actions 공식 문서](https://docs.github.com/en/actions)
- [GitHub Marketplace](https://github.com/marketplace?type=actions)
- [Workflow 문법 참조](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions)
- [Actions 예제 모음](https://github.com/sdras/awesome-actions)

---

더 자세한 정보는 [GitHub Actions 공식 문서](https://docs.github.com/en/actions)를 참고하세요.
