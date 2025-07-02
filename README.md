# 🤖 [Shift+AI] 업무 생산성 향상을 위한 AI AGENT 활용법 - 코드 템플릿

## 📋 과정 개요

- **과정명**: [Shift+AI] 업무 생산성 향상을 위한 AI AGENT 활용법
- **시간**: 8시간 (09:00-18:00)
- **난이도**: 초급 (ChatGPT 사용 경험만 있으면 OK)
- **완성물**: 실무용 AI Agent 4개

## 🎯 구현할 프로젝트

1. **기본 챗봇** - OpenAI API 연동 웹 챗봇
2. **이메일 자동분류봇** - Gmail 연동 + AI 분류 + 알림
3. **RAG 문서봇** - Google Drive 문서 기반 QA 시스템
4. **스케줄 예약봇** - 텔레그램 + 구글 캘린더 연동

## 🚀 빠른 시작

### 1단계: 환경 설정
```bash
# 1. 리포지토리 클론
git clone https://github.com/jeong-wooseok/mc_n8n.git
cd mc_n8n

# 2. 도커컴포즈 실행
docker compose --profile gpu-nvidia up -d --build
# gpu없는 경우는
# docker compose up -d --build
```

### 2단계: 서비스 접속
- **n8n 워크플로우**: http://localhost:5678
- **OpenWebUI**: http://localhost:3000  
- **Ollama API**: http://localhost:11434

### 3단계: credential 설정
- n8n에서 직접 credential을 만들고 실행하기

## 📁 프로젝트 구조

```
mc_n8n/
├── docker-compose.yml      # Docker 환경 설정
├── n8n/
│   └── workflows/         # n8n 워크플로우 템플릿
│       ├── basic_chatbot.json
│       ├── email_classifier.json
│       ├── rag_document_bot.json
│       └── schedule_bot.json
├── data/
│   └── documents/         # RAG용 문서 폴더
└── logs/                  # 로그 파일
```

## 🛠️ 기술 스택 (n8n AI Starter Kit 기반)

| 도구 | 역할 | 포트 | 설명 |
|------|------|------|------|
| **n8n** | 워크플로우 엔진 | 5678 | 비주얼 자동화 플랫폼 |
| **PostgreSQL** | 메인 데이터베이스 | 5432 | n8n 및 프로젝트 데이터 |
| **Qdrant** | 벡터 데이터베이스 | 6333 | 고성능 벡터 검색 |
| **Ollama** | 로컬 LLM 서버 | 11434 | 오픈소스 모델 실행 |
| **OpenWebUI** | LLM 인터페이스 | 3000 | 사용자 친화적 AI 채팅 |

## 📖 실습 가이드

### 프로젝트 1: 기본 챗봇
```bash
# 1. n8n에서 워크플로우 임포트
# n8n/workflows/basic_chatbot.json 파일 사용

# 2. 테스트 요청
curl -X POST http://localhost:5678/webhook/chat \
  -H "Content-Type: application/json" \
  -d '{"message": "안녕하세요!"}'
```

### 프로젝트 2: 이메일 자동분류봇
```bash
# Gmail API 설정 필요:
# 1. Google Cloud Console에서 Gmail API 활성화
# 2. OAuth 2.0 설정
# 3. n8n에서 Gmail Credential 추가
```

### 프로젝트 3: 고급 RAG 문서봇
```bash
# 1. Qdrant 컬렉션 생성
curl -X PUT "http://localhost:6333/collections/documents" \
-H "Content-Type: application/json" \
-d '{
  "vectors": {
    "size": 1536,
    "distance": "Cosine"
  }
}'

# 2. 문서 임베딩 및 저장 워크플로우 실행
# 3. PostgreSQL에서 대화 로그 확인
```

### 프로젝트 4: 스케줄 예약봇
```bash
# 1. 텔레그램 봇 생성 (@BotFather)
# 2. Google Calendar API 설정
# 3. 봇과 대화로 일정 관리
```

## 🔧 문제 해결

### Docker 관련
```bash
# 컨테이너 상태 확인
docker-compose ps

# 로그 확인
docker-compose logs -f

# 재시작
docker-compose restart

# 완전 초기화
docker-compose down -v
docker system prune -f
```

### API 연결 오류
1. **OpenAI API 401 오류**: API 키 확인 및 크레딧 잔액 확인
2. **Gmail API 인증 실패**: OAuth 스코프 권한 재설정
3. **텔레그램 봇 응답 없음**: 봇 토큰과 Chat ID 확인

### 성능 최적화
```bash
# Ollama 모델 확인
docker exec ollama_ai_course ollama list

# 메모리 사용량 확인
docker stats

# n8n 워크플로우 실행 이력
# n8n UI > Executions 메뉴에서 확인
```

## 🔑 필수 API 키

| 서비스 | 필수도 | 비용 | 획득 방법 |
|--------|--------|------|-----------|
| **OpenAI** | 필수 | ~$5 | platform.openai.com |
| **Telegram** | 선택 | 무료 | @BotFather |
| **Google** | 선택 | 무료 | console.cloud.google.com |

## 📚 참고 자료

### 공식 문서
- [n8n Documentation](https://docs.n8n.io)
- [OpenAI API Docs](https://platform.openai.com/docs)
- [Ollama GitHub](https://github.com/ollama/ollama)

### 추가 학습
- [LangChain 튜토리얼](https://python.langchain.com)
- [프롬프트 엔지니어링 가이드](https://www.promptingguide.ai)
- [Multi-Agent 시스템 설계](https://github.com/microsoft/autogen)

## 💬 지원 및 커뮤니티

- **강사 이메일**: aieeiee030303@gmail.com

## 🎓 수료증 및 다음 단계

### 수료 기준
- [ ] 4개 프로젝트 모두 완성

## 📄 라이선스

MIT License - 자유롭게 사용, 수정, 배포 가능

---

## ⭐ 프로젝트가 도움이 되셨다면 Star를 눌러주세요!

**Happy Coding! 🚀** 
