---
**네이버 검색 API를 연동하고**, 이를 **AI 챗봇의 툴로 구성** 하는 방법을 실습합니다. 나아가 **구글 검색 API와 통합된 검색 기능** 을 구현하고, 복수 검색 결과를 **머지 및 어그리게이션** 하여 사용자에게 반환하는 워크플로우 구성법도 다룹니다.

---

### 📌 주요 내용 요약

#### 1\. 네이버 검색 API 연동 준비

- 네이버 개발자 센터에 별도 가입 필요
- "애플리케이션 등록"에서 검색 API 사용 설정
- 인증 방식은 `Client ID` 와 `Client Secret` 을 **HTTP 헤더** 로 전달
- 하루 **25,000건 무료** 제공

#### 2\. N8N에서 HTTP Request 노드 구성

- `HTTP Request` 노드 생성 → 네이버 검색 URL (`https://openapi.naver.com/v1/search/blog.json`)
- 헤더에 `X-Naver-Client-Id`, `X-Naver-Client-Secret` 추가
- 파라미터는 `query` 로 설정 → AI 에이전트가 입력

#### 3\. 툴로 구성하여 AI가 활용할 수 있도록 설정

- `Call N8N Workflow Tool` 을 활용해 별도의 검색 워크플로우 등록
- AI Agent는 해당 워크플로우를 **툴처럼 호출 가능**
- 외부 입력 변수(`query`)를 기반으로 네이버 및 구글 검색 실행

#### 4\. 검색 결과 머지 및 반환 처리

- 네이버, 구글, 웹 검색 결과를 각각 수집
- `Merge` 노드로 결과 통합 (3개까지 병합 가능)
- `Aggregate` 노드를 통해 `items` 리스트를 하나로 병합
- 검색 결과가 JSON 형태로 AI에 반환되어 응답 생성에 사용됨

#### 5\. 검색 종류 확장: 블로그, 웹, 뉴스 등

- 네이버 API는 `blog`, `webkr`, `news` 등 다양하게 사용 가능
- 주소 뒤의 path만 바꾸면 됨 (예: `blog.json` → `webkr.json`)
- 필요 시 노드 복제 후 주소만 변경

#### 6\. 테스트 및 디버깅 팁

- 워크플로우에 `Manual Trigger` 와 `Set` 노드를 추가하여 **로컬 테스트 가능**
- AI Agent 호출 없이 변수(`query`) 설정 후 단독 테스트 가능
- 복수 API 결과의 구조가 다를 경우에도 `items` 기준으로 머지 가능

#### 7\. 퍼블릭 공유 방법

- `Make Chat Publicly Available` 옵션으로 외부 공유 URL 생성 가능
- 공유기에서 **5678 포트 포워딩** 필요
- 공유 URL을 통해 다른 사람도 직접 검색 챗봇 체험 가능

---

### 🛠 실전 팁 & 기술 포인트

- **API 헤더 인증**: 대부분의 국내 API는 Key 방식보다 Header 기반 인증이 많음 → 패턴 익히면 쉽게 재활용 가능
- **HTTP Request 노드 범용성**: OpenAI, Google, Naver 등 거의 모든 외부 API와 통신 가능
- **워크플로우 재사용성**: 기능별로 모듈화된 워크플로우를 별도 툴로 등록하면 재활용 및 유지보수 용이
- **검색 결과 포맷 통일 어려움**: 네이버/구글은 반환 필드가 다르므로, 결과 통합 시 `items` 만 기준 삼아 처리
- **AI Agent 구조 분리**: 툴 기능만 분리한 서브 워크플로우를 만들면 메인 챗봇이 깔끔하게 동작 가능

---

사이트 링크:

- [네이버 서비스 API 공식 문서](https://developers.naver.com/docs/serviceapi/search/blog/blog.md#%EB%B8%94%EB%A1%9C%EA%B7%B8)
- [네이버 개발자 센터](https://developers.naver.com/main/)
- [네이버 개발자 센터 어플리케이션 등록](https://developers.naver.com/apps/#/register)

---

네이버 검색 API 주소:

```bash
https://openapi.naver.com/v1/search/blog
https://openapi.naver.com/v1/search/webkr
```

---

구글 검색 API 주소:

```bash
https://www.googleapis.com/customsearch/v1
```