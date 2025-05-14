---
title: "PKCE(Proof Key for Code Exchange)"
date: 2025-05-14
tags: [security, oauth, authentication, pkce, authorization]
category: "웹 보안"
author: "Claude"
status: "완성"
related:
  - "OAuth2.0"
  - "인증 프로토콜"
  - "모바일 앱 보안"
---
# PKCE(Proof Key for Code Exchange)

PKCE(Proof Key for Code Exchange, "픽시"라고 발음)는 [[OAuth 2.0]] 인증 프로토콜의 보안을 강화하기 위한 확장 기능입니다.

## 1. PKCE의 등장 배경

PKCE는 다음과 같은 이유로 등장했습니다:

- **모바일 앱의 보안 취약점 해결**: 전통적인 [[OAuth 2.0]]의 [[Authorization Code]] 플로우는 client_secret을 안전하게 저장할 수 있는 서버 사이드 애플리케이션을 위해 설계되었습니다. 하지만 모바일 앱과 같은 [[퍼블릭 클라이언트]]는 코드를 리버스 엔지니어링하여 client_secret을 추출할 수 있는 위험이 있었습니다.

- **코드 가로채기 공격 방지**: 특히 모바일 애플리케이션에서 URI 스킴을 통해 리디렉션될 때 인증 코드가 가로채일 수 있는 취약점이 있었습니다. 악의적인 앱이 같은 URI 스킴을 등록하여 인증 코드를 가로챌 수 있었습니다.

- **[[CSRF]] 공격 방지 강화**: 기존 OAuth 플로우에서도 state 파라미터를 사용하여 CSRF 공격을 방지했지만, PKCE는 추가적인 보안 계층을 제공합니다.

## 2. PKCE의 사용 사례

PKCE는 다음과 같은 상황에서 사용됩니다:

- **[[모바일 애플리케이션]]**: 네이티브 모바일 앱에서 OAuth 인증을 안전하게 구현하기 위해 사용됩니다.

- **[[단일 페이지 애플리케이션]](SPA)**: 브라우저에서 실행되는 JavaScript 기반 SPA는 client_secret을 안전하게 보관할 수 없기 때문에 PKCE를 사용합니다.

- **[[공개 클라이언트]](Public Clients)**: client_secret을 안전하게 저장할 수 없는 모든 종류의 클라이언트에서 사용됩니다.

- **[[IoT 디바이스]]**: 제한된 보안 기능을 가진 IoT 디바이스에서도 OAuth 인증을 안전하게 구현하기 위해 사용됩니다.

## 3. PKCE의 필요성

PKCE는 다음과 같은 경우에 반드시 필요합니다:

- **[[공개 클라이언트]](모바일 앱, SPA 등)**: OAuth 2.0 보안 권장사항에 따르면 모든 공개 클라이언트는 PKCE를 사용해야 합니다.

- **[[RFC 8252]] (OAuth for Native Apps)**: 모바일 앱에서 OAuth를 사용할 때 PKCE를 의무적으로 사용하도록 권장하고 있습니다.

- **현대적인 보안 요구사항**: 최신 [[OAuth 2.1]] 초안에서는 모든 OAuth 클라이언트(비밀 클라이언트 포함)에 PKCE 사용을 권장하고 있습니다.

반면, 전통적인 서버 사이드 웹 애플리케이션에서는 client_secret을 안전하게 저장할 수 있기 때문에 PKCE가 절대적으로 필요하지는 않지만, 추가적인 보안 계층으로 사용하는 것이 권장됩니다.

## 4. PKCE의 장단점

### 장점:

- **향상된 보안**: 인증 코드 가로채기 공격으로부터 보호합니다.
- **client_secret 불필요**: 공개 클라이언트가 client_secret 없이도 안전하게 인증할 수 있습니다.
- **구현 용이성**: 기존 OAuth 흐름에 비교적 쉽게 추가할 수 있습니다.
- **표준 호환성**: 대부분의 현대적인 OAuth 서버는 PKCE를 지원합니다.
- **미래 지향적**: [[OAuth 2.1]] 초안에서 필수 컴포넌트로 포함되어 있습니다.

