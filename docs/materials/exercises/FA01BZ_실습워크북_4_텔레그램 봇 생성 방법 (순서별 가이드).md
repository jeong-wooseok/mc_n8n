
## 텔레그램 봇 생성 방법 (순서별 가이드)

### 1. 텔레그램 앱 설치 및 로그인

- 텔레그램 앱을 PC 또는 모바일에 설치하고 로그인합니다.


### 2. BotFather 찾기

- 텔레그램 내 검색창에서 **BotFather**를 검색하여 채팅방에 들어갑니다.


### 3. 새 봇 생성 명령어 입력

- 채팅창에 `/newbot`을 입력하고 전송합니다.
- BotFather가 봇 생성을 안내하는 메시지를 보냅니다[^1][^2][^3].


### 4. 봇 이름 입력

- 원하는 봇의 **이름**을 입력합니다. (예: MyFirstBot)


### 5. 봇 사용자 이름 입력

- 봇의 **사용자 이름(username)** 을 입력합니다.
- 반드시 `_bot` 또는 `bot`으로 끝나야 합니다. (예: myfirst_test_bot)


### 6. API 토큰 발급 확인

- BotFather가 **API 토큰**(HTTP API token)을 발급해줍니다.
- 이 토큰은 봇을 프로그래밍할 때 사용하므로 안전하게 보관합니다.


### 7. 봇 채팅방 접속 및 시작

- BotFather가 안내한 봇 주소를 클릭해 봇 채팅방에 접속합니다.
- "시작" 버튼을 눌러 봇을 활성화합니다[^3][^4].


### 8. 봇 프로그래밍 및 활용

- 발급받은 API 토큰을 사용해 파이썬 등으로 봇을 개발할 수 있습니다.
- 예시 라이브러리: `python-telegram-bot`, `telepot` 등
- 기본적인 메시지 전송, 자동응답, 알림 기능 등을 구현할 수 있습니다[^1][^5].

## 문제해결
- 챗ID가 오류가 나는 등 문제가 발생할 경우, 웹훅 설정을 확인하려면 다음과 같이 입력합니다.
```
https://api.telegram.org/bot{my_bot_token}/getWebhookInfo
```
- 웹훅을 설정하려면, 다음 URL 형식을 사용하여 GET 요청을 합니다:
```
https://api.telegram.org/bot{my_bot_token}/setWebhook?url={내 웹훅주소}
```
- localhost의 경우 ngrok등을 활용하여 고정된 주소로 웹훅을 지정하세요


#### 참고

- BotFather는 봇의 이름, 설명, 프로필 사진, 명령어 등 다양한 설정을 지원합니다.
- 봇을 그룹에 추가하거나, 관리자 권한을 부여할 수도 있습니다.

**이 가이드는 텔레그램 공식 및 다양한 개발자 블로그의 정보를 바탕으로 작성되었습니다.**[^1][^2][^3][^4]

<div style="text-align: center">⁂</div>

[^1]: https://dodonam.tistory.com/357

[^2]: https://wikidocs.net/185060

[^3]: https://blog.cosmosfarm.com/archives/1070/텔레그램-봇-telegram-bot-만들기/

[^4]: https://recording-it.tistory.com/105

[^5]: https://steemit.com/krdev/@maanya/30

[^6]: https://junesker.tistory.com/6

[^7]: https://jow1025.tistory.com/317

[^8]: https://hogni.tistory.com/77

[^9]: https://midoriiroplace.tistory.com/62

[^10]: https://apidog.com/kr/blog/beginners-guide-to-telegram-bot-api-2/

# 텔레그램 봇 웹훅 설정 방법

## 웹훅 설정 프로세스

**기본 개념**
웹훅은 텔레그램 봇이 봇 API를 수동으로 폴링하는 대신 자동으로 업데이트를 받을 수 있게 해줍니다.

**설정 방법**
웹훅을 설정하려면, 다음 URL 형식을 사용하여 GET 요청을 합니다:
```
https://api.telegram.org/bot{my_bot_token}/setWebhook?url={ngrok웹훅주소}
```
Where:
- 내_봇_토큰`: BotFather의 봇 토큰
- `url_to_send_updates_to`: 봇 업데이트를 처리하는 HTTPS 엔드포인트


**검증**
웹훅 설정을 확인하려면 다음을 사용합니다:
```
https://api.telegram.org/bot{my_bot_token}/getWebhookInfo


```

성공적인 응답은 다음과 같습니다:
```json
{
 “ok": true,
 “결과": {
   “url": “https://www.example.com/my-telegram-bot/”,
   “has_custom_certificate": false,
   “pending_update_count": 0,
   “max_connections": 40
 }
}
```

