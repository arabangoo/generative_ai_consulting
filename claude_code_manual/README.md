## [클로드코드 활용]

클로드코드는 리눅스나 macOS에서 사용 가능하지만 몇 가지 설정을 통해 윈도우에서도 사용 가능하다.
클로드코드 사용을 위해서는 클로드 계정이 Pro 플랜 이상이어야 한다.

---

## 목차

1. [Claude Code란?](#1-claude-code란)
2. [설치 방법](#2-설치-방법)
   - [WSL에서 클로드코드 실행](#wsl에서-클로드코드-실행)
   - [윈도우 CMD에서 클로드코드 실행](#윈도우-cmd에서-클로드코드-실행)
   - [VS Code에서 클로드코드 실행](#vs-code에서-클로드코드-실행)
   - [네이티브 설치 (최신 권장)](#네이티브-설치-최신-권장)
3. [핵심 개념 5가지](#3-핵심-개념-5가지)
4. [실행 모드](#4-실행-모드)
5. [주요 슬래시 명령어](#5-주요-슬래시-명령어)
6. [CLAUDE.md - 프로젝트 메모리](#6-claudemd---프로젝트-메모리)
7. [권한 관리](#7-권한-관리)
8. [MCP 서버 연결](#8-mcp-서버-연결)
9. [Hooks - 자동화 트리거](#9-hooks---자동화-트리거)
10. [Skills - 재사용 가능한 워크플로우](#10-skills---재사용-가능한-워크플로우)
11. [Sub-agents - 격리된 작업 처리](#11-sub-agents---격리된-작업-처리)
12. [Agent Teams - 멀티 에이전트 협업](#12-agent-teams---멀티-에이전트-협업)
13. [워크플로우 가이드 (EPCC 사이클)](#13-워크플로우-가이드-epcc-사이클)
14. [Git Worktree 병렬 개발](#14-git-worktree-병렬-개발)
15. [효과적인 프롬프트 작성법](#15-효과적인-프롬프트-작성법)
16. [컨텍스트 관리 전략](#16-컨텍스트-관리-전략)
17. [비용 절약 방법](#17-비용-절약-방법)
18. [자동화 및 CI/CD 통합](#18-자동화-및-cicd-통합)
19. [자주 발생하는 문제 및 해결법](#19-자주-발생하는-문제-및-해결법)
20. [프레임워크/언어별 활용 전략](#20-프레임워크언어별-활용-전략)

---

## 1. Claude Code란?

Claude Code는 Anthropic이 만든 **에이전트 코딩 도구**로, 터미널에서 자연어 명령으로 코드를 작성·수정·실행할 수 있다.

### 일반 챗봇과의 차이점

| 구분 | Claude.ai (웹) | Claude Code |
|------|--------------|-------------|
| 파일 접근 | 불가 | 로컬 파일 직접 읽기/수정 |
| 명령 실행 | 불가 | bash, git, npm 등 실행 |
| 작업 범위 | 단일 대화 | 수십 개 파일 동시 처리 |
| 자율성 | 낮음 | 높음 (에이전트 루프) |
| 사용 환경 | 브라우저 | 터미널, VS Code, JetBrains, Desktop App |

### Claude Code가 할 수 있는 것

- **기능 구축 및 버그 수정**: 원하는 것을 자연어로 설명하면 여러 파일에 걸쳐 코드를 작성하고 동작을 확인
- **지루한 작업 자동화**: 테스트 작성, lint 오류 수정, 병합 충돌 해결, 릴리스 노트 작성
- **커밋 및 PR 생성**: git과 직접 연동하여 변경 사항 스테이징, 커밋 메시지 작성, PR 열기
- **MCP로 외부 도구 연결**: GitHub, Slack, DB, Jira 등 외부 서비스와 연동
- **CI/CD 자동화**: GitHub Actions, GitLab CI/CD와 통합하여 코드 리뷰 자동화
- **멀티 에이전트 협업**: 여러 Claude 에이전트가 동시에 다른 작업을 처리

### 사용 가능한 환경

```
터미널(CLI) | VS Code | JetBrains IDE | Desktop App | 웹 브라우저 | iOS 앱
```

---

## 2. 설치 방법

### 요구 사항

- Claude 계정 (Pro $20/월 이상, 또는 Max $100/월)
- Node.js 18 이상 (npm 설치 방식의 경우)

---

### [WSL에서 클로드코드 실행]
(1) 윈도우 cmd에서 wsl 설치 명령어를 통해 wsl을 설치하도록 하자.   
아래 명령어는 최신 버전의 우분투 리눅스 환경을 설치하는 명령어다.   
명령어 (1) : wsl --install   
<img width="800" height="200" alt="image_1" src="https://github.com/user-attachments/assets/d275dd32-d7b0-40c4-b3c8-609c7037a100" />
<br/>

(2) wsl 버전이 낮다면 우선 아래 링크를 통해 wsl을 업데이트 하자.   
"wsl -l -v" 명령어를 실행했을 때 리눅스 버전이 2로 나오면 된다.   
https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi   
<img width="500" height="100" alt="image_2" src="https://github.com/user-attachments/assets/289e740c-17e8-4791-b292-ab15f0eaf506" />
<br/>

(3) wsl 명령어로 우분투에 진입하면 되지만 기본 배포판 충돌 문제가 나면 아래 순서로 해결한다.   
명령어 (1) : wsl --set-default Ubuntu   
명령어 (2) : wsl   
<img width="400" height="140" alt="image_3" src="https://github.com/user-attachments/assets/d1100879-e97a-4337-8c8f-71418772dbcf" />
<br/>

(4) 이제 wsl 리눅스 환경 내에서 Node.js를 설치하도록 하자.   
명령어 (1) : apt update   
명령어 (2) : curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -   
명령어 (3) : apt-get install -y nodejs   
명령어 (4) : node --version   
명령어 (5) : npm --version   
<img width="300" height="100" alt="image_4" src="https://github.com/user-attachments/assets/3919076c-9bd6-4f75-a934-821981b38adc" />
<br/>

(5) 다음은 wsl 리눅스 환경 내에서 클로드코드를 설치하도록 하자.   
클로드코드가 설치되면 claude 명령어로 실행하고 로그인과 환경설정을 하면 된다.    
명령어 (1) : npm install -g @anthropic-ai/claude-code   
명령어 (2) : claude   
<img width="1000" height="400" alt="image_5" src="https://github.com/user-attachments/assets/ba3f33e6-96fc-4b49-93eb-254d5681751c" />
<img width="1000" height="300" alt="image_6" src="https://github.com/user-attachments/assets/cc4c31f7-c65f-4f7f-90c1-0c66c6a39a62" />
<br/>

---

### [윈도우 CMD에서 클로드코드 실행]
(1) 윈도우 cmd에서 바로 클로드코드를 설치하고 실행할 수도 있다.   
다만 해당 구성은 개인이 만든 윈도우용 클로드코드 설치법이다.   
GitHub : https://github.com/somersby10ml/win-claude-code   
명령어 (1) : npm install -g @anthropic-ai/claude-code --ignore-scripts   
명령어 (2) : npx win-claude-code@latest   
<br/>
(2) 윈도우 cmd에서 바로 클로드코드를 실행할 때는 아래 명령어를 사용한다.   
명령어 (1) : npx win-claude-code   
<img width="600" height="360" alt="image_7" src="https://github.com/user-attachments/assets/9b7fa4e2-1b43-429c-8eb1-d22b63f02e58" />
<br/>

(3) 아래 명령어로 설치하면 더 간단히 윈도우 cmd에서 클로드코드를 실행할 수 있다.   
명령어 (1) : npm install -g win-claude-code   
명령어 (2) : win-claude-code   
<img width="1000" height="300" alt="image_8" src="https://github.com/user-attachments/assets/257203d0-3573-4dfb-8bea-31a56e352068" />
<br/>

---

### [VS Code에서 클로드코드 실행]
(1) VS Code에서는 Extension에서 Claude Code for VSCode를 검색해서 설치한다.   
<img width="300" height="360" alt="image_9" src="https://github.com/user-attachments/assets/7f243254-40db-4c2d-97f5-bf3e65553374" />
<br/>

(2) VS Code에서 터미널창을 열고 클로드코드를 실행한다.   
본인이 선호하는 환경에 접근해서 클로드코드를 사용할 수 있다.   
<img width="900" height="400" alt="image_10" src="https://github.com/user-attachments/assets/31fadc55-b35a-426c-b45a-feca6672a205" />
<img width="350" height="400" alt="image_11" src="https://github.com/user-attachments/assets/863b621b-d6b9-47c2-8dbb-88a867baeed5" />

---

### [네이티브 설치 (최신 권장)]

Anthropic이 공식적으로 권장하는 최신 설치 방식이다. 자동 업데이트를 지원한다.

**macOS / Linux / WSL:**
```bash
curl -fsSL https://claude.ai/install.sh | bash
```

**Windows PowerShell:**
```powershell
irm https://claude.ai/install.ps1 | iex
```

**Windows CMD:**
```batch
curl -fsSL https://claude.ai/install.cmd -o install.cmd && install.cmd && del install.cmd
```
> Windows는 [Git for Windows](https://git-scm.com/downloads/win)가 필요하다.

**Homebrew (macOS):**
```bash
brew install --cask claude-code
```

**WinGet (Windows):**
```powershell
winget install Anthropic.ClaudeCode
```

설치 후 프로젝트 폴더로 이동하여 실행:
```bash
cd your-project
claude
```

### JetBrains IDE 설치

JetBrains Marketplace에서 **Claude Code 플러그인**을 검색하여 설치한다.
IntelliJ IDEA, PyCharm, WebStorm 등 모든 JetBrains IDE에서 사용 가능하다.

### 웹에서 실행 (설치 불필요)

브라우저에서 바로 사용: [claude.ai/code](https://claude.ai/code)
로컬 설정 없이 오래 실행되는 작업을 시작하거나 여러 작업을 병렬로 실행할 수 있다.

---

## 3. 핵심 개념 5가지

Claude Code를 이해하기 위한 핵심 개념 5가지다.

| 개념 | 설명 |
|------|------|
| **Context (컨텍스트)** | Claude의 작업 기억. 모든 메시지, 읽은 파일, 명령 출력을 포함. 가득 차면 성능 저하 |
| **Tools (도구)** | 파일 읽기/쓰기, bash 실행, 웹 검색 등 Claude가 실제로 수행할 수 있는 능력 |
| **Permissions (권한)** | 각 작업 전 사용자 승인 여부. 허용 목록으로 자동화 가능 |
| **Memory (메모리)** | CLAUDE.md 파일로 세션 간 정보 유지. 프로젝트 규칙, 스타일 가이드 등 저장 |
| **Session (세션)** | 하나의 대화 흐름. `--continue`로 이전 세션을 이어갈 수 있음 |

### 에이전트 루프 동작 방식

```
사용자 입력
    ↓
Claude가 컨텍스트 분석
    ↓
필요한 도구 결정 (파일 읽기, bash 실행 등)
    ↓
권한 요청 (필요 시)
    ↓
도구 실행
    ↓
결과를 컨텍스트에 추가
    ↓
응답 생성 또는 다음 도구 실행
    ↓
작업 완료
```

---

## 4. 실행 모드

Claude Code는 3가지 모드로 작동한다. `Shift+Tab`으로 전환 가능하다.

| 모드 | 설명 | 사용 시기 |
|------|------|----------|
| **기본 모드 (Default)** | 요청 → 제안 → 승인 → 실행 | 중요한 변경사항이 있을 때 |
| **자동 승인 모드 (Auto)** | 승인 없이 모든 작업 자동 실행 | 반복 작업, 빠른 프로토타이핑 |
| **계획 모드 (Plan Mode)** | 파일을 읽되 변경하지 않음. 계획만 수립 | 복잡한 기능 설계 전, 코드 분석 |

### Plan Mode 진입 방법

```bash
# 터미널에서
/plan

# 또는 Shift+Tab으로 모드 전환
```

> **팁**: 복잡한 작업은 Plan Mode에서 먼저 탐색한 뒤 Normal Mode로 전환하여 구현하라.

---

## 5. 주요 슬래시 명령어

Claude Code 세션 내에서 사용하는 명령어들이다.

### 세션 관리

| 명령어 | 설명 |
|--------|------|
| `/clear` | 컨텍스트 완전 초기화 (새 작업 시작 시) |
| `/compact` | 컨텍스트를 요약하여 압축 (토큰 절약) |
| `/compact <지시사항>` | 특정 내용을 보존하며 압축 (예: `/compact Focus on API changes`) |
| `/rewind` | 이전 체크포인트로 되돌리기 |
| `/rename <이름>` | 현재 세션에 이름 붙이기 |
| `/cost` | 현재 세션 토큰/비용 확인 |

### 정보 조회

| 명령어 | 설명 |
|--------|------|
| `/memory` | 현재 로드된 CLAUDE.md 내용 확인 |
| `/mcp` | 연결된 MCP 서버 및 토큰 비용 확인 |
| `/permissions` | 현재 권한 설정 확인 및 변경 |
| `/hooks` | Hooks 대화형 설정 |
| `/status` | 현재 세션 상태 확인 |

### 설정 및 초기화

| 명령어 | 설명 |
|--------|------|
| `/init` | 현재 프로젝트 분석하여 CLAUDE.md 자동 생성 |
| `/plan` | Plan Mode 진입 |
| `/sandbox` | OS 수준 격리 활성화 |
| `/plugin` | 마켓플레이스에서 플러그인 탐색 및 설치 |

### 이전 세션 재개

```bash
# 터미널에서
claude --continue      # 가장 최근 세션 이어서 시작
claude --resume        # 최근 세션 목록에서 선택
```

---

## 6. CLAUDE.md - 프로젝트 메모리

CLAUDE.md는 Claude가 **매 세션 시작 시 자동으로 읽는 마크다운 파일**이다. 세션마다 초기화되는 컨텍스트를 지속적으로 유지하는 핵심 기능이다.

### 파일 위치 계층 구조

```
~/.claude/CLAUDE.md          ← 전역 설정 (모든 프로젝트에 적용)
./CLAUDE.md                  ← 프로젝트 루트 (팀과 git으로 공유)
./CLAUDE.local.md            ← 개인 설정 (.gitignore에 추가)
./.claude/CLAUDE.md          ← 프로젝트 전용 숨김 설정
./src/backend/CLAUDE.md      ← 하위 디렉토리별 설정 (자동 로드)
./src/frontend/CLAUDE.md     ← 프론트엔드 전용 규칙
```

모든 레벨의 CLAUDE.md가 **누적 적용**된다 (충돌 시 더 구체적인 설정이 우선).

### 초기 CLAUDE.md 자동 생성

```bash
/init
```

프로젝트를 분석하여 빌드 시스템, 테스트 프레임워크, 코드 패턴을 감지하고 초안을 만들어준다.

### CLAUDE.md 작성 예시

```markdown
# 프로젝트 개요
이 프로젝트는 React + TypeScript 기반의 전자상거래 플랫폼입니다.
참고: @README.md, @package.json

# 기술 스택
- Frontend: React 18, TypeScript, Tailwind CSS
- Backend: Node.js, Express, PostgreSQL
- 패키지 매니저: pnpm (npm 사용 금지)

# 코드 스타일
- ES 모듈(import/export) 사용, CommonJS(require) 금지
- 가능하면 구조 분해 import 사용
- 함수형 컴포넌트만 사용 (클래스 컴포넌트 금지)
- 타입 any 사용 금지

# 워크플로우
- 코드 변경 후 반드시 타입 체크 실행: `pnpm type-check`
- 커밋 전 lint 확인: `pnpm lint`
- 테스트는 단일 테스트 우선 실행 (전체 스위트 X)

# 중요 규칙
- IMPORTANT: .env 파일 절대 수정 금지
- IMPORTANT: migrations/ 폴더 직접 수정 금지
- 새 API 엔드포인트는 반드시 테스트 작성

# 일반적인 함정
- API 응답은 항상 camelCase 변환 필요
- 날짜 처리 시 항상 UTC 기준으로 저장
```

### CLAUDE.md 작성 원칙

**포함할 내용:**
- Claude가 코드만 보고는 알 수 없는 bash 명령
- 기본값과 다른 코드 스타일 규칙
- 테스트 지시사항 및 선호하는 테스트 러너
- 프로젝트 특정 아키텍처 결정
- 개발 환경 특이사항 (필수 환경변수 등)
- 명백하지 않은 동작이나 일반적인 함정

**제외할 내용:**
- Claude가 코드를 읽어서 파악할 수 있는 것
- Claude가 이미 아는 표준 언어 규칙
- 자주 변경되는 정보
- 상세한 API 문서 (문서 링크로 대체)
- 파일별 코드베이스 설명

> **경험 법칙**: CLAUDE.md를 **500줄 이하**로 유지하라. 증가하면 참조 콘텐츠를 Skills로 이동하라.

### 파일 가져오기 문법

```markdown
# CLAUDE.md 내에서 다른 파일 참조
프로젝트 개요는 @README.md를 참조하고
사용 가능한 명령은 @package.json을 참조하십시오.

# 추가 지시사항
- Git 워크플로우: @docs/git-workflow.md
- API 규칙: @docs/api-conventions.md
- 개인 설정: @~/.claude/my-preferences.md
```

---

## 7. 권한 관리

### 기본 권한 모드

기본적으로 Claude Code는 파일 쓰기, bash 실행, MCP 도구 사용 등 시스템을 수정하는 작업에 권한을 요청한다.

### Shift+Tab으로 모드 전환

| 모드 | 권한 동작 |
|------|----------|
| 기본 모드 | 모든 변경 작업마다 승인 요청 |
| 자동 승인 모드 | 모든 작업 자동 실행 |
| 계획 모드 | 읽기만 허용, 변경 불가 |

### 특정 도구 허용 목록 추가

```bash
/permissions
```

자주 사용하는 안전한 명령들을 허용 목록에 추가하면 반복 승인을 줄일 수 있다.

### 설정 파일로 권한 관리

`.claude/settings.json` 또는 `.claude/settings.local.json`:

```json
{
  "permissions": {
    "allow": [
      "Bash(npm run lint)",
      "Bash(npm run test:*)",
      "Bash(git add *)",
      "Bash(git commit *)",
      "Bash(git status)"
    ],
    "deny": [
      "Bash(rm -rf *)",
      "Bash(curl *)"
    ]
  }
}
```

### 위험한 자율 모드

```bash
claude --dangerously-skip-permissions
```

> **주의**: 모든 권한 확인을 무시한다. 인터넷 액세스가 없는 샌드박스 환경에서만 사용하라.

### 샌드박스 모드

```bash
/sandbox
```

OS 수준 격리를 활성화하여 파일 시스템 및 네트워크 액세스를 제한하면서도 미리 정의된 경계 내에서 자율적으로 작동하게 한다.

---

## 8. MCP 서버 연결

MCP(Model Context Protocol)는 Claude를 외부 서비스에 연결하는 개방형 표준이다.

### 주요 MCP 서버 목록

| 서버 | 기능 | 설치 명령 |
|------|------|----------|
| GitHub | 저장소, PR, 이슈 관리 | `claude mcp add github npx @github/mcp-server` |
| PostgreSQL | 데이터베이스 쿼리 | `claude mcp add postgres npx @modelcontextprotocol/server-postgres postgresql://...` |
| Playwright | 브라우저 자동화/테스트 | `claude mcp add playwright npx @playwright/mcp@latest` |
| Slack | 메시지 전송 및 읽기 | `claude mcp add slack npx @slack/mcp-server` |
| Notion | 문서 읽기/쓰기 | `claude mcp add notion npx @notion/mcp-server` |
| Figma | 디자인 파일 읽기 | `claude mcp add figma npx @figma/mcp-server` |

### MCP 서버 추가

```bash
# 기본 추가 방식
claude mcp add <서버명> <실행명령>

# 예시: Playwright 추가
claude mcp add playwright npx @playwright/mcp@latest

# 예시: PostgreSQL 추가
claude mcp add postgres npx @modelcontextprotocol/server-postgres postgresql://localhost/mydb
```

### MCP 서버 범위 설정

```bash
# 현재 프로젝트에만 적용 (기본값)
claude mcp add --scope project <서버명> <명령>

# 모든 프로젝트에 적용 (사용자 전역)
claude mcp add --scope user <서버명> <명령>
```

### MCP 상태 확인

```bash
/mcp
```

연결된 서버 목록과 각 서버의 토큰 비용을 확인할 수 있다. 사용하지 않는 서버는 연결 해제하여 토큰을 절약하라.

### MCP + Skill 조합 예시

```markdown
# .claude/skills/db-query/SKILL.md
---
name: db-query
description: 데이터베이스 쿼리 작업 수행
---
# 데이터베이스 쿼리 가이드

MCP postgres 서버가 연결되어 있습니다.

주요 테이블:
- users: 사용자 정보 (id, email, created_at)
- orders: 주문 정보 (id, user_id, total, status)
- products: 상품 정보 (id, name, price, stock)

쿼리 패턴:
- 항상 LIMIT 사용하여 대용량 조회 방지
- N+1 쿼리 방지를 위해 JOIN 활용
- 트랜잭션이 필요한 작업은 BEGIN/COMMIT 사용
```

---

## 9. Hooks - 자동화 트리거

Hooks는 특정 이벤트 발생 시 **자동으로 스크립트를 실행**하는 기능이다. CLAUDE.md 지시사항과 달리 Hook은 결정론적이며 항상 실행이 보장된다.

### Hook 이벤트 종류

| 이벤트 | 발생 시점 |
|--------|----------|
| `PreToolUse` | 도구 실행 전 |
| `PostToolUse` | 도구 실행 후 |
| `SessionStart` | 세션 시작 시 |
| `SessionEnd` | 세션 종료 시 |
| `UserPromptSubmit` | 사용자 입력 제출 시 |

### Hook 설정 방법

`.claude/settings.json` 또는 `.claude/settings.local.json`:

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit",
        "hooks": [
          {
            "type": "command",
            "command": "npx prettier --write $CLAUDE_TOOL_FILE"
          }
        ]
      }
    ],
    "PreToolUse": [
      {
        "matcher": "Write|Edit",
        "hooks": [
          {
            "type": "command",
            "command": "bash -c 'if [[ \"$CLAUDE_TOOL_FILE\" == *\".env\"* ]]; then echo \".env 파일은 수정할 수 없습니다\"; exit 2; fi'"
          }
        ]
      }
    ]
  }
}
```

### 대화형 Hook 설정

```bash
/hooks
```

### Hook 활용 사례

**1. 파일 저장 후 자동 포맷팅:**
```json
{
  "hooks": {
    "PostToolUse": [{
      "matcher": "Write|Edit",
      "hooks": [{"type": "command", "command": "npx eslint --fix $CLAUDE_TOOL_FILE"}]
    }]
  }
}
```

**2. 보호 파일 수정 차단:**
```json
{
  "hooks": {
    "PreToolUse": [{
      "matcher": "Write|Edit",
      "hooks": [{"type": "command", "command": "bash -c 'echo \"$CLAUDE_TOOL_FILE\" | grep -q \"migrations/\" && exit 2 || exit 0'"}]
    }]
  }
}
```

**3. 세션 시작 시 환경 체크:**
```json
{
  "hooks": {
    "SessionStart": [{
      "hooks": [{"type": "command", "command": "bash -c 'node --version && npm --version'"}]
    }]
  }
}
```

### Hook 종료 코드 의미

| 종료 코드 | 의미 |
|----------|------|
| `0` | 성공, 계속 진행 |
| `1` | 오류, 경고 메시지 표시 |
| `2` | 작업 차단, 실행 중단 |

> **팁**: Claude에게 Hook을 대신 만들어달라고 할 수 있다.
> 예: *"모든 파일 편집 후 eslint를 실행하는 hook을 작성해줘"*

---

## 10. Skills - 재사용 가능한 워크플로우

Skills는 Claude에게 **도메인 지식과 재사용 가능한 워크플로우**를 제공하는 마크다운 파일이다. `/명령어`로 직접 호출하거나 Claude가 자동으로 로드한다.

### Skill 위치

```
~/.claude/skills/         ← 사용자 전역 (모든 프로젝트)
./.claude/skills/         ← 현재 프로젝트 전용
```

### Skill 파일 구조

```
.claude/skills/
  ├── api-conventions/
  │   └── SKILL.md        ← API 규칙 참조 문서
  ├── deploy/
  │   └── SKILL.md        ← /deploy 명령으로 호출
  ├── review/
  │   └── SKILL.md        ← /review 명령으로 호출
  └── fix-issue/
      └── SKILL.md        ← /fix-issue 명령으로 호출
```

### Skill 작성 예시 - 참조 문서형

```markdown
# .claude/skills/api-conventions/SKILL.md
---
name: api-conventions
description: 우리 서비스의 REST API 설계 규칙 (Claude가 API 작업 시 자동 로드)
---
# REST API 규칙

## URL 규칙
- URL 경로에 kebab-case 사용: /users/user-profile
- JSON 속성에 camelCase 사용
- 목록 엔드포인트에 항상 페이지네이션 포함
- URL 경로에서 API 버전 지정: /v1/, /v2/

## 응답 형식
```json
{
  "data": { ... },
  "meta": { "total": 100, "page": 1 },
  "error": null
}
```

## 에러 처리
- 400: 잘못된 요청 (클라이언트 오류)
- 401: 인증 필요
- 403: 권한 없음
- 404: 리소스 없음
- 500: 서버 오류
```

### Skill 작성 예시 - 워크플로우형

```markdown
# .claude/skills/fix-issue/SKILL.md
---
name: fix-issue
description: GitHub 이슈 분석 및 수정 워크플로우
disable-model-invocation: true
---
GitHub 이슈를 분석하고 수정하십시오: $ARGUMENTS

1. `gh issue view $ARGUMENTS`로 이슈 세부사항 확인
2. 이슈에 설명된 문제 파악
3. 관련 파일 코드베이스 검색
4. 필요한 변경사항 구현
5. 수정을 확인하는 테스트 작성 및 실행
6. `pnpm lint && pnpm type-check` 확인
7. 설명적인 커밋 메시지로 커밋
8. PR 생성
```

호출 방법: `/fix-issue 1234`

### Skill Frontmatter 옵션

```yaml
---
name: skill-name          # 슬래시 명령어 이름
description: 설명         # Claude가 언제 이 Skill을 쓸지 판단하는 기준
disable-model-invocation: true  # true면 직접 호출할 때만 실행 (자동 로드 X)
context: fork             # 격리된 컨텍스트에서 실행 (Subagent처럼)
---
```

> **팁**: `disable-model-invocation: true`는 부작용이 있는 워크플로우에 필수.
> 컨텍스트 비용을 0으로 줄이고 실수로 실행되는 것을 방지한다.

### CLAUDE.md vs Skills 비교

| 기준 | CLAUDE.md | Skills |
|------|-----------|--------|
| 로드 시점 | 모든 세션, 자동 | 온디맨드 (요청 시) |
| 최적 용도 | "항상 X를 해라" 규칙 | 참조 자료, 호출 가능한 워크플로우 |
| 워크플로우 트리거 | 불가 | `/name`으로 가능 |
| 컨텍스트 비용 | 모든 요청마다 발생 | 사용할 때만 발생 |

---

## 11. Sub-agents - 격리된 작업 처리

Sub-agents는 **별도의 독립적인 컨텍스트 윈도우**에서 실행되어 결과만 메인 세션에 반환한다. 컨텍스트 오염 없이 대규모 탐색 작업을 처리할 수 있다.

### Sub-agent를 사용해야 할 때

- 수십~수백 개의 파일을 읽어야 하는 조사 작업
- 메인 대화를 복잡하게 만들지 않고 특화된 분석이 필요할 때
- 병렬로 여러 작업을 동시에 처리할 때
- 컨텍스트 윈도우가 거의 가득 찼을 때

### Sub-agent 사용법

```
"subagent를 사용하여 인증 시스템이 토큰 새로 고침을 어떻게 처리하는지 조사해줘"

"subagent를 사용하여 이 코드를 보안 취약점과 엣지 케이스에 대해 검토해줘"

"세 개의 subagent를 병렬로 사용해서 각각 보안, 성능, 코드 품질 측면에서 PR을 리뷰해줘"
```

### 커스텀 Sub-agent 정의

```markdown
# .claude/agents/security-reviewer.md
---
name: security-reviewer
description: 보안 취약점 전문 코드 리뷰어
tools: Read, Grep, Glob, Bash
model: claude-opus-4-6
---
당신은 선임 보안 엔지니어입니다. 다음 항목에 대해 코드를 검토하십시오:

- SQL/XSS/Command Injection 취약점
- 인증 및 권한 부여 결함
- 코드 내 하드코딩된 비밀값 또는 자격 증명
- 안전하지 않은 데이터 처리 패턴
- OWASP Top 10 취약점

각 발견 사항에 대해 다음을 제공하십시오:
1. 취약점 유형
2. 위험도 (High/Medium/Low)
3. 구체적인 코드 위치 (파일명:줄번호)
4. 권장 수정 방법
```

### Subagent 범위 우선순위

```
관리 정책 > CLI 플래그 > 프로젝트 > 사용자 > 플러그인
```

---

## 12. Agent Teams - 멀티 에이전트 협업

Agent Teams는 **여러 독립적인 Claude Code 세션**이 서로 통신하며 협업하는 기능이다. (현재 실험적 기능, 기본 비활성화)

### Subagents vs Agent Teams

| 구분 | Subagents | Agent Teams |
|------|-----------|-------------|
| 통신 방식 | 메인 에이전트에게만 결과 보고 | 팀원끼리 직접 메시지 교환 |
| 조정 | 메인 에이전트가 모든 작업 관리 | 공유 작업 목록으로 자체 조정 |
| 최적 용도 | 결과만 중요한 집중된 작업 | 토론과 협업이 필요한 복잡한 작업 |
| 토큰 비용 | 낮음 (요약만 메인으로 전달) | 높음 (각각 별도 Claude 인스턴스) |

### Writer/Reviewer 패턴 예시

```
[세션 A - 작성자]
"API 엔드포인트에 대한 속도 제한기를 구현해줘"

[세션 B - 검토자]
"@src/middleware/rateLimiter.ts를 검토해줘.
엣지 케이스, 경쟁 조건, 기존 미들웨어와의 일관성을 확인해줘"

[세션 A - 수정]
"검토 피드백: [세션 B 결과]. 이 문제들을 해결해줘"
```

### Agent Teams 활성화

```bash
claude config set experimental.agentTeams true
```

---

## 13. 워크플로우 가이드 (EPCC 사이클)

효과적인 Claude Code 사용을 위한 **EPCC 사이클**이다.

### EPCC 사이클

```
E → Explore (탐색)
P → Plan (계획)
C → Code (구현)
C → Commit (커밋)
```

### 1단계: 탐색 (Plan Mode에서)

```bash
# Plan Mode 진입
/plan

# 탐색 예시
"@src/auth 디렉토리를 읽고 세션과 로그인을 어떻게 처리하는지 이해해줘"
"환경변수는 어떻게 관리하는지 살펴봐줘"
```

### 2단계: 계획 수립

```
"Google OAuth를 추가하고 싶어. 어떤 파일을 변경해야 해?
세션 흐름은 어떻게 돼? 상세한 구현 계획을 작성해줘"
```

> **팁**: `Ctrl+G`로 계획을 텍스트 편집기에서 직접 수정할 수 있다.

### 3단계: 구현 (Normal Mode로 전환)

```
"계획에 따라 OAuth 흐름을 구현해줘.
콜백 핸들러 테스트를 작성하고, 테스트 실행 후 실패를 수정해줘"
```

### 4단계: 커밋

```
"설명적인 커밋 메시지로 커밋하고 PR을 열어줘"
```

### 언제 Plan Mode를 사용하는가

| 사용해야 할 때 | 건너뛰어도 될 때 |
|-------------|---------------|
| 여러 파일을 수정할 때 | 오타 수정 |
| 접근 방식이 불확실할 때 | 로그 줄 추가 |
| 수정할 코드가 낯설 때 | 변수 이름 변경 |
| diff를 한 문장으로 설명 불가 | 간단한 버그 수정 |

---

## 14. Git Worktree 병렬 개발

Git Worktrees를 사용하면 **브랜치 전환 없이 여러 기능을 동시에 개발**할 수 있다. 각 Claude 세션이 독립된 작업 디렉토리를 갖는다.

### Worktree 생성 및 사용

```bash
# 새 worktree 생성
git worktree add ../feature-auth -b feature/auth
git worktree add ../feature-payment -b feature/payment

# 각각 다른 터미널에서 Claude 실행
cd ../feature-auth && claude
cd ../feature-payment && claude

# 완료 후 worktree 제거
git worktree remove ../feature-auth
```

### Worktree 장점

- 브랜치 전환 없이 여러 기능 동시 개발
- 각 Claude 세션이 완전히 독립된 환경
- 코드 충돌 없이 병렬 작업 가능

---

## 15. 효과적인 프롬프트 작성법

### 핵심 원칙: 구체적으로 작성하라

| 나쁜 예 | 좋은 예 |
|---------|---------|
| "버그를 수정해줘" | "로그인 후 세션 시간 초과 시 재로그인이 실패해. `src/auth/`의 인증 흐름, 특히 토큰 새로 고침을 확인해줘. 문제를 재현하는 실패 테스트를 먼저 작성한 뒤 수정해줘" |
| "테스트를 추가해줘" | "로그아웃 상태에서의 엣지 케이스를 다루는 `foo.py` 테스트를 작성해줘. mock 객체는 피해줘" |
| "더 좋게 만들어줘" | "현재 대시보드 스크린샷이야. [이미지 붙여넣기] 이 디자인을 구현하고, 결과물 스크린샷을 찍어서 원본과 비교해줘" |

### 검증 기준을 함께 제시하라

```
"validateEmail 함수를 작성해줘.
테스트 케이스:
- user@example.com → true
- invalid → false
- user@.com → false
구현 후 테스트를 실행해줘"
```

### @ 기호로 파일 직접 참조

```
"@src/components/Button.tsx를 보고 동일한 패턴으로 Input 컴포넌트를 만들어줘"

"@src/api/users.ts의 패턴을 참고해서 새 products API를 만들어줘"
```

### 데이터 파이프로 전달

```bash
# 로그 파일 직접 분석
cat error.log | claude -p "이 로그에서 이상한 점을 찾아줘"

# git diff를 Claude에게 전달
git diff main --name-only | claude -p "변경된 파일들에서 보안 이슈를 검토해줘"
```

### Claude가 인터뷰하게 하기

큰 기능을 만들기 전, Claude에게 먼저 인터뷰를 요청하자:

```
"장바구니 기능을 만들고 싶어.
기술 구현, UI/UX, 엣지 케이스, 트레이드오프에 대해 나를 인터뷰해줘.
당연한 질문은 하지 말고, 내가 미처 생각하지 못한 어려운 부분을 파고들어줘.
다 다루면 완전한 사양을 SPEC.md에 작성해줘"
```

---

## 16. 컨텍스트 관리 전략

컨텍스트 윈도우 관리는 **Claude Code 성능을 좌우하는 가장 중요한 요소**다.

### 컨텍스트가 가득 찰수록 성능이 저하된다

- Claude가 이전 지시사항을 "잊기" 시작함
- 더 많은 실수를 할 수 있음
- 응답 속도가 느려질 수 있음

### 컨텍스트 관리 방법

**1. 작업 간 주기적 초기화:**
```bash
/clear    # 컨텍스트 완전 초기화
```

**2. 압축으로 중요 내용 유지:**
```bash
/compact                              # 자동 압축
/compact "API 변경사항에 집중해줘"    # 특정 내용 보존하며 압축
```

**3. 특정 지점부터 요약:**
```
Esc + Esc 또는 /rewind → 메시지 체크포인트 선택 → "Summarize from here"
```

**4. CLAUDE.md에 압축 지시사항 추가:**
```markdown
# 압축 지시사항
압축 시 항상 보존할 내용:
- 수정된 파일 전체 목록
- 실패한 테스트 명령어
- 현재 진행 중인 작업
```

### 언제 /clear를 사용해야 하나

- 관련 없는 새 작업을 시작할 때
- 같은 문제에 대해 Claude를 2번 이상 수정했을 때
- 디버깅 세션 후 새로운 기능 개발 시작 시
- 컨텍스트 사용량이 70% 이상일 때

### Subagent로 조사 격리

```
"subagent를 사용하여 결제 모듈의 의존성을 분석해줘.
주요 발견사항만 요약해서 알려줘"
```

---

## 17. 비용 절약 방법

### 비용 확인

```bash
/cost    # 현재 세션의 토큰/비용 확인
```

### 효과적인 비용 절약 전략

**1. 모델 선택 최적화:**
```
- 단순 작업: claude-haiku-4-5 (가장 저렴)
- 일반 개발: claude-sonnet-4-6 (균형)
- 복잡한 설계: claude-opus-4-6 (최고 품질)
```

**2. /compact 적극 활용:**
```bash
/compact    # 컨텍스트 압축으로 토큰 절약
```

**3. CLAUDE.md 다이어트:**
- 불필요한 내용 제거 (500줄 이하 유지)
- 세밀한 규칙은 Skills로 분리

**4. .claudeignore 파일 설정:**
```
# .claudeignore
node_modules/
dist/
.git/
*.log
*.lock
coverage/
.next/
.cache/
```

**5. MCP 서버 최소화:**
- 사용하지 않는 MCP 서버 연결 해제
- `/mcp`로 각 서버의 토큰 비용 확인 후 정리

**6. Skills로 참조 문서 분리:**
- CLAUDE.md에 상세 문서 직접 작성 대신
- Skills 파일로 분리하여 필요할 때만 로드

**7. 작업 단위를 작게 유지:**
- 한 세션에 너무 많은 작업 집중 금지
- 완료된 작업은 `/clear`로 초기화

---

## 18. 자동화 및 CI/CD 통합

### 비대화형 모드 실행

```bash
# 단일 명령 실행
claude -p "이 프로젝트가 무엇을 하는지 설명해줘"

# JSON 출력 (스크립트용)
claude -p "모든 API 엔드포인트를 나열해줘" --output-format json

# 스트리밍 JSON (실시간 처리)
claude -p "이 로그 파일을 분석해줘" --output-format stream-json

# 허용 도구 제한 (자동화 시 안전)
claude -p "파일을 마이그레이션해줘" --allowedTools "Read,Edit,Bash(git commit *)"
```

### GitHub Actions 통합

```yaml
# .github/workflows/claude-review.yml
name: Claude Code Review
on:
  pull_request:
    types: [opened, synchronize]

jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Claude Code Review
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
        run: |
          npm install -g @anthropic-ai/claude-code
          git diff ${{ github.base_ref }}..HEAD --name-only | \
            claude -p "변경된 파일들을 검토하고 보안 이슈와 버그를 찾아줘" \
            --output-format json
```

### 대량 파일 처리 (Fan-out 패턴)

```bash
# 1. 작업 대상 파일 목록 생성
claude -p "ES5에서 ES6으로 마이그레이션이 필요한 파일 목록을 만들어줘" > files.txt

# 2. 파일별 병렬 처리
while IFS= read -r file; do
  claude -p "ES5에서 ES6으로 $file을 마이그레이션해줘. 성공하면 OK, 실패하면 FAIL을 반환해줘" \
    --allowedTools "Read,Edit,Bash(git commit *)" &
done < files.txt
wait

# 3. 결과 확인
claude -p "마이그레이션 결과를 검토하고 실패한 파일이 있으면 원인을 분석해줘"
```

### 로그 모니터링 자동화

```bash
# 이상 로그 감지 및 Slack 알림
tail -f app.log | claude -p "이상한 패턴이 보이면 Slack으로 알려줘"
```

### 번역 자동화

```bash
# CI에서 번역 자동화
claude -p "새로 추가된 문자열을 프랑스어로 번역하고 PR을 만들어줘"
```

---

## 19. 자주 발생하는 문제 및 해결법

### 설치/실행 문제

| 문제 | 해결법 |
|------|--------|
| `claude` 명령어를 찾을 수 없음 | `npm install -g @anthropic-ai/claude-code` 재실행 또는 PATH 확인 |
| 로그인 오류 | `claude auth logout` 후 재로그인 |
| 느린 속도 | `/compact`으로 컨텍스트 압축, 불필요한 MCP 서버 제거 |
| 권한 요청이 너무 많음 | `/permissions`에서 안전한 명령 허용 목록 추가 |
| WSL에서 클립보드 안됨 | `clip.exe` 활용 또는 VS Code 터미널 사용 |

### 동작 문제

| 문제 | 해결법 |
|------|--------|
| Claude가 CLAUDE.md 규칙 무시 | 파일이 너무 길어 노이즈에 묻힘 → 불필요한 내용 제거 |
| 반복적인 실수 | `/clear`로 컨텍스트 초기화 후 더 명확한 프롬프트로 재시작 |
| 잘못된 방향으로 작업 | `Esc`로 즉시 중단, `Esc+Esc` 또는 `/rewind`로 되감기 |
| 파일 변경을 원하지 않는 실수 | `Esc+Esc` 또는 `/rewind`로 이전 상태 복구 |
| 컨텍스트 한도 초과 | `/clear` 또는 `/compact`, 작업 분할 |

### 보안 체크리스트

```
□ .env 파일은 .gitignore에 추가됨
□ API 키가 코드에 하드코딩되지 않음
□ .claudeignore에 민감한 파일 경로 추가
□ PreToolUse Hook으로 중요 파일 보호
□ 프로덕션 DB에는 읽기 전용 MCP 연결 사용
□ --dangerously-skip-permissions는 격리 환경에서만 사용
```

### 일반적인 실패 패턴과 해결책

| 패턴 | 증상 | 해결책 |
|------|------|--------|
| **주방 싱크 세션** | 관련 없는 작업들로 컨텍스트 오염 | 작업 간 `/clear` 사용 |
| **반복적 수정** | 같은 문제를 계속 수정 | 2번 실패 후 `/clear` + 더 나은 초기 프롬프트 |
| **과도한 CLAUDE.md** | Claude가 규칙 절반을 무시 | 무자비하게 정리, 중요한 것만 유지 |
| **무한 탐색** | 수백 개 파일을 읽어 컨텍스트 소모 | 범위를 좁게 지정하거나 subagent 활용 |
| **신뢰-검증 간격** | 그럴듯해 보이지만 동작 안 하는 코드 | 항상 테스트/검증 기준 제공 |

---

## 20. 프레임워크/언어별 활용 전략

### React / Next.js

```markdown
# CLAUDE.md 추가 내용
## React 규칙
- 함수형 컴포넌트만 사용 (클래스 컴포넌트 금지)
- 상태 관리: Zustand 사용 (Redux 금지)
- 스타일링: Tailwind CSS (CSS Modules 병행 가능)
- 데이터 페칭: React Query (SWR 금지)
- 컴포넌트 파일명: PascalCase.tsx
- 훅 파일명: use*.ts

## Next.js 규칙
- App Router 사용 (Pages Router 금지)
- 서버 컴포넌트 우선, 필요 시 'use client'
- API 라우트: /app/api/ 구조 준수
```

### Node.js / Express

```markdown
# CLAUDE.md 추가 내용
## 백엔드 규칙
- 비동기 처리: async/await (콜백 금지)
- 에러 처리: try-catch + 중앙 에러 미들웨어
- 유효성 검사: zod 사용
- ORM: Prisma 사용
- 환경변수: dotenv + .env.example 유지
```

### Python / Django / FastAPI

```markdown
# CLAUDE.md 추가 내용
## Python 규칙
- Python 버전: 3.11+
- 패키지 관리: poetry (pip 금지)
- 타입 힌팅 필수 사용
- 포맷터: black, linter: ruff
- 테스트: pytest (unittest 금지)

## Django 규칙
- CBV 보다 FBV 선호
- DRF 사용 시 ViewSet 패턴 준수
- 마이그레이션은 직접 수정 금지
```

### TypeScript

```markdown
# CLAUDE.md 추가 내용
## TypeScript 규칙
- any 타입 사용 금지 (unknown 사용)
- strict 모드 필수
- interface 보다 type 선호 (단, 클래스/extends는 interface)
- 타입 단언(as) 최소화
- null 체크: Optional Chaining(?.) 사용
```

### 바이브코딩 (비개발자 활용)

코딩 경험 없이도 Claude Code로 소프트웨어를 만들 수 있다:

**회의록 자동 정리:**
```
"meeting_notes/ 폴더의 오늘 회의 녹취록을 읽고
주요 결정사항과 액션 아이템을 정리해서 summary.md로 만들어줘"
```

**주간 보고서 자동화:**
```
"reports/ 폴더의 이번 주 일일 보고서를 읽고
팀 전체 주간 요약을 만들어줘"
```

**SNS 콘텐츠 생성:**
```
"이 블로그 글을 읽고
인스타그램, 트위터, LinkedIn용으로 각각 다른 스타일의 포스팅을 만들어줘"
```

---

## 참고 자료

- **공식 문서**: [code.claude.com](https://code.claude.com)
- **한국어 공식 문서**: [code.claude.com/docs/ko](https://code.claude.com/docs/ko/overview)



