## [로컬 PC에서 LLM 구동하기 (Ollama, LM Studio)]

Ollama, LM Studio는 로컬 PC에 다운로드한 LLM을 구동할 수 있는 프로그램이다.   
무료 오픈소스 LLM을 로컬 PC에 다운로드한 후 Ollama, LM Studio로 LLM을 구동하면    
프라이빗 환경에서 보안을 유지하면서 AI와 대화하거나 AI 서비스를 개발할 수 있다.   
<br/>

---
<br/>
   
1. Ollama 설치    
   
(1) Ollama 공식 웹사이트 주소는 아래와 같다.   
사용하는 OS 종류에 맞게 프로그램을 다운로드 하면 된다.   
https://ollama.com/    
<img width="400" alt="image1" src="https://github.com/user-attachments/assets/9781a411-3a16-4cf6-b95d-935e1a0e3a31" />
<br/>
        
(2) 다운로드한 설치파일을 실행해서 Ollama를 설치하자.        
<img width="600" alt="image2" src="https://github.com/user-attachments/assets/29bdda31-c20d-4169-9d71-d97c0cb036cc" />
<br/> 

만약 리눅스 환경이라면 아래 이미지와 같이 설치하면 된다.   
만약에 NVIDIA GPU 가 있으면 그에 맞는 환경이 셋업된다.   
<img width="1000" alt="image3" src="https://github.com/user-attachments/assets/4db10912-e619-4ef7-a1b1-55aff7b876ac" />
<br/>
                                  
(3) ollama가 설치되면 cli 환경에서 ollama run [LLM] 명령어로 원하는 LLM을 구동할 수 있다.   
예를 들어 Llama 3 모델을 실행하려면 아래와 같이 명령어를 실행하면 된다.   

명령어 : ollama run llama3   

만약 해당 모델이 설치되어 있지 않다면 자동으로 다운로드 후 실행된다.   
<img width="600" alt="image4" src="https://github.com/user-attachments/assets/f50df17a-678e-4bc6-92fe-6c1e8e0205ac" />
<br/>
   
(4) LLM을 구동하면 프롬프트창이 열리면서 AI와 대화할 수 있다.   

```
>>> 하늘은 왜 파란가요?   
하늘이 파랗게 보이는 이유는 빛의 산란 때문입니다.   
태양 빛 중 파장이 짧은 푸른색 계열의 빛이 대기 중 입자에 의해 강하게 산란되어 우리 눈에 많이 들어오기 때문이죠.   
반면 파장이 긴 빨간색 계열은 대기를 통과해 직진하는 경향이 있습니다.   
이런 레일리 산란 현상 때문에 하늘은 푸르게 보이는 것입니다.
```
<br/>

