# FA01BZ n8n 워크플로우 사용 가이드

## 📋 개요

이 가이드는 FA01BZ AI Agent 과정에서 사용하는 n8n 워크플로우들의 올바른 사용법을 설명합니다.

## 🚀 워크플로우 설정 가이드

### 1. Basic Chatbot (기본 챗봇)

**주요 노드:**
- `Chat Trigger`: 채팅 입력 받기
- `OpenAI Chat`: GPT API 호출
- `Log to Google Sheets`: 대화 기록 저장

**설정 **
1. **OpenAI Credential 설정**
   ```
   - n8n 설정 > Credentials > Create New
   - Type: OpenAI
   - API Key: sk-your-api-key
   ```

2. **Google Sheets Credential 설정**
   ```
   - Type: Google Sheets OAuth2 API
   - Google OAuth 설정 필요
   ```

3. **워크플로우 임포트**
   ```
   - n8n 메인 > Import
   - basic_chatbot.json 파일 선택
   ```

4. **테스트 실행**
   ```
   - Chat Trigger 노드 클릭
   - "Test Step" 또는 "Execute Workflow"
   - 채팅창에 메시지 입력
   ```

### 2. Advanced RAG Document Bot (RAG 문서봇)


## 🔄 워크플로우 활성화

### 1. Chat Trigger 기반 워크플로우
```
1. 워크플로우 저장 (Ctrl+S)
2. "Active" 토글 버튼 ON
3. n8n 우측 상단 "Chat" 버튼 클릭
4. 채팅창에서 테스트
```

### 2. 접근 URL (Chat Interface)
```
- n8n 인터페이스: http://localhost:5678
- 채팅 인터페이스: 워크플로우 내에서 "Chat" 버튼
```

## 📊 모니터링 및 디버깅

### 1. 실행 로그 확인
```
n8n > Executions 메뉴
- 성공/실패 상태 확인
- 각 노드별 입출력 데이터 검토
- 오류 메시지 분석
```

### 2. 일반적인 오류 해결

**OpenAI API 오류:**
```
- 401 Unauthorized: API 키 확인
- 429 Rate Limit: 잠시 대기 후 재시도
- 토큰 초과: max_tokens 값 조정
```

**Qdrant 연결 오류:**
```
- 503 Service Unavailable: Qdrant 컨테이너 상태 확인
- 404 Collection Not Found: 컬렉션 생성 필요
- docker-compose ps 로 서비스 확인
```

**PostgreSQL 연결 오류:**
```
- Connection refused: 데이터베이스 서비스 확인
- Authentication failed: 계정 정보 확인
- Table doesn't exist: 초기화 스크립트 실행 확인
```

## 🎯 실습 시나리오

### 시나리오 1: 기본 챗봇 테스트
```
1. 워크플로우 활성화
2. "안녕하세요!" 입력
3. AI 응답 확인
4. Google Sheets에 로그 기록 확인
```

### 시나리오 2: RAG 문서봇 테스트
```
1. 테스트 문서 Qdrant에 업로드
2. 문서 관련 질문 입력: "연차 사용 규정은?"
3. 컨텍스트 기반 답변 확인
4. 출처 정보 표시 확인
```

## 🔧 고급 설정

### 1. 프롬프트 튜닝
```
// System 프롬프트 예시
당신은 FA01BZ 교육과정의 전문 AI 어시스턴트입니다.
- n8n 워크플로우 관련 질문에 전문적으로 답변
- 한국어로 친근하게 응답
- 실습 관련 구체적인 가이드 제공
```

### 2. 성능 최적화
```json
{
  "temperature": 0.7,
  "max_tokens": 500,
  "top_p": 0.9,
  "frequency_penalty": 0.1
}
```

### 3. 커스텀 응답 형식
```javascript
// Context Assembly 노드에서 응답 형식 정의
return [{
  answer: aiResponse,
  confidence: score,
  sources: documents,
  timestamp: new Date().toISOString(),
  session_id: uuid()
}];
```

## 🚀 다음 단계

1. **워크플로우 커스터마이징**: 본인 용도에 맞게 수정
2. **추가 노드 연결**: telegram 추가
3. **성능 모니터링**: 응답 시간, 정확도 측정
4. **실무 배포**: 프로덕션 환경 설정

---
