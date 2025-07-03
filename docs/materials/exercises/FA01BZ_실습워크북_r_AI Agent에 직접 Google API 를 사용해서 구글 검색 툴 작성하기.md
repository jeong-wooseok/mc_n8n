---

OpenAI 대신 **Google Gemini API** 와 **Google Custom Search API** 를 활용한 AI 에이전트 구성 방법을 실습합니다.  
구글 클라우드 콘솔에서 프로젝트를 생성하고 API 키를 발급받는 전 과정을 다루며, N8N에서 구글 검색 툴을 직접 구현해 AI가 실시간 정보를 검색할 수 있도록 만드는 방법을 배웁니다.

---

### 📌 주요 내용 요약

#### 1\. Google Cloud Console 설정 및 무료 크레딧 안내

- 구글 계정으로 로그인 → [cloud.google.com](https://cloud.google.com/) 접속
- 무료 체험 등록 시 약 **$300 (한화 약 44만원)** 크레딧 제공
	- 크레딧은 **가입일로부터 일정 기간 내에만 유효**
	- 사용 중 **일반 계정 전환** 하지 않으면, 크레딧 소진 시 리소스 자동 삭제됨
- 프로젝트 생성 후 모든 API는 **해당 프로젝트 단위** 로 연동됨 → **현재 선택 중인 프로젝트 확인 필수**

#### 2\. Gemini API 설정 및 N8N 연동

- `Gemini API` 활성화 후, **서비스 계정 생성 → API 키 발급**
- API 키 발급 시 **“Generative Language API”** 로 제한 설정 → 보안 강화
- N8N에서 기존 OpenAI 노드를 제거 후, `Google Gemini Chat` 노드로 교체
	- 크레덴셜에 API 키 입력 → 연결 테스트
- Gemini 모델 종류:
	- `Gemini 2.0 Flash`: 가장 저렴하고 빠름 → 100만 토큰당 입력 $0.10 / 출력$ 0.40
	- `Pro`, `EXP`, `8B`, `Light`, `Preview` 등 다양한 버전 존재

#### 3\. Google 검색 기능(API) 구현

- `Custom Search API` 활성화 후, 별도 **검색 엔진 생성**
	- `CX 값` (검색 엔진 ID) 확보 필요
	- 전체 웹 검색 설정 권장
- N8N에서 **HTTP Request 노드** 로 구성:
	- 메서드: GET
	- URL: `https://www.googleapis.com/customsearch/v1`
	- Query 파라미터:
		- `cx`: 검색 엔진 ID (CX)
		- `key`: API 키
		- `q`: 검색어 (AI가 자동으로 입력하게 설정)
- **디스크립션 필드에 반드시 기능 설명 기입** → AI가 툴을 인식하고 활용할 수 있도록 하기 위함

#### 4\. 테스트 및 동작 확인

- 검색 예시: “요즘 인기 있는 영화 추천해 줘”
	- AI가 Custom Search API를 호출하여 결과 가져옴
- 시간 정보 등도 실시간으로 검색 가능 → RAG(검색 기반 응답) 구성의 핵심

---

### 🛠 실전 팁 & 기술 포인트

- Google Cloud Console 사용 시 **항상 프로젝트 이름과 선택 상태 확인**
- `API Key` 보안 설정 중요 → 반드시 사용할 API로 제한
- `Custom Search Engine` 의 `CX 값` 은 별도 개발자 페이지에서 확인 가능
- HTTP Request 노드의 **Query 파라미터 3종**: `cx`, `key`, `q` 반드시 정확히 설정
- `디스크립션` 필드에 기능 설명 없을 경우 AI가 해당 노드를 무시함
- Gemini는 API 가격이 저렴하지만, **성능은 상황에 따라 OpenAI 대비 떨어질 수 있음**

---

사이트 링크:

- [구글 클라우드 콘솔](https://cloud.google.com/cloud-console?hl=ko)
- [Gemini API Pricing](https://ai.google.dev/gemini-api/docs/pricing?hl=ko)
- [프로그래밍 검색 엔진 공식 문서](https://developers.google.com/custom-search/docs/tutorial/creatingcse?hl=ko)
- [구글 프로그래밍 검색 엔진 제어판](https://programmablesearchengine.google.com/controlpanel/create?hl=ko)