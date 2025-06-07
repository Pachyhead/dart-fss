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
  - 관련 이슈: https://github.com/Pachyhead/dart-fss/issues/3

# Requirements
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

# How to Install & Run

### Directory Structure
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
