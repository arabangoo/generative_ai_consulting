## [VSCode에서 AWS CodeWhisperer, ChatGPT 사용]

1. VSCode 설치하기
     
(1) 먼저 아래 공식 사이트 다운로드 페이지에 들어가도록 한다.   
<br/>
VSCode 공식 다운로드 웹페이지 : https://code.visualstudio.com/Download   

각자 사용하는 OS에 맞는 버전을 다운로드 받아 설치해준다.   
<br/> 
                
(2) 설치창에서 아래 세 가지 항목은 가능한 체크하도록 한다.              
![image1](https://github.com/user-attachments/assets/7ef8af05-39f4-4fe9-b6f1-a92f630882a7)
<br/><br/>
        
(3) 설치가 완료되면 VSCode를 실행한 후 Ctrl + Shift + P 키를 눌러 나타나는 커맨드 창에 language를 입력한다.   
커맨드창 아래에 아래와 같이 항목들이 나타나면 Configure Display Language를 선택하고 한국어 언어팩을 설치한다.      
그 이후 VSCode를 재실행하면 한국어 언어팩이 적용된다.      
![image2](https://github.com/user-attachments/assets/1b6448ca-92fd-42f8-a31d-59c085e94032)
<br/><br/>
                               
(4) 표시 언어를 다시 영어로 바꾸고 싶다면 동일하게 커맨드 창에 language를 입력한다.   
그 다음 Configure Display Language를 선택하고 English (en) 를 선택하면 된다.   
![image3](https://github.com/user-attachments/assets/25afb51b-05ba-4535-be40-bd36f03da20d)
<br/><br/>

---
<br/>

2. VSCode에 AWS CodeWhisperer 사용하기     
CodeWhisperer(이하 코드위스퍼러)는 AWS에서 제공하는 코딩 어시스턴스이다.       
지원하는 언어는 파이썬, 자바, 자바스크립트, 타이프스크립트, C#, PHP, SQL 등으로       
개인은 무료로 사용이 가능하며 vscode나 pycharm 같은 코드 편집기에 설치하여 사용할 수 있다.      
사용자가 코드를 작성할 때 사용할만한 코드를 제안해 주는 형태이기 때문에       
코딩에 대해서 아예 모르는 사용자들은 큰 도움을 받지 못할 수 있지만       
코딩을 학습하는 사용자나 어느 정도 코딩이 가능한 사용자에게는 생각보다 큰 도움이 될 것이다.      

(1) 코드위스퍼러는 AWS Toolkit에 포함되어 있어 사용하기 위해서는 이를 설치해야 한다.      
VSCode 왼쪽에 확장(Extensions)을 클릭 후 [aws tookit]을 검색하고 설치(install) 버튼을 눌러 설치를 진행하면 된다.   
![image4](https://github.com/user-attachments/assets/585ccd1f-ec3a-4631-9660-f2cc4469b1f3)
<br/>

(2) 설치가 끝나면 왼쪽에 [aws]가 생겼을 것이다.      
이를 클릭하고 [CodeWhisperer]의 하단에 [Start]를 눌러보자.      
그 다음, 커맨드창에 나타나는 "Use a personal email to sign up and sign in with AWS Builder ID"를 클릭한다.      
팝업창이 뜨면 "Copy Code for AWS Builder ID"를 클릭해준다.      
![image5](https://github.com/user-attachments/assets/d07df50c-92fb-40f8-87d1-dd189824bdcb)
![image6](https://github.com/user-attachments/assets/b75d1b07-5dce-44d6-a3c1-a365a2d0c410)
<br/>

(3) 코드 등록을 위한 사이트에 방문할 것인지 물으면 "열기(Open)" 버튼을 클릭한다.   
사이트가 나타나면 방금 전 복사한 코드를 붙여 넣고 "Next"를 누르자.   
![image7](https://github.com/user-attachments/assets/0e9c8122-ceaf-4714-b3f7-1f6a47450403)
<br/>

(4)  코드를 입력하면 코드 등록을 위한 AWS Builder ID를 생성해야 한다.   
ID 생성은 이메일 주소를 넣고 이름 입력, 이메일 코드 확인, 사용할 비밀번호를 입력하면 된다.   
(이미 AWS Builder ID가 있다면 로그인을 하면 된다.)   
![image8](https://github.com/user-attachments/assets/71ebcc1a-a6ea-4e9d-8031-72e06621168c)
<br/>

(5) ID를 생성했거나 로그인을 했다면 마지막으로 VSCode에 접근 권한을 줄 것인지 물어보는데 [Allow] 버튼을 클릭하자.        
![image9](https://github.com/user-attachments/assets/ddbb6513-8368-4a57-89e6-30903428dcc2)
<br/>

(6) 별다른 문제가 없다면 아래와 같이 등록이 완료된다.   
(혹시나 권한 관련 문제가 발생한다면 VSCode에서 코드 발급부터 다시 진행하면 된다.)   
![image10](https://github.com/user-attachments/assets/c45df823-96f8-44a6-a894-65889a64f07b)
<br/>

(7) CodeWhisperer 사용 방법은 간단하다.   
CodeWhisperer의 Auto-Suggestions가 실행 중인 상태에서 코드를 입력하면 ai가 코드를 제안해 준다.   
이를 적용할 것이라면 [Tab]키를 누르면 되며 적용하지 않을 것이라면 무시하고 원하는 코드를 입력하면 된다.   
처음 제안한 코드를 무시하더라도 입력된 부분을 기준으로 계속 새로운 제안을 해준다.   
<br/>

추가 내용 :     
- 코드 제안 기능은 무료로 충분하다.   
CodeWhisperer은 유료 버전도 존재하긴 하지만 핵심이라고 볼 수 있는 코드 제안 기능은 무료로 사용해도 큰 차이가 없다고 한다.   
코파일럿의 사용이 망설여진다면 대신 CodeWhisperer를 사용해 보는 것도 좋지 않을까 한다.
<br/>

- 서비스 개선 사용 여부 확인   
CodeWhisperer의 설정을 보면 사용한 코드를 서비스 개선을 위해 공유 것인지 선택하는 부분이 있다.   
싫다면 설정을 확인해 보고 Share Code Whisperer Content With AWS 부분의 체크를 해제하면 된다.   
<br/>

---
<br/>

3. VSCode에서 ChatGPT 사용하기
   
(1) VSCode 실행 후 [확장 프로그램] 메뉴로 들어가서 검색창에서 "chatgpt"를 검색한다.       
ChatGPT 관련 여러 플러그인이 나올 텐데 여기서는 ChatGPT - Genie AI를 설치해주자.   
![image11](https://github.com/user-attachments/assets/376da129-bc59-411a-8e30-c2b785ac7f2c)
<br/>

(2) 설치가 끝나면 왼쪽에 램프 아이콘이 생겼을 것이다.          
이를 클릭하고 "Change API Key" 문구를 누르거나 "Ask a question"창에 아무 말이나 적으면 "API Key"를 입력하라고 뜬다.      
![image12](https://github.com/user-attachments/assets/9c812523-fdc3-48ff-85b2-26033c89859d)
<br/>
 
(3) OpenAI의 API키 발급 사이트에서 API키를 발급받는다.   
<br/>
OpenAI API Key 웹페이지 : https://platform.openai.com/settings/organization/api-keys   
![image13](https://github.com/user-attachments/assets/dfbc1117-e407-4d1e-8583-79843ee8ef48)
<br/>

(4) VSCode로 돌아와 API Key 등록칸에 키를 기입하고 엔터를 치면 API키가 등록이 된다.   
그 이후, "Ask a question"창에서 ChatGPT를 이용하는 방식으로 코드 관련 질문을 한다.   
ChatGPT가 답변해준 창에서 "+ New"를 클릭하면 코드가 오른쪽으로 복사되며 파일이 만들어진다.   
![image14](https://github.com/user-attachments/assets/045106d3-af9e-4cb8-8686-02749405c98e)
<br/>

(5) 여기서 추가적인 질문을 하면서 오른쪽 파일을 수정하며 코드를 작성해나가면 된다.      
![image15](https://github.com/user-attachments/assets/c904d56b-0a45-4b9f-9320-9729c09ce4ed)
<br/>

(6) VSCode에서 AWS 코드위스퍼러와 ChatGPT를 활용하여 본인만의 개발 코드를 만들어보자.   
물론 파이썬 등의 개발 코드에 관해서 기본 이론은 어느 정도 공부해둬야 한다.   
개발 코드 작성에 능숙해질수록 인프라 관리를 빠르게 자동화하거나 최적화 할 수 있다.   
