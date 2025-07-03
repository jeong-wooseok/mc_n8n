### 이번 강의 핵심 요약

**이메일 수신을 자동 감지해 스팸 여부를 판단하고, 스팸일 경우 자동으로 삭제(혹은 이동)** 하는 AI 에이전트를 n8n과 n8n 커뮤니티 노드를 활용해 구현합니다.  
IMAP 설정, 프롬프트 엔지니어링, 분기 처리, 커뮤니티 노드 설치와 활용법까지 실습 중심으로 다룹니다.

  "prompt":"이미지 프롬프트",
  "ratio":"이미지 ratio",
  "result":"true or false",
  "lora_weight":"Lora Address",
  "trigger_words":"trigger_words"

---

### 📌 주요 내용 요약

#### 1\. IMAP 기반 이메일 자동 수신 설정

- **트리거 노드**: Email Trigger (IMAP) 사용
- **IMAP 설정 방법**:
	- 네이버 메일 환경설정 → IMAP 활성화
	- 2단계 인증 시, 애플리케이션 비밀번호 생성 필요
	- 호스트: `imap.naver.com`, 포트: `993`, SSL 사용
- **기본 메일함 연결**: INBOX 또는 원하는 폴더 지정 가능

#### 2\. 이메일 수신 → AI 분석 흐름 구성

- 이메일이 오면 자동으로 워크플로우 시작
- **제목(subject) + 본문(text)** 을 AI 프롬프트에 입력
- AI가 **스팸 여부를 판단** → JSON 형태의 structured output 사용
	- 예시 출력:
		```json
		{ 
		    "is_spam": "yes", 
		    "reason": "제목과 내용 모두 광고성 문구를 포함" 
		}
		```

#### 3\. AI 판단 결과로 분기 처리

- **스위치 노드** 를 이용해 `is_spam` 값이 `yes` 인지 확인
- `yes` 인 경우에만 메일 이동 기능 실행

#### 4\. 엠파렌 커뮤니티 노드 설치 및 활용

- 엠파렌 커뮤니티 노드 검색 및 설치: `empare-node-imap`
- 설치 후 새로고침(F4) → 기능 활성화됨
- **메일 이동 기능 사용**:
	- `getMany` 로 UID 추출
	- `move` 기능으로 특정 메일함(예: 휴지통 `Deleted`)으로 이동
	- 필수 입력값: UID, source mailbox, destination mailbox

#### 5\. 고급 팁 & 주의사항

- 이메일 본문은 HTML과 텍스트가 분리되어 있음 → 필요시 선택
- structured output 사용 시 AI 응답이 정확하고 후처리에 용이
- 커뮤니티 노드는 별도 설치 후 사용해야 하며, UI에서 바로 검색되지 않으면 F4 새로고침 필요
- 네이버 메일함의 이름은 영어로 표기됨 (예: Junk, Deleted 등)

---

### 🛠 실전 팁 & 기술 포인트

- **AI 판단 결과를 JSON으로 강제 설정**: `structured output parser` 활용
- **메일 이동에는 UID가 반드시 필요**: getMany로 동적 검색 필요
- **이메일 수신 트리거가 자동으로 작동되므로 테스트 시 주의**
- **프롬프트 작성이 가장 핵심**: 좋은 판단 결과를 위한 키 포인트

---

사이트 링크:

- [네이버 메일 IMAP 설정](https://mail.naver.com/v2/settings/smtp/imap)
- [네이버 보안 설정](https://nid.naver.com/user2/help/myInfoV2?m=viewSecurity&lang=ko_KR)
- [n8n-nodes-imap 링크](https://www.npmjs.com/package/n8n-nodes-imap)

