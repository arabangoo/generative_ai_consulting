## [MCP(Model Context Protocol) 개념 및 활용]

(1) MCP란 무엇인가?   
MCP는 AI 모델이 외부 데이터 및 도구와 상호 작용할 수 있도록 설계된 표준 프로토콜이다.   
이를 통해 AI는 기존 API 방식보다 더 유연하고 효율적인 방식으로 데이터를 가져오고, 명령을 실행할 수 있다.   
쉽게 말해, MCP는 AI 모델을 위한 통합 인터페이스다.    
MCP는 다양한 AI 모델이 다양한 데이터 소스와 도구에 표준화된 방식으로 연결할 수 있게 한다.    
기존 API 방식에서는 개발자가 각각의 API를 직접 구현해야 했지만,    
MCP를 사용하면 AI 모델이 동적으로 데이터를 검색하고, 필요한 정보를 가져올 수 있도록 표준화된 방식을 제공한다.     
<br/>

![image_1](https://github.com/user-attachments/assets/b203c5f9-0509-4978-9264-6e27e019ccb4)
<br/>

앤트로픽 MCP 소개 - https://www.anthropic.com/news/model-context-protocol   
![image_2](https://github.com/user-attachments/assets/14d555b0-5267-4fa4-b7c7-4ebdfcf93989)
<br/>

MCP와 기존 API 방식 비교   
<img width="900" alt="image_3" src="https://github.com/user-attachments/assets/c8234507-3749-40cf-998e-61dde909d195" />

---
<br/>
   
(2) MCP의 작동 방식    
MCP는 크게 MCP 호스트(Host), MCP 서버(Server), MCP 클라이언트(Client)로 구성된다.   
      
[MCP의 주요 구성 요소]      
1. MCP 호스트 → AI 모델을 실행하는 애플리케이션 (예: Claude, ChatGPT)      
<img width="500" alt="image_4" src="https://github.com/user-attachments/assets/38ab667a-3219-4c09-8350-f946670d500e" />
<br/><br/>
                 
2. MCP 클라이언트 → MCP 서버와 연결되어 데이터를 주고받는 역할                  
<img width="700" alt="image_5" src="https://github.com/user-attachments/assets/9c1e21b8-3923-4757-b72e-96a46a3a7b13" />
<br/><br/>

3. MCP 서버 → 특정 기능을 제공하는 서비스 (예: 일정 관리, 이메일 전송 등)         
<img width="600" alt="image_6" src="https://github.com/user-attachments/assets/b05578d9-f26f-42d9-8595-9a39ed33dd2a" />
<br/><br/>
                                  
4. 데이터 소스 → AI 모델이 접근할 수 있는 로컬 파일, 데이터베이스, 외부 API   
<img width="800" alt="image_7" src="https://github.com/user-attachments/assets/dc341965-62ed-4997-9d75-08a6616e1286" />   
<img width="1200" alt="image_8" src="https://github.com/user-attachments/assets/17fe0e65-0533-4f26-b2bb-7002ac3f2c7b" />
   
---
<br/>
   
(3) MCP 서버 리포지토리      
1. smithery 웹사이트 - https://smithery.ai/             
<img width="1600" alt="image_9" src="https://github.com/user-attachments/assets/a067feb4-6859-4793-9a92-5a865d7c997b" />
<br/><br/>

2. glama 웹사이트 - https://glama.ai/mcp/servers      
<img width="1200" alt="image_10" src="https://github.com/user-attachments/assets/9800f1bd-6d29-4f58-ad0b-58e79028fb39" />
<br/><br/>

3. pulsemcp 웹사이트 - https://www.pulsemcp.com/
<img width="1400" alt="image_11" src="https://github.com/user-attachments/assets/221efa40-26c7-4f91-abbe-c8d1db4340c8" />
<br/><br/>

---
<br/>
   
(4) MCP 구조와 기능      
[MCP 구조]      
우리는 chatgpt, claude, grok3, gemini와 같은 LLM 서비스를 이용하거나       
LLM 모델이 탑재된 cursor와 같은 AI 도구를 이용하게 되며 이러한 도구들을 MCP Client라고 한다.       
MCP Client들은 특정 프로그램과 연결하여 데이터를 조회하거나 특정 액션의 수행을 요청한다.       
그러기 위해서는 MCP Client는 특정 프로그램과 연결을 해야하는데 직접 연결할 수 없으니       
특정 프로그램을 제어할 수 있는 서버를 중간에 두고 MCP Client는 특정 프로그램을 제어할 수 있는 서버와 연결한다.       
여기서 해당 서버를 MCP Server라고 표현한다.       
이때 MCP Client와 MCP Server간 데이터를 주고받는 방법이 MCP다.      
그리고 MCP Client의 claude는 웹 기반 claude가 아닌 Claude Desktop이다.      
![image_12](https://github.com/user-attachments/assets/46cb8d3d-35f3-42d8-99b0-722a98867a82)
<br/>
   
[MCP의 대표적 기능]    
MCP는 기본적으로 4개의 기능을 가진다.    
resources, prompts, tools, sampling이 그것이다.   

1. resources : MCP 서버가 데이터베이스나 파일과 같은 리소스를 클라이언트에게 제공하는 것을 의미한다. 
   이를 통해 클라이언트는 필요한 데이터를 직접 액세스할 수 있습니다.​
   
3. prompts : MCP 서버가 재사용 가능한 프롬프트 템플릿을 클라이언트에게 제공하는 것을 의미한다. 
   이러한 템플릿은 일관된 메시지 시퀀스와 워크플로우를 정의하여, 언어 모델의 동작을 예측 가능하게 안내한다.
   
5. tools : MCP 서버가 클라이언트에게 실행 가능한 기능을 노출하는 것을 의미하며, 
   이를 통해 클라이언트는 외부 시스템과 상호작용하거나 특정 작업을 수행할 수 있다. 
   예를 들어, 서버는 데이터베이스 업데이트나 원격 시스템 액세스와 같은 기능을 도구로 제공할 수 있다. ​
   
7. sampling : MCP 서버가 클라이언트를 통해 대형 언어 모델(LLM)의 출력을 요청하는 기능을 의미한다. 
   이를 통해 서버는 클라이언트의 LLM을 활용하여 복잡한 작업을 수행할 수 있으며, 
   클라이언트는 이러한 요청을 검토하고 승인함으로써 보안과 프라이버시를 유지할 수 있다.
<img width="700" alt="image_13" src="https://github.com/user-attachments/assets/121046fc-0020-44ae-b73a-6c8316ab1f4a" />   
<img width="700" alt="image_14" src="https://github.com/user-attachments/assets/7913da5a-5974-4255-bf5e-bca4763b7aa1" />

---
<br/>

(5) MCP 사용 가이드   

[클로드 데스크탑에서 MCP 사용]   
1. 클로드 데스크탑을 다운로드 한다.   
다운로드 링크 : https://claude.ai/download   
<br/>

2. "파일 -> 설정 -> 개발자" 경로에서 MCP를 설정할 수 있다.
![image_15](https://github.com/user-attachments/assets/e736e217-b1bf-4e8e-9639-000f77f26ecf)   
<img width="1000" alt="image_16" src="https://github.com/user-attachments/assets/531037b7-3bae-40c2-a9a4-2830757c9974" />
<br/><br/>   
   
3. 현재 smithery.ai와 같은 MCP 서버 사이트에서 원하는 MCP 서버를 선택한다.         
smithery.ai에는 벌써 3천여개 가까운 MCP 서버가 등록되어 있다.         
로컬 데이터 처리, 명령어 실행 모드, 웹 브라우저 자동화 같은 로컬 머신 기능에다가          
웹 검색, 외부 API 연동 같은 다양한 MCP 서버가 만들어져 있다.         
                     
여기서는 일단 Desktop Commander라는 MCP 서버를 선택하도록 하겠다.         

"Claude" 항목을 선택 후 "npm"이라는 항목에서 인스톨 명령어를 확인한 후 cmd창에서 실행한다.      
<img width="800" alt="image_17" src="https://github.com/user-attachments/assets/d607a881-409a-4691-805d-3ed6f127f4ab" />    
<img width="1000" alt="image_18" src="https://github.com/user-attachments/assets/c3e3a3e0-add6-4512-aa06-5f7153d66e8e" />   
<br/>
다른 방법으로는 JSON 내용을 복사한 후 클로드의 설정 편집을 클릭하자.   
<img width="800" alt="image_19" src="https://github.com/user-attachments/assets/b91beadd-4547-48fc-8757-78d75ddebd0c" />
<img width="800" alt="image_20" src="https://github.com/user-attachments/assets/c8bce7ec-99b5-45a6-94ed-612e9b672506" />
<br/>

클로드 폴더가 열리고 "claude_desktop_config.json" 파일이 확인되면       
메모장으로 파일을 열어 json 내용을 기입한다.          
<img width="800" alt="image_21" src="https://github.com/user-attachments/assets/6d1475c6-4332-49f2-a91b-b438ccef2765" />
<br/>

위에서 설명한 방법 두 개 중 하나만 수행하면 되고 중복해서 수행하는 일은 없도록 하자.   
                
일단 위와 같이 MCP 서버를 적용하면 클로드 데스크탑은 다시 재시작을 해야한다.   
그러면 아래와 같이 망치 아이콘이 확인될 텐데 MCP 서버가 적용되었다는 뜻이다.    
<img width="700" alt="image_22" src="https://github.com/user-attachments/assets/299cb835-5586-4702-b602-5c449692ee25" />
<br/>

프롬프트를 기입하면 MCP 서버를 허용하겠냐는 팝업창이 뜨는데 허용해주면 된다.   
올바른 프롬프트만 기입하면 MCP 서버가 실행되고 사용자의 지시를 따르기 위해 OS 명령어를 실행한다.        
그리고 잠시 기다리면 클로드 데스크탑에 응답 결과를 표시하기 시작한다.    
<img width="800" alt="image_23" src="https://github.com/user-attachments/assets/6afb7dc6-dd2d-4446-a42c-ed4635e9b3af" />
<br/><br/>  

[커서에서 MCP 사용]   
1. Cursor는 아래 사이트 링크에서 다운로드 할 수 있다.   
다운로드 링크 : https://www.cursor.com/
![image_24](https://github.com/user-attachments/assets/c6c5e6cd-8520-4b27-8cf8-297e4c3f0356)
<br/>

2. 설치 파일을 실행하고 나서 아래와 같이 옵션을 설정한다.      
설치시 CommandLine은 Installed "cursor"를 선택하면 된다.      
![image_25](https://github.com/user-attachments/assets/c3b9c05f-2d5d-446a-9f4e-f5354622c2d5)   
![image_26](https://github.com/user-attachments/assets/8ba36b45-4d90-43cb-b424-6bb79e7b283a)
<br/>

3. VSCode 확장 프로그램 연동이 되기까지 시간이 걸리니 기다린다.       
![image_27](https://github.com/user-attachments/assets/fc6916e0-e7ce-481e-add6-80c98470c59d)
<br/>

4. 다음은 Cursor 인공지능을 활용할 것이기에 "Help Improve Cursor" 옵션을 선택한다.   
![image_28](https://github.com/user-attachments/assets/c4615300-c4fe-47ba-aaf1-2df62973f438)
<br/>

5. 마지막으로 Cursor 로그인까지 완료하면 VSCode와 비슷한 화면이 뜨는 것을 확인할 수 있다.      
이제부터는 VSCode가 아닌 인공지능 기반 코드 에디터 Cursor를 통해 코드 개발을 할 수 있다.   
![image_29](https://github.com/user-attachments/assets/4c8dadce-62a7-4056-a52f-ce3e16ac174a)
<br/>

6. 좌측 상단을 보면 VSCode와 연동된 확장 프로그램과 설치 가능한 확장 프로그램 목록을 확인할 수 있다.   
![image_30](https://github.com/user-attachments/assets/6b074335-5596-4522-ac35-a629a446268d)
<br/>

7. "File -> Preferences -> Cursor Settings" 경로에서 MCP 서버 설정을 할 수 있다.      
<img width="800" alt="image_31" src="https://github.com/user-attachments/assets/eedc6250-0d4d-411e-9254-80daf275f60a" />
<img width="800" alt="image_32" src="https://github.com/user-attachments/assets/3edeabe7-9446-4ce6-8865-1c2a2d466eda" />   
<br/><br/>   
   
smithery.ai 사이트에서 Desktop Commander라는 MCP 서버를 선택하도록 하겠다.            
Cursor 항목을 선택하고 JSON 내용을 복사한다.            
<img width="1000" alt="image_33" src="https://github.com/user-attachments/assets/cf74201d-ad30-476a-9f7b-db9443593776" />
<img width="600" alt="image_34" src="https://github.com/user-attachments/assets/0fee4fa4-78c6-462a-b23b-bdcc7a91e077" />
<br/>

MCP 서버가 제대로 적용이 된 것을 확인한 후 "ctrl+k" 단축키로 AI 채팅창을 띄우고 AI와 소통하며 MCP 서버가 작동하는 것을 확인한다.    
<img width="1600" alt="image_35" src="https://github.com/user-attachments/assets/44cffb7f-8e99-45f9-a17c-6edd7e2b8138" />
      
