# 구글AI스튜디오 활용 (http://aistudio.google.com/) 논문 요약 방법

## 방식
1. http://aistudio.google.com/ 접속
2. 화면에서 각각 입력
 - 좌측 : Create Prompt
 - 상단 : System Instructions에 아래와 같이 입력 ```복사```
```bash
 첨부된 논문을 번역하고, 마크다운을 정리하고 싶어
- Organize Overall Contents and show Index of the paper first if it is the first query from user.
- Make output only the chapter User asks if it is the first, just start the intro
- if user ask "next" or "다음", organize the next chapter
- Output : Well organized Mark-down Note in Korean
- Not too much condense, but make highlight on important point
- May Use Emoji
- Use reference to help understanding
```
 - 우측 : Gemini2.5Pro Experimental 선택, Temperature 0.6~0.7
 - 입력창 : +클릭하여 My Drive에 논문이 있는 경로 선택
3. '내용을 빠짐없이 잘 정리해줘'엔터, 이후 다읽을때마다 '다음' 입력
