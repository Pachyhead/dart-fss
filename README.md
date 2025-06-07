# Dart-fss
대한민국 금융감독원에서 운영하는 다트([DART](https://dart.fss.or.kr)) 사이트 크롤링 및 재무제표 추출을 위한 라이브러리

# Goal
- 크롤링 데이터 저장 시 json 형식으로도 저장할 수 있도록 기능 추가
  - 기존에는 xlsx 형식 저장만 지원
  - 관련 이슈: https://github.com/Pachyhead/dart-fss/issues/1
- 크롤링 데이터 저장 시 금액 단위를 사용자가 지정할 수 있도록 기능 추가
  - 지원 단위: 원, 십원, 백원, 천원, 만원, 십만원, 백만원, 천만원, 억원
  - 관련 이슈: https://github.com/Pachyhead/dart-fss/issues/2
- KRX 사이트가 다운된 경우, 크롤링 과정에서 거래정지 목록을 조회하는데 실패하고 프로그램이 예상치 못하게 종료되는 문제를 개선
  - 사용자 입력을 통해 거래정지 목록 조회 과정을 SKIP하고 진행할지 여부 선택
  - 참고: 현재는 KRX 사이트의 문제가 해결되어 정상적으로 거래정지 목록을 조회함.
    - 발생 이력: https://github.com/josw123/dart-fss/issues/193
    - 개인 이슈 트래킹: https://github.com/Pachyhead/dart-fss/issues/3
    - 문제 시점에서 테스트한 Github Action: https://github.com/Pachyhead/dart-fss/actions/runs/15497416263

# Requirements
#### 운영체제
- ubuntu-latest: Ubuntu 24.04 (2025.06.08 기준) 
  - 소스: https://github.com/actions/runner-images
 
#### 파이썬 버전
- 3.9, 3.10, 3.11
 
#### 파이썬 라이브러리
- xmltodict
- requests
- arelle-release
- numpy
- pandas
- tqdm
- yaspin
- fake-useragent>=1.5
- beautifulsoup4
- appdirs

> 테스트 github action: https://github.com/Pachyhead/dart-fss/actions/runs/15510087027

# How to Install & Run
## 1단계: Docker 설치하기

만약 Docker가 설치되어 있지 않다면, 먼저 Docker를 설치해야 합니다.

* **Windows 또는 macOS 사용자:**
    * [Docker Desktop 공식 다운로드 페이지](https://www.docker.com/products/docker-desktop/)에 접속하여 운영체제에 맞는 설치 파일을 다운로드하고 실행하여 설치를 완료합니다.
* **Linux 사용자:**
    * 각 배포판의 공식 문서에 따라 Docker를 설치합니다. (예: [Ubuntu Docker 설치 가이드](https://docs.docker.com/engine/install/ubuntu/))

설치가 완료되면 터미널(Windows의 경우 PowerShell 또는 CMD, macOS/Linux의 경우 터미널)에서 다음 명령어를 입력하여 Docker가 정상적으로 설치되었는지 확인합니다.

```bash
docker --version
```
Docker 버전 정보가 출력되면 정상적으로 설치된 것입니다.

## 2단계: 전달받은 Docker 이미지 불러오기
전달받은 Docker 이미지 파일(예: dart-app.tar)을 로컬 Docker 환경으로 불러옵니다. 터미널에서 아래 명령어를 실행하세요. 이미지 파일이 있는 경로에서 실행하거나, 파일 경로를 정확히 지정해야 합니다.
```
docker load -i dart-app.tar
```

```
dart_fss/
├── __init__.py        # dart_fss 패키지의 진입점, 주요 기능 노출
├── _version.py        # versioneer를 통해 관리되는 라이브러리 버전 정보
├── api/               # Open DART API와의 상호작용 관리
│   ├── filings/       #   └ 공시 정보 API
│   ├── finance/       #   └ 재무 정보 API
│   ├── info/          #   └ 사업보고서 주요 정보 API
│   ├── issue/         #   └ 주요사항보고서 API (발행 관련)
│   ├── market/        #   └ 시장 관련 API (상장, 거래정지 등)
│   ├── registration/  #   └ 증권신고서 API
│   └── shareholder/   #   └ 지분공시 종합 정보 API
├── auth/              # Open DART API 인증키 설정 및 관리
├── corp/              # 회사(종목) 정보 표현 및 관리
├── errors/            # 라이브러리 내 사용자 정의 오류 클래스 정의
├── filings/           # 회사 공시(보고서) 검색 및 처리
├── fs/                # 재무제표 추출 및 표현
├── tests/             # 라이브러리 기능 및 정확성 검증을 위한 테스트 스크립트
├── utils/             # 라이브러리 전반에서 사용되는 공통 유틸리티 함수 모음
└── xbrl/              # XBRL(재무보고용 국제표준 전산언어) 데이터 파싱 및 처리
```

Error occurred during getting browser(s): random, but was suppressed with fallback.
