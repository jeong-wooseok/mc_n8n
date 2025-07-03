> 첫 시간의 환경 셋팅에 대한 가이드라인입니다. 

## 도커환경 셋팅 
1. docker desktop을 더블클릭합니다.
2. General에서 Expose daemon on tcp://localhost:2375 without TLS을 체크 후 Apply & restart합니다.


## 깃클론 및 도커컴포즈 실행 
1. win +r 입력

2. cmd 입력

3. git clone https://github.com/jeong-wooseok/mc_n8n

4. cd mc_n8n

5. 주소부분을 블록 후 우클릭 합니다. 
```cmd
C:\Users\student\mc_n8n
```

6. win+e 입력

7. 주소입력줄 클릭 후 ctrl+v

8. env.example파일에서 .example을 지웁니다. 
즉 .env인 상태로 저장합니다.

9. 도커컴포즈를 실행합니다.
```cmd
docker compose --profile gpu-nvidia up -d
```