### 단점:

- **약간의 복잡성 증가**: 기존 OAuth 플로우보다 구현 과정이 약간 더 복잡해집니다.
- **서버 지원 필요**: 모든 인증 서버가 PKCE를 지원하는 것은 아닙니다(특히 오래된 시스템).
- **추가적인 암호화 작업**: code_verifier 생성 및 code_challenge 계산을 위한 추가 작업이 필요합니다.
- **브라우저 호환성**: 일부 오래된 브라우저에서는 필요한 암호화 함수가 지원되지 않을 수 있습니다.

## 5. PKCE의 사용 방법

PKCE 플로우는 다음과 같이 작동합니다:

### 1. code_verifier 생성
클라이언트는 암호학적으로 안전한 난수 문자열(code_verifier)을 생성합니다:
```javascript
// 예시 코드 (JavaScript)
function generateCodeVerifier() {
  const array = new Uint8Array(32);
  window.crypto.getRandomValues(array);
  return base64UrlEncode(array);
}
```

### 2. code_challenge 생성
code_verifier에서 code_challenge를 생성합니다:
```javascript
// 메서드 1: plain
function generateCodeChallengePlain(codeVerifier) {
  return codeVerifier; // 변환 없이 그대로 사용
}

// 메서드 2: S256 (권장)
async function generateCodeChallengeS256(codeVerifier) {
  const encoder = new TextEncoder();
  const data = encoder.encode(codeVerifier);
  const digest = await window.crypto.subtle.digest('SHA-256', data);
  return base64UrlEncode(new Uint8Array(digest));
}
```

### 3. 인증 요청
클라이언트는 code_challenge와 code_challenge_method를 포함한 인증 요청을 보냅니다:
```
GET /authorize?
    response_type=code&
    client_id=CLIENT_ID&
    redirect_uri=REDIRECT_URI&
    code_challenge=CODE_CHALLENGE&
    code_challenge_method=S256&
    state=STATE
```

### 4. 인증 코드 교환
인증 코드를 받은 후, 클라이언트는 원래의 code_verifier와 함께 토큰 요청을 보냅니다:
```
POST /token
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&
client_id=CLIENT_ID&
code=AUTHORIZATION_CODE&
redirect_uri=REDIRECT_URI&
code_verifier=CODE_VERIFIER
```

### 5. 인증 서버 검증
인증 서버는 다음을 수행합니다:
- 전송된 code_verifier를 받아 동일한 알고리즘(S256 또는 plain)으로 challenge를 계산
- 계산된 challenge가 초기 인증 요청의 code_challenge와 일치하는지 확인
- 일치하면 액세스 토큰 발급, 불일치하면 요청 거부

이러한 방식으로 PKCE는 인증 코드가 원래 요청한 클라이언트에게만 유효하도록 보장합니다.

## PKCE 흐름 다이어그램

```
+--------+                                  +---------------+
|        |--(A)-- Authorization Request --->|               |
|        |       + code_challenge          |               |
|        |                                  |               |
|        |<-(B)---- Authorization Code -----|               |
|        |                                  |               |
|클라이언트|                                  | 인증 서버       |
|        |--(C)-- Token Request ---------->|               |
|        |       + code_verifier           |               |
|        |                                  |               |
|        |<-(D)------ Access Token --------|               |
+--------+                                  +---------------+
```

## 결론

[[PKCE]]는 [[OAuth 2.0]] 인증 흐름에 중요한 보안 강화를 제공합니다. 특히 모바일 앱과 SPA와 같은 공개 클라이언트에서는 필수적인 요소로 간주되며, 최신 보안 권장사항에 따라 모든 종류의 클라이언트에서 사용이 권장됩니다.

## 참고 자료
- [[RFC 7636]] - PKCE 표준 문서
- [[RFC 8252]] - OAuth for Native Apps
- OAuth 2.0 Security Best Current Practice
