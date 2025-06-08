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
## 1단계: tar 파일을 docker image로 로드
```
docker load -i final_2022040020.v1.tar
```
## 2단계: 로드된 image로부터 docker container 생성
```
docker run -dit final_2022040020:v1
```
## 3단계: 생성된 docker container id 확인
```
docker ps
```
이후 final_2022040020:v1에 해당하는 id를 찾음
## 4단계: docker container에 진입
```
docker exec -it <container id> /bin/bash
```
## 5단계: 홈 디렉토리에 진입
```
# cd ~
```
## 6단계: test.py 파일 실행
```
python3 test.py
```
이후 등장하는 "Error occurred during getting browser(s): random, but was suppressed with fallback." 메시지는 cli 환경에서 적절한 브라우저를 찾지 못해 발생하는 것으로 이후 동작에 영향을 주지 않음
## 7단계: fsdata/ 디렉토리에 적절한 파일이 생성되었는지 확인
test.py 파일에 따르면, is.json, bs.json ,cf.json, cis.json 파일이 생성되어 있어야 함

## 8단계: 실행 종료 및 컨테이너 나오기
```
q 입력
```

# Directory Structure
```
.
├── .github/                     # GitHub 관련 설정 폴더
│   └── workflows/               # GitHub Actions 워크플로 설정
│       └── test_all.yml         # 다양한 파이썬 버전에서 코드를 테스트하는 워크플로
├── dart_fss/                    # 메인 라이브러리 소스 코드 패키지
│   ├── __init__.py              # 패키지 초기화 및 주요 기능 노출
│   ├── _version.py              # Git 태그를 기반으로 버전 정보를 관리
│   ├── api/                     # Open DART API 인터페이스
│   │   ├── __init__.py          # API 모듈을 패키지로 인식
│   │   ├── filings/             # 공시 정보 관련 API (회사 정보, 문서 다운로드 등)
│   │   ├── finance/             # 재무 정보 관련 API (XBRL 재무제표 등)
│   │   ├── info/                # 사업보고서의 주요 정보 관련 API
│   │   ├── issue/               # 발행 관련 주요사항 보고서 API
│   │   ├── market/              # 시장 관련 API (상장, 거래정지 등)
│   │   ├── registration/        # 증권신고서 관련 API
│   │   ├── shareholder/         # 지분공시 종합 정보 API
│   │   └── helper.py            # API 요청을 위한 헬퍼 함수
│   ├── auth/                    # API 인증키 설정 및 관리
│   │   └── auth.py              # API 키 설정 및 유효성 검증 로직
│   ├── corp/                    # 회사 정보 관리
│   │   ├── corp.py              # 개별 회사(Corp) 객체 정의
│   │   └── corp_list.py         # 전체 회사 목록(CorpList) 관리 및 검색
│   ├── errors/                  # 사용자 정의 에러
│   │   ├── checker.py           # API 응답 상태를 확인하고 적절한 에러를 발생
│   │   └── errors.py            # API 키 오류, 데이터 없음 등 사용자 정의 예외 클래스 정의
│   ├── filings/                 # 공시(보고서) 검색 및 처리
│   │   ├── pages.py             # 공시 보고서 내의 개별 페이지(Page) 객체 정의
│   │   ├── reports.py           # 공시 보고서(Report) 객체 및 관련 파일/보고서 처리
│   │   ├── search.py            # 공시 보고서 검색 기능
│   │   ├── search_result.py     # 검색 결과를 담는 SearchResults 객체 정의
│   │   └── xbrl_viewer.py       # DART XBRL 뷰어 페이지 처리
│   ├── fs/                      # 재무제표 추출 및 표현
│   │   ├── extract.py           # 재무제표 추출 로직
│   │   └── fs.py                # 추출된 재무제표를 담는 FinancialStatement 객체 정의
│   ├── tests/                   # 라이브러리 테스트 코드
│   │   ├── test_api_filings.py  # 공시 정보 API 테스트
│   │   ├── test_corp.py         # 회사 정보 관련 기능 테스트
│   │   └── test_fs.py           # 재무제표 추출 기능 테스트
│   ├── utils/                   # 공통 유틸리티 함수
│   │   ├── cache.py             # 캐시 데코레이터
│   │   ├── file.py              # 파일 처리(압축 해제, 폴더 생성 등) 유틸리티
│   │   ├── request.py           # HTTP 요청을 보내기 위한 클래스
│   │   └── string.py            # 문자열 처리(비교, 단위 변환 등) 유틸리티
│   └── xbrl/                    # XBRL 데이터 파싱 및 처리
│       ├── dart_xbrl.py         # XBRL 문서를 관리하는 DartXbrl 객체 정의
│       └── xbrl.py              # XBRL 파일 로딩 함수
├── docs/                        # 문서화 관련 파일
│   ├── conf.py                  # Sphinx 문서 생성기 설정 파일
│   ├── requirements.txt         # 문서 생성을 위한 의존성 목록
│   └── *.rst                    # reStructuredText 형식의 문서 소스 파일
├── fsdata/                      # test.py 실행 후 결과 파일이 저장되는 폴더
├── test.py                      # test 시 사용할 임시 파일       
├── .readthedocs.yml             # Read the Docs 빌드 설정 파일
├── conftest.py                  # Pytest 테스트 환경 설정 파일
├── README.md                    # 프로젝트 개요 및 설치/사용법 안내
├── requirements.txt             # 프로젝트 실행에 필요한 라이브러리 목록
├── setup.cfg                    # 패키지 설정 파일 (versioneer 등)
├── setup.py                     # 패키지 설치 및 배포 스크립트
└── versioneer.py                # Git 태그를 이용해 버전 번호를 관리하는 도구
```


