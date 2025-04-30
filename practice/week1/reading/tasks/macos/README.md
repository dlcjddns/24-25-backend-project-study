# REST API 개발 환경 구축(MacOS)

## Git
**만일 Git이 설치되어 있지 않다면**, 아래 절차를 따라 Git을 설치합니다. 이미 설치되어 있다면, 아래 [설치 확인](#설치-확인)을 진행하여 설치가 진행되었는지 확인하고, 설치되어 있을 경우 이 단계를 건너뛰셔도 됩니다.

#### 방법 1: Homebrew 사용 (권장)

터미널을 열고 Homebrew가 설치되어 있는지 확인합니다:
```bash
brew --version
```

Homebrew가 설치되어 있지 않다면, 다음 명령어로 설치합니다:
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Git을 설치합니다:
```bash
brew install git
```

#### 방법 2: 설치 프로그램 사용

[Git 공식 웹사이트](https://git-scm.com/download/mac)에서 macOS용 설치 프로그램을 다운로드합니다.

다운로드한 .dmg 파일을 실행하고 안내에 따라 설치합니다.

### 설치 확인
터미널에서 다음 명령어를 실행하여 Git이 올바르게 설치되었는지 확인합니다:
```bash
git --version
```
현재 Git 프로그램의 버전이 나온다면 성공입니다. 만약 git 명령어를 찾지 못한다고 나올 경우, 재설치를 진행하여야 합니다.

### 개인 정보 설정
설치 완료 후, Commit 메시지 등에 추가될 개인 정보를 설정합니다. 이메일은 **"반드시"** Github에서 사용하는 이메일이어야 합니다!
```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

## Docker Desktop
### 시스템 요구사항

- macOS 11 Big Sur 이상
- Apple Silicon(M1/M2) 또는 Intel 칩
- 최소 4GB RAM

### 설치 과정

1. Docker Desktop 공식 웹사이트에서 macOS용 설치 파일을 다운로드합니다.
1. 다운로드한 .dmg 파일을 실행합니다.
1. Docker 아이콘을 Applications 폴더로 드래그합니다.
1. Applications 폴더에서 Docker를 실행합니다.
1. 초기 설정을 완료합니다. 필요에 따라 권한 요청에 동의합니다.

설치가 정상적으로 진행되지 않을 경우 Homebrew를 통한 설치 (대안)
```bash
brew install --cask docker
```

만일 위 설치 과정에서 오류가 발생하였을 경우, 스터디장에게 물어보시기 바랍니다!