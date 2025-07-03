
---

## 📄 프로젝트 1 - 기본 챗봇 만들기

### 🎯 목표: n8n으로 OpenAI API 연동 챗봇 구현
### ✅ Step 1: 새 워크플로우 생성
1. n8n 화면에서 "+" 버튼 클릭
2. "Add first step" 클릭
3. "Chat Trigger" 선택

### ✅ Step 2: Chat Trigger 설정
**Chat Trigger 노드 설정:**
- Make Chat Publicly Available: 활성화
- Mode: Hosted Chat
- Authentication: None
- 나머지는 기본값 유지

### ✅ Step 3: AI Agent 노드 추가
1. "+" 버튼으로 노드 추가
2. "AI Agent" 검색 후 선택
3. Chat Trigger와 AI Agent를 연결

### ✅ Step 4: Chat Model 연결
**AI Agent 노드에서:**
1. "+ Chat Model" 버튼 클릭
2. "OpenAI Chat Model" 선택
3. Credential 설정:
   - "Create New" 클릭
   - API Key에 준비한 OpenAI 키 입력
   - 연결 테스트 후 저장

### ✅ Step 5: 계산기 도구 추가
**AI Agent에 Tool 연결:**
1. AI Agent 노드에서 "+ Tool" 버튼 클릭
2. "Calulator Tool" 선택

**테스트 방법:**
✅  n8n에서 직접 입력합니다.
✅ Live 상태에서 웹훅 주소에서 chat을 입력합니다.
✅ 웹훅주소의 chat 디자인을 변경합니다.

> 기본적인 AI Agents를 만들어 봤습니다. Output Parsor 기능 및 기타 Agents 노드도 익숙해지도록 하나씩 열어서 확인해보겠습니다. 

---

## 📄프로젝트 2 - 이메일 자동분류봇

### 🎯 목표: Gmail 연동하여 수신 메일 자동 분류 및 알림

### ✅ Step 1: Gmail API 설정 (중요!)
**Google Cloud Console 설정:**
1. https://console.cloud.google.com 접속
2. 새 프로젝트 생성 (이름: AI-Agent-Project)
3. "APIs & Services" → "Enable APIs"
4. "Gmail API" 검색 후 활성화

**OAuth 2.0 설정:**
1. "Credentials" → "Create Credentials" → "OAuth 2.0 Client IDs"
2. Application type: "Desktop application"
3. Name: "n8n Gmail Bot"
4. Client ID와 Client Secret 저장

### ✅ Step 2: n8n에서 Gmail 연동
1. 새 워크플로우 생성 (이름: Email_Classifier)
2. "Gmail Trigger" 노드 추가
3. Trigger on: `Email Received`

**Credential 설정:**
1. "Create New" → "Google OAuth2 API"
2. Client ID와 Secret 입력
3. 인증 프로세스 완료 (구글 로그인)

### ✅ Step 3: AI 분류 로직 구성
**OpenAI 노드 추가:**
- Model: `gpt-4.1-nano`
- 프롬프트:
```
이메일을 다음 4개 카테고리로 분류해주세요:
1. 업무 - 회사, 일, 비즈니스 관련
2. 개인 - 가족, 친구, 개인적 내용  
3. 광고 - 마케팅, 프로모션, 쇼핑
4. 긴급 - 즉시 확인이 필요한 중요한 내용

결과는 정확히 이 JSON 형식으로만 응답하세요:
{"category": "업무", "priority": 8, "summary": "회의 일정 관련"}

분류할 이메일:
제목: {{ $json.subject }}
발신자: {{ $json.from }}
내용: {{ $json.bodyPlainText }}
```

### ✅ Step 4: 분류 결과 처리
**IF 노드 추가:**
1. "IF" 노드로 조건 분기
2. Condition: `{{ $json.category }} = "긴급"`

**텔레그램 알림 (긴급 메일용):**
1. "Telegram" 노드 추가
2. 봇 토큰 설정
3. 메시지 템플릿:
```
🚨 긴급 메일 도착!

📧 발신자: {{ $json.from }}
📝 제목: {{ $json.subject }}
⭐ 중요도: {{ $json.priority }}/10
📄 요약: {{ $json.summary }}
```

### ✅ Step 5: Gmail 라벨 자동 적용
**Gmail 노드 추가:**
1. Operation: `Add Label`
2. Message ID: `{{ $json.id }}`
3. Labels: `AI-{{ $json.category }}`

**워크플로우 구조:**
```
Gmail Trigger → OpenAI 분류 → IF 조건 
                                ├─ 긴급: 텔레그램 알림
                                └─ 일반: Gmail 라벨만
```

---

## 📄 프로젝트 3 - RAG 문서 챗봇

### 🎯 목표: Google Drive 문서 기반 QA 시스템 구축

