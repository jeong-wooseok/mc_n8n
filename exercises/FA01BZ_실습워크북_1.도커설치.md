# FA01BZ AI Agent 구현 실무 - 실습 워크북

## 📋 개요
이 워크북은 **ChatGPT를 넘어선 나만의 AI Agent 만들기** 과정을 위한 실습 가이드입니다.  
8시간 동안 Docker 기반 개발환경을 구축하고, n8n과 OpenAI API를 활용해 **실무에서 바로 사용 가능한 4개의 AI Agent**를 완성합니다.

**필요한 준비물:**
- 인터넷 연결
- Google 계정
- OpenAI API 키 (선택사항)

---

### ✅ Step 1: Docker 설치 확인
```bash
# 터미널(CMD)에서 실행
docker --version
docker-compose --version
```

**결과 확인:**
- Docker version 20.x 이상 출력 ✅
- docker-compose version 1.29 이상 출력 ✅

**미설치시 대응:**
1. https://docker.com/products/docker-desktop 접속
2. 운영체제에 맞는 버전 다운로드
3. 설치 후 재부팅
4. 위 명령어로 재확인

### ✅ Step 2: 프로젝트 폴더 생성
```bash
# 바탕화면에 작업 폴더 생성
cd Desktop
mkdir ai-agent-course
cd ai-agent-course
```

### ✅ Step 3: AI Starter Kit 다운로드
**n8n 공식 Self-hosted AI Starter Kit 사용**

```bash
# GitHub에서 템플릿 다운로드
git clone https://github.com/jeong-wooseok/mc_n8n.git
cd mc_n8n.git
```

**주요 구성 요소:**
- ✅ **n8n**: 워크플로우 엔진 (포트 5678)
- ✅ **Ollama**: 로컬 LLM 서버 (포트 11434) 
- ✅ **Qdrant**: 벡터 데이터베이스 (포트 6333)
- ✅ **PostgreSQL**: 메인 데이터베이스 (포트 5432)
- ✅ **OpenWebUI**: AI 채팅 인터페이스 (포트 3000)

### ✅ Step 4: 환경 실행

```bash
# Docker 컨테이너 시작 / GPU가 있는 환경 기준입니다
docker compose --profile gpu-nvidia up -d
```

```bash
# 실행 상태 확인
docker compose ps
```

**확인사항:**
- State가 모두 "Up" 상태 ✅
- 5개 서비스 정상 실행 ✅

---

### ✅ Step 5: 서비스 접속 확인
**브라우저에서 다음 주소들을 열어보세요:**

1. **n8n**: http://localhost:5678
   - 첫 계정 생성 (이메일 + 비밀번호)
   - 메인 화면 진입 확인 ✅

2. **OpenWebUI**: http://localhost:3000  
   - 첫 계정 생성 (관리자 권한)
   - 대시보드 화면 확인 ✅

3. **Ollama**: http://localhost:11434
   - 브라우저에 "Ollama is running" 메시지 ✅

### ✅ Step 6: AI 모델 다운로드
```bash
# 가벼운 한국어 모델 다운로드 (약 2GB)
docker exec ollama ollama pull exaone3.5:2.4b
# docker exec ollama ollama pull llama3.2:3b
```

**진행상황 확인:**
- 다운로드 진행률 표시 ✅
- "success" 메시지 확인 ✅

### ✅ Step 7: API 키 준비
**OpenAI API (필수):**
1. https://platform.openai.com 접속
2. API Keys → Create new secret key
3. 키 복사 후 안전한 곳에 저장
4. 최소 5달러 크레딧 충전 권장

**텔레그램 봇 토큰 (선택):**
1. 텔레그램에서 @BotFather 검색
2. `/newbot` 명령어 입력
3. 봇 이름과 사용자명 설정
4. 발급받은 토큰 저장

---

**📅 작성일**: 2025년 7월 2일
**🔄 최종 업데이트**: 2025년 7월 2일
**📝 작성자**: 일하는 ai