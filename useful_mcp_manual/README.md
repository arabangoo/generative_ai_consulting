## [유용한 MCP 서버 모음]

[Desktop Commander MCP 서버]   
(1) 개요       
Desktop Commander는 로컬 PC 환경에서 파일 시스템 작업, 터미널 명령 실행, 프로세스 관리 등을 자동화할 수 있는 MCP 서버다.    
Claude가 직접 사용자의 컴퓨터와 상호작용할 수 있게 해주는 강력한 도구다.        
<br/>

(2) 경로   
https://github.com/wonderwhy-er/DesktopCommanderMCP   
![image1](https://github.com/user-attachments/assets/aaaa76d1-eaf6-4e80-a564-206a78f307cf)
<br/><br/>

(3) 활용 가능 작업   
1. 파일 시스템 관리      
- 프로젝트 디렉토리 구조 분석 및 정리   
- 로그 파일 모니터링 및 분석   
- 설정 파일 자동 생성 및 수정   
- 대량 파일 이름 변경 및 정리   

2. 개발 작업 자동화   
- Git 저장소 상태 확인 및 커밋   
- 프로젝트 빌드 및 테스트 실행   
- 환경 설정 스크립트 생성   
- 코드 품질 검사 도구 실행   

3. 시스템 모니터링   
- CPU, 메모리 사용량 확인   
- 실행 중인 프로세스 관리   
- 서비스 상태 점검   
- 디스크 사용량 분석
<br/> 

(4) MCP 설정   
```txt  
{
  "mcpServers": {
    "desktop-commander": {
      "command": "npx",
      "args": [
        "-y",
        "@wonderwhy-er/desktop-commander"
      ]
    }
  }
}
```
<br/>

---
<br/>
   
[Playwright MCP 서버]   
(1) 개요      
Playwright MCP 서버는 웹 자동화를 통해 웹사이트 상호작용, 데이터 수집, 스크린샷 캡처 등을 수행할 수 있는 도구다.    
Claude가 실제 브라우저를 조작하여 웹 작업을 자동화할 수 있게 해준다.          
<br/>

(2) 경로         
https://github.com/microsoft/playwright-mcp            
<img width="1200" height="800" alt="image2" src="https://github.com/user-attachments/assets/f6e8c504-5a5f-4563-b4ec-9182d7f5c746" />
<br/><br/>

(3) 활용 가능 작업   
1. 웹 자동화   
- 웹사이트 상태 모니터링   
- 폼 자동 입력 및 제출   
- 로그인 자동화   
- 반복적인 웹 작업 스케줄링   

2. 데이터 수집   
- 뉴스 사이트 헤드라인 수집   
- 경쟁사 가격 정보 모니터링   
- 소셜 미디어 트렌드 분석   
- API 문서 자동 스크래핑   

3. 품질 보증   
- 웹사이트 시각적 회귀 테스트   
- 다양한 브라우저 호환성 확인   
- 성능 측정 및 보고서 생성   
- 사용자 경험 테스트 자동화      
<br/>

(4) MCP 설정      
```txt  
{
  "mcpServers": {
    "playwright": {
      "command": "npx",
      "args": [
        "@playwright/mcp@latest"
      ]
    }
  }
}
```
<br/>

(5) 주의사항      
1. 최초 실행 전 npx playwright install 명령으로 브라우저 설치 필요   
2. 시스템 리소스 사용량이 높을 수 있음   
<br/>

---
<br/>

[AWS API MCP 서버]      
(1) 개요         
AWS API MCP 서버는 MCP 클라이언트가 AWS CLI 명령을 통해 AWS 서비스 및 리소스와 상호작용할 수 있도록 하는 MCP 서버다.            
<br/>

(2) 경로   
https://github.com/awslabs/mcp/tree/main/src/aws-api-mcp-server               
<img width="1200" height="800" alt="image3" src="https://github.com/user-attachments/assets/b9b4e500-89be-4a5f-8c4d-9d30561061ee" />
<br/><br/>

(3) 활용 가능 작업   
1. 인프라 관리   
- EC2 인스턴스 생성, 관리, 모니터링   
- VPC, 서브넷, 보안 그룹 설정   
- Auto Scaling 그룹 구성   
- ELB/ALB 로드 밸런서 관리   

2. 스토리지 서비스   
- S3 버킷 생성 및 객체 관리   
- EBS 볼륨 생성 및 연결   
- EFS 파일 시스템 구성   
- 백업 및 스냅샷 자동화   

3. 데이터베이스 관리   
- RDS 인스턴스 생성 및 관리   
- DynamoDB 테이블 운영   
- Aurora 클러스터 구성   
- 데이터베이스 백업 및 복원   

4. 컨테이너 및 서버리스      
- ECS/EKS 클러스터 관리   
- Lambda 함수 배포 및 관리   
- Fargate 서비스 운영   
- API Gateway 구성   

5. 모니터링 및 로깅   
- CloudWatch 메트릭 및 알람 설정   
- CloudTrail 로그 분석   
- Systems Manager 활용   
- X-Ray 추적 설정       
<br/>

(4) MCP 설정 (서울 리전 대상)        
```txt  
{
  "mcpServers": {
    "awslabs.aws-api-mcp-server": {
      "command": "python",
      "args": [
        "-m",
        "awslabs.aws_api_mcp_server.server"
      ],
      "env": {
        "AWS_REGION": "ap-northeast-2",
        "AWS_API_MCP_PROFILE_NAME": "default",
        "READ_OPERATIONS_ONLY": "false"
      },
      "disabled": false,
      "autoApprove": []
    }
  }
}
```
<br/>

(5) 주의사항      
1. python 활용 버전으로 json 등록 전 "pip install awslabs.aws-api-mcp-server" 명령어 실행 필요   
<img width="1200" height="800" alt="image4" src="https://github.com/user-attachments/assets/c0605424-38b0-44c3-b53b-d9c0902aca66" />
<br/>

---
<br/>

[유용한 MCP 서버 목록 정리]         
1. Figma MCP : AI 코딩 도구들이 Figma 디자인 데이터를 직접 액세스할 수 있도록 지원한다.    
   이를 통해 디자인을 보다 정확하게 구현할 수 있다.   
   링크 - https://github.com/GLips/Figma-Context-MCP
<br/> 
           
2. Blender MCP : Blender와 Claude AI를 연결하여 자연어 명령을 통해    
   3D 모델을 생성, 수정, 향상시킬 수 있도록 한다.   
   링크 - https://github.com/ahujasid/blender-mcp
<br/>
               
3. Ableton MCP : LLM과 Ableton Live 간의 통신을 가능하게 하여   
   자연어로 음악 트랙 생성, 클립 조작, 세션 컨트롤 등을 수행할 수 있다.   
   링크 - https://github.com/Simon-Kansara/ableton-live-mcp-server
<br/>
         
4. Firecrwal MCP : 웹 스크래핑 기능을 AI 어시스턴트에 통합하여   
   다양한 웹 페이지의 데이터를 추출하고 검색할 수 있도록 한다.   
   링크 - https://github.com/mendableai/firecrawl-mcp-server
<br/>
                 
5. Youtube MCP :  AI 모델이 YouTube 콘텐츠와 상호작용할 수 있도록 하여    
   비디오 정보 검색, 채널 비디오 목록화, 비디오 통계 조회 등을 지원한다.   
   링크 - https://github.com/ZubeidHendricks/youtube-mcp-server
<br/>
               
6. Perplexity MCP : Sonar API와 통합되어 AI 어시스턴트가 실시간 웹 검색을 수행하고   
   최신 정보를 제공할 수 있도록 한다.   
   링크 - https://docs.perplexity.ai/guides/mcp-server
<br/>
             
7. Suparbase MCP : AI 도구들이 Supabase 플랫폼과 상호작용하여 데이터베이스 생성,    
   테이블 관리, 데이터 쿼리 등을 수행할 수 있도록 한다.   
   링크 - https://supabase.com/blog/mcp-server
<br/>
           
8. Tripo MCP : AI 어시스턴트와 Tripo AI를 연결하여 자연어 명령을 통해    
   3D 에셋을 생성하고 Blender에 직접 가져올 수 있도록 한다.   
   링크 - https://github.com/VAST-AI-Research/tripo-mcp
<br/>
              
9. Unity MCP : Unity MCP 서버는 Unity와 AI 모델 간의 양방향 통신을 가능하게 하여   
   에셋 관리, 씬 컨트롤, 스크립트 통합 등 다양한 작업을 자동화할 수 있다.   
   링크 - https://github.com/justinpbarnett/unity-mcp
<br/>
                  
10. Magic MCP : Magic Component Platform(MCP)은 개발자들이 자연어 설명을 통해    
    현대적인 UI 컴포넌트를 즉시 생성할 수 있도록 지원하는 AI 기반 도구이다.   
    링크 - https://github.com/21st-dev/magic-mcp
<br/>
                
11. Sequential thinking MCP : 복잡한 문제를 단계별로 분석하고 해결할 수 있도록    
    AI 모델에 구조화된 사고 프로세스를 제공한다.   
    링크 - https://github.com/modelcontextprotocol/servers/tree/main/src/sequentialthinking
<br/>
                
12. FileSystem MCP : AI 모델이 파일 시스템과 상호작용하여    
    파일 읽기, 쓰기, 디렉토리 생성 등 다양한 파일 작업을 수행할 수 있도록 한다.   
    링크 - https://github.com/modelcontextprotocol/servers/tree/main/src/filesystem      

