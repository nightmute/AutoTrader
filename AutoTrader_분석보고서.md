# AutoTrader 프로젝트 분석 보고서

## 📋 프로젝트 개요
**AutoTrader**는 Python 기반의 알고리즘 트레이딩 자동화 플랫폼입니다. 현재 **더 이상 유지보수되지 않는 아카이브 상태**이며, v1.1.2가 최종 버전입니다.

- **개발자**: Kieran Mackle
- **라이선스**: GNU General Public License v3.0
- **현재 상태**: 아카이브됨 (유지보수 중단)
- **최종 버전**: 1.1.2
- **PyPI 다운로드**: 월간 다운로드 수 존재 (배지 표시)

## 🏗️ 아키텍처 구조

### 핵심 컴포넌트
- **AutoTrader**: 메인 엔진 (`autotrader.py`)
- **AutoBot**: 전략별 봇 인스턴스 (`autobot.py`) 
- **Strategy**: 추상 전략 클래스 (`strategy.py`)
- **AutoPlot**: 시각화 도구 (`autoplot.py`)
- **Brokers**: 다중 브로커 지원 모듈
- **Indicators**: 기술적 지표 라이브러리

### 폴더 구조
```
autotrader/
├── autotrader.py       # 메인 엔진
├── autobot.py         # 봇 관리
├── strategy.py        # 전략 인터페이스
├── autoplot.py        # 시각화
├── indicators.py      # 기술 지표
├── utilities.py       # 유틸리티
├── brokers/          # 브로커 통합
│   ├── virtual.py    # 백테스팅
│   ├── oanda.py      # Oanda API
│   ├── ib.py         # Interactive Brokers
│   ├── ccxt.py       # 암호화폐 거래소
│   └── yahoo.py      # Yahoo Finance
└── comms/            # 알림 시스템
    ├── notifier.py   # 알림 인터페이스
    └── tg.py         # 텔레그램 통합
```

## 🔌 브로커 통합 현황

| 브로커 | 자산 클래스 | 상태 |
|--------|-------------|------|
| **Virtual Broker** | 모든 자산 | ✅ 완전 지원 (백테스팅) |
| **Oanda** | Forex CFDs | ✅ 완전 지원 |
| **Interactive Brokers** | 다양한 자산 | 🔄 진행 중 |
| **CCXT** | 암호화폐 | 🔄 진행 중 |
| **Yahoo Finance** | 주식 데이터 | ✅ 데이터 제공 |

### 브로커별 특징
- **Virtual Broker**: 완전한 백테스팅 환경, 모든 주문 유형 시뮬레이션
- **Oanda**: v20 API 사용, 실시간 Forex 거래 지원
- **Interactive Brokers**: ib_insync 라이브러리 활용
- **CCXT**: 다중 암호화폐 거래소 지원 (200+ 거래소)

## 💻 기술 스택

### 핵심 의존성
- **Python 3.11+** (최신 Python 요구)
- **pandas >= 1.3.4** - 데이터 처리 및 조작
- **numpy >= 1.20.3** - 수치 계산
- **bokeh == 3.3.0** - 대화형 차트 (버전 고정)
- **scipy >= 1.7.1** - 수치 계산 및 최적화
- **finta >= 1.3** - 기술적 지표 라이브러리

### 브로커별 의존성
- **v20 >= 3.0.25.0** - Oanda API
- **ib_insync >= 0.9.70** - Interactive Brokers
- **ccxt >= 2.0.53** - 암호화폐 거래소
- **yfinance >= 0.1.67** - Yahoo Finance 데이터

### 기타 기능 라이브러리
- **python-telegram-bot >= 13.14** - 텔레그램 알림
- **prometheus-client >= 0.15.0** - 메트릭 모니터링
- **tqdm >= 4.64.0** - 진행률 표시
- **click >= 8.1.3** - CLI 인터페이스
- **requests >= 2.28.1** - HTTP 클라이언트
- **PyYAML** - 설정 파일 처리

