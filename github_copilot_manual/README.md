## [AI 코드 어시스턴트 GitHub Copilot]

GitHub Copilot은 Github와 OpenAI가 공동 개발한 인공지능 기반 코드 작성 도구이다.   
주석이나 함수 이름에 담긴 의미를 파악하여 코드를 자동 완성하며   
개발자가 코드 작성 시 효율적으로 작업할 수 있도록 도움을 준다.    
      
GitHub Copilot은 다음과 같은 특징이 있다.   
(1) 비주얼 스튜디오 코드와 같은 개발 환경에서 사용한다.   
(2) 자바스크립트, 파이썬, 자바, C언어 등 다양한 언어를 지원한다.   
(3) 함수나 변수를 만들면 관련된 코드를 자동으로 제안한다.    
(4) 주석을 작성하면 해당 설명에 맞는 코드를 생성할 수 있다.   
(5) Github 리포지토리의 파일 경로 검색, 지식 기반 검색, 웹 검색 등의 기능을 제공한다.    
<br/>
   
1. GitHub Copilot 설치   
   
(1) GitHub Copilot 공식 웹사이트 주소는 아래와 같다.   
해당 웹사이트를 통해 VS Code에 설치할 수도 있다.   
https://github.com/features/copilot   
![image1](https://github.com/user-attachments/assets/7333e32b-567f-41ef-b750-3b48d60340a1)
![image2](https://github.com/user-attachments/assets/f2b357f9-bc03-4302-a7f3-86263df31322)
![image3](https://github.com/user-attachments/assets/2b6fb806-b282-4e69-843e-615eff58532d)
<br/>
        
(2) VS Code 내에서 확장 프로그램으로 설치할 수도 있다.      
![image4](https://github.com/user-attachments/assets/db662561-e9b9-4854-a2ac-fec10f1cb5a9)
<br/> 
                
2. GitHub Copilot 활용   
      
(1) GitHub Copilot 설치가 완료되면 Ctrl+Shift+I 단축키로 채팅창을 연다.   
이 채팅창에서 ChatGPT나 클로드와 대화하듯이 해당 파일 내용에 대해 AI와 대화를 나눌 수 있다.   
또한 AI 기능을 통해 코드를 개선하거나 편집할 수 있다.                
![image5](https://github.com/user-attachments/assets/fe57e2d8-fb71-41c0-a9b2-d5a43a1bfa0a)
<br/>
           
(2) GitHub Copilot 채팅창의 내용은 이미지와 같이 구성된다.          
![image6](https://github.com/user-attachments/assets/1749c273-88cb-4159-a6f3-9e00514de7a2)
<br/>
                                  
(3) 기획서 내용을 바탕으로 코드를 제안해달라고 AI에게 요청할 수도 있다.       
다음은 자바 코딩 예제를 작성해 달라고 AI에게 요청한 것이다.         
![image7](https://github.com/user-attachments/assets/279c3e3c-273e-4317-9236-d1e4f27a209e)
![image8](https://github.com/user-attachments/assets/e17b62be-1baf-4b8b-9a1c-807ef6c61382)
![image9](https://github.com/user-attachments/assets/1a52afe5-d85c-4405-a354-3aaeb5751e16)
<br/>
   
(4) 인라인 챗(Inline Chat)은 소스 코드가 있는 영역에서 AI를 불러 사용하는 기능이다.       
커서가 있는 곳, 또는 선택한 코드에 대한 요청을 할 수 있다.       
작업 흐름을 유지하면서 코파일럿의 도움을 받을 수 있어 유용하다.       
단축키 Ctrl+I를 눌러 호출하면 된다.       
다음은 인라인 챗에 요청해 얻은 소스 코드가 미리보기되는 모습이다.       
[수락]을 클릭해야 코드에 반영되며 [취소]를 누르면 반영되지 않는다.      
![image10](https://github.com/user-attachments/assets/30614188-44a2-431a-822c-4e9821a62a34)
<br/>

3. GitHub Copilot 예약어
      
(1) 채팅 참가자(@로 시작하는 예약어)   
채팅 참가자는 사용자에게 관련도 높은 답변을 제공할 수 있도록 질문의 범위와 의도를 표현하는 기능이다.    
해당 분야의 전문가와 함께 대화한다고 생각하면 쉽다.    
현재 아래와 같은 참여자를 언급할 수 있다.     
- @workspace : 작업 공간의 코드에 대한 컨텍스트를 제공하며 이를 탐색하고 관련 파일이나 클래스를 찾는 데 도움이 될 수 있다.    
- @vscode : VS Code 편집기 자체의 명령과 기능에 대해 알고 있으며, 이를 사용하는 데 도움을 줄 수 있다.    
- @terminal : 통합 터미널 셸, 버퍼 및 현재 선택에 대한 질문을 할 수 있다.       
![image11](https://github.com/user-attachments/assets/6942d760-8e34-44e6-8c69-2006b66adc39)
<br/>
      
(2) 슬래시 명령(/로 시작하는 예약어)    
슬래시 명령은 GitHub Copilot 채팅에서 의도를 정확히 하는 데에 도움이 된다.     
예를 들어, Java 새 프로젝트를 만들고 싶다면 @workspace /new Java 라고 작성할 수 있다.   
- /help : GitHub Copilot 사용에 대한 도움말 받기   
- /doc : 코드 문서 생성   
- /clear : 새로운 채팅 세션을 시작하기   
- @workspace /explain : 선택된 코드가 어떻게 작동하는지 설명 받기   
- @workspace /fix : 선택된 코드의 문제에 대한 해결책을 제안 받기   
- @workspace /tests : 선택된 코드에 대한 단위 테스트를 생성하기   
- @workspace /new : 새 작업 공간 또는 새 파일에 대한 스캐폴드 코드를 생성하기   
- @workspace /newNotebook : 새 Jupyter Notebook을 만들기   
- @vscode /api : VS Code 확장 개발에 대해 질문하기   
- @vscode /search : 검색 보기에 대한 쿼리 매개변수 생성하기   
- @vscode /runCommand : VS Code 명령 검색 또는 실행하기   
- @terminal /explain : 터미널 기능이나 셸 명령을 설명하기           
![image12](https://github.com/user-attachments/assets/6eed12f7-b245-4cca-abea-8a0a1678a1de)
<br/>
   
(3) 채팅 컨텍스트(채팅 변수)    
@workspace 또는 @vscode와 같은 채팅 참여자들은 도메인별 컨텍스트를 포함한 채팅 변수를 제공할 수 있다.    
채팅 프롬프트에서 # 기호를 사용하여 채팅 변수를 참조할 수 있다.   
- #codebase : 현재 워크스페이스에 대한 내용을 포함하기    
- #editor : 활성된 에디터의 코드   
- #file : 채팅 프롬프트에 컨텍스트로 작업 공간에 지정된 파일을 포함하기   
- #git : 현재 git 저장소에 대한 정보   
- #selection : 활성된 에디터에서 볼 수 있는 소스 코드   
- #terminalLastCommand : 활성된 터미널의 마지막 실행 명령   
- #terminalSelection : 활성된 터미널 선택   
![image13](https://github.com/user-attachments/assets/325387e3-c448-4db8-a00e-da6612f3a702)  
<br/>
   
---
<br/>
   
GitHub Copilot Docs : https://docs.github.com/ko/copilot         
<img width="2000" alt="image14" src="https://github.com/user-attachments/assets/8f1eba14-38f8-49a5-a3bd-3abc12cff736" />
<br/>
     
Microsoft Learn Challenge : https://learn.microsoft.com/ko-kr/visualstudio/ide/visual-studio-github-copilot-install-and-states?view=vs-2022      
![image15](https://github.com/user-attachments/assets/0d4de954-4c3d-4539-9430-e6954a330ccd)
      
