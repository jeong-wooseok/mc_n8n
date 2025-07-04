
## OpenAI API 키 발급 방법

1. **OpenAI 공식 사이트 접속**
    - [https://platform.openai.com/](https://platform.openai.com/)에 접속합니다.
      ```
      https://platform.openai.com/api-keys
      ```
2. **회원가입 또는 로그인**
    - 계정이 없다면 회원가입(Sign Up), 이미 있다면 로그인(Log In)합니다[^1_1][^1_2].
3. **결제 정보 등록(필요 시)**
    - 일부 서비스는 신용카드 등록이 필요할 수 있습니다. 좌측 메뉴에서 Billing → Payment methods에서 신용카드를 등록합니다[^1_1][^1_3].
4. **API 키 발급**
    - 로그인 후, 우측 상단 프로필 이미지를 클릭하거나, 좌측 메뉴에서 **API Keys** 또는 **API 키 관리** 메뉴로 이동합니다.
    - **Create new secret key** 버튼을 클릭합니다.
    - 키 이름(Name)과 프로젝트(Project)를 입력(선택 사항) 후, **생성(Create)** 버튼을 누릅니다.
    - 생성된 API 키를 복사하여 안전한 곳에 보관합니다. **이 창을 닫으면 다시 확인할 수 없으니 반드시 복사해두세요**[^1_1][^1_4][^1_2].
5. **API 키 사용**
    - 발급받은 키는 환경 변수, .env 파일, 또는 코드 내에 입력하여 사용할 수 있습니다.
    - 예시: `.env` 파일에 `OPENAI_API_KEY=발급받은_키` 형태로 저장[^1_1][^1_2].

## Replicate API 키(토큰) 발급 방법

1. **Replicate 공식 사이트 접속**
    - [https://replicate.com/](https://replicate.com/)에 접속합니다.
2. **회원가입 또는 로그인**
    - 계정이 없다면 회원가입(Sign Up), 이미 있다면 로그인(Log In)합니다[^1_5][^1_6].
3. **API 토큰(키) 발급**
    - 로그인 후, 대시보드 또는 계정 설정(Settings) 페이지로 이동합니다.
    - 좌측 메뉴에서 **API Tokens** 또는 **API 토큰** 항목을 선택합니다.
    - **새 토큰 생성(Create new token)** 버튼을 클릭합니다.
    - 토큰 이름을 입력(선택 사항)하고 **생성(Create)** 버튼을 누릅니다.
    - 생성된 API 토큰을 복사하여 안전한 곳에 보관합니다. **이 토큰은 생성 시에만 확인할 수 있습니다**[^1_7][^1_8][^1_6].
4. **API 토큰 사용**
    - 발급받은 토큰을 환경 변수(`REPLICATE_API_TOKEN`)로 설정하거나, 코드 내에서 인증 헤더에 사용합니다.
    - 예시:

```bash
export REPLICATE_API_TOKEN=발급받은_토큰
```

    - 또는 API 호출 시 헤더에 `Authorization: Bearer 발급받은_토큰`을 추가합니다[^1_8][^1_6].

**참고:**

- 두 서비스 모두 API 키(토큰)는 외부에 노출되지 않도록 주의해야 하며, 유출 시 악용 및 과금 위험이 있습니다.
- 키를 분실한 경우, 새로 발급받아 기존 키를 폐기(삭제)할 수 있습니다.

<div style="text-align: center">⁂</div>

[^1_1]: https://wikidocs.net/233342

[^1_2]: https://wikidocs.net/196075

[^1_3]: https://velog.io/@ji1kang/OpenAI의-API-Key-발급-받고-테스트-하기

[^1_4]: https://photalk.tistory.com/88

[^1_5]: https://www.toolify.ai/ko/ai-news-kr/ai-api-chatgpt-openai-assemblyai-replicate-2653427

[^1_6]: https://www.toolify.ai/ko/ai-news-kr/replicate-2623733

[^1_7]: https://apidog.com/kr/blog/replicate-api-3/

[^1_8]: https://www.jiniai.biz/2024/12/16/replicate-api-누구나-쉽게-ai-모델을-활용하는-방법/

[^1_9]: https://mixedcode.com/blog/detail?pid=6

[^1_10]: https://herojoon-dev.tistory.com/247

[^1_11]: https://blog.highoutputclub.com/how-to-use-chatgpt-api-with-replit/

[^1_12]: https://lobehub.com/ko/mcp/gerred-mcp-server-replicate

[^1_13]: https://replicate.com/docs/reference/http

[^1_14]: https://wikidocs.net/202060

[^1_15]: https://marcus-story.tistory.com/66

[^1_16]: https://lobehub.com/ko/mcp/pierrunoyt-replicate-flux-kontext-max-mcp-server

[^1_17]: https://platform.openai.com/api-keys

[^1_18]: https://guide.bati.ai/service/api/ai