### 개발 도구
- **pytest >= 7.1.1** - 테스트 프레임워크
- **black >= 22.10.0** - 코드 포매터
- **sphinx** 관련 패키지 - 문서화
- **commitizen >= 2.35.0** - 커밋 메시지 표준화

## 🎯 주요 기능

### 1. 백테스팅 시스템
- **Virtual Broker**: 리스크 없는 시뮬레이션 환경
- 다양한 주문 유형 지원:
  - Market Orders (시장가 주문)
  - Limit Orders (지정가 주문)
  - Stop-Loss Orders (손절 주문)
  - Take-Profit Orders (익절 주문)
- 크로스 거래소 차익거래 시뮬레이션
- 포트폴리오 전략 지원
- 동적 포지션 크기 계산

### 2. 시각화 (AutoPlot)
- **Bokeh** 기반 대화형 차트
- 거래 신호, 진입/청산 포인트 표시
- 손익 분석 시각화
- OHLC 캔들스틱 차트
- 기술적 지표 오버레이
- 실시간 업데이트 지원

### 3. 기술적 지표 (Indicators)
- **SuperTrend**: 트렌드 추종 지표
- **MACD**: 이동평균 수렴확산
- **RSI**: 상대강도지수
- **Swing Detection**: 스윙 포인트 감지
- **finta** 라이브러리 활용
- 커스텀 지표 개발 가능

### 4. 전략 개발 프레임워크
- 추상 클래스 기반 전략 인터페이스
- `generate_signal()` 메서드로 매매 신호 생성
- 파라미터 최적화 지원 (scipy.optimize.brute 사용)
- 멀티 봇 실행 지원
- 리스크 관리 기능

### 5. 데이터 관리
- **DataStream**: 통합 데이터 인터페이스
- **LocalDataStream**: 로컬 데이터 처리
- OHLC 데이터 자동 다운로드
- 실시간 데이터 스트리밍
- 다중 시간대 지원

### 6. 통신 및 알림
- **Telegram 봇** 통합
- 거래 알림 자동 전송
- 포트폴리오 상태 리포트
- 에러 알림 시스템

### 7. CLI 인터페이스
- 명령어 기반 실행 (`autotrader` 명령)
- 설정 파일 기반 실행
- 백테스트 및 라이브 트레이딩 모드 전환

## 📊 코드 품질 분석

### 프로젝트 규모
- **23개 Python 파일**
- **총 13,832 라인**
- 중간 규모의 체계적인 프로젝트
- 잘 구조화된 모듈 설계

### 코드 품질 메트릭
- **77개 TODO/FIXME 주석**: 개선 필요한 부분들 존재
- **76개 print문**: 디버깅/로깅 코드 산재
- **ABC 패턴**: 추상 클래스로 확장성 확보
- **타입 힌팅**: 현대적 Python 코딩 스타일 적용

### 설계 패턴
- **Abstract Factory**: 브로커 인터페이스
- **Strategy Pattern**: 트레이딩 전략 추상화
- **Observer Pattern**: 데이터 스트림 및 알림
- **Template Method**: 봇 실행 플로우

### 장점 ✅
- 모듈화된 아키텍처로 높은 확장성
- 다중 브로커 지원을 위한 추상화 설계
- 포괄적인 백테스팅 시스템
- 풍부한 문서화 (Read the Docs)
- CLI 인터페이스 제공
- 타입 힌팅으로 코드 안정성 향상
- 테스트 코드 포함
- 표준화된 커밋 메시지 (Conventional Commits)

### 개선 영역 ⚠️
- 더 이상 유지보수되지 않음 (아카이브 상태)
- 일부 브로커 통합 미완성 (IB, CCXT)
- print문 대신 logging 사용 권장
- TODO 주석이 많아 미완성 기능 존재
- 에러 처리 부분 개선 필요
- 의존성 버전 관리 (특히 bokeh 고정 버전)

