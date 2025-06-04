# Dart-fss
대한민국 금융감독원에서 운영하는 다트([DART](https://dart.fss.or.kr)) 사이트 크롤링 및 재무제표 추출을 위한 라이브러리

# Goal

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
