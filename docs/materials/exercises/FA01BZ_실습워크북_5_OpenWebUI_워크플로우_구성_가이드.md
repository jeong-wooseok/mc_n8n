## OpenWebUI 워크플로우 구성 가이드 (순서별 가이드)

### 1. 개요

OpenWebUI와 n8n을 연동하여 AI 채팅 인터페이스를 구축하는 워크플로우입니다. 이 가이드에서는 OpenWebUI의 **함수(Pipe)** 기능을 사용하여 n8n 워크플로우를 호출하고, **보안 토큰(Bearer Token)**을 이용해 안전하게 통신하는 방법을 다룹니다.

참고영상 바로가기
```
https://www.youtube.com/watch?v=qj8gt5wr22M&list=PLuySsHNr_M9KhsnbaoLuSJw3rpGZidvS8&index=20
```


### 2. 사전 준비사항

- n8n 환경이 구축되어 있어야 합니다.
- OpenWebUI 서비스가 실행 중이어야 합니다 (포트 3000).
- OpenAI API 키가 필요합니다.
- `4_1_n8n_openwebui함수.json` 파일이 준비되어 있어야 합니다.

### 3. 워크플로우 구성 요소

#### 3.1 Webhook 노드
- **HTTP 메서드**: POST
- **경로**: invoke
- **역할**: OpenWebUI 함수에서 전송되는 채팅 메시지를 수신하는 보안 진입점

#### 3.2 AI Agent 노드
- **프롬프트 타입**: define
- **입력 텍스트**: `{{ $json.body.chatInput }}`
- **역할**: 사용자 입력을 분석하고 적절한 AI 응답을 생성

#### 3.3 OpenAI Chat Model 노드
- **모델**: gpt-4.1-mini
- **역할**: 자연어 처리 및 응답 생성을 담당하는 AI 엔진
- **필수 설정**: OpenAI API 키 인증 정보

#### 3.4 Respond to Webhook 노드
- **역할**: 처리된 AI 응답을 OpenWebUI 함수로 반환
- **응답 데이터**: JSON 형식으로 `{"output": "AI 응답 내용"}` 구조를 가집니다.

### 4. 단계별 설정 방법

#### 4.1 새 워크플로우 생성 및 인증 설정
1. n8n 워크플로우 편집 화면에서 **새 워크플로우**를 생성합니다.
2. **Credentials** 메뉴에서 **New**를 클릭합니다.
3. **Header Auth**를 선택하고, 원하는 **Credential name**과 **Header Value**(예: `my-secret-token`)를 입력한 후 저장합니다. 이 값은 OpenWebUI와 n8n이 통신할 때 사용할 비밀 키입니다.

#### 4.2 Webhook 노드 설정
1. **Webhook** 노드를 캔버스에 추가합니다.
2. 노드 설정:
   - **HTTP Method**: `POST`
   - **Path**: `invoke`
   - **Response Mode**: `Response Node` 선택
3. 설정 완료 후 **웹훅 URL**을 복사해 둡니다.

#### 4.3 AI Agent 노드 설정
1. **AI Agent** 노드를 Webhook 노드 다음에 추가합니다.
2. 노드 설정:
   - **Text**: `{{ $json.body.chatInput }}` 입력

#### 4.4 OpenAI Chat Model 노드 설정
1. **OpenAI Chat Model** 노드를 추가하고 AI Agent의 **Language Model** 입력으로 연결합니다.
2. **Model**: `gpt-4.1-mini` 선택
3. **Credentials**: OpenAI API 키 설정

#### 4.5 Respond to Webhook 노드 설정
1. **Respond to Webhook** 노드를 마지막에 추가하고 AI Agent 노드의 출력을 연결합니다.
2. **Response Data**: `Use Response Body` 선택
3. **Source**: `JSON`
4. **Property Name**: `output`
5. **Value**: `{{ $json.output }}` (또는 AI Agent에서 나온 결과값을 나타내는 표현식)
   - 이렇게 설정하면 `{"output": "AI가 생성한 답변"}` 형태로 응답이 구성됩니다.

### 5. OpenWebUI 연동 설정

#### 5.1 OpenWebUI 접속 및 함수 생성
1. 브라우저에서 `http://localhost:3000`으로 접속하여 관리자 계정으로 로그인합니다.
2. 좌측 메뉴에서 **Admin Panel** > **Functions**로 이동합니다.
3. **New function** 버튼을 클릭합니다.
4. `4_1_n8n_openwebui함수.json` 파일을 열어 모든 내용을 복사한 뒤, 함수 편집기에 붙여넣고 **Import** 버튼을 누릅니다.

#### 5.2 함수(Pipe) 설정
1. 생성된 **N8N Pipe** 함수를 클릭하여 설정 화면으로 들어갑니다.
2. **Valves** 섹션에서 다음 값을 수정합니다.
   - **n8n_url**: n8n Webhook 노드에서 복사한 URL을 붙여넣습니다. (예: `http://localhost:5678/webhook/invoke`)
   - **n8n_bearer_token**: n8n에서 설정한 Header Auth의 **Header Value** (비밀 키)를 입력합니다. (예: `my-secret-token`)
3. 우측 상단의 **Save** 버튼을 눌러 설정을 저장합니다.

### 6. 실제 동작 테스트

1. OpenWebUI 채팅 화면으로 돌아옵니다.
2. 모델 선택 목록에서 방금 생성한 **N8N Pipe**를 선택합니다.
3. 새 채팅을 시작하고 메시지를 입력합니다.
4. n8n 워크플로우를 통해 AI 응답이 정상적으로 반환되는지 확인합니다.

### 7. 문제해결

#### 7.1 401 Unauthorized 오류 발생 시
- n8n Webhook 노드에 설정된 **Header Auth**와 OpenWebUI 함수에 입력한 **n8n_bearer_token** 값이 일치하는지 확인하세요.

#### 7.2 응답을 받지 못하는 경우
- OpenWebUI 함수 설정의 **n8n_url**이 정확한지 확인하세요.
- n8n 워크플로우가 **활성화(Active)** 상태인지 확인하세요.
- **Respond to Webhook** 노드의 `output` 필드 설정이 올바른지 확인하세요.

### 참고 자료

- OpenWebUI 공식 문서: https://docs.openwebui.com/
- n8n 공식 문서: https://docs.n8n.io/
- OpenAI API 문서: https://platform.openai.com/docs/

**이 가이드는 OpenWebUI와 n8n을 활용한 AI 채팅 시스템 구축을 위한 실습 가이드입니다.**

<div style="text-align: center">⁂</div>

**📅 작성일**: 2025년 7월 6일  
**🔄 최종 업데이트**: 2025년 7월 6일  
**📝 작성자**: 일하는 ai