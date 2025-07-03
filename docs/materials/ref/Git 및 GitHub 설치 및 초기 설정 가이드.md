# CH03_03. github 초기 설정가이드

## GitHub 계정 생성

1. GitHub 웹사이트(https://github.com)에 접속합니다
2. "Sign up" 버튼을 클릭합니다
3. 이메일 주소, 비밀번호, 사용자명을 입력합니다
4. 계정 생성 과정을 완료합니다

## SSH 키 생성 및 GitHub에 등록

1. SSH 키 생성:

```bash
ssh-keygen -t ed25519 -C "이메일@example.com"
```

2. SSH 키 복사:
    - Windows: `clip &lt; ~/.ssh/id_ed25519.pub`
    - macOS: `pbcopy &lt; ~/.ssh/id_ed25519.pub`
    - Linux: `cat ~/.ssh/id_ed25519.pub`
3. GitHub 웹사이트에 로그인하고 Settings > SSH and GPG keys > New SSH key로 이동
4. 복사한 키를 붙여넣고 "Add SSH key" 클릭

## 기본 Git 명령어

### 저장소 초기화

```bash
git init
```


### 원격 저장소 복제

```bash
git clone https://github.com/사용자명/저장소명.git
```


### 변경 사항 확인

```bash
git status
```


### 변경 사항 스테이징

```bash
git add 파일명
git add .  # 모든 변경 사항 스테이징
```


### 변경 사항 커밋

```bash
git commit -m "커밋 메시지"
```


### 원격 저장소에 푸시

```bash
git push origin main
```


### 원격 저장소에서 변경 사항 가져오기

```bash
git pull origin main
```


## GitHub Desktop 사용법

### 저장소 복제

1. GitHub Desktop 실행
2. "File" > "Clone Repository" 클릭
3. GitHub 탭에서 복제할 저장소 선택 또는 URL 입력
4. 로컬 경로 선택 후 "Clone" 클릭

### 변경 사항 커밋

1. 변경된 파일 확인
2. 커밋 메시지 입력
3. "Commit to main" 버튼 클릭

### 변경 사항 푸시

1. 커밋 후 "Push origin" 버튼 클릭

### 변경 사항 가져오기

1. "Fetch origin" 버튼 클릭하여 변경 사항 확인
2. 변경 사항이 있으면 "Pull origin" 버튼 클릭