### ✅ Step 1: Google Drive API 활성화
**Google Cloud Console:**
1. "Drive API" 검색 후 활성화
2. "Service Account" 생성
3. JSON 키 파일 다운로드

### ✅ Step 2: 문서 수집 워크플로우
**새 워크플로우 생성:** `Document_RAG`

**Schedule Trigger 설정:**
- Trigger Interval: `Every Hour`
- 문서 업데이트 주기적 확인

**Google Drive 노드:**
1. Operation: `List`
2. Folder ID: 본인 Google Drive 폴더
3. File Types: `.pdf, .docx, .txt`

### ✅ Step 3: 문서 처리 파이프라인
**Text Extraction:**
- PDF: `PDF Extract` 노드
- Word: `Microsoft Word` 노드  
- 일반 텍스트: `Read File` 노드

**Text Chunking:**
```javascript
// 텍스트 분할 로직
const text = $json.content;
const chunkSize = 1000;
const overlap = 200;
const chunks = [];

for (let i = 0; i < text.length; i += chunkSize - overlap) {
  chunks.push(text.slice(i, i + chunkSize));
}

return chunks.map(chunk => ({ text: chunk }));
```

### ✅ Step 4: 검색 및 답변 워크플로우
**Webhook Trigger:**
- Path: `ask`
- Method: `POST`

**벡터 검색 시뮬레이션:**
```javascript
// 간단한 키워드 검색
const question = $json.question;
const documents = $json.documents; // 사전 수집 문서들
const keywords = question.split(' ');

const relevantDocs = documents.filter(doc => 
  keywords.some(keyword => 
    doc.content.includes(keyword)
  )
);

return relevantDocs.slice(0, 3); // 상위 3개 문서
```

**답변 생성:**
```
다음 문서들을 참고하여 질문에 정확하게 답변해주세요.
문서에 없는 내용은 "해당 정보를 찾을 수 없습니다"라고 답변하세요.

참고 문서:
{{ $json.documents }}

질문: {{ $json.question }}

답변 형식:
- 답변 내용
- 출처: [문서명]
```

---



### 🚀 성능 최적화 팁

**1. API 비용 절약:**
- 토큰 수 제한: `max_tokens: 150`
- 모델 선택: 간단한 작업은 `gpt-4.1-mini`
- 캐싱: 중복 질문 결과 저장

**2. 오류 처리:**
```javascript
// Error 노드에서 사용
try {
  // 메인 로직
  return $json;
} catch (error) {
  return {
    error: true,
    message: "처리 중 오류가 발생했습니다.",
    details: error.message
  };
}
```

**3. 모니터링 대시보드:**
- Google Sheets로 사용 로그 기록
- 일일/주간 사용량 통계
- 에러 발생률 추적

### 🔧 문제 해결 가이드

**자주 발생하는 문제들:**

**1. Docker 실행 오류**
```bash
# 포트 충돌 해결
docker-compose down
netstat -ano | findstr :5678
# 해당 프로세스 종료 후 재시작
```

**2. OpenAI API 오류**
- 401 Unauthorized: API 키 확인
- 429 Rate Limit: 잠시 대기 후 재시도
- 400 Bad Request: 프롬프트 형식 확인

**3. Gmail API 인증 실패**
- OAuth 스코프 권한 재확인
- Google 계정 2단계 인증 설정
- API 할당량 초과 여부 확인

### 🎯 실습 체크리스트

**완성해야 할 4개 프로젝트:**
- [ ] 기본 챗봇 (웹훅 응답 정상)
- [ ] 이메일 분류봇 (Gmail 연동 + 분류)
- [ ] RAG 문서봇 (Drive 연동 + QA)
- [ ] 스케줄 봇 (캘린더 연동)

**최종 확인사항:**
- [ ] 모든 워크플로우 정상 실행
- [ ] API 연결 상태 양호
- [ ] 텔레그램 알림 수신 확인
- [ ] 실제 업무 적용 계획 수립

### 📚 추가 학습 리소스

**공식 문서:**
- n8n 문서: https://docs.n8n.io
- OpenAI API: https://platform.openai.com/docs
- Google APIs: https://developers.google.com

---

## 🎉 수고하셨습니다!

**오늘 성취한 것들:**
✅ Docker 기반 AI 개발환경 구축  
✅ n8n으로 시각적 워크플로우 작성  
✅ OpenAI API 연동 챗봇 개발  
✅ Gmail, Calendar, Telegram 연동  
✅ 실무 적용 가능한 4개 Agent 완성  

**연락처:**
- 이메일: instructor@aiagent.com
- GitHub: github.com/aiagent-course
- 카톡방: [QR코드 또는 링크]

**Keep Learning, Keep Building! 🚀** 