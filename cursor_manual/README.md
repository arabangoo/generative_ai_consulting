## [인공지능 기반 코드 에디터 Cursor IDE]

Cursor는 Anysphere라는 외국계 AI 소프트웨어 연구실에서 개발한 AI기반 코드 에디터이다.   
OpenAI가 지원하는 IDE이며 Visual Studio Code 에디터를 기반으로 만들어졌다.   
Cursor는 다음과 같은 특징들을 가지고 있다.   
(1) VS Code를 Forking 해서 만든 프로젝트   
(2) VS Code의 모든 기능 및 확장프로그램 사용가능   
(3) 코드베이스 인덱싱   
(4) privacy mode 지원   
![image1](https://github.com/user-attachments/assets/ecf67783-35d3-4f08-8f74-44e4f16bab53)
<br/>

1. Cursor 다운로드 및 설치
   
(1) Cursor는 아래 사이트 링크에서 다운로드 할 수 있다.   
https://www.cursor.com/
![image2](https://github.com/user-attachments/assets/63941868-ddad-40d2-b8c3-0e8ca8a3e8a1)
<br/>
     
(2) 설치 파일을 실행하고 나서 아래와 같이 옵션을 설정한다.   
설치시 CommandLine은 Installed "cursor"를 선택하면 된다.   
![image3](https://github.com/user-attachments/assets/051c0240-5810-45e7-9510-31565d9bf41d)
![image4](https://github.com/user-attachments/assets/be02fb75-9b5a-4504-9ddb-5803be72dbf2)
<br/> 
                
(3) VSCode 확장 프로그램 연동이 되기까지 시간이 걸리니 기다린다.               
![image5](https://github.com/user-attachments/assets/462616e2-91cd-43e6-92a4-47d52d87349a)
<br/>
        
(4) 다음은 Cursor 인공지능을 활용할 것이기에 "Help Improve Cursor" 옵션을 선택한다.     
![image6](https://github.com/user-attachments/assets/77832587-23b6-4570-932d-de473c554d73)
<br/>
                               
(5) 마지막으로 Cursor 로그인까지 완료하면 VSCode와 비슷한 화면이 뜨는 것을 확인할 수 있다.   
이제부터는 VSCode가 아닌 인공지능 기반 코드 에디터 Cursor를 통해 코드 개발을 할 수 있다.    
![image7](https://github.com/user-attachments/assets/a549b08f-7fc0-4cdf-a682-be9c7f9faae8)
<br/>

(6) 좌측 상단을 보면 VSCode와 연동된 확장 프로그램과 설치 가능한 확장 프로그램 목록을 확인할 수 있다.      
![image8](https://github.com/user-attachments/assets/79cfe797-4f5b-42ac-a076-f9bd73fe0835)
<br/>

---

2. Cursor 단축키   
Cursor에서 코딩을 도와주는 AI 기능은 크게 코드 수정, LLM Chat, Copilot++ 3가지가 있다.    
따라서 커맨드도 많이 외우지 않아도 되고 딱 3가지 커맨드만 알고 있으면 된다.

(1) Ctrl + L : LLM 채팅창 활성화      
에디터 화면 오른쪽에 LLM에게 대화할 수 있는 채팅창을 여는 커맨드      
복잡한 버그를 수정하거나 코드를 기술적인 도움이 필요한 경우 사용      
코드를 드래그 후 Ctrl + L 커맨드 입력 시 채팅창으로 코드가 자동으로 복사됨      
@Codebase, @Docs, @Web 등 여러 가지 심볼을 프롬프트로 사용 가능      
![image9](https://github.com/user-attachments/assets/728c0fe6-c67b-46ad-8f68-342e968a7f85)
<br/>

(2) Ctrl + K : 코드 생성줄 활성화      
간단한 코드를 생성하는 커맨드      
에디터 내 사용자 쿼리를 입력할 수 있는 창이 나타남      
간단한 함수를 만들거나 테일윈드, 부트스트랩 등 CSS 클래스명 붙일 때 편리      
@Codebase, @Docs, @Web 등 여러 가지 심볼을 프롬프트로 사용 가능         
![image10](https://github.com/user-attachments/assets/854633f6-53c3-4c52-a042-e06c7ff62a2a)
<br/>

(3) tab : 자동 코드 추천   
에디터에서 약 1초간 동작이 없는 경우 Cursor Copilot++ 이(가) 자동으로 코드를 추천   
tab키를 누르면 Cursor Copilot++ 이(가) 자동으로 만들어주는 코드를 적용   
<br/>

여기서 중요한 것은 심볼인데 이 심볼들을 조합해서 프롬프트를 효율적으로 만들어서 LLM에게 전달 할 수 있다.    
여기서는 대표적으로 중요한 심볼 3가지만 소개하겠다.   

@Codebase : 코드베이스 기반 검색 기능   
@Docs : 문서기반 검색 기능 - 설정 ( [Cursor Settings > Features] )에서 문서를 업로드하거나 채팅창에서 @Add로 문서 등록 가능   
@Web : 웹 기반 검색 기능 - 설정  ( [Cursor Settings > Features] )에서 항상 사용할지 체크 가능(default : 사용 안 함)   
<br/>

---

3. Cursor 활용 팁
   
(1) 분기문 생성   
switch, if 문 등 특정 상황에 따른 분기문을 작성할 때 번거로운 반복 작업을 줄여준다.    
유니온 타입이 잘 선언되어 있다면, 그 내용까지 잘 이해해서 제안한다.
![image11](https://github.com/user-attachments/assets/b242c7dc-e404-46cb-8860-39ec479cbec9)
<br/>

(2) 함수 생성   
이름을 잘 짓거나, 함수의 기능을 설명하면 바로 코드를 생성해준다.   
![image12](https://github.com/user-attachments/assets/5fdc3add-7b60-4ab6-9918-48b77f089503)
<br/>

(3) 타입 생성   
개발 단계에서 생성할 많은 타입들을 자동 생성할 수 있다.   
![image13](https://github.com/user-attachments/assets/c01d246f-ee37-4a30-b638-8144b55b7088)
<br/>

(4) 보일러 플레이트 생성   
Next.js, Svelte, express 등 특정 기술에 맞는 보일러 플레이트를 빠르게 생성할 수 있다.   
![image14](https://github.com/user-attachments/assets/eb273736-fc7c-4391-a593-5b3f0dbb04f4)
![image15](https://github.com/user-attachments/assets/a927ea7e-93a0-4296-be39-217690f52eaa)
<br/>

(5) 리팩터링 기능   
에디터에서 AI가 리팩터링한 내용을 반영할 수 있다.   
![image16](https://github.com/user-attachments/assets/2c156b0a-59b2-42b0-852c-403b3fd532d0)
<br/>

(6) 코드 베이스 질문   
사람의 개성처럼 프로젝트마다 다른 부분들이 있는데 이를 효과적으로 고려해서 원하는 코드를 얻어낼 수 있다.   
![image17](https://github.com/user-attachments/assets/86422d9d-291a-4f19-b206-18e215a83fff)
<br/>

---

Cursor의 구독 플랜은 다음과 같다.    
회원가입 후 14일 동안은 Pro 플랜을 사용할 수 있다.    
추후에 Pro로 업그레이드하려면 월 US$20.00 달러로 한화로 약 27000원 정도다.    
그래도 GPT4에 코파일럿 기능까지 3만 원 이내로 사용 가능하다면 나름 괜찮은 가격이다.    
Cursor 플랜 비용 사이트 : https://www.cursor.com/pricing   
![image18](https://github.com/user-attachments/assets/d0a926d2-4e5a-4469-9428-5a5f3da84faa)
   
