# Git 및 GitHub 설치 및 초기 설정 가이드

## Git 설치하기

### Windows에서 설치하기

1. Git 공식 웹사이트(https://git-scm.com/downloads)에서 최신 버전을 다운로드합니다
2. 다운로드한 설치 파일을 실행합니다
3. 라이선스 동의 후 "Next"를 클릭합니다
4. 설치 위치를 선택하고 "Next"를 클릭합니다
5. 컴포넌트 선택 화면에서 기본 설정을 유지하고 "Next"를 클릭합니다
6. 시작 메뉴 폴더 선택 후 "Next"를 클릭합니다
7. Git에서 사용할 기본 에디터 선택 후 "Next"를 클릭합니다
8. "Git from the command line and also from 3rd-party software" 옵션 선택 후 "Next"를 클릭합니다
9. "Use OpenSSH" 옵션 선택 후 "Next"를 클릭합니다
10. "Use the OpenSSL library" 옵션 선택 후 "Next"를 클릭합니다
11. "Checkout Windows-style, commit Unix-style line endings" 옵션 선택 후 "Next"를 클릭합니다
12. "Use MinTTY" 옵션 선택 후 "Next"를 클릭합니다
13. 기본 옵션 유지하고 "Install"을 클릭합니다

### macOS에서 설치하기

1. 터미널을 엽니다
2. Homebrew가 설치되어 있다면 다음 명령어를 실행합니다:

```bash
brew install git
```

3. 또는 Git 공식 웹사이트에서 macOS용 설치 파일을 다운로드하여 설치할 수 있습니다

### Linux에서 설치하기

Ubuntu/Debian:

```bash
sudo apt-get update
sudo apt-get install git
```

Fedora:

```bash
sudo dnf install git
```


## GitHub Desktop 설치하기

### Windows 및 macOS

1. GitHub Desktop 공식 웹사이트(https://desktop.github.com)에 접속합니다
2. "Download for Windows" 또는 "Download for macOS" 버튼을 클릭합니다
3. 다운로드한 설치 파일을 실행합니다
4. 설치가 완료되면 GitHub Desktop이 자동으로 실행됩니다

### Linux

GitHub Desktop은 공식적으로 Linux를 지원하지 않지만, 커뮤니티 버전을 사용할 수 있습니다:

```bash
sudo apt-get install gdebi-core
wget https://github.com/shiftkey/desktop/releases/download/release-2.9.3-linux3/GitHubDesktop-linux-2.9.3-linux3.deb
sudo gdebi GitHubDesktop-linux-2.9.3-linux3.deb
```


## Git 초기 설정

### 사용자 정보 설정

```bash
git config --global user.name "홍길동"
git config --global user.email "이메일@example.com"
```


### 기본 브랜치명 설정

```bash
git config --global init.defaultBranch main
```


### 에디터 설정

```bash
git config --global core.editor "code --wait"  # VS Code 사용 시
```


### 설정 확인

```bash
git config --list
```


## GitHub 계정 설정

### GitHub 계정 생성

1. GitHub 웹사이트(https://github.com)에 접속합니다
2. "Sign up" 버튼을 클릭합니다
3. 이메일 주소, 비밀번호, 사용자명을 입력합니다
4. 계정 생성 과정을 완료합니다

### SSH 키 생성 및 GitHub에 등록

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
