# N8N 완벽 활용 가이드

## 📋 목차

1. [N8N 소개](#n8n-소개)
2. [설치 및 환경 구성](#설치-및-환경-구성)
3. [핵심 개념](#핵심-개념)
4. [크레덴셜 관리](#크레덴셜-관리)
5. [기본 워크플로우 작성](#기본-워크플로우-작성)
6. [주요 노드 활용법](#주요-노드-활용법)
7. [실전 활용 사례](#실전-활용-사례)
8. [한국 서비스 통합 사례](#한국-서비스-통합-사례)
9. [고급 기능](#고급-기능)
10. [보안 및 베스트 프랙티스](#보안-및-베스트-프랙티스)
11. [프로덕션 배포 체크리스트](#프로덕션-배포-체크리스트)
12. [트러블슈팅](#트러블슈팅)
13. [FAQ](#faq)
14. [부록](#부록)

---

## N8N 소개

### N8N이란?

N8N은 강력한 오픈소스 워크플로우 자동화 도구입니다. "Nodemation"의 약자로, 노드 기반의 자동화를 의미합니다.

**주요 특징:**
- 🔓 **오픈소스**: 완전한 소스 코드 접근 및 커스터마이징 가능
- 🏠 **셀프 호스팅**: 자체 서버에서 운영 가능 (데이터 주권 확보)
- 🔌 **400+ 통합**: 다양한 서비스와 API 연동
- 💻 **코드 확장성**: JavaScript/TypeScript로 커스텀 노드 작성
- 🎨 **직관적 UI**: 드래그 앤 드롭 인터페이스
- 🔄 **실시간 실행**: 즉시 결과 확인 가능

### MAKE.COM vs N8N 비교

| 기능 | N8N | MAKE.COM |
|-----|-----|----------|
| 라이선스 | 오픈소스 (Fair-code) | 클라우드 SaaS |
| 호스팅 | 셀프/클라우드 선택 가능 | 클라우드만 |
| 가격 | 무료 (셀프호스팅) | 월 구독 |
| 커스터마이징 | 코드 레벨 수정 가능 | 제한적 |
| 데이터 주권 | 완전한 제어 | 벤더 의존 |
| 학습 곡선 | 중간 | 쉬움 |
| 개발자 친화성 | 매우 높음 | 보통 |

### N8N이 적합한 경우

✅ 데이터 보안이 중요한 기업 환경  
✅ 복잡한 커스텀 로직이 필요한 경우  
✅ 기존 시스템과 깊은 통합이 필요한 경우  
✅ 비용 절감이 중요한 스타트업  
✅ 개발자 리소스가 있는 팀  

---

## 설치 및 환경 구성

### 설치 방법 선택

#### 1. Docker를 이용한 설치 (권장)

**사전 요구사항:**
- Docker Desktop (Windows/Mac) 또는 Docker Engine (Linux)
- 최소 2GB RAM, 10GB 디스크 공간

**기본 설치:**

```bash
# N8N 컨테이너 실행
docker run -it --rm \
  --name n8n \
  -p 5678:5678 \
  -v ~/.n8n:/home/node/.n8n \
  n8nio/n8n
```

**PostgreSQL과 함께 운영 환경 구성:**

```yaml
# docker-compose.yml
version: '3.8'

services:
  postgres:
    image: postgres:15
    restart: always
    environment:
      POSTGRES_DB: n8n
      POSTGRES_USER: n8n
      POSTGRES_PASSWORD: n8n_password
    volumes:
      - postgres_data:/var/lib/postgresql/data
    healthcheck:
      test: ['CMD-SHELL', 'pg_isready -U n8n']
      interval: 5s
      timeout: 5s
      retries: 10

  n8n:
    image: n8nio/n8n:latest
    restart: always
    ports:
      - "5678:5678"
    environment:
      - DB_TYPE=postgresdb
      - DB_POSTGRESDB_HOST=postgres
      - DB_POSTGRESDB_PORT=5432
      - DB_POSTGRESDB_DATABASE=n8n
      - DB_POSTGRESDB_USER=n8n
      - DB_POSTGRESDB_PASSWORD=n8n_password
      - N8N_BASIC_AUTH_ACTIVE=true
      - N8N_BASIC_AUTH_USER=admin
      - N8N_BASIC_AUTH_PASSWORD=change_this_password
      - N8N_HOST=localhost
      - N8N_PORT=5678
      - N8N_PROTOCOL=http
      - NODE_ENV=production
      - WEBHOOK_URL=http://localhost:5678/
      - GENERIC_TIMEZONE=Asia/Seoul
    volumes:
      - n8n_data:/home/node/.n8n
    depends_on:
      postgres:
        condition: service_healthy

volumes:
  postgres_data:
  n8n_data:
```

**실행:**

```bash
docker-compose up -d
```

#### 2. NPM을 이용한 설치

```bash
# Node.js 18+ 필요
npm install -g n8n

# 실행
n8n start
```

#### 3. Windows 환경 설치

**방법 1: Docker Desktop 사용 (권장)**

1. **Docker Desktop 설치**
   - [Docker Desktop for Windows](https://www.docker.com/products/docker-desktop) 다운로드
   - WSL 2 백엔드 활성화 필요

2. **N8N 실행**
   ```powershell
   # PowerShell에서 실행
   docker run -d --restart unless-stopped `
     --name n8n `
     -p 5678:5678 `
     -v ${HOME}\.n8n:/home/node/.n8n `
     n8nio/n8n
   ```

3. **접속**
   - 브라우저에서 `http://localhost:5678` 접속

**방법 2: NPM 사용**

1. **Node.js 설치**
   - [Node.js 공식 사이트](https://nodejs.org/)에서 LTS 버전 다운로드 (18.x 이상)
   - 설치 시 "Add to PATH" 옵션 체크

2. **N8N 설치**
   ```powershell
   # PowerShell 관리자 권한으로 실행
   npm install -g n8n

   # 실행
   n8n start
   ```

3. **서비스로 등록 (선택사항)**
   ```powershell
   # NSSM (Non-Sucking Service Manager) 설치
   choco install nssm

   # N8N 서비스 등록
   nssm install n8n "C:\Program Files\nodejs\n8n.cmd"
   nssm set n8n AppDirectory "C:\Users\YourUsername\.n8n"
   nssm start n8n
   ```

**방법 3: WSL2 + Docker 사용**

1. **WSL2 설치**
   ```powershell
   wsl --install
   wsl --set-default-version 2
   ```

2. **Ubuntu 설치**
   ```powershell
   wsl --install -d Ubuntu-22.04
   ```

3. **WSL 내에서 Docker 설정**
   ```bash
   # Ubuntu 터미널에서
   curl -fsSL https://get.docker.com -o get-docker.sh
   sudo sh get-docker.sh
   sudo usermod -aG docker $USER

   # Docker Compose 설치
   sudo apt install docker-compose

   # N8N 실행
   docker run -d --restart unless-stopped \
     --name n8n \
     -p 5678:5678 \
     -v ~/.n8n:/home/node/.n8n \
     n8nio/n8n
   ```

**Windows 방화벽 설정:**

```powershell
# PowerShell 관리자 권한으로 실행
New-NetFirewallRule -DisplayName "N8N" -Direction Inbound -LocalPort 5678 -Protocol TCP -Action Allow
```

#### 4. AWS에서 운영 환경 구축

**EC2 인스턴스 설정:**

```bash
# Ubuntu 22.04 LTS 기준
sudo apt update
sudo apt install docker.io docker-compose -y
sudo systemctl enable docker
sudo systemctl start docker

# N8N 설치
git clone https://github.com/n8n-io/n8n.git
cd n8n
docker-compose up -d
```

**보안 그룹 설정:**
- 인바운드 규칙: 5678 포트 (사용자 IP만 허용)
- HTTPS 설정을 위해 443 포트도 열기 (프로덕션 환경)

**도메인 및 SSL 설정 (프로덕션):**

```bash
# Let's Encrypt 인증서 설치
sudo apt install certbot python3-certbot-nginx
sudo certbot --nginx -d your-domain.com
```

### 초기 설정

1. **브라우저에서 접속**: `http://localhost:5678`
2. **계정 생성**: 관리자 계정 설정
3. **기본 설정**:
   - 타임존: Asia/Seoul
   - 언어: 한국어 (선택사항)
   - 이메일 알림 설정

### 환경 변수 설정

중요한 환경 변수들:

```bash
# 데이터베이스
DB_TYPE=postgresdb
DB_POSTGRESDB_HOST=localhost
DB_POSTGRESDB_PORT=5432
DB_POSTGRESDB_DATABASE=n8n
DB_POSTGRESDB_USER=n8n
DB_POSTGRESDB_PASSWORD=secure_password

# 인증
N8N_BASIC_AUTH_ACTIVE=true
N8N_BASIC_AUTH_USER=your_username
N8N_BASIC_AUTH_PASSWORD=your_secure_password

# 웹훅 URL
WEBHOOK_URL=https://your-domain.com/

# 실행 모드
EXECUTIONS_MODE=queue # 대규모 워크플로우의 경우
N8N_PAYLOAD_SIZE_MAX=16 # MB

# 로그 레벨
N8N_LOG_LEVEL=info # debug, info, warn, error

# 타임존
GENERIC_TIMEZONE=Asia/Seoul
```

---

## 핵심 개념

### 워크플로우 (Workflow)

워크플로우는 N8N에서 자동화 작업의 기본 단위입니다. 여러 노드들을 연결하여 데이터가 흐르는 경로를 정의합니다.

**구성 요소:**
1. **노드 (Nodes)**: 각 작업 단계
2. **연결 (Connections)**: 노드 간 데이터 흐름
3. **실행 (Executions)**: 워크플로우 실행 기록

### 노드 (Nodes)

노드는 워크플로우의 개별 작업 단위입니다.

#### 노드 유형:

**1. 트리거 노드 (Trigger Nodes)**
- 워크플로우 시작점
- 예: Webhook, Schedule, Manual Trigger

**2. 일반 노드 (Regular Nodes)**
- 데이터 처리 및 작업 수행
- 예: HTTP Request, Set, Function

**3. 출력 노드 (Output Nodes)**
- 최종 결과 저장 또는 전송
- 예: Email, Slack, Database

### 데이터 구조

N8N은 JSON 형식으로 데이터를 처리합니다.

**기본 구조:**

```json
[
  {
    "json": {
      "field1": "value1",
      "field2": "value2",
      "nested": {
        "subfield": "subvalue"
      }
    }
  }
]
```

### 표현식 (Expressions)

N8N의 표현식 언어를 사용하여 동적으로 데이터를 참조하고 변환할 수 있습니다.

**기본 문법:**

```javascript
// 이전 노드 데이터 참조
{{ $json.fieldName }}

// 특정 노드 데이터 참조
{{ $node["Node Name"].json.fieldName }}

// 현재 아이템 인덱스
{{ $itemIndex }}

// 워크플로우 메타데이터
{{ $workflow.name }}
{{ $workflow.id }}

// 날짜/시간
{{ $now }}
{{ $today }}

// JavaScript 함수 사용
{{ new Date().toISOString() }}
{{ "text".toUpperCase() }}
{{ Math.random() }}
```

**고급 표현식:**

```javascript
// 조건부 로직
{{ $json.amount > 1000 ? "high" : "low" }}

// 배열 필터링
{{ $json.items.filter(item => item.price > 100) }}

// 문자열 조작
{{ $json.email.split('@')[1] }}

// 객체 변환
{{ Object.keys($json).length }}
```

---

## 크레덴셜 관리

N8N에서 크레덴셜(Credentials)은 외부 서비스와의 인증 정보를 안전하게 저장하고 관리하는 기능입니다.

### 크레덴셜 타입

#### 1. API Key 인증

**사용 사례**: OpenAI, SendGrid, Mailgun 등

**설정 방법:**

1. 왼쪽 메뉴에서 "Credentials" 클릭
2. "Add Credential" 선택
3. "Header Auth" 또는 해당 서비스 선택
4. 정보 입력:
   ```
   Name: My API Key
   API Key: sk-xxxxxxxxxxxxxxxxxxxx
   ```

**노드에서 사용:**
```javascript
// HTTP Request 노드
Authentication: Generic Credential Type
Credential for Generic Credential Type: My API Key
```

#### 2. OAuth2 인증

**사용 사례**: Google, GitHub, Slack, Microsoft 등

**Google OAuth2 설정 예시:**

1. **Google Cloud Console 설정**
   - https://console.cloud.google.com 접속
   - 프로젝트 생성
   - "APIs & Services" → "Credentials"
   - "Create Credentials" → "OAuth 2.0 Client ID"
   - Application type: Web application
   - Authorized redirect URIs: `http://localhost:5678/rest/oauth2-credential/callback`

2. **N8N에서 설정**
   ```
   Name: Google OAuth
   Client ID: [Google Console에서 생성한 ID]
   Client Secret: [Google Console에서 생성한 Secret]
   ```

3. **권한 승인**
   - "Connect my account" 클릭
   - Google 로그인 및 권한 승인

#### 3. 베이직 인증 (Basic Auth)

**사용 사례**: 내부 API, 레거시 시스템

**설정:**
```
Credential Type: Basic Auth
User: admin
Password: your_password
```

#### 4. 토큰 기반 인증

**사용 사례**: GitHub, GitLab, JWT 기반 API

**GitHub 예시:**
```
Credential Type: GitHub
Access Token: ghp_xxxxxxxxxxxxxxxxxxxx
```

**토큰 생성 (GitHub):**
1. Settings → Developer settings → Personal access tokens
2. Generate new token
3. 필요한 권한(scope) 선택
4. 생성된 토큰 복사하여 N8N에 입력

#### 5. AWS 인증

**설정:**
```
Credential Type: AWS
Access Key ID: AKIAIOSFODNN7EXAMPLE
Secret Access Key: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
Region: ap-northeast-2
```

**IAM 사용자 생성 권장:**
- 최소 권한 원칙 적용
- 프로그래매틱 액세스만 활성화
- MFA 설정 권장

#### 6. 데이터베이스 연결

**PostgreSQL 예시:**
```
Host: localhost
Database: mydb
User: postgres
Password: secure_password
Port: 5432
SSL: Prefer
```

**연결 문자열 방식:**
```
Connection String: postgresql://user:password@localhost:5432/mydb?sslmode=require
```

### 크레덴셜 보안 베스트 프랙티스

#### 1. 환경별 크레덴셜 분리

```bash
# 개발 환경
DEV_API_KEY=dev-key-xxxx

# 프로덕션 환경
PROD_API_KEY=prod-key-xxxx
```

#### 2. 크레덴셜 암호화

N8N은 기본적으로 크레덴셜을 암호화하여 저장합니다.

**암호화 키 설정:**
```bash
# docker-compose.yml
environment:
  - N8N_ENCRYPTION_KEY=your-very-long-secure-random-key
```

**암호화 키 생성:**
```bash
openssl rand -base64 32
```

#### 3. 크레덴셜 공유 제한

- 팀 환경에서는 역할 기반 접근 제어 설정
- 필요한 사용자에게만 크레덴셜 공유
- 정기적으로 사용하지 않는 크레덴셜 삭제

#### 4. 크레덴셜 로테이션

정기적으로 크레덴셜을 갱신하는 것이 좋습니다:

```javascript
// 크레덴셜 만료 알림 워크플로우
[Schedule: Monthly]
  → [Function: Check Credential Age]
  → [IF: > 90 days]
  → [Email: Rotation Reminder]
```

#### 5. 감사 로그

크레덴셜 사용 추적:

```javascript
// Function 노드에서
console.log({
  action: 'credential_used',
  credentialName: 'API Key',
  workflow: $workflow.name,
  timestamp: new Date().toISOString()
});
```

### 크레덴셜 백업 및 복구

#### 백업

```bash
# 크레덴셜은 데이터베이스에 암호화되어 저장됨
# PostgreSQL 백업
docker exec postgres pg_dump -U n8n n8n > backup.sql

# 중요: 암호화 키도 함께 백업 필요
echo $N8N_ENCRYPTION_KEY > encryption_key.txt
```

#### 복구

```bash
# 데이터베이스 복원
docker exec -i postgres psql -U n8n n8n < backup.sql

# 동일한 암호화 키 사용 필수
export N8N_ENCRYPTION_KEY=$(cat encryption_key.txt)
```

### 크레덴셜 문제 해결

#### 연결 테스트

대부분의 크레덴셜은 "Test" 버튼을 제공합니다:

1. 크레덴셜 편집 화면에서 "Test" 클릭
2. 성공 시: "Connection successful" 메시지
3. 실패 시: 에러 메시지 확인

#### 일반적인 문제

**OAuth2 토큰 만료:**
```
Error: Token expired
해결: "Reconnect" 버튼 클릭하여 재인증
```

**IP 화이트리스트:**
```
Error: Access denied from IP
해결: N8N 서버 IP를 서비스 화이트리스트에 추가
```

**권한 부족:**
```
Error: Insufficient permissions
해결: API 키 또는 OAuth 스코프 확인 및 권한 추가
```

### 크레덴셜 템플릿

자주 사용하는 서비스별 크레덴셜 설정 예시:

#### Slack
```
Credential Type: Slack OAuth2
OAuth Scopes:
  - chat:write
  - channels:read
  - users:read
```

#### Gmail (SMTP)
```
Credential Type: SMTP
Host: smtp.gmail.com
Port: 587
User: your-email@gmail.com
Password: [앱 비밀번호]
Secure: true
```

#### Notion
```
Credential Type: Notion API
API Token: secret_xxxxxxxxxxxx
```

#### Stripe
```
Credential Type: Stripe API
Secret Key: sk_live_xxxxxxxxxxxx
```

---

## 기본 워크플로우 작성

### 첫 번째 워크플로우: 간단한 웹훅 수신

**목표**: 외부에서 데이터를 받아 이메일로 전송하는 워크플로우 생성

**단계:**

1. **Webhook 노드 추가**
   - 왼쪽 패널에서 "Webhook" 검색
   - 드래그하여 캔버스에 배치
   - HTTP Method: POST
   - Path: `test-webhook`
   - 웹훅 URL 복사: `http://localhost:5678/webhook/test-webhook`

2. **Set 노드로 데이터 변환**
   - "Set" 노드 추가
   - Webhook 노드와 연결
   - 필드 설정:
     - Name: `processed_data`
     - Value: `{{ $json.body }}`

3. **Send Email 노드 추가**
   - SMTP 설정 (Gmail 예시):
     - Host: `smtp.gmail.com`
     - Port: `587`
     - User: 이메일 주소
     - Password: 앱 비밀번호
   - To: 수신자 이메일
   - Subject: `New webhook data received`
   - Text: `{{ JSON.stringify($json, null, 2) }}`

4. **테스트**
   - "Execute Workflow" 클릭
   - 터미널에서 테스트:
   
   ```bash
   curl -X POST http://localhost:5678/webhook/test-webhook \
     -H "Content-Type: application/json" \
     -d '{"name": "test", "value": 123}'
   ```

5. **활성화**
   - 우측 상단 토글을 "Active"로 변경

### 두 번째 워크플로우: 스케줄 기반 작업

**목표**: 매일 아침 9시에 특정 API 데이터를 수집하여 Slack에 전송

**단계:**

1. **Schedule Trigger 노드**
   - Trigger Interval: Cron
   - Cron Expression: `0 9 * * *` (매일 오전 9시)

2. **HTTP Request 노드**
   - Method: GET
   - URL: `https://api.example.com/data`
   - Authentication: Bearer Token (필요시)

3. **Function 노드로 데이터 가공**
   ```javascript
   const items = $input.all();
   const processedData = items.map(item => ({
     json: {
       title: item.json.title,
       value: item.json.value,
       timestamp: new Date().toISOString()
     }
   }));
   return processedData;
   ```

4. **Slack 노드**
   - 크레덴셜 설정 (OAuth2)
   - Channel: `#general`
   - Message: 
   ```
   📊 Daily Report - {{ $now.format('YYYY-MM-DD') }}
   
   {{ $json.title }}: {{ $json.value }}
   ```

---

## 주요 노드 활용법

### 1. HTTP Request 노드

**용도**: 외부 API 호출

**설정 예시:**

```javascript
// GET 요청
Method: GET
URL: https://api.github.com/users/{{$json.username}}
Authentication: Bearer Token

// POST 요청
Method: POST
URL: https://api.example.com/create
Body Content Type: JSON
Body:
{
  "name": "{{ $json.name }}",
  "email": "{{ $json.email }}",
  "timestamp": "{{ $now }}"
}

// 헤더 추가
Headers:
  Content-Type: application/json
  X-API-Key: your_api_key
```

**고급 기능:**

- **Pagination**: 자동으로 다음 페이지 가져오기
- **Batching**: 여러 요청을 일괄 처리
- **Retry**: 실패 시 재시도 설정

### 2. Code 노드 (Function 노드)

**용도**: JavaScript/Python 코드로 복잡한 로직 구현

**JavaScript 예시:**

```javascript
// 모든 입력 아이템 가져오기
const items = $input.all();

// 데이터 변환
const transformed = items.map(item => {
  const data = item.json;
  
  // 복잡한 계산
  const total = data.items.reduce((sum, item) => sum + item.price, 0);
  
  // 조건부 로직
  const category = total > 1000 ? 'premium' : 'standard';
  
  return {
    json: {
      orderId: data.id,
      totalAmount: total,
      category: category,
      processedAt: new Date().toISOString()
    }
  };
});

return transformed;
```

**Python 예시:**

```python
import pandas as pd
from datetime import datetime

# 입력 데이터
items = _input.all()

# DataFrame 생성
df = pd.DataFrame([item['json'] for item in items])

# 데이터 분석
result = {
    'total_count': len(df),
    'average_value': df['value'].mean(),
    'max_value': df['value'].max(),
    'timestamp': datetime.now().isoformat()
}

return [{'json': result}]
```

### 3. IF 노드

**용도**: 조건부 분기 처리

**설정:**

```javascript
// 조건 1: 금액이 1000 이상
Condition 1:
  Value 1: {{ $json.amount }}
  Operation: Larger
  Value 2: 1000

// 조건 2: 상태가 'active'
Condition 2:
  Value 1: {{ $json.status }}
  Operation: Equal
  Value 2: active

// 조건 연산자: AND / OR
```

### 4. Switch 노드

**용도**: 여러 경로로 분기

**설정:**

```javascript
// Mode: Rules

Rule 1 - High Priority:
  Conditions: {{ $json.priority === 'high' }}
  Output: 0

Rule 2 - Medium Priority:
  Conditions: {{ $json.priority === 'medium' }}
  Output: 1

Rule 3 - Low Priority:
  Conditions: {{ $json.priority === 'low' }}
  Output: 2

Fallback Output: 3
```

### 5. Loop Over Items 노드

**용도**: 배열의 각 요소를 개별적으로 처리

**사용 시나리오**: 100개의 사용자에게 개별 이메일 발송

```
[데이터 소스] → [Loop Over Items] → [Send Email] → [Loop 종료]
```

### 6. Merge 노드

**용도**: 여러 데이터 스트림 병합

**모드:**

1. **Append**: 단순 결합
2. **Merge By Position**: 위치 기반 병합
3. **Merge By Key**: 키 기반 조인 (SQL JOIN과 유사)

**예시 - Merge By Key:**

```javascript
Mode: Merge By Key
Property Input 1: id
Property Input 2: user_id
Join: Inner Join
```

### 7. SplitInBatches 노드

**용도**: 대량 데이터를 작은 배치로 분할 처리

**설정:**

```javascript
Batch Size: 100
Options:
  - Reset: 각 실행마다 리셋
```

**사용 예시**: 10,000개 레코드를 100개씩 나누어 API 호출

---

## 실전 활용 사례

### 사례 1: GitHub 이슈 자동 트래킹

**목표**: 새로운 GitHub 이슈가 생성되면 Slack에 알림

**워크플로우:**

1. **GitHub Trigger**
   - Repository: `owner/repo`
   - Events: `issues`
   - Filter: `opened`

2. **Set 노드**
   ```javascript
   issueNumber: {{ $json.issue.number }}
   issueTitle: {{ $json.issue.title }}
   issueUrl: {{ $json.issue.html_url }}
   author: {{ $json.issue.user.login }}
   ```

3. **Slack 노드**
   ```
   🚨 New Issue #{{ $json.issueNumber }}
   
   Title: {{ $json.issueTitle }}
   Author: @{{ $json.author }}
   
   Link: {{ $json.issueUrl }}
   ```

### 사례 2: 고객 데이터 동기화

**목표**: CRM에서 새 고객이 추가되면 여러 시스템에 자동 동기화

**워크플로우:**

1. **Webhook Trigger** (CRM에서 호출)

2. **Function - 데이터 검증**
   ```javascript
   const customer = $json;
   
   // 필수 필드 검증
   const requiredFields = ['email', 'name', 'company'];
   const missingFields = requiredFields.filter(field => !customer[field]);
   
   if (missingFields.length > 0) {
     throw new Error(`Missing fields: ${missingFields.join(', ')}`);
   }
   
   return [{
     json: {
       ...customer,
       createdAt: new Date().toISOString()
     }
   }];
   ```

3. **분기 처리**
   - **경로 1**: Salesforce에 연락처 추가
   - **경로 2**: Mailchimp 메일링 리스트 추가
   - **경로 3**: 내부 데이터베이스 저장

4. **Merge** - 모든 결과 병합

5. **Slack 알림** - 동기화 완료 알림

### 사례 3: 웹 스크래핑 및 데이터 분석

**목표**: 경쟁사 웹사이트 가격 모니터링

**워크플로우:**

1. **Schedule Trigger** - 매일 오전 10시

2. **HTTP Request**
   ```javascript
   Method: GET
   URL: https://competitor.com/products
   Headers:
     User-Agent: Mozilla/5.0...
   ```

3. **HTML Extract**
   - CSS Selector: `.product-price`
   - Extract: Multiple

4. **Function - 데이터 파싱**
   ```javascript
   const items = $input.all();
   const prices = items.map(item => {
     const priceText = item.json.price;
     const price = parseFloat(priceText.replace(/[^0-9.]/g, ''));
     
     return {
       json: {
         product: item.json.name,
         price: price,
         currency: 'USD',
         scrapedAt: new Date().toISOString()
       }
     };
   });
   
   return prices;
   ```

5. **PostgreSQL** - 가격 이력 저장

6. **IF 노드** - 가격 변동 감지
   - 조건: 이전 가격 대비 5% 이상 변동

7. **Email 알림** - 가격 변동 알림

### 사례 4: 문서 자동 생성

**목표**: 템플릿 기반 계약서 자동 생성

**워크플로우:**

1. **Webhook** - 계약 정보 수신

2. **Google Docs** - 템플릿 복사

3. **Function - 플레이스홀더 치환**
   ```javascript
   const docContent = $json.content;
   const data = $node["Webhook"].json;
   
   let updatedContent = docContent;
   updatedContent = updatedContent.replace('{{CLIENT_NAME}}', data.clientName);
   updatedContent = updatedContent.replace('{{CONTRACT_DATE}}', data.date);
   updatedContent = updatedContent.replace('{{AMOUNT}}', data.amount);
   
   return [{ json: { content: updatedContent } }];
   ```

4. **Google Docs** - 문서 업데이트

5. **PDF 변환**

6. **Email** - 계약서 발송

---

## 한국 서비스 통합 사례

한국에서 자주 사용하는 서비스들과 N8N을 연동하는 실전 예제입니다.

### 사례 1: 카카오톡 알림 전송

**목표**: 중요 이벤트 발생 시 카카오톡 알림톡/친구톡 발송

**준비사항:**
- 카카오 비즈니스 계정
- 카카오톡 채널 개설
- 알림톡 템플릿 등록

**워크플로우:**

1. **Webhook Trigger** - 이벤트 수신

2. **Function - 데이터 준비**
   ```javascript
   const eventData = $json;

   return [{
     json: {
       phoneNumber: eventData.phone.replace(/-/g, ''),
       templateCode: 'ORDER_CONFIRM_001',
       variables: {
         customer_name: eventData.name,
         order_number: eventData.orderId,
         amount: eventData.amount.toLocaleString('ko-KR')
       }
     }
   }];
   ```

3. **HTTP Request - 카카오 API 호출**
   ```javascript
   Method: POST
   URL: https://kapi.kakao.com/v1/api/talk/send
   Authentication: Bearer Token
   Headers:
     Authorization: Bearer {{$credentials.kakaoAccessToken}}
     Content-Type: application/json
   Body:
   {
     "phone_number": "{{ $json.phoneNumber }}",
     "template_code": "{{ $json.templateCode }}",
     "template_args": {{ JSON.stringify($json.variables) }}
   }
   ```

4. **Function - 결과 로깅**
   ```javascript
   const response = $json;

   console.log({
     status: response.result_code === 0 ? 'success' : 'failed',
     phone: $node["Function"].json.phoneNumber,
     timestamp: new Date().toISOString()
   });

   return $input.all();
   ```

### 사례 2: 네이버 클라우드 SMS 발송

**목표**: 긴급 알림을 SMS로 전송

**준비사항:**
- 네이버 클라우드 플랫폼 계정
- Simple & Easy Notification Service (SENS) 활성화
- 발신번호 등록

**워크플로우:**

1. **Schedule/Webhook Trigger**

2. **Function - SMS 메시지 구성**
   ```javascript
   const recipients = [
     { to: '01012345678', name: '홍길동' },
     { to: '01087654321', name: '김철수' }
   ];

   const messages = recipients.map(recipient => ({
     to: recipient.to,
     content: `[긴급] ${recipient.name}님, 시스템 점검이 ${$now.plus({hours: 1}).toFormat('HH:mm')}에 시작됩니다.`
   }));

   return [{ json: { messages } }];
   ```

3. **HTTP Request - 네이버 SENS API**
   ```javascript
   Method: POST
   URL: https://sens.apigw.ntruss.com/sms/v2/services/{{$credentials.naverServiceId}}/messages
   Headers:
     Content-Type: application/json
     x-ncp-apigw-timestamp: {{ Date.now() }}
     x-ncp-iam-access-key: {{ $credentials.naverAccessKey }}
     x-ncp-apigw-signature-v2: {{ generateSignature() }}
   Body:
   {
     "type": "SMS",
     "from": "01012345678",
     "content": "기본 메시지",
     "messages": {{ JSON.stringify($json.messages) }}
   }
   ```

### 사례 3: 토스페이먼츠 결제 웹훅 처리

**목표**: 결제 완료 시 자동으로 주문 처리 및 알림

**워크플로우:**

1. **Webhook Trigger**
   - HTTP Method: POST
   - Path: `toss-payment-webhook`
   - Response Mode: Last Node

2. **Function - 서명 검증**
   ```javascript
   const crypto = require('crypto');

   const receivedSignature = $node["Webhook"].context.headers['x-toss-signature'];
   const payload = JSON.stringify($json);
   const secret = $credentials.tossSecretKey;

   const expectedSignature = crypto
     .createHmac('sha256', secret)
     .update(payload)
     .digest('base64');

   if (receivedSignature !== expectedSignature) {
     throw new Error('Invalid signature - potential security threat');
   }

   return [{
     json: {
       orderId: $json.orderId,
       paymentKey: $json.paymentKey,
       amount: $json.totalAmount,
       status: $json.status,
       method: $json.method,
       customerName: $json.customerName,
       customerEmail: $json.customerEmail
     }
   }];
   ```

3. **Switch - 결제 상태별 분기**
   ```javascript
   Rule 1 - 결제 성공:
     Condition: {{ $json.status === 'DONE' }}
     Output: 0

   Rule 2 - 결제 대기:
     Condition: {{ $json.status === 'WAITING_FOR_DEPOSIT' }}
     Output: 1

   Rule 3 - 결제 실패:
     Condition: {{ $json.status === 'ABORTED' || $json.status === 'CANCELED' }}
     Output: 2
   ```

4. **결제 성공 경로**:
   - PostgreSQL: 주문 상태 업데이트
   - HTTP Request: 재고 차감 API 호출
   - Send Email: 결제 완료 이메일
   - 카카오톡: 주문 확인 알림톡
   - Slack: 관리자 채널 알림

5. **Webhook Response**
   ```javascript
   {
     "status": "success",
     "message": "Payment processed successfully"
   }
   ```

### 사례 4: 배달의민족(우아한형제들) API 연동

**목표**: 신규 주문 자동 처리

**워크플로우:**

1. **Schedule Trigger** - 1분마다 신규 주문 확인

2. **HTTP Request - 주문 조회**
   ```javascript
   Method: GET
   URL: https://api.baemin.com/v1/orders
   Headers:
     Authorization: Bearer {{ $credentials.baeminAccessToken }}
   Query Parameters:
     status: new
     since: {{ $now.minus({minutes: 5}).toISO() }}
   ```

3. **Function - 주문 데이터 파싱**
   ```javascript
   const orders = $json.data.orders;

   if (!orders || orders.length === 0) {
     return [];
   }

   return orders.map(order => ({
     json: {
       orderId: order.id,
       orderNumber: order.orderNumber,
       customerName: order.customer.name,
       customerPhone: order.customer.phone,
       address: order.deliveryAddress.full,
       items: order.items.map(item => ({
         name: item.name,
         quantity: item.quantity,
         price: item.price
       })),
       totalAmount: order.payment.totalAmount,
       requestedTime: order.requestedDeliveryTime
     }
   }));
   ```

4. **Loop Over Items** - 각 주문 처리

5. **분기 처리**:
   - **Database**: 주문 저장
   - **Printer API**: 주방 프린터로 주문서 출력
   - **SMS**: 고객에게 접수 확인 문자
   - **POS 시스템**: 재고 연동

6. **HTTP Request - 주문 접수 확인**
   ```javascript
   Method: POST
   URL: https://api.baemin.com/v1/orders/{{ $json.orderId }}/accept
   Headers:
     Authorization: Bearer {{ $credentials.baeminAccessToken }}
   Body:
   {
     "estimatedCookingTime": 20
   }
   ```

### 사례 5: 쿠팡 파트너스 API - 상품 모니터링

**목표**: 쿠팡 특정 상품의 가격/재고 변동 모니터링

**워크플로우:**

1. **Schedule Trigger** - 매시간

2. **HTTP Request - 쿠팡 상품 조회**
   ```javascript
   Method: GET
   URL: https://api-gateway.coupang.com/v2/providers/affiliate_open_api/apis/openapi/products/{{productId}}
   Headers:
     Authorization: Bearer {{ generateCoupangAuth() }}
   ```

3. **Function - 가격 변동 감지**
   ```javascript
   const currentData = $json;
   const previousData = await getPreviousData($json.productId);

   const priceChanged = currentData.price !== previousData.price;
   const stockChanged = currentData.stock !== previousData.stock;

   return [{
     json: {
       productId: currentData.productId,
       productName: currentData.productName,
       currentPrice: currentData.price,
       previousPrice: previousData.price,
       priceChange: currentData.price - previousData.price,
       currentStock: currentData.stock,
       stockAvailable: currentData.stock > 0,
       hasChanges: priceChanged || stockChanged
     }
   }];
   ```

4. **IF** - 변동 사항 있는 경우만

5. **알림 발송**:
   - Slack: #product-alerts 채널
   - Email: 담당자에게 상세 정보
   - Google Sheets: 가격 이력 기록

### 사례 6: 한국 공공 데이터 포털 활용

**목표**: 날씨 정보 기반 마케팅 자동화

**워크플로우:**

1. **Schedule Trigger** - 매일 오전 7시

2. **HTTP Request - 기상청 단기예보 API**
   ```javascript
   Method: GET
   URL: http://apis.data.go.kr/1360000/VilageFcstInfoService_2.0/getVilageFcst
   Query Parameters:
     serviceKey: {{ decodeURIComponent($credentials.dataGoKrApiKey) }}
     numOfRows: 10
     pageNo: 1
     base_date: {{ $now.toFormat('yyyyMMdd') }}
     base_time: 0500
     nx: 60
     ny: 127
   ```

3. **Function - 날씨 데이터 분석**
   ```javascript
   const items = $json.response.body.items.item;

   // 기온, 강수확률, 하늘상태 추출
   const temperature = items.find(i => i.category === 'TMP')?.fcstValue;
   const rainProbability = items.find(i => i.category === 'POP')?.fcstValue;
   const sky = items.find(i => i.category === 'SKY')?.fcstValue;

   // 날씨 기반 마케팅 전략
   let campaign = null;

   if (parseInt(rainProbability) > 70) {
     campaign = {
       type: 'rainy_day',
       products: ['우산', '레인부츠', '방수팩'],
       discount: 15,
       message: '비 오는 날 특별 할인!'
     };
   } else if (parseInt(temperature) > 30) {
     campaign = {
       type: 'hot_day',
       products: ['아이스크림', '선풍기', '에어컨'],
       discount: 20,
       message: '무더위 대비 쿨링 특가!'
     };
   }

   return [{ json: { weather: { temperature, rainProbability, sky }, campaign } }];
   ```

4. **IF** - 캠페인이 있는 경우

5. **마케팅 실행**:
   - Email: 타겟 고객에게 날씨 기반 프로모션
   - SMS: 긴급 할인 정보
   - SNS 자동 게시: 인스타그램/페이스북
   - 홈페이지 배너 자동 변경

### 사례 7: 네이버 검색광고 API 연동

**목표**: 광고 성과 자동 리포팅

**워크플로우:**

1. **Schedule Trigger** - 매일 오전 9시

2. **HTTP Request - 네이버 광고 API**
   ```javascript
   Method: GET
   URL: https://api.naver.com/stats
   Headers:
     X-API-KEY: {{ $credentials.naverAdApiKey }}
     X-Customer: {{ $credentials.naverCustomerId }}
   Query:
     dateFrom: {{ $now.minus({days: 1}).toFormat('yyyy-MM-dd') }}
     dateTo: {{ $now.minus({days: 1}).toFormat('yyyy-MM-dd') }}
   ```

3. **Function - 성과 분석**
   ```javascript
   const stats = $json.data;

   const analysis = {
     date: $now.minus({days: 1}).toFormat('yyyy-MM-dd'),
     총노출수: stats.impressions,
     총클릭수: stats.clicks,
     총비용: stats.cost,
     CTR: ((stats.clicks / stats.impressions) * 100).toFixed(2) + '%',
     평균CPC: (stats.cost / stats.clicks).toFixed(0) + '원',
     전환수: stats.conversions,
     전환율: ((stats.conversions / stats.clicks) * 100).toFixed(2) + '%',
     ROAS: ((stats.revenue / stats.cost) * 100).toFixed(0) + '%'
   };

   // 성과 평가
   const performance = analysis.ROAS > 200 ? '우수' :
                      analysis.ROAS > 100 ? '양호' : '개선필요';

   return [{ json: { ...analysis, 성과평가: performance } }];
   ```

4. **Google Sheets** - 데이터 기록

5. **분기 처리**:
   - **우수**: Slack 축하 메시지
   - **양호**: 정상 리포트
   - **개선필요**: 담당자 알림 + 상세 분석 요청

6. **Email** - 일간 리포트 발송
   ```
   Subject: [네이버 광고] {{ $now.minus({days: 1}).toFormat('yyyy-MM-dd') }} 성과 리포트

   안녕하세요,

   어제 광고 성과를 알려드립니다.

   📊 주요 지표
   - 노출수: {{ $json.총노출수.toLocaleString() }}
   - 클릭수: {{ $json.총클릭수.toLocaleString() }}
   - 비용: {{ $json.총비용.toLocaleString() }}원
   - ROAS: {{ $json.ROAS }}
   - 성과 평가: {{ $json.성과평가 }}

   상세 데이터는 첨부된 스프레드시트를 확인해주세요.
   ```

### 한국 서비스 크레덴셜 가이드

#### 카카오 개발자 계정

1. https://developers.kakao.com 접속
2. 애플리케이션 추가
3. REST API 키 복사
4. 플랫폼 설정 → Redirect URI 등록
5. N8N에서 OAuth2 설정

#### 네이버 클라우드 플랫폼

1. https://console.ncloud.com 접속
2. API 인증키 관리
3. Access Key ID, Secret Key 생성
4. N8N Credentials에 저장

#### 공공 데이터 포털

1. https://www.data.go.kr 회원가입
2. 원하는 API 신청
3. 인증키 발급 (일반 인증키/서비스 키)
4. N8N HTTP Request 노드에서 Query Parameter로 사용

## 고급 기능

### 1. 서브워크플로우 (Sub-workflows)

복잡한 로직을 재사용 가능한 모듈로 분리합니다.

**예시: 이메일 검증 서브워크플로우**

**메인 워크플로우:**
```
[Webhook] → [Execute Workflow: Email Validator] → [Process Result]
```

**서브워크플로우 (Email Validator):**
```
[Workflow Trigger] → [Function: Validate] → [HTTP: Check MX Records] → [Return]
```

**설정:**
- Execute Workflow 노드에서 서브워크플로우 선택
- 데이터는 자동으로 전달됨
- 서브워크플로우의 결과가 반환됨

### 2. 에러 핸들링

**에러 워크플로우 생성:**

1. **Settings → Error Workflow** 활성화

2. **에러 워크플로우 구성:**
   ```
   [Error Trigger] → [Function: Parse Error] → [Slack/Email Notification] → [Database Log]
   ```

3. **Function - 에러 정보 추출:**
   ```javascript
   const error = $json;
   
   return [{
     json: {
       workflowId: error.workflow.id,
       workflowName: error.workflow.name,
       executionId: error.execution.id,
       errorMessage: error.error.message,
       errorStack: error.error.stack,
       timestamp: new Date().toISOString()
     }
   }];
   ```

**노드별 에러 핸들링:**

```javascript
// Try-Catch 패턴
try {
  const result = dangerousOperation();
  return [{ json: result }];
} catch (error) {
  return [{
    json: {
      error: true,
      message: error.message,
      fallbackValue: null
    }
  }];
}
```

### 3. 버전 관리 및 백업

**Git 통합:**

```bash
# N8N 워크플로우 export
n8n export:workflow --all --output=./workflows/

# Git 커밋
git add workflows/
git commit -m "Update workflows"
git push
```

**자동 백업 워크플로우:**

```
[Schedule: Daily] → [N8N API: Export All] → [AWS S3: Upload] → [Slack: Notification]
```

### 4. 성능 최적화

**배치 처리:**

```javascript
// 1000개 아이템을 100개씩 배치로 처리
[Data Source] → [SplitInBatches: 100] → [Process Batch] → [Loop Back]
```

**큐 모드 사용:**

```bash
# docker-compose.yml
environment:
  - EXECUTIONS_MODE=queue
  - QUEUE_BULL_REDIS_HOST=redis
```

**병렬 처리:**

```javascript
// 설정
Settings → Concurrency → Max: 10
```

### 5. 웹훅 보안

**HMAC 서명 검증:**

```javascript
const crypto = require('crypto');

const receivedSignature = $node["Webhook"].context.headers['x-hub-signature'];
const payload = JSON.stringify($json);
const secret = '{{$credentials.webhookSecret}}';

const expectedSignature = 'sha256=' + 
  crypto.createHmac('sha256', secret)
    .update(payload)
    .digest('hex');

if (receivedSignature !== expectedSignature) {
  throw new Error('Invalid signature');
}

return $input.all();
```

**IP 화이트리스트:**

```javascript
const allowedIPs = ['203.0.113.0', '198.51.100.0'];
const clientIP = $node["Webhook"].context.headers['x-forwarded-for'] || 
                 $node["Webhook"].context.clientIP;

if (!allowedIPs.includes(clientIP)) {
  throw new Error('Unauthorized IP: ' + clientIP);
}

return $input.all();
```

### 6. 데이터베이스 연동

**PostgreSQL 예시:**

```sql
-- 조회
SELECT * FROM customers 
WHERE created_at > '{{ $now.minus({days: 7}).toISODate() }}'
ORDER BY id DESC
LIMIT 100;

-- 삽입
INSERT INTO logs (workflow_id, status, message, created_at)
VALUES (
  '{{ $workflow.id }}',
  '{{ $json.status }}',
  '{{ $json.message }}',
  NOW()
);

-- 업데이트
UPDATE orders 
SET status = 'processed', 
    processed_at = NOW()
WHERE id = {{ $json.orderId }};
```

**트랜잭션 처리:**

```javascript
// Function 노드에서
const { Client } = require('pg');
const client = new Client({
  host: 'localhost',
  database: 'n8n',
  user: 'n8n',
  password: 'password'
});

await client.connect();

try {
  await client.query('BEGIN');
  
  await client.query('INSERT INTO orders ...');
  await client.query('UPDATE inventory ...');
  
  await client.query('COMMIT');
  
  return [{ json: { success: true } }];
} catch (error) {
  await client.query('ROLLBACK');
  throw error;
} finally {
  await client.end();
}
```

### 7. 커스텀 노드 개발

**노드 생성 CLI:**

```bash
# N8N 노드 생성기 설치
npm install -g n8n-node-dev

# 새 노드 생성
n8n-node-dev new

# 노드 빌드
npm run build

# 로컬 설치
npm link
```

**커스텀 노드 예시 (TypeScript):**

```typescript
import {
  IExecuteFunctions,
  INodeExecutionData,
  INodeType,
  INodeTypeDescription,
} from 'n8n-workflow';

export class MyCustomNode implements INodeType {
  description: INodeTypeDescription = {
    displayName: 'My Custom Node',
    name: 'myCustomNode',
    group: ['transform'],
    version: 1,
    description: 'Custom node for specific business logic',
    defaults: {
      name: 'My Custom Node',
    },
    inputs: ['main'],
    outputs: ['main'],
    properties: [
      {
        displayName: 'API Key',
        name: 'apiKey',
        type: 'string',
        default: '',
        required: true,
      },
    ],
  };

  async execute(this: IExecuteFunctions): Promise<INodeExecutionData[][]> {
    const items = this.getInputData();
    const returnData: INodeExecutionData[] = [];

    for (let i = 0; i < items.length; i++) {
      const apiKey = this.getNodeParameter('apiKey', i) as string;
      
      // 커스텀 로직
      const result = await this.helpers.request({
        method: 'GET',
        url: 'https://api.example.com/data',
        headers: {
          'Authorization': `Bearer ${apiKey}`,
        },
      });

      returnData.push({
        json: result,
      });
    }

    return [returnData];
  }
}
```

---

## 보안 및 베스트 프랙티스

### 보안 체크리스트

#### 1. 인증 및 권한

✅ **기본 인증 활성화**
```bash
N8N_BASIC_AUTH_ACTIVE=true
N8N_BASIC_AUTH_USER=admin
N8N_BASIC_AUTH_PASSWORD=strong_password_here
```

✅ **HTTPS 사용 (프로덕션)**
```bash
N8N_PROTOCOL=https
N8N_SSL_KEY=/path/to/private.key
N8N_SSL_CERT=/path/to/certificate.crt
```

✅ **크레덴셜 암호화**
- 모든 API 키와 비밀번호는 N8N Credentials에 저장
- 환경 변수로 민감 정보 관리
- 절대 워크플로우에 하드코딩 금지

#### 2. 네트워크 보안

✅ **방화벽 설정**
```bash
# UFW (Ubuntu)
sudo ufw allow from YOUR_IP to any port 5678
sudo ufw enable
```

✅ **리버스 프록시 사용 (Nginx)**
```nginx
server {
    listen 80;
    server_name n8n.your-domain.com;
    
    location / {
        proxy_pass http://localhost:5678;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

✅ **Webhook 보안**
- HMAC 서명 검증
- IP 화이트리스트
- Rate limiting

#### 3. 데이터 보안

✅ **데이터베이스 암호화**
```bash
# PostgreSQL SSL 연결
DB_POSTGRESDB_SSL_ENABLED=true
DB_POSTGRESDB_SSL_REJECT_UNAUTHORIZED=true
```

✅ **실행 데이터 보존 정책**
```bash
EXECUTIONS_DATA_PRUNE=true
EXECUTIONS_DATA_MAX_AGE=336 # 14일
```

✅ **민감 데이터 마스킹**
```javascript
// Function 노드에서
const maskedData = {
  ...data,
  creditCard: data.creditCard.replace(/\d(?=\d{4})/g, '*'),
  ssn: data.ssn.replace(/\d{5}/, '*****')
};

return [{ json: maskedData }];
```

### 성능 베스트 프랙티스

#### 1. 워크플로우 설계

✅ **작은 단위로 분할**
- 복잡한 로직은 서브워크플로우로 분리
- 재사용 가능한 컴포넌트 생성

✅ **불필요한 데이터 제거**
```javascript
// 필요한 필드만 선택
const cleanData = items.map(item => ({
  json: {
    id: item.json.id,
    name: item.json.name,
    // 불필요한 대용량 필드 제외
  }
}));
```

✅ **배치 처리 활용**
```javascript
// 1000개를 한 번에 처리하지 말고
[Data Source] → [SplitInBatches: 100] → [Process] → [Loop]
```

#### 2. API 호출 최적화

✅ **Rate Limiting 존중**
```javascript
// Wait 노드 활용
[API Call] → [Wait: 1 second] → [Next Call]
```

✅ **캐싱 구현**
```javascript
// Redis 또는 메모리 캐시 사용
const cacheKey = `user_${userId}`;
let userData = await redis.get(cacheKey);

if (!userData) {
  userData = await fetchFromAPI(userId);
  await redis.setex(cacheKey, 3600, JSON.stringify(userData));
}

return [{ json: userData }];
```

✅ **병렬 처리**
```javascript
// Split In Batches에서 동시 실행 설정
Batch Size: 10
Concurrency: 5
```

#### 3. 모니터링 및 로깅

✅ **실행 로그 수집**
```javascript
// Function 노드에서 구조화된 로깅
console.log(JSON.stringify({
  level: 'info',
  workflow: $workflow.name,
  execution: $execution.id,
  message: 'Processing started',
  data: { itemCount: items.length }
}));
```

✅ **성능 메트릭 추적**
```javascript
const startTime = Date.now();

// 작업 수행
const result = await processData();

const duration = Date.now() - startTime;

// 메트릭 전송 (CloudWatch, Datadog 등)
await sendMetric('workflow.execution.duration', duration, {
  workflow: $workflow.name
});
```

### 코드 품질

✅ **표준 명명 규칙**
- 워크플로우: `kebab-case` (예: `customer-onboarding`)
- 노드: `PascalCase` (예: `ProcessCustomerData`)
- 변수: `camelCase` (예: `customerEmail`)

✅ **주석 작성**
```javascript
// Function 노드
/**
 * 고객 데이터 검증 및 변환
 * 
 * 입력: 원시 고객 정보
 * 출력: 검증되고 표준화된 고객 데이터
 * 
 * 비즈니스 규칙:
 * - 이메일은 필수
 * - 전화번호는 국제 형식으로 변환
 * - 중복 고객 확인
 */

const customer = $json;

// 이메일 검증
if (!customer.email || !isValidEmail(customer.email)) {
  throw new Error('Invalid email address');
}

// ... 나머지 로직
```

✅ **에러 메시지 명확화**
```javascript
// 나쁜 예
throw new Error('Error');

// 좋은 예
throw new Error(
  `Failed to process customer ${customer.id}: ` +
  `Invalid email format '${customer.email}'`
);
```

### 운영 베스트 프랙티스

✅ **환경 분리**
- 개발 (Development)
- 스테이징 (Staging)
- 프로덕션 (Production)

각 환경별로 별도의 N8N 인스턴스 운영

✅ **CI/CD 파이프라인**

```yaml
# .github/workflows/deploy-n8n.yml
name: Deploy N8N Workflows

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      
      - name: Export workflows
        run: |
          n8n export:workflow --all --output=./workflows/
      
      - name: Deploy to production
        run: |
          n8n import:workflow --input=./workflows/ --separate
        env:
          N8N_HOST: ${{ secrets.N8N_PROD_HOST }}
          N8N_API_KEY: ${{ secrets.N8N_API_KEY }}
```

✅ **백업 전략**
```bash
# 자동 백업 스크립트
#!/bin/bash

DATE=$(date +%Y%m%d_%H%M%S)
BACKUP_DIR="/backups/n8n"

# 워크플로우 백업
docker exec n8n n8n export:workflow --all --output=/data/backup_${DATE}.json

# 데이터베이스 백업
docker exec postgres pg_dump -U n8n n8n > ${BACKUP_DIR}/db_${DATE}.sql

# S3 업로드
aws s3 cp ${BACKUP_DIR}/db_${DATE}.sql s3://my-backup-bucket/n8n/

# 7일 이상 된 백업 삭제
find ${BACKUP_DIR} -mtime +7 -delete
```

✅ **모니터링 대시보드**

Grafana + Prometheus 설정:

```yaml
# prometheus.yml
scrape_configs:
  - job_name: 'n8n'
    static_configs:
      - targets: ['n8n:5678']
```

---

## 프로덕션 배포 체크리스트

N8N을 프로덕션 환경에 배포하기 전 반드시 확인해야 할 사항들입니다.

### 1. 보안 설정

#### 필수 보안 설정
- [ ] **HTTPS 활성화**
  ```bash
  N8N_PROTOCOL=https
  N8N_SSL_KEY=/path/to/ssl/private.key
  N8N_SSL_CERT=/path/to/ssl/certificate.crt
  ```

- [ ] **강력한 인증 설정**
  ```bash
  N8N_BASIC_AUTH_ACTIVE=true
  N8N_BASIC_AUTH_USER=<strong-username>
  N8N_BASIC_AUTH_PASSWORD=<complex-password-min-16-chars>
  ```

- [ ] **암호화 키 설정**
  ```bash
  # 안전한 랜덤 키 생성
  N8N_ENCRYPTION_KEY=$(openssl rand -base64 32)

  # 키를 안전한 곳에 백업 (KMS, Vault 등)
  ```

- [ ] **웹훅 보안**
  - HMAC 서명 검증 구현
  - IP 화이트리스트 설정
  - Rate limiting 활성화

#### 네트워크 보안
- [ ] **방화벽 규칙**
  ```bash
  # 필요한 IP만 허용
  sudo ufw allow from <your-ip>/32 to any port 5678
  sudo ufw enable
  ```

- [ ] **리버스 프록시 설정** (Nginx/Caddy)
  ```nginx
  server {
      listen 443 ssl http2;
      server_name n8n.yourdomain.com;

      ssl_certificate /etc/letsencrypt/live/n8n.yourdomain.com/fullchain.pem;
      ssl_certificate_key /etc/letsencrypt/live/n8n.yourdomain.com/privkey.pem;

      # 보안 헤더
      add_header Strict-Transport-Security "max-age=31536000" always;
      add_header X-Frame-Options "SAMEORIGIN" always;
      add_header X-Content-Type-Options "nosniff" always;

      location / {
          proxy_pass http://localhost:5678;
          proxy_set_header Host $host;
          proxy_set_header X-Real-IP $remote_addr;
          proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
          proxy_set_header X-Forwarded-Proto $scheme;

          # WebSocket 지원
          proxy_http_version 1.1;
          proxy_set_header Upgrade $http_upgrade;
          proxy_set_header Connection "upgrade";
      }
  }
  ```

- [ ] **WAF (Web Application Firewall)** 설정 (Cloudflare, AWS WAF 등)

### 2. 인프라 설정

#### 데이터베이스
- [ ] **PostgreSQL 프로덕션 설정**
  ```bash
  DB_TYPE=postgresdb
  DB_POSTGRESDB_HOST=<rds-endpoint>
  DB_POSTGRESDB_SSL_ENABLED=true
  DB_POSTGRESDB_SSL_REJECT_UNAUTHORIZED=true
  DB_POSTGRESDB_POOL_SIZE=20
  ```

- [ ] **데이터베이스 백업 자동화**
  ```bash
  # Cron 작업
  0 2 * * * /usr/local/bin/backup-n8n-db.sh
  ```

- [ ] **데이터베이스 모니터링** (CPU, 메모리, 연결 수, 쿼리 성능)

#### Redis (Queue 모드 사용 시)
- [ ] **Redis 설정**
  ```bash
  EXECUTIONS_MODE=queue
  QUEUE_BULL_REDIS_HOST=<redis-endpoint>
  QUEUE_BULL_REDIS_PORT=6379
  QUEUE_BULL_REDIS_PASSWORD=<strong-password>
  QUEUE_BULL_REDIS_TLS=true
  ```

- [ ] **Redis 백업 및 복제** 설정

#### 컴퓨팅 리소스
- [ ] **충분한 리소스 할당**
  - 최소: 2 vCPU, 4GB RAM
  - 권장: 4 vCPU, 8GB RAM (중규모)
  - 대규모: 8+ vCPU, 16GB+ RAM

- [ ] **스왑 메모리 설정** (메모리 부족 대비)
  ```bash
  sudo fallocate -l 4G /swapfile
  sudo chmod 600 /swapfile
  sudo mkswap /swapfile
  sudo swapon /swapfile
  ```

- [ ] **자동 재시작 설정**
  ```yaml
  # docker-compose.yml
  services:
    n8n:
      restart: unless-stopped
  ```

### 3. 성능 최적화

- [ ] **환경 변수 최적화**
  ```bash
  # 실행 타임아웃
  EXECUTIONS_TIMEOUT=3600
  EXECUTIONS_TIMEOUT_MAX=7200

  # 페이로드 크기
  N8N_PAYLOAD_SIZE_MAX=32

  # Node.js 메모리
  NODE_OPTIONS=--max-old-space-size=4096

  # 워커 수 (Queue 모드)
  N8N_CONCURRENCY_PRODUCTION=10
  ```

- [ ] **실행 데이터 정리**
  ```bash
  EXECUTIONS_DATA_PRUNE=true
  EXECUTIONS_DATA_MAX_AGE=168  # 7일
  EXECUTIONS_DATA_SAVE_ON_SUCCESS=lastNodeFinished
  EXECUTIONS_DATA_SAVE_ON_ERROR=all
  ```

- [ ] **CDN 설정** (정적 리소스)

### 4. 모니터링 및 로깅

#### 로그 관리
- [ ] **구조화된 로깅**
  ```bash
  N8N_LOG_LEVEL=info
  N8N_LOG_OUTPUT=file
  N8N_LOG_FILE_LOCATION=/var/log/n8n/
  ```

- [ ] **로그 수집** (ELK, CloudWatch Logs, Datadog 등)

- [ ] **로그 로테이션**
  ```bash
  # /etc/logrotate.d/n8n
  /var/log/n8n/*.log {
      daily
      rotate 14
      compress
      delaycompress
      notifempty
      create 0640 n8n n8n
      sharedscripts
  }
  ```

#### 메트릭 수집
- [ ] **Prometheus + Grafana 설정**
  ```bash
  N8N_METRICS=true
  N8N_METRICS_PREFIX=n8n_
  ```

- [ ] **주요 메트릭 모니터링**
  - 워크플로우 실행 성공/실패율
  - 평균 실행 시간
  - 큐 크기 (Queue 모드)
  - 데이터베이스 연결 수
  - 메모리/CPU 사용률

#### 알림 설정
- [ ] **중요 이벤트 알림**
  - 워크플로우 실패
  - 시스템 에러
  - 리소스 임계값 초과
  - 크레덴셜 만료

- [ ] **헬스체크 엔드포인트**
  ```bash
  # 정기적으로 확인
  curl https://n8n.yourdomain.com/healthz
  ```

### 5. 백업 및 복구

- [ ] **전체 백업 전략**
  ```bash
  #!/bin/bash
  # backup-n8n.sh

  DATE=$(date +%Y%m%d_%H%M%S)
  BACKUP_DIR="/backups/n8n/${DATE}"

  mkdir -p ${BACKUP_DIR}

  # 워크플로우 백업
  docker exec n8n n8n export:workflow --all --output=/data/workflows.json
  docker cp n8n:/data/workflows.json ${BACKUP_DIR}/

  # 크레덴셜 백업 (주의: 암호화된 상태)
  docker exec n8n n8n export:credentials --all --output=/data/credentials.json
  docker cp n8n:/data/credentials.json ${BACKUP_DIR}/

  # 데이터베이스 백업
  docker exec postgres pg_dump -U n8n n8n | gzip > ${BACKUP_DIR}/database.sql.gz

  # 환경 변수 백업
  cp .env ${BACKUP_DIR}/

  # S3 업로드
  aws s3 sync ${BACKUP_DIR} s3://my-n8n-backups/${DATE}/

  # 30일 이상 로컬 백업 삭제
  find /backups/n8n -mtime +30 -type d -exec rm -rf {} +
  ```

- [ ] **백업 테스트** (정기적으로 복구 테스트 수행)

- [ ] **재해 복구 계획** (DR Plan) 문서화

### 6. 고가용성 (HA)

- [ ] **로드 밸런서 설정**
  ```yaml
  # AWS Application Load Balancer
  # 또는 Nginx Load Balancer

  upstream n8n_backend {
      server n8n-1:5678;
      server n8n-2:5678;
      server n8n-3:5678;
  }
  ```

- [ ] **Queue 모드 활성화** (여러 워커 인스턴스)

- [ ] **데이터베이스 복제** (Master-Slave)

- [ ] **Redis 클러스터/Sentinel**

### 7. CI/CD 파이프라인

- [ ] **자동화된 배포**
  ```yaml
  # .github/workflows/deploy.yml
  name: Deploy N8N

  on:
    push:
      branches: [main]

  jobs:
    deploy:
      runs-on: ubuntu-latest
      steps:
        - uses: actions/checkout@v2

        - name: Deploy to production
          env:
            SSH_PRIVATE_KEY: ${{ secrets.SSH_PRIVATE_KEY }}
          run: |
            echo "$SSH_PRIVATE_KEY" > key.pem
            chmod 600 key.pem
            ssh -i key.pem user@server << 'EOF'
              cd /opt/n8n
              git pull
              docker-compose pull
              docker-compose up -d
              docker-compose logs -f n8n &
              sleep 30
              curl -f http://localhost:5678/healthz || exit 1
            EOF
  ```

- [ ] **블루-그린 배포** 또는 **카나리 배포** 전략

### 8. 규정 준수

- [ ] **GDPR 준수** (EU 사용자 데이터 처리 시)
  - 데이터 보존 정책
  - 데이터 삭제 프로세스
  - 데이터 처리 동의

- [ ] **감사 로그** 활성화
  ```javascript
  // 모든 워크플로우 실행 로깅
  console.log(JSON.stringify({
    type: 'workflow_execution',
    workflowId: $workflow.id,
    workflowName: $workflow.name,
    executionId: $execution.id,
    userId: $execution.userId,
    timestamp: new Date().toISOString()
  }));
  ```

- [ ] **접근 제어** (RBAC) 설정

### 9. 문서화

- [ ] **시스템 아키텍처 다이어그램**

- [ ] **워크플로우 문서화**
  - 각 워크플로우의 목적
  - 입력/출력 스펙
  - 의존성
  - 담당자

- [ ] **Runbook 작성**
  - 배포 절차
  - 롤백 절차
  - 장애 대응 절차
  - 일반적인 문제 해결 방법

- [ ] **온보딩 가이드** (신규 팀원용)

### 10. 테스트

- [ ] **스테이징 환경** 구축 (프로덕션과 동일 구성)

- [ ] **부하 테스트**
  ```bash
  # Apache Bench 예시
  ab -n 1000 -c 10 https://n8n.yourdomain.com/webhook/test
  ```

- [ ] **워크플로우 테스트 자동화**
  ```javascript
  // 테스트 워크플로우
  [Schedule: Daily]
    → [HTTP Request: Test Endpoint]
    → [IF: Response OK]
    → [Success: Log]
    → [Failure: Alert Team]
  ```

---

## 트러블슈팅

### 일반적인 문제 및 해결

#### 1. 워크플로우 실행 실패

**증상**: 워크플로우가 시작되지 않거나 중간에 멈춤

**해결 방법:**

```javascript
// 1. 실행 로그 확인
Settings → Executions → 해당 실행 클릭 → 에러 메시지 확인

// 2. 각 노드의 출력 데이터 검증
노드 클릭 → "Execute Node" → 출력 확인

// 3. 표현식 테스트
{{ $json.fieldName }} 표현식이 올바른지 확인
```

**일반적인 원인:**
- 필드명 오타
- null/undefined 데이터 참조
- 잘못된 데이터 타입
- API 인증 실패

#### 2. 메모리 부족

**증상**: 
```
Error: JavaScript heap out of memory
```

**해결 방법:**

```bash
# Node.js 힙 메모리 증가
NODE_OPTIONS=--max-old-space-size=4096

# Docker Compose
services:
  n8n:
    environment:
      - NODE_OPTIONS=--max-old-space-size=4096
```

**예방:**
- 대용량 데이터는 배치 처리
- 불필요한 필드 제거
- 실행 데이터 정기 삭제

#### 3. 웹훅 응답 없음

**증상**: 웹훅 URL로 요청했지만 응답이 없음

**진단:**

```bash
# 1. N8N 로그 확인
docker logs n8n

# 2. 네트워크 접근 테스트
curl -v http://localhost:5678/webhook/test-webhook

# 3. 방화벽 확인
sudo ufw status
```

**해결:**
- 워크플로우가 Active 상태인지 확인
- 웹훅 URL이 정확한지 확인
- 네트워크/방화벽 설정 검토

#### 4. 크레덴셜 연결 실패

**증상**: API 인증 실패, 401/403 에러

**해결 방법:**

```javascript
// 1. 크레덴셜 재검증
Settings → Credentials → [해당 크레덴셜] → Test

// 2. API 키/토큰 유효기간 확인
// 많은 서비스에서 토큰이 만료됨

// 3. OAuth2 재인증
// OAuth2 토큰은 주기적으로 갱신 필요

// 4. 권한 범위 확인
// API가 요구하는 스코프가 있는지 확인
```

#### 5. 데이터베이스 연결 문제

**증상**:
```
Error: Connection terminated unexpectedly
```

**해결:**

```bash
# PostgreSQL 상태 확인
docker exec postgres pg_isready

# 연결 테스트
docker exec -it postgres psql -U n8n -d n8n

# 연결 풀 설정 조정
DB_POSTGRESDB_POOL_SIZE=20
```

#### 6. 성능 저하

**증상**: 워크플로우 실행이 느려짐

**진단 및 해결:**

```javascript
// 1. 실행 시간 분석
각 노드의 실행 시간 확인 (Executions 로그)

// 2. 병목 지점 식별
- API 호출이 느린가?
- 데이터 처리 로직이 비효율적인가?
- 데이터베이스 쿼리가 최적화되어 있는가?

// 3. 최적화 적용
- 배치 크기 조정
- 불필요한 노드 제거
- 인덱스 추가 (데이터베이스)
- 캐싱 도입
```

#### 7. 타임아웃 에러

**증상**:
```
Error: Timeout of 300000ms exceeded
```

**해결:**

```javascript
// HTTP Request 노드
Settings → Timeout → 600000 (10분)

// 환경 변수
EXECUTIONS_TIMEOUT=3600 # 1시간
EXECUTIONS_TIMEOUT_MAX=7200 # 2시간
```

### 디버깅 팁

#### 1. 단계별 실행

```javascript
// "Execute Node" 버튼으로 각 노드를 개별 실행
// 데이터 흐름을 단계별로 확인
```

#### 2. 로그 레벨 조정

```bash
# 상세 로그 활성화
N8N_LOG_LEVEL=debug

# 로그 출력
N8N_LOG_OUTPUT=console,file
```

#### 3. 테스트 데이터 사용

```javascript
// Manual Trigger 노드 활용
// 실제 데이터 대신 테스트 JSON 사용

{
  "testMode": true,
  "sampleData": {
    "id": 123,
    "name": "Test User"
  }
}
```

#### 4. Function 노드에서 중간 결과 출력

```javascript
const data = $json;

// 중간 결과 확인
console.log('Processing data:', data);

const result = processData(data);

// 결과 확인
console.log('Result:', result);

return [{ json: result }];
```

### 유용한 커뮤니티 리소스

**공식 문서:**
- https://docs.n8n.io

**커뮤니티 포럼:**
- https://community.n8n.io

**GitHub:**
- https://github.com/n8n-io/n8n

**워크플로우 템플릿:**
- https://n8n.io/workflows

**Discord:**
- https://discord.gg/n8n

---

## FAQ

자주 묻는 질문과 답변입니다.

### 일반 질문

**Q: N8N은 무료인가요?**

A: 셀프 호스팅은 완전 무료입니다. N8N Cloud는 유료 구독 모델이며, 소스 코드는 Fair-code 라이선스로 제공됩니다. 상업적 사용도 가능하지만, N8N을 서비스로 재판매하는 것은 제한됩니다.

**Q: MAKE.COM과 어떻게 다른가요?**

A:
- N8N: 오픈소스, 셀프 호스팅 가능, 코드 레벨 커스터마이징 가능, 무료 (셀프 호스팅 시)
- MAKE: 클라우드 전용, 사용하기 더 쉬움, 월 구독료 필요

**Q: Zapier나 Integromat 대신 N8N을 선택해야 하는 이유는?**

A: 데이터 보안이 중요하거나, 복잡한 커스텀 로직이 필요하거나, 비용을 절감하고 싶거나, 개발자 리소스가 있는 경우 N8N이 더 적합합니다.

**Q: N8N 클라우드 vs 셀프 호스팅, 어떤 것을 선택해야 하나요?**

A:
- **N8N Cloud**: 빠른 시작, 유지보수 불필요, 자동 업데이트, 월 $20부터
- **셀프 호스팅**: 완전한 제어, 데이터 주권, 무료 (인프라 비용 제외), 커스터마이징 가능

### 설치 및 설정

**Q: Docker 없이 설치할 수 있나요?**

A: 네, NPM으로 설치 가능합니다:
```bash
npm install -g n8n
n8n start
```

**Q: Windows에서 사용할 수 있나요?**

A: 네, 다음 방법들이 있습니다:
1. Docker Desktop for Windows (권장)
2. NPM 직접 설치
3. WSL2 + Docker

**Q: 데이터베이스는 필수인가요?**

A: SQLite가 기본이지만, 프로덕션 환경에서는 PostgreSQL을 강력히 권장합니다.

**Q: N8N을 업그레이드하려면 어떻게 하나요?**

A:
```bash
# Docker
docker pull n8nio/n8n:latest
docker-compose up -d

# NPM
npm update -g n8n
```

**중요**: 업그레이드 전 반드시 백업하세요!

### 워크플로우 및 사용

**Q: 워크플로우를 다른 N8N 인스턴스로 이동할 수 있나요?**

A: 네, Export/Import 기능을 사용하세요:
```bash
# Export
n8n export:workflow --all --output=workflows.json

# Import
n8n import:workflow --input=workflows.json
```

**Q: 한 워크플로우에서 몇 개의 노드까지 사용할 수 있나요?**

A: 기술적 제한은 없지만, 성능상 100개 이하를 권장합니다. 복잡한 로직은 서브워크플로우로 분리하세요.

**Q: 워크플로우를 버전 관리할 수 있나요?**

A: N8N 자체는 버전 관리 기능이 제한적이지만, Git에 워크플로우 JSON 파일을 커밋하여 관리할 수 있습니다.

**Q: 워크플로우가 실패했을 때 자동으로 재시도할 수 있나요?**

A: 네, 노드 설정에서 "Retry On Fail" 옵션을 활성화하거나, Error Trigger와 조합하여 커스텀 재시도 로직을 구현할 수 있습니다.

**Q: 스케줄 트리거가 정확한 시간에 실행되지 않아요.**

A:
- 서버 타임존 확인: `GENERIC_TIMEZONE=Asia/Seoul`
- Cron 표현식 검증: [crontab.guru](https://crontab.guru) 사용
- 시스템 시간 동기화 확인

### 성능 및 확장성

**Q: N8N이 느려요. 어떻게 최적화하나요?**

A:
1. Queue 모드 활성화 (Redis 필요)
2. 불필요한 데이터 제거 (Set 노드 사용)
3. 배치 처리 (SplitInBatches 노드)
4. 실행 데이터 정리 활성화
5. 충분한 리소스 할당 (최소 4GB RAM)

**Q: 동시에 몇 개의 워크플로우를 실행할 수 있나요?**

A:
- Regular 모드: 리소스에 따라 다르지만, 보통 10-20개
- Queue 모드: 수백 개 이상 (워커 인스턴스 추가 가능)

**Q: 큰 파일을 처리할 수 있나요?**

A: N8N_PAYLOAD_SIZE_MAX 환경 변수로 조정 가능하지만, 매우 큰 파일(100MB+)은 외부 스토리지 사용을 권장합니다.

### 보안

**Q: N8N은 안전한가요?**

A: 기본 설정은 보안에 취약할 수 있습니다. 프로덕션에서는:
- HTTPS 활성화
- 강력한 비밀번호
- 방화벽 설정
- 정기적인 업데이트

**Q: 크레덴셜은 어떻게 저장되나요?**

A: 데이터베이스에 암호화되어 저장됩니다. N8N_ENCRYPTION_KEY를 안전하게 관리하세요.

**Q: 2FA(2단계 인증)를 설정할 수 있나요?**

A: 현재 N8N 자체에는 2FA가 없지만, 리버스 프록시(Nginx + OAuth2 Proxy)를 통해 구현할 수 있습니다.

### 통합 및 연동

**Q: N8N에 없는 서비스와 연동하려면?**

A: HTTP Request 노드로 API를 직접 호출하거나, 커스텀 노드를 개발할 수 있습니다.

**Q: 웹훅이 작동하지 않아요.**

A:
1. 워크플로우가 Active 상태인지 확인
2. WEBHOOK_URL 환경 변수 확인
3. 방화벽/포트 열림 확인
4. HTTPS 필요 여부 확인 (일부 서비스)

**Q: OAuth2 인증이 계속 실패해요.**

A:
1. Redirect URI가 정확한지 확인 (`http://your-domain/rest/oauth2-credential/callback`)
2. Client ID/Secret 확인
3. 필요한 스코프가 모두 포함되었는지 확인
4. 서비스의 API 사용 제한 확인

### 문제 해결

**Q: "JavaScript heap out of memory" 에러가 발생해요.**

A:
```bash
NODE_OPTIONS=--max-old-space-size=4096
```

**Q: 데이터베이스 연결이 계속 끊겨요.**

A:
- 연결 풀 크기 증가: `DB_POSTGRESDB_POOL_SIZE=20`
- 데이터베이스 서버 확인
- 네트워크 안정성 확인

**Q: 워크플로우가 자꾸 timeout 됩니다.**

A:
```bash
EXECUTIONS_TIMEOUT=7200  # 2시간
EXECUTIONS_TIMEOUT_MAX=14400  # 4시간
```

**Q: N8N 로그는 어디서 확인하나요?**

A:
```bash
# Docker
docker logs n8n -f

# 시스템 로그
tail -f /var/log/n8n/n8n.log
```

---

## 부록

### A. 주요 노드 빠른 참조 (치트시트)

#### 트리거 노드

| 노드 | 용도 | 주요 설정 |
|------|------|----------|
| **Webhook** | HTTP 요청 수신 | Path, Method, Response Mode |
| **Schedule Trigger** | 정기 실행 | Cron Expression, Timezone |
| **Manual Trigger** | 수동 실행/테스트 | - |
| **Email Trigger (IMAP)** | 이메일 수신 시 | Host, User, Password, Mailbox |
| **Webhook (Wait for Response)** | 동기식 응답 | Path, Response Data |

#### 데이터 변환 노드

| 노드 | 용도 | 사용 예시 |
|------|------|----------|
| **Set** | 필드 추가/수정/삭제 | 데이터 구조 변경, 불필요한 필드 제거 |
| **Function** | JavaScript 코드 실행 | 복잡한 로직, 데이터 변환 |
| **Code** | Python/JavaScript | 고급 데이터 처리, 라이브러리 사용 |
| **Item Lists** | 배열 조작 | 분할, 병합, 집계 |
| **Date & Time** | 날짜/시간 처리 | 포맷 변환, 시간대 변경 |

#### 조건 및 흐름 제어

| 노드 | 용도 | 사용 예시 |
|------|------|----------|
| **IF** | 조건부 분기 (2개 경로) | True/False 조건 |
| **Switch** | 다중 분기 (3개+ 경로) | 값에 따라 다른 처리 |
| **Merge** | 데이터 병합 | 여러 소스 결합 |
| **Split In Batches** | 배치 처리 | 대량 데이터를 작은 단위로 |
| **Loop Over Items** | 아이템별 반복 | 각 항목에 개별 작업 |

#### HTTP 및 API

| 노드 | 용도 | 주요 설정 |
|------|------|----------|
| **HTTP Request** | REST API 호출 | Method, URL, Auth, Headers, Body |
| **GraphQL** | GraphQL API | Query, Variables |
| **SSH** | 원격 명령 실행 | Host, Command |
| **FTP** | 파일 전송 | Upload, Download, List |

#### 데이터베이스

| 노드 | 용도 | 지원 DB |
|------|------|---------|
| **PostgreSQL** | PostgreSQL 쿼리 | SELECT, INSERT, UPDATE, DELETE |
| **MySQL** | MySQL 쿼리 | SELECT, INSERT, UPDATE, DELETE |
| **MongoDB** | MongoDB 작업 | Find, Insert, Update, Delete, Aggregate |
| **Redis** | Redis 작업 | Get, Set, Delete, Increment |

#### 파일 처리

| 노드 | 용도 | 기능 |
|------|------|------|
| **Read/Write Binary File** | 로컬 파일 | 파일 읽기/쓰기 |
| **Spreadsheet File** | Excel/CSV | 읽기, 쓰기, 파싱 |
| **PDF** | PDF 처리 | 생성, 변환, 텍스트 추출 |
| **Compress** | 압축/해제 | ZIP, GZIP |

#### 알림 및 메시징

| 노드 | 용도 | 특징 |
|------|------|------|
| **Send Email** | 이메일 발송 | SMTP, 첨부파일 |
| **Slack** | Slack 메시지 | 채널, DM, 파일 업로드 |
| **Discord** | Discord 메시지 | 채널, Webhook |
| **Telegram** | 텔레그램 메시지 | 봇 API |
| **Twilio** | SMS 발송 | 문자 메시지, 음성 |

#### 클라우드 서비스

| 노드 | 용도 | 주요 기능 |
|------|------|----------|
| **AWS S3** | 파일 스토리지 | Upload, Download, List, Delete |
| **Google Drive** | 파일 관리 | Upload, Download, Share |
| **Dropbox** | 파일 동기화 | Upload, Download, Move |
| **AWS Lambda** | 서버리스 함수 | Invoke Function |
| **Google Sheets** | 스프레드시트 | Read, Write, Append |

#### 개발 도구

| 노드 | 용도 | 사용 예시 |
|------|------|----------|
| **GitHub** | Git 작업 | Issue, PR, Commit 관리 |
| **GitLab** | Git 작업 | Pipeline, Merge Request |
| **Jira** | 이슈 트래킹 | 티켓 생성, 업데이트, 검색 |
| **Linear** | 프로젝트 관리 | Issue, Project 관리 |

#### 유틸리티

| 노드 | 용도 | 기능 |
|------|------|------|
| **Wait** | 지연 실행 | 일정 시간 대기 |
| **Execute Workflow** | 서브워크플로우 | 다른 워크플로우 호출 |
| **Error Trigger** | 에러 처리 | 실패 시 특별 처리 |
| **Sticky Note** | 문서화 | 워크플로우 설명 |
| **No Op** | 디버깅 | 데이터 확인용 |

### 노드 선택 가이드

**데이터 변환이 필요할 때:**
- 간단한 변환: **Set** 노드
- 복잡한 로직: **Function** 노드
- 외부 라이브러리 필요: **Code** 노드

**API 호출이 필요할 때:**
- REST API: **HTTP Request** 노드
- 전용 노드가 있는 서비스: 해당 서비스 노드 사용 (Slack, GitHub 등)
- GraphQL: **GraphQL** 노드

**조건부 처리가 필요할 때:**
- 2가지 경로: **IF** 노드
- 3가지 이상: **Switch** 노드
- 복잡한 조건: **Function** 노드에서 분기

**대량 데이터 처리:**
- 순차 처리: **Loop Over Items**
- 배치 처리: **Split In Batches**
- 병렬 처리: **HTTP Request** 노드의 Batch 기능

### B. 환경 변수 전체 목록

```bash
# 기본 설정
N8N_PORT=5678
N8N_HOST=localhost
N8N_PROTOCOL=http
N8N_EDITOR_BASE_URL=http://localhost:5678

# 인증
N8N_BASIC_AUTH_ACTIVE=true
N8N_BASIC_AUTH_USER=admin
N8N_BASIC_AUTH_PASSWORD=password

# 데이터베이스
DB_TYPE=postgresdb
DB_POSTGRESDB_HOST=localhost
DB_POSTGRESDB_PORT=5432
DB_POSTGRESDB_DATABASE=n8n
DB_POSTGRESDB_USER=n8n
DB_POSTGRESDB_PASSWORD=password
DB_POSTGRESDB_SCHEMA=public
DB_POSTGRESDB_SSL_ENABLED=false

# 실행
EXECUTIONS_MODE=regular # regular | queue
EXECUTIONS_TIMEOUT=3600
EXECUTIONS_TIMEOUT_MAX=7200
EXECUTIONS_DATA_SAVE_ON_ERROR=all
EXECUTIONS_DATA_SAVE_ON_SUCCESS=all
EXECUTIONS_DATA_SAVE_ON_PROGRESS=false
EXECUTIONS_DATA_SAVE_MANUAL_EXECUTIONS=true
EXECUTIONS_DATA_PRUNE=true
EXECUTIONS_DATA_MAX_AGE=336 # hours

# 웹훅
WEBHOOK_URL=http://localhost:5678/

# 로그
N8N_LOG_LEVEL=info # error | warn | info | verbose | debug
N8N_LOG_OUTPUT=console # console | file | console,file

# 보안
N8N_PAYLOAD_SIZE_MAX=16 # MB
N8N_METRICS=false

# 타임존
GENERIC_TIMEZONE=Asia/Seoul

# 큐 (Redis 사용 시)
QUEUE_BULL_REDIS_HOST=localhost
QUEUE_BULL_REDIS_PORT=6379
QUEUE_BULL_REDIS_DB=0
QUEUE_BULL_REDIS_PASSWORD=

# 외부 훅
EXTERNAL_HOOK_FILES=/data/hooks.js

# 사용자 관리
N8N_USER_MANAGEMENT_DISABLED=false
N8N_EMAIL_MODE=smtp
N8N_SMTP_HOST=smtp.gmail.com
N8N_SMTP_PORT=587
N8N_SMTP_USER=your-email@gmail.com
N8N_SMTP_PASS=your-app-password
N8N_SMTP_SENDER=noreply@yourdomain.com
```

### B. 자주 사용하는 표현식

```javascript
// 날짜 및 시간
{{ $now }} // 현재 시간
{{ $today }} // 오늘 날짜
{{ $now.minus({days: 7}) }} // 7일 전
{{ $now.plus({hours: 1}) }} // 1시간 후
{{ $now.toFormat('yyyy-MM-dd HH:mm:ss') }} // 포맷팅

// 문자열 조작
{{ $json.text.toLowerCase() }} // 소문자 변환
{{ $json.text.toUpperCase() }} // 대문자 변환
{{ $json.text.trim() }} // 공백 제거
{{ $json.text.replace('old', 'new') }} // 치환
{{ $json.text.split(',') }} // 분할
{{ $json.email.split('@')[1] }} // 도메인 추출

// 배열 처리
{{ $json.items.length }} // 길이
{{ $json.items[0] }} // 첫 번째 요소
{{ $json.items.filter(i => i.price > 100) }} // 필터링
{{ $json.items.map(i => i.name) }} // 매핑
{{ $json.items.reduce((sum, i) => sum + i.price, 0) }} // 합계

// 객체 처리
{{ Object.keys($json) }} // 키 목록
{{ Object.values($json) }} // 값 목록
{{ Object.entries($json) }} // 키-값 쌍

// 조건부
{{ $json.amount > 1000 ? "high" : "low" }} // 삼항 연산자
{{ $json.status === "active" && $json.verified }} // AND
{{ $json.type === "A" || $json.type === "B" }} // OR

// JSON 파싱
{{ JSON.parse($json.stringData) }}
{{ JSON.stringify($json.objectData) }}

// 수학
{{ Math.round($json.value) }} // 반올림
{{ Math.floor($json.value) }} // 내림
{{ Math.ceil($json.value) }} // 올림
{{ Math.max(...$json.numbers) }} // 최댓값
{{ Math.min(...$json.numbers) }} // 최솟값
{{ Math.random() }} // 랜덤 (0-1)

// 노드 참조
{{ $node["Node Name"].json.field }} // 특정 노드 데이터
{{ $node["Node Name"].parameter.field }} // 노드 파라미터

// 워크플로우 메타데이터
{{ $workflow.id }} // 워크플로우 ID
{{ $workflow.name }} // 워크플로우 이름
{{ $execution.id }} // 실행 ID
{{ $execution.mode }} // 실행 모드
{{ $itemIndex }} // 현재 아이템 인덱스

// 환경 변수
{{ $env.MY_VARIABLE }} // 환경 변수 참조
```

### C. API 레퍼런스

**워크플로우 목록 조회:**

```bash
curl -X GET http://localhost:5678/api/v1/workflows \
  -H "X-N8N-API-KEY: your-api-key"
```

**워크플로우 실행:**

```bash
curl -X POST http://localhost:5678/api/v1/workflows/:id/execute \
  -H "X-N8N-API-KEY: your-api-key" \
  -H "Content-Type: application/json" \
  -d '{"data": {"key": "value"}}'
```

**실행 결과 조회:**

```bash
curl -X GET http://localhost:5678/api/v1/executions/:id \
  -H "X-N8N-API-KEY: your-api-key"
```

**워크플로우 생성:**

```bash
curl -X POST http://localhost:5678/api/v1/workflows \
  -H "X-N8N-API-KEY: your-api-key" \
  -H "Content-Type: application/json" \
  -d @workflow.json
```

### D. 성능 벤치마크

**테스트 환경:**
- AWS EC2 t3.medium (2 vCPU, 4GB RAM)
- PostgreSQL 15
- N8N 1.x

**결과:**

| 워크플로우 타입 | 처리량 (req/sec) | 평균 응답 시간 (ms) |
|---------------|-----------------|-------------------|
| 간단한 웹훅 | 500 | 20 |
| HTTP + 데이터 변환 | 200 | 50 |
| 데이터베이스 쓰기 | 150 | 67 |
| 복잡한 로직 (10+ 노드) | 50 | 200 |
| 외부 API 호출 | 20-100 | 100-500 (API 의존) |

**스케일링 권장사항:**
- < 100 req/sec: 단일 인스턴스
- 100-500 req/sec: Queue 모드 + 다중 워커
- > 500 req/sec: 로드 밸런서 + 여러 N8N 인스턴스