(5) 긴 문장을 입력하려고 할 때는 """로 문장을 감싼다.       

```
>>> """   
안녕하세요.   
오늘도 좋은 하루 되세요!   
"""
```
<br/>

(6) 일부 멀티모달 모델은 이미지 첨부도 가능하다.   

```
>>> 이 이미지에 무엇이 있나요? /path/to/image.png   
이미지에는 푸른 하늘을 배경으로 한 해바라기 꽃이 클로즈업되어 있습니다.   
노란 꽃잎이 선명하게 보이고, 가운데 꽃술도 잘 보이네요.   
매우 생동감 있는 사진입니다.
```
<br/>

(7) Ollama 라이브러리의 모델은 프롬프트로 커스터마이징할 수 있다.    
예를 들어 llama3 모델을 수정하려면 아래와 같다.   

모델 다운로드    
명령어 : ollama pull llama3   

Modelfile 생성 :   

```
FROM llama3   
# 온도 설정 (높을수록 창의적, 낮을수록 정확함)     
PARAMETER temperature 1   
# 시스템 메시지 설정   
SYSTEM """   
당신은 슈퍼마리오입니다. 마리오처럼 대답해주세요.   
"""   
```

모델 생성 및 실행   
명령어 (1) : ollama create mario -f ./Modelfile   
명령어 (2) : ollama run mario   
<br/>

(8) Ollama의 파이썬 메소드는 아래 이미지와 같다.   
<img width="600" alt="image5" src="https://github.com/user-attachments/assets/a1cb86aa-9d33-40f4-8e00-0930600d49a1" />
<br/><br/>

(9) Ollama Rest API 사용   
Ollama는 다른 어플리케이션과 통합하는데 사용할 수 있는 Rest API를 제공하고 있다.   
Python의 requests 라이브러리를 사용한 예는 아래 이미지와 같다.   
<img width="500" alt="image6" src="https://github.com/user-attachments/assets/5f22b855-48e5-4c8b-a6e4-0de775030b4e" />
<br/><br/>

[Ollama의 성능 향상을 위한 하드웨어 업그레이드]   
- CPU의 파워 업   
Ollama는 CPU로 실행할 수도 있기에, 당연한 이야기지만 최신 CPU를 탑재하면 성능이 크게 향상된다.   
예를 들어 인텔 코어 i9 또는 AMD 라이젠 9 프로세서는 Ollama의 성능을 크게 향상시킬 수 있다.   

- 효율 향상을 위한 RAM 증가   
RAM은 특히 큰 모델을 다룰 때 Ollama의 성능에 중요한 역할을 합니다.   
16GB 이상 (7B 파라미터의 작은 모델에 대해서)   
32GB 이상 (13B 파라미터의 중간 정도의 모델에 대해서)   
64GB 이상 (30B+ 파라미터의 큰 모델에 대해서)   

- GPU 액셀러레이션 활용   
GPU는 특히 큰 모델에 대해 Ollama의 성능을 극적으로 향상시킬 수 있다.   
GPU 액셀러레이션을 활성화(이용 가능한 경우) → $ export OLLAMA_CUDA=1   
CUDA 지원을 갖춘 엔비디아 GPU(예: RTX3080, RTX4090 등)   
작은 모델에는 최소 8GB VRAM을, 큰 모델에는 16GB 이상의 VRAM을 탑재한 GPU   
<br/>

---
<br/>

2. LM Studio 설치

(1) LM Studio 공식 웹사이트 주소는 아래와 같다.      
사용하는 OS 종류에 맞게 프로그램을 다운로드 하면 된다.      
https://lmstudio.ai/   
<img width="1800" alt="image7" src="https://github.com/user-attachments/assets/9e9ed9ab-0746-40e4-8bf5-5fb2e4a6e8bd" />
<br/><br/>

(2) 프로그램을 다운로드 후 설치하면 LM Studio가 추천하는 LLM을 추가로 다운로드 하거나 해당 단계를 건너뛸 수 있다.      
<img width="1600" alt="image8" src="https://github.com/user-attachments/assets/41650117-23ce-43e7-a9a2-a65fc0afb269" />
<br/><br/>
      
(3) 만약 위 단계를 건너뛰면 기본 화면 사이드바의 "Discover" 항목에서 원하는 모델을 찾고 다운로드 할 수 있다.      
<img width="2000" alt="image9" src="https://github.com/user-attachments/assets/4dd387ca-1664-42b7-b902-05a3d547d36f" />
<br/><br/>

(4) 다운로드한 모델을 AI Chat 화면에서 사용해볼 수 있다.   
![image10](https://github.com/user-attachments/assets/daf5a429-5b72-44ba-a10b-c3340ded804d)
<br/><br/>

---
<br/>

3. Ollama와 LM Studio의 차이점      
<img width="600" alt="image11" src="https://github.com/user-attachments/assets/b2cff3a7-3ba5-479d-9d88-637981b3625b" />
<br/><br/>

[LM Studio 사용이 적합한 경우]   
(1) 사용 친화적 환경 선호 사용자 : 그래픽 인터페이스를 통해 쉽게 LLM을 사용해보고 싶은 경우   
(2) 다양한 모델 실험 : Hugging Face에서 제공하는 다양한 모델을 쉽게 다운로드하고 테스트하고 싶을 때   
(3) 채팅 인터페이스 필요 : 모델과 직접 대화하며 결과를 즉시 확인하고 싶은 경우   
(4) OpenAI API 호환성 필요 : 기존 OpenAI API를 사용하는 애플리케이션과 통합하고 싶을 때    
<br/>

[Ollama 사용이 적합한 경우]   
(1) 개발자 또는 기술에 능숙한 사용자 : 명령어 인터페이스에 익숙하고 더 많은 제어를 원하는 경우   
(2) 자동화 및 스크립팅 : 모델 실행을 자동화하거나 다른 스크립트와 통합하고 싶을 때   
(3) 커스텀 모델 설정 : Modelfile을 통해 모델 구성을 세밀하게 조정하고 싶은 경우   
(4) 오픈 소스 선호 : 완전히 오픈 소스인 솔루션을 사용하고 싶거나 기여하고 싶을 때   
(5) 리소스 효율성 : 더 가벼운 솔루션을 원하거나 시스템 리소스를 최적화하고 싶을 때   
      
