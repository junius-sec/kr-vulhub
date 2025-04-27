# Apache Commons Text 원격 코드 실행 취약점 (CVE-2022-42889)

Apache Commons Text의 원격 코드 실행 취약점 (CVE-2022-42889, 일명 **"Text4Shell"**) 을 시연하는 Docker 기반 환경을 제공합니다.

---

## 📋 취약점 개요

- **취약점 ID:** CVE-2022-42889
- **라이브러리:** Apache Commons Text
- **문제 기능:** 문자열 보간 (string interpolation)
- **CVSS 점수:** 9.8 (**심각**)
- **영향 받는 버전:** Apache Commons Text **1.5 ~ 1.9**
- **수정된 버전:** Apache Commons Text **1.10.0 이상**

---

## ⚙️ 취약점 발생 원리

Apache Commons Text의 `StringSubstitutor` 클래스는 `${prefix:name}` 형식의 문자열 템플릿을 평가합니다.  
취약한 버전에서는 다음과 같은 **위험한 접두사(prefix)** 가 기본적으로 활성화되어 있습니다:

- `script:` : JVM의 스크립트 엔진을 사용해 코드 실행
- `dns:` : DNS 레코드 조회
- `url:` : 외부 URL에서 데이터 로드

이러한 접두사를 이용해 공격자는 다음과 같은 **악성 표현식**을 주입할 수 있습니다:

```text
${script:javascript:java.lang.Runtime.getRuntime().exec('touch /tmp/hacked')}