## 📁 파일별 주요 기능

### 핵심 파일
- **`autotrader.py`**: 메인 엔진, 백테스팅, 최적화 기능
- **`autobot.py`**: 개별 전략 봇 관리, 실행 제어
- **`strategy.py`**: 전략 개발을 위한 추상 인터페이스
- **`autoplot.py`**: Bokeh 기반 시각화 도구
- **`utilities.py`**: 공통 유틸리티, 데이터 스트림, 분석 도구
- **`indicators.py`**: 기술적 지표 라이브러리

### 브로커 모듈 (`brokers/`)
- **`broker.py`**: 브로커 추상 인터페이스
- **`virtual.py`**: 백테스팅용 가상 브로커
- **`oanda.py`**: Oanda v20 API 통합
- **`ib.py`**: Interactive Brokers 통합
- **`ccxt.py`**: 암호화폐 거래소 통합
- **`trading.py`**: 주문, 포지션, 거래 데이터 모델

### 기타
- **`bin/cli.py`**: 명령줄 인터페이스
- **`comms/`**: 텔레그램 등 통신 모듈
- **`package_data/`**: 정적 자원 (JavaScript, 설정)

## 🎯 사용 사례

### 적합한 용도
- **백테스팅 연구**: Virtual Broker로 전략 검증
- **Forex 트레이딩**: Oanda 통합으로 실시간 거래
- **교육 목적**: 알고리즘 트레이딩 학습
- **프로토타이핑**: 트레이딩 아이디어 신속 검증
- **연구 개발**: 새로운 전략 개발 및 테스트

### 제한사항
- **유지보수 중단**: 보안 패치 및 업데이트 없음
- **일부 브로커 미완성**: IB, CCXT 통합 불완전
- **의존성 문제**: 고정된 라이브러리 버전으로 인한 호환성 이슈
- **프로덕션 리스크**: 실제 자금 거래 시 주의 필요

## 🚀 설치 및 사용

### 기본 설치
```bash
pip install autotrader
```

### 전체 기능 설치
```bash
pip install autotrader[all]
```

### 개발 환경 설정
```bash
pip install -e .[all]
pre-commit install
```

### CLI 사용법
```bash
autotrader [command] [options]
```

## 📚 관련 리소스

- **공식 문서**: https://autotrader.readthedocs.io/
- **GitHub 저장소**: https://github.com/kieran-mackle/AutoTrader
- **예제 전략**: https://github.com/kieran-mackle/autotrader-demo
- **PyPI 패키지**: https://pypi.org/project/autotrader

## 📈 결론 및 권장사항

### 종합 평가
AutoTrader는 **교육적 가치가 높은 종합적인 알고리즘 트레이딩 프레임워크**입니다. 백테스팅과 시각화 기능이 우수하고, 모듈화된 설계로 확장성을 고려했습니다.

### 권장 사용 시나리오

#### ✅ 권장
- **학습 및 연구**: 알고리즘 트레이딩 개념 학습
- **전략 프로토타이핑**: 아이디어 빠른 검증
- **백테스팅**: 과거 데이터를 통한 전략 성능 분석
- **Forex 모의 거래**: Oanda 데모 계정 활용

#### ⚠️ 주의 필요
- **프로덕션 환경**: 실제 자금 거래 시 충분한 검토 필요
- **최신 기능**: 새로운 브로커나 기능 필요 시 대안 고려
- **보안 업데이트**: 정기적인 의존성 점검 필요

### 대안 고려사항
- **활발히 개발되는 프레임워크**: Backtrader, Zipline, QuantConnect
- **최신 기술 스택**: Python 3.12+, 최신 라이브러리 버전
- **클라우드 플랫폼**: AWS, GCP 기반 트레이딩 시스템

---

**분석 일자**: 2025-08-11  
**분석 버전**: AutoTrader v1.1.2  
**분석자**: Claude Code Assistant