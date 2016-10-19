---
author: WilliamsJason
title: "디바이스 포털 Fiddler API 참조"
description: "프로그래밍 방식으로 Fiddler 추적을 사용/사용하지 않는 방법을 알아봅니다."
translationtype: Human Translation
ms.sourcegitcommit: 3cc2a4bd1859e46a73f3e806489eac7381fa6c17
ms.openlocfilehash: bd215058c71118d8b3e5ce81e2302ce8b151c3f6

---

# Fiddler 설정 API 참조   
REST API를 사용하여 devkit에서 Fiddler 네트워크 추적을 사용하거나 사용하지 않을 수 있습니다.

## Fiddler 추적 사용

**요청**

다음 요청을 사용하여 devkit에 대한 Fiddler 추적을 사용할 수 있습니다.  그렇게 하려면 먼저 디바이스를 다시 시작해야 합니다.

메서드      | 요청 URI
:------     | :-----
POST | /ext/fiddler
<br />
**URI 매개 변수**

요청 URI에 다음과 같은 추가 매개 변수를 지정할 수 있습니다.

| URI 매개 변수      | 설명     | 
| ------------------ |-----------------|
| proxyAddress       | Fiddler를 실행하는 디바이스의 IP 주소 또는 호스트 이름입니다. |
| proxyPort          | Fiddler에서 트래픽을 모니터링하는 데 사용하는 포트입니다. 기본값은 8888입니다. |
| updateCert(옵션)| 루트 Fiddler 인증서가 제공되었는지 여부를 나타내는 부울 값입니다. Fiddler가 이 devkit에 구성되지 않았거나 다른 호스트에 대해 구성된 경우 이 값은 true여야 합니다.  |
<br>

**요청 헤더**

- 없음

**요청 본문**

- updateCert가 false이거나 제공되지 않은 경우에는 없음입니다. 그 외의 경우에는 HTTP 본문을 준수하며, FiddlerRoot.cer 파일을 포함하는 다중 파트입니다.

**응답**   

- 없음  

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
204 | Fiddler 사용 요청이 수락되었습니다. 다음에 디바이스가 다시 부팅될 때 Fiddler가 사용됩니다.
4XX | 오류 코드
5XX | 오류 코드

## devkit에서 Fiddler 추적 사용 중지

**요청**

다음 요청을 사용하여 디바이스에서 Fiddler 추적을 사용하지 않을 수 있습니다. 그렇게 하려면 먼저 디바이스를 다시 시작해야 합니다.

메서드      | 요청 URI
:------     | :-----
DELETE | /ext/fiddler
<br />
**URI 매개 변수**

- 없음

**요청 헤더**

- 없음

**요청 본문**   

- 없음

**응답**   

- 없음 

**상태 코드**

이 API에서 예상되는 상태 코드는 다음과 같습니다.

HTTP 상태 코드      | 설명
:------     | :-----
204 | Fiddler 추적 사용 중지 요청에 성공했습니다. 디바이스를 다음에 다시 부팅할 때 추적이 사용되지 않습니다.
4XX | 오류 코드
5XX | 오류 코드

<br />
**사용 가능한 디바이스 패밀리**

* Windows Xbox

## 참고 항목
- [Xbox에서 UWP용 Fiddler 구성](uwp-fiddler.md)




<!--HONumber=Aug16_HO3-->


