---
tags:
- 보안
- 인증시스템
- 토큰관리
- 이상행동탐지
- OAuth2.0
- Security
- AuthenticationSystem
- TokenManagement
- AnomalyDetection
- OAuth2.0
aliases:
- OAuth 2.0 Token Anomaly Detection
- 토큰 이상행동 탐지
CMDS: "[[📚 104 Terminologies]]"
index: "[[🏷 Research Notes]]"
author: "[[신희준]]"
---
## 토큰 이상행동 탐지 (OAuth 2.0 Token Anomaly Detection)
#### What is OAuth 2.0 Token Anomaly Detection
- 정의 (Definition):
	- OAuth 2.0 토큰 이상행동 탐지는 OAuth 2.0 인증 시스템에서 발행된 액세스 토큰 및 리프레시 토큰의 비정상적인 사용 패턴을 식별하고 분석하는 보안 프로세스를 의미함[(RFC 8693)](https://www.rfc-editor.org/info/rfc8693).
	- 이는 사용자의 일반적인 행동 패턴과 벗어난 토큰 사용을 실시간으로 모니터링하고, 토큰 탈취나 악용 시도를 탐지하는 프로세스임.
	- 토큰 이상행동 탐지는 베이스라인 프로파일링, 실시간 행동 분석, 토큰 사용 분석, 그리고 적절한 대응 메커니즘으로 구성됨.
- 예시 (Examples):
	- 짧은 시간 내에 지리적으로 멀리 떨어진 위치에서의 동일 토큰 사용 탐지
	- 사용자의 평소 행동 패턴과 다른 시간대 또는 비정상적인 빈도의 API 호출 탐지
	- 토큰 메타데이터(발급 시간, 만료 시간, 발급자) 변조 시도 탐지
	- 사용자 권한을 벗어나는 리소스에 대한 접근 시도 감지

## Literature Review
#### Jones et al., 2020
- [OAuth 2.0 Token Exchange](https://www.rfc-editor.org/info/rfc8693)
	- Source: RFC 8693, IETF
- 주요 내용 (Key points):
	- OAuth 2.0 토큰 교환 프로토콜을 정의하여 클라이언트가 보안 토큰을 안전하게 교환할 수 있는 방법을 제공
	- 위임(delegation) 및 대리(impersonation) 시나리오를 지원하는 토큰 교환 메커니즘 설명
	- 다양한 토큰 타입(JWT, SAML, OAuth 액세스 토큰 등)에 대한 식별자 정의
	- act(actor) 및 may_act 클레임을 통한 토큰 내 위임 의미론 표현 방법 제시

#### IETF, 2020
- [OAuth 2.0 Security Best Practices](https://datatracker.ietf.org/doc/draft-ietf-oauth-security-topics/)
	- Source: IETF Draft
- 주요 내용 (Key points):
	- OAuth 2.0 구현 시 발생할 수 있는 다양한 보안 위협과 위험을 분석
	- 토큰 탈취 방지를 위한 토큰 바인딩 기술(DPoP, MTLS) 권장
	- 짧은 토큰 수명, 토큰 교환, 토큰 철회 메커니즘 등 보안 모범 사례 제공
	- 토큰 사용 모니터링 및 이상 탐지의 중요성 강조

## 관련 개념 (Related Concepts)
- [[토큰 바인딩 (Token Binding)]] #TokenBinding
	- 특정 클라이언트 또는 디바이스에 토큰을 암호학적으로 바인딩하여 토큰 탈취 시에도 다른 환경에서 사용할 수 없도록 하는 기술
- [[행동 기반 인증 (Behavioral Authentication)]] #BehavioralAuthentication
	- 사용자의 행동 패턴(타이핑 습관, 마우스 움직임, 앱 사용 패턴 등)을 분석하여 본인 여부를 지속적으로 검증하는 방식
- [[위험 기반 인증 (Risk-Based Authentication)]] #RiskBasedAuthentication
	- 로그인 시도의 위험도를 평가하여 위험도에 따라 추가 인증 요소를 요구하는 동적 인증 방식
- [[컨텍스트 인식 접근 제어 (Context-Aware Access Control)]] #ContextAwareAccessControl
	- 사용자 컨텍스트(위치, 시간, 디바이스, 네트워크 등)를 고려하여 접근 권한을 동적으로 결정하는 접근 제어 모델

## 이상행동 탐지 데이터 포인트
- 사용자 행동 패턴:
	- 로그인 시간과 위치 데이터
	- 디바이스 지문(Fingerprint) 정보
	- IP 주소 및 네트워크 정보
	- 세션 행동 패턴(API 호출 순서, 빈도)
- 토큰 사용 패턴:
	- API 요청 빈도
	- 동시 접속 정보
	- 리소스 접근 패턴
	- 토큰 갱신 패턴
- 시스템 메타데이터:
	- User-Agent 문자열
	- HTTP 헤더 정보
	- 요청 타임스탬프
	- JWT 서명 검증 결과

## 이상행동 탐지 구현 전략
- 중앙 집중식 로깅 및 모니터링:
	- 모든 토큰 관련 이벤트 통합 로깅
	- 토큰 사용의 모든 측면 추적(발급, 갱신, 사용, 만료)
	- SIEM(보안 정보 및 이벤트 관리) 시스템과 통합
- 토큰 자체 보안 강화:
	- 짧은 만료 시간 설정
	- RFC 8693 기반 토큰 교환 메커니즘 구현
	- 효율적인 토큰 철회 메커니즘
- 점진적 인증 및 권한 부여:
	- 위험도에 기반한 추가 인증 요구
	- 민감 작업에 대한 단계적 접근 제한

## 이상행동 대응 메커니즘
- 실시간 대응:
	- 탈취 의심 시 즉시 토큰 취소
	- 관련 세션 종료
	- 위험도에 따른 계정 일시 잠금
- 사용자 알림:
	- 푸시 알림
	- 이메일 경고
	- SMS 확인
- 보안팀 알림 및 조사:
	- 보안 이벤트 로깅
	- 위협 인텔리전스 활용
	- 포렌식 증거 수집