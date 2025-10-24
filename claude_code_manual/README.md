## [클로드코드 활용]    

클로드코드는 리눅스나 macOS에서 사용 가능하지만 몇 가지 설정을 통해 윈도우에서도 사용 가능하다.   
클로드코드 사용을 위해서는 클로드 계정이 Pro 플랜 이상이어야 한다.   
   
[wsl에서 클로드코드 실행]   
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

[윈도우 cmd에서 클로드코드 실행]      
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

[VS Code에서 클로드코드 실행]   
(1) VS Code에서는 Extension에서 Claude Code for VSCode를 검색해서 설치한다.   
<img width="300" height="360" alt="image_9" src="https://github.com/user-attachments/assets/7f243254-40db-4c2d-97f5-bf3e65553374" />
<br/>
   
(2) VS Code에서 터미널창을 열고 클로드코드를 실행한다.   
본인이 선호하는 환경에 접근해서 클로드코드를 사용할 수 있다.   
<img width="900" height="400" alt="image_10" src="https://github.com/user-attachments/assets/31fadc55-b35a-426c-b45a-feca6672a205" />
<img width="350" height="400" alt="image_11" src="https://github.com/user-attachments/assets/863b621b-d6b9-47c2-8dbb-88a867baeed5" />
