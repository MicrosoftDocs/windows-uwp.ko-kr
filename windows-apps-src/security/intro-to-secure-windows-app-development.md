---
title: 보안 Windows 앱 개발 소개
description: 이 소개 문서를 통해 앱 설계자와 개발자는 UWP (secure 유니버설 Windows 플랫폼) 앱을 빠르게 만들 수 있는 다양 한 Windows 10 플랫폼 기능을 보다 잘 이해할 수 있습니다.
ms.assetid: 6AFF9D09-77C2-4811-BB1A-BBF4A6FF511E
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 보안
ms.localizationpriority: medium
ms.openlocfilehash: 891c177879aff35f741ea9cc819ca4fd771a9437
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161957"
---
# <a name="intro-to-secure-windows-app-development"></a>보안 Windows 앱 개발 소개




이 소개 문서를 통해 앱 설계자와 개발자는 UWP (secure 유니버설 Windows 플랫폼) 앱을 빠르게 만들 수 있는 다양 한 Windows 10 플랫폼 기능을 보다 잘 이해할 수 있습니다. 인증, 데이터 처리 및 미사용 데이터의 각 단계에서 사용할 수 있는 Windows 보안 기능을 사용 하는 방법에 대해 자세히 설명 합니다. 각 항목에 포함 된 추가 리소스를 검토 하 여 각 항목에 대 한 자세한 정보를 찾을 수 있습니다.

## <a name="1-introduction"></a>1 소개


보안 앱을 개발 하는 것은 어려울 수 있습니다. 오늘날의 기동성이 뛰어난 모바일, 소셜, 클라우드 및 복잡 한 엔터프라이즈 앱 환경에서 고객은 앱을 사용 가능 하 게 하 고 그 어느 때 보다 빠르게 업데이트할 것을 요구 합니다. 또한 다양 한 유형의 장치를 사용 하 여 앱 환경을 만드는 복잡성에 추가 합니다. UWP (Windows 10 유니버설 Windows 플랫폼) 용으로 빌드하는 경우 데스크톱, 랩톱, 태블릿 및 모바일 장치의 기존 목록이 포함 될 수 있습니다. 사물 인터넷, Xbox One, Microsoft Surface Hub 및 HoloLens를 확장 하는 새 장치 목록에 추가 합니다. 개발자는 앱이 관련 된 모든 플랫폼 또는 장치에서 데이터를 안전 하 게 통신 하 고 저장 하도록 해야 합니다.

Windows 10 보안 기능을 활용 하는 경우의 몇 가지 이점은 다음과 같습니다.

-   보안 구성 요소 및 기술에 일관 된 Api를 사용 하 여 Windows 10을 지 원하는 모든 장치에서 표준화 된 보안을 사용할 수 있습니다.
-   이러한 보안 시나리오를 포함 하는 사용자 지정 코드를 구현 하는 경우 보다 더 작은 코드를 작성, 테스트 및 유지 관리 합니다.
-   운영 체제를 사용 하 여 앱에서 해당 리소스 및 로컬 또는 원격 시스템 리소스에 액세스 하는 방법을 제어 하기 때문에 앱은 안정적이 고 안전 하 게 유지 됩니다.

인증 하는 동안 특정 서비스에 대 한 액세스를 요청 하는 사용자의 id가 확인 됩니다. Windows Hello는 windows 앱에서 보다 안전한 인증 메커니즘을 만드는 데 도움이 되는 Windows 10의 구성 요소입니다. 이 기능을 사용 하면 사용자의 지문, 얼굴 또는 조리개와 같은 개인 식별 번호 (PIN) 또는 생체 인식을 사용 하 여 앱에 대 한 multi-factor authentication을 구현할 수 있습니다.

비행 데이터는 연결 및 연결을 통해 전송 되는 메시지를 나타냅니다. 이에 대 한 예는 웹 서비스를 사용 하 여 원격 서버에서 데이터를 검색 하는 것입니다. SSL(Secure Sockets Layer) (SSL) 및 보안 하이퍼텍스트 전송 프로토콜 (HTTPS)을 사용 하면 연결의 보안이 보장 됩니다. 중간 파티가 이러한 메시지에 액세스 하거나 권한이 없는 앱이 웹 서비스와 통신 하지 못하도록 하는 것은 비행 중인 데이터를 보호 하기 위한 핵심입니다.

마지막으로, 미사용 데이터는 메모리 또는 저장소 미디어에 있는 데이터와 관련이 있습니다. Windows 10에는 앱 간의 무단 데이터 액세스를 방지 하는 앱 모델이 있으며, 암호화 Api를 제공 하 여 장치의 데이터를 더욱 안전 하 게 보호 합니다. 자격 증명 보관 이라는 기능을 사용 하 여 장치에 사용자 자격 증명을 안전 하 게 저장 하 고, 운영 체제에서 다른 앱이 액세스할 수 없도록 할 수 있습니다.

## <a name="2-authentication-factors"></a>2 인증 요소


데이터를 보호 하려면 해당 데이터에 대 한 액세스를 요청 하는 사용자를 식별 하 고 요청 하는 데이터 리소스에 액세스할 수 있도록 권한을 부여 해야 합니다. 사용자를 식별 하는 프로세스를 인증 이라고 하 고 리소스에 대 한 액세스 권한을 결정 하는 것을 권한 부여 라고 합니다. 이러한 작업은 밀접 하 게 관련 된 작업 이며, 사용자는 구별할 수 없습니다. 이러한 작업은 여러 가지 요인에 따라 비교적 단순 하거나 복잡 한 작업 일 수 있습니다. 예를 들어 데이터가 하나의 서버에 있는지 또는 여러 시스템에 분산 되어 있는지 여부입니다. 인증 및 권한 부여 서비스를 제공 하는 서버를 id 공급자 라고 합니다.

사용자는 특정 서비스 및/또는 앱을 사용 하 여 자신을 인증 하기 위해 사용자가 알고 있는 자격 증명, 보유 한 항목 및/또는 다른 항목을 사용 하 여 자격 증명을 사용 합니다. 이러한 각을 인증 요소 라고 합니다.

-   **사용자가 알고 있는** 것은 일반적으로 암호 이지만 개인 식별 번호 (PIN) 또는 "비밀" 질문 및 답변 쌍 일 수도 있습니다.
-   **사용자** 가 가장 자주 사용 되는 것은 사용자에 게 고유한 인증 데이터를 포함 하는 USB 스틱과 같은 하드웨어 메모리 장치입니다.
-   **사용자는 종종 사용자** 의 지문을 포함 하지만 사용자의 음성, 얼굴, ocular (눈) 특성, 동작 패턴 등 널리 사용 되는 요소가 있습니다. 데이터를 데이터로 저장 하는 경우 이러한 측정을 생체 인식 이라고 합니다.

사용자가 만든 암호는 자체의 인증 요소 이지만 종종 충분 하지 않습니다. 암호를 알고 있는 사용자는 해당 암호를 소유 하는 사용자를 가장할 수 있습니다. 스마트 카드는 더 높은 수준의 보안을 제공할 수 있지만 도난당 하거나, 손실 되거나, 위치가 잘못 되었을 수 있습니다. 사용자가 지문을 사용 하 여 또는 ocular 검색을 통해 사용자를 인증할 수 있는 시스템은 가장 높은 수준의 보안을 제공할 수 있지만, 모든 사용자가 사용 하지 못할 수도 있는 고가의 특수 하드웨어 (예: 얼굴 인식을 위한 Intel RealSense 카메라)가 필요 합니다.

시스템에서 사용 하는 인증 방법 디자인은 데이터 보안의 복잡 하 고 중요 한 측면입니다. 일반적으로 인증에 사용 하는 요소 수가 많을 수록 시스템이 더 안전 하 게 보호 됩니다. 동시에 인증을 사용할 수 있어야 합니다. 일반적으로 사용자가 하루에 여러 번 로그인 하므로 프로세스가 빨라야 합니다. 인증 유형을 선택 하는 것은 보안과 사용 편의성을 절충 하는 것입니다. 단일 단계 인증은 가장 안전 하 고 사용 하기 쉽고, multi-factor authentication은 더 안전 하지만 더 많은 요인이 추가 됨에 따라 더 복잡 하 게 됩니다.

## <a name="21-single-factor-authentication"></a>2.1 단일 단계 인증


이 인증 형태는 단일 사용자 자격 증명을 기반으로 합니다. 일반적으로 암호 이지만 PIN (개인 식별 번호) 일 수도 있습니다.

단일 단계 인증 프로세스는 다음과 같습니다.

-   사용자가 id 공급자에 대 한 사용자 이름과 암호를 제공 합니다. Id 공급자는 사용자의 id를 확인 하는 서버 프로세스입니다.
-   ID 공급자가 사용자 이름 및 암호가 시스템에 저장된 것과 동일한지 확인합니다. 대부분의 경우 암호는 암호화 되어 다른 사용자가 읽을 수 없도록 추가 보안이 제공 됩니다.
-   Id 공급자는 인증의 성공 여부를 나타내는 인증 상태를 반환 합니다.
-   성공 하면 데이터 교환이 시작 됩니다. 실패 한 경우 사용자를 다시 인증 해야 합니다.

![단일 단계 인증](images/secure-sfa.png)

현재이 인증 방법은 서비스에서 가장 일반적으로 사용 되는 것입니다. 인증의 유일한 수단으로 사용 되는 경우 가장 안전 하지 않은 인증 형식 이기도 합니다. 암호 복잡성 요구 사항, "본인 확인 질문" 및 일반 암호 변경으로 인해 암호를 보다 안전 하 게 사용할 수 있지만 사용자에 게 더 많은 부담을 주는 것은 해커에 게 효과적인 deterrent 아닙니다.

암호를 사용 하는 경우 두 개 이상의 요인이 있는 시스템 보다 성공적으로 추측 하는 것이 더 쉬울 수 있습니다. 사용자 계정 및 웹 상점에서 해시 된 암호를 사용 하 여 데이터베이스를 도용 하는 경우 다른 웹 사이트에서 사용 되는 암호를 사용할 수 있습니다. 복잡 한 암호가 기억나지 않으므로 사용자는 항상 계정을 다시 사용 하는 경향이 있습니다. IT 부서의 경우 암호를 관리 하면 다시 설정 메커니즘을 제공 하 고, 암호를 자주 업데이트 해야 하며, 안전한 방식으로 저장 하는 것이 복잡 해질 수 있습니다.

모든 단점에 대해, 단일 단계 인증은 자격 증명의 사용자 제어를 제공 합니다. 인증서를 만들고 수정 하며 인증 프로세스에 대 한 키보드만 필요 합니다. Multi-factor authentication의 단일 요소를 구분 하는 주요 측면입니다.

## <a name="211-web-authentication-broker"></a>2.1.1 웹 인증 브로커


앞에서 설명한 것 처럼 IT 부서에서 암호 인증을 사용 하는 문제 중 하나는 사용자 이름/암호의 기본을 관리 하 고 메커니즘을 다시 설정 하는 등의 추가 오버 헤드입니다. 점차 널리 사용 되는 옵션은 인증을 위해 개방형 표준인 OAuth를 통해 인증을 제공 하는 타사 id 공급자를 사용 하는 것입니다.

OAuth를 사용 하면 IT 부서에서 사용자 이름 및 암호를 사용 하 여 데이터베이스를 유지 관리 하 고, 암호 기능을 다시 설정 하는 것이 Facebook, Twitter, Microsoft 등의 타사 id 공급자로 효과적으로 "아웃소싱" 할 수 있습니다.

사용자는 이러한 플랫폼에서 id를 완전히 제어할 수 있지만 사용자가 인증 된 후 인증 된 사용자에 게 권한을 부여 하는 데 사용할 수 있는 승인 된 후 앱에서 토큰을 요청할 수 있습니다.

Windows 10의 웹 인증 브로커는 응용 프로그램에서 OAuth 및 Openid connect와 같은 인증 및 권한 부여 프로토콜을 사용할 수 있는 Api 및 인프라 집합을 제공 합니다. 앱은 [**Webauthenticationbroker**](/uwp/api/Windows.Security.Authentication.Web.WebAuthenticationBroker) API를 통해 인증 작업을 시작할 수 있으며,이로 인해 [**webauthenticationbroker**](/uwp/api/Windows.Security.Authentication.Web.WebAuthenticationResult)가 반환 됩니다. 통신 흐름의 개요는 다음 그림에 나와 있습니다.

![wab 워크플로](images/secure-wab.png)

앱은 브로커 역할을 하며 앱의 [**웹 보기**](/uwp/api/Windows.UI.Xaml.Controls.WebView) 를 통해 id 공급자를 사용 하 여 인증을 시작 합니다. Id 공급자는 사용자를 인증 한 경우 id 공급자 로부터 사용자에 대 한 정보를 요청 하는 데 사용할 수 있는 토큰을 앱에 반환 합니다. 보안 조치로 id 공급자를 사용 하 여 인증 프로세스를 브로커 하려면 앱을 id 공급자에 등록 해야 합니다. 이 등록 단계는 공급자 마다 다릅니다.

[**Webauthenticationbroker**](/uwp/api/Windows.Security.Authentication.Web.WebAuthenticationBroker) API를 호출 하 여 공급자와 통신 하는 일반적인 워크플로는 다음과 같습니다.

-   Id 공급자에 게 보낼 요청 문자열을 생성 합니다. 문자열 수와 각 문자열의 정보는 각 웹 서비스에 대해 다르지만 일반적으로 URL을 포함 하는 두 개의 URI 문자열을 포함 합니다. 하나는 인증 요청을 보내는 것이 고, 하나는 권한 부여가 완료 된 후 사용자가 리디렉션되는 URL입니다.
-   [**AuthenticateAsync**](/uwp/api/windows.security.authentication.web.webauthenticationbroker.authenticateasync)를 호출 하 여 요청 문자열을 전달 하 고 id 공급자의 응답을 기다립니다.
-   응답이 수신 될 때 상태를 가져오려면 [**Webauthenticationresult. ResponseStatus**](/uwp/api/windows.security.authentication.web.webauthenticationresult.responsestatus) 를 호출 합니다.
-   통신이 성공적으로 완료 되 면 id 공급자가 반환 하는 응답 문자열을 처리 합니다. 실패 한 경우 오류를 처리 합니다.

통신이 성공적으로 완료 되 면 id 공급자가 반환 하는 응답 문자열을 처리 합니다. 실패 한 경우 오류를 처리 합니다.

이 프로세스에 대 한 샘플 c # 코드는 다음과 같습니다. 자세한 내용 및 자세한 연습은 [Webauthenticationbroker](web-authentication-broker.md)를 참조 하세요. 전체 코드 샘플을 보려면 [GitHub의 Webauthenticationbroker 샘플](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/WebAuthenticationBroker)을 확인 하세요.

```cs
string startURL = "https://<providerendpoint>?client_id=<clientid>";
string endURL = "http://<AppEndPoint>";

var startURI = new System.Uri(startURL);
var endURI = new System.Uri(endURL);

try
{
    WebAuthenticationResult webAuthenticationResult = 
        await WebAuthenticationBroker.AuthenticateAsync( 
            WebAuthenticationOptions.None, startURI, endURI);

    switch (webAuthenticationResult.ResponseStatus)
    {
        case WebAuthenticationStatus.Success:
            // Successful authentication. 
            break;
        case WebAuthenticationStatus.ErrorHttp:
            // HTTP error. 
            break;
        default:
            // Other error.
        break;
    }
}
catch (Exception ex)
{
    // Authentication failed. Handle parameter, SSL/TLS, and
    // Network Unavailable errors here. 
}
```

## <a name="22-multi-factor-authentication"></a>2.2 multi-factor authentication


Multi-factor authentication은 둘 이상의 인증 요소를 사용 합니다. 일반적으로 암호와 같은 "사용자가 알고 있는 것"은 휴대폰 또는 스마트 카드 일 수 있는 "보유 하 고 있는"와 결합 됩니다. 공격자가 사용자의 암호를 검색 하는 경우에도 장치 또는 카드 없이 계정에 액세스할 수 없습니다. 장치 또는 카드만 손상 된 경우에는 암호 없이 공격자에 게 유용 하지 않습니다. 따라서 multi-factor authentication은 보다 안전 하지만 단일 단계 인증 보다 더 복잡 합니다.

Multi-factor authentication을 사용 하는 서비스는 사용자에 게 두 번째 자격 증명을 받는 방법에 대 한 선택 항목을 제공 하는 경우가 많습니다. 이러한 유형의 인증 예는 SMS를 사용 하 여 사용자의 휴대폰에 확인 코드를 전송 하는 일반적으로 사용 되는 프로세스입니다.

-   사용자가 id 공급자에 대 한 사용자 이름과 암호를 제공 합니다.
-   Id 공급자는 사용자 이름 및 암호를 단일 단계 권한 부여에서 확인 한 후 시스템에 저장 된 사용자의 휴대폰 번호를 조회 합니다.
-   서버는 생성 된 확인 코드를 포함 하는 SMS 메시지를 사용자의 휴대폰으로 보냅니다.
-   사용자가 id 공급자에 게 확인 코드를 제공 합니다. 사용자에 게 표시 되는 양식
-   Id 공급자는 두 자격 증명의 인증에 성공 했는지 여부를 나타내는 인증 상태를 반환 합니다.
-   성공 하면 데이터 교환이 시작 됩니다. 그렇지 않으면 사용자를 다시 인증 해야 합니다.

![2 단계 인증](images/secure-tfa.png)

여기에서 볼 수 있듯이이 프로세스는 사용자가 만들거나 제공 하는 대신 두 번째 사용자 자격 증명이 사용자에 게 전송 된다는 점에서 단일 단계 인증과도 다릅니다. 따라서 사용자는 필요한 자격 증명을 완벽 하 게 제어할 수 없습니다. 스마트 카드가 두 번째 자격 증명으로 사용 되는 경우에도 적용 됩니다. 조직에서 사용자에 게이를 만들고 제공 합니다.

## <a name="221-azure-active-directory"></a>2.2.1 Azure Active Directory


Azure AD (Azure Active Directory)는 단일 단계 또는 multi-factor authentication에서 id 공급자로 사용할 수 있는 클라우드 기반 id 및 액세스 관리 서비스입니다. 확인 코드를 사용 하거나 사용 하지 않고 Azure AD 인증을 사용할 수 있습니다.

Azure AD는 단일 단계 인증을 구현할 수도 있지만 기업은 일반적으로 multi-factor authentication의 보안을 강화 해야 합니다. Multi-factor authentication 구성에서 Azure AD 계정을 사용 하 여 인증 하는 사용자는 휴대폰 또는 Azure Authenticator 모바일 앱에 SMS 메시지로 전송 되는 확인 코드를 선택할 수 있습니다.

또한 Azure AD를 OAuth 공급자로 사용할 수 있으며, 표준 사용자에 게 다양 한 플랫폼에서 앱에 대 한 인증 및 권한 부여 메커니즘을 제공 합니다. 자세히 알아보려면 [Azure의](https://azure.microsoft.com/services/multi-factor-authentication/) [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) 및 Multi-Factor Authentication를 참조 하세요.

## <a name="24-windows-hello"></a>2.4 Windows Hello


Windows 10에서는 편리한 multi-factor authentication 메커니즘이 운영 체제에 기본 제공 됩니다. Windows Hello는 Windows 10에 기본 제공 되는 새로운 생체 인식 로그인 시스템입니다. Windows Hello는 운영 체제에 직접 빌드되기 때문에 사용자 장치의 잠금을 해제 하는 데 얼굴 또는 지문 id를 허용 합니다. Windows 보안 자격 증명 저장소는 장치에서 생체 인식 데이터를 보호 합니다.

Windows Hello는 장치에서 개별 사용자를 인식할 수 있는 강력한 방법을 제공 하며,이를 통해 사용자와 요청 된 서비스 또는 데이터 항목 간의 경로에서 첫 번째 부분의 주소를 확인할 수 있습니다. 장치에서 사용자를 인식 한 후에도 요청 된 리소스에 대 한 액세스 권한을 부여할지 여부를 결정 하기 전에 사용자를 인증 해야 합니다. Windows Hello는 Windows에 완전히 통합 되 고 재사용 가능한 암호를 특정 장치 및 생체 인식 제스처 나 PIN의 조합으로 대체 하는 강력한 2 단계 인증 (2FA)도 제공 합니다. PIN은 Microsoft 계정 등록의 일부로 사용자가 지정 합니다.

그러나 Windows Hello는 기존 2FA 시스템을 대체 하는 것은 아닙니다. 이는 개념적으로 스마트 카드와 유사 합니다. 인증은 문자열 비교 대신 암호화 기본 형식을 사용 하 여 수행 되 고 사용자의 키 자료는 변조 방지 하드웨어 내에서 안전 합니다. Microsoft Hello에는 스마트 카드 배포에 필요한 추가 인프라 구성 요소가 필요 하지 않습니다. 특히 인증서를 관리 하기 위해 PKI (공개 키 인프라)가 필요 하지 않습니다 (현재 없는 경우). Windows Hello는 스마트 카드의 주요 이점 (가상 스마트 카드에 대 한 배포 유연성 및 실제 스마트 카드에 대 한 강력한 보안)을 단점 없이 결합 합니다.

사용자가 인증 하려면 먼저 장치를 Windows Hello에 등록 해야 합니다. Windows Hello는 한 당사자가 공개 키를 사용 하 여 다른 파티가 개인 키를 사용 하 여 암호를 해독할 수 있는 데이터를 암호화 하는 비대칭 (공개/개인 키) 암호화를 사용 합니다. Windows Hello의 경우 공개/개인 키 쌍 집합을 만들고 개인 키를 장치의 TPM (신뢰할 수 있는 플랫폼 모듈) 칩에 씁니다. 장치를 등록 한 후 UWP 앱에서 시스템 Api를 호출 하 여 사용자의 공개 키를 검색할 수 있습니다 .이 키는 서버에서 사용자를 등록 하는 데 사용할 수 있습니다.

앱의 등록 워크플로는 다음과 같습니다.

![windows hello 등록](images/secure-passport.png)

수집 하는 등록 정보에는이 간단한 시나리오에서 수행 하는 것 보다 더 많은 식별 정보가 포함 될 수 있습니다. 예를 들어 앱이 은행에 대해 하나의 보안 서비스에 액세스 하는 경우 등록 프로세스의 일부로 id 및 기타 사물 증명을 요청 해야 합니다. 모든 조건이 충족 되 면이 사용자의 공개 키는 백 엔드에 저장 되 고 다음에 사용자가 서비스를 사용할 때의 유효성을 검사 하는 데 사용 됩니다.

Windows Hello에 대 한 자세한 내용은 windows [hello 가이드](/windows/keep-secure/microsoft-passport-guide) 및 [windows hello 개발자 가이드](microsoft-passport.md)를 참조 하세요.

## <a name="3-data-in-flight-security-methods"></a>3 데이터 비행 보안 방법


데이터 비행 보안 방법은 네트워크에 연결 된 장치 간에 전송 되는 데이터에 적용 됩니다. 데이터는 개인 회사 인트라넷의 보안 수준이 높은 환경에서 시스템이 나 웹의 비보안 환경에서 클라이언트와 웹 서비스 간에 전송 될 수 있습니다. Windows 10 앱은 네트워킹 Api를 통해 SSL과 같은 표준을 지원 하 고, 개발자가 앱에 적절 한 수준의 보안을 보장할 수 있는 Azure API Management와 같은 기술을 사용 하 여 작업 합니다.

## <a name="31-remote-system-authentication"></a>3.1 원격 시스템 인증


원격 컴퓨터 시스템과의 통신이 발생하게 되는 두 가지 일반적인 시나리오가 있습니다.

-   로컬 서버는 직접 연결을 통해 사용자를 인증 합니다. 예를 들어 서버와 클라이언트가 회사 인트라넷에 있는 경우입니다.
-   웹 서비스는 인터넷을 통해와 통신 합니다.

웹 서비스 통신에 대 한 보안 요구 사항은 데이터는 더 이상 보안 네트워크의 일부일 뿐 아니라 악의적인 공격자가 데이터를 가로챌 가능성이 높기 때문에 직접 연결 시나리오 보다 더 높습니다. 다양 한 유형의 장치가 서비스에 액세스 하기 때문에 WCF와는 달리 RESTful 서비스로 빌드됩니다. 예를 들어 서비스에 대 한 인증 및 권한 부여를 통해 새로운 과제가 도입 됩니다. 보안 원격 시스템 통신을 위한 두 가지 요구 사항에 대해 설명 합니다.

첫 번째 요구 사항은 메시지 기밀성이 있습니다. 즉, 클라이언트와 웹 서비스 간에 전달 되는 정보 (예: 사용자의 id 및 기타 개인 정보)는 전송 중에 제 3 자가 읽을 수 없어야 합니다. 이 작업은 일반적으로 메시지가 전송되는 연결을 암호화하고 메시지 자체를 암호화하여 수행됩니다. 개인/공개 키 암호화에서는 공개 키를 누구나 사용할 수 있으며 특정 수신자에 게 보낼 메시지를 암호화 하는 데 사용 됩니다. 개인 키는 수신자만 보유 하 고 메시지의 암호를 해독 하는 데 사용 됩니다.

두 번째 요구 사항은 메시지 무결성입니다. 클라이언트와 웹 서비스는 수신 하는 메시지가 상대방에 게 전송 하려는 메시지 인지, 전송 중에 변경 되지 않았는지를 확인할 수 있어야 합니다. 이는 디지털 서명을 사용 하 여 메시지를 서명 하 고 인증서 인증을 사용 하 여 수행 됩니다.

## <a name="32-ssl-connections"></a>3.2 SSL 연결


클라이언트에 대 한 보안 연결을 설정 하 고 유지 관리 하기 위해 웹 서비스는 HTTPS (보안 하이퍼텍스트 전송 프로토콜)에서 지원 되는 SSL(Secure Sockets Layer) (SSL)를 사용할 수 있습니다. SSL은 서버 인증서 뿐만 아니라 공개 키 암호화를 지원 하 여 메시지 기밀성과 무결성을 제공 합니다. SSL은 TLS (전송 계층 보안)로 대체 되지만 TLS는 일반적으로 SSL (보통 수준의) 이라고 합니다.

클라이언트에서 서버에 있는 리소스에 대 한 액세스를 요청 하면 SSL은 서버와의 협상 프로세스를 시작 합니다. 이를 SSL 핸드셰이크 라고 합니다. 암호화 수준, 공개 및 개인 암호화 키 집합, 클라이언트 및 서버 인증서의 id 정보는 SSL 연결 기간 동안 모든 통신의 기반으로 합의 됩니다. 서버에서 지금 클라이언트를 인증 해야 할 수도 있습니다. 연결이 설정 되 면 연결이 닫힐 때까지 모든 메시지가 협상 된 공개 키로 암호화 됩니다.

## <a name="321-ssl-pinning"></a>3.2.1 SSL 고정


SSL은 암호화 및 인증서를 사용 하 여 메시지 기밀성을 제공할 수 있지만 클라이언트가 통신 하는 서버가 올바른 서버 인지 확인 하는 것은 아무 작업도 수행 하지 않습니다. 서버 동작은 권한이 없는 타사에 의해 모방 될 수 있으며, 클라이언트에서 전송 하는 중요 한 데이터를 가로챌 수 있습니다. 이를 방지 하기 위해 SSL 고정 이라는 기술은 서버의 인증서가 클라이언트에서 예상 하 고 신뢰 하는 인증서 인지 확인 하는 데 사용 됩니다.

앱에서 SSL 고정을 구현 하는 몇 가지 다른 방법이 있습니다. 각 방법에는 고유한 장단점이 있습니다. 가장 쉬운 방법은 응용 프로그램의 패키지 매니페스트에 있는 인증서 선언을 통하는 것입니다. 이 선언을 사용 하면 앱 패키지에서 디지털 인증서를 설치 하 고 해당 인증서에 독점적인 신뢰를 지정할 수 있습니다. 그러면 인증서 체인에 해당 인증서가 있는 앱과 서버 간에만 SSL 연결이 허용 됩니다. 또한이 메커니즘을 사용 하면 신뢰할 수 있는 공용 인증 기관에서 타사 종속성이 필요 하지 않으므로 자체 서명 된 인증서를 안전 하 게 사용할 수 있습니다.

![ssl 매니페스트](images/secure-ssl-manifest.png)

유효성 검사 논리를 보다 효과적으로 제어 하기 위해 Api를 사용 하 여 HTTPS 요청에 대 한 응답으로 서버에서 반환 된 인증서의 유효성을 검사할 수 있습니다. 이 메서드는 요청을 보내고 응답을 검사 해야 하므로 실제로 요청에서 중요 한 정보를 보내기 전에이를 유효성 검사로 추가 해야 합니다.

다음 c # 코드는이 SSL 고정 메서드를 보여 줍니다. **Validatesslroot** 메서드는 [**httpclient**](/uwp/api/Windows.Web.Http.HttpClient) 클래스를 사용 하 여 HTTP 요청을 실행 합니다. 클라이언트에서 응답을 보낸 후에는 [**RequestMessage ServerIntermediateCertificates**](/uwp/api/windows.web.http.httptransportinformation.serverintermediatecertificates) 컬렉션을 사용 하 여 서버에서 반환 된 인증서를 검사 합니다. 클라이언트는 포함 된 지문을 사용 하 여 전체 인증서 체인의 유효성을 검사할 수 있습니다. 이 방법에서는 서버 인증서가 만료 되 고 갱신 될 때 앱에서 인증서 지문을 업데이트 해야 합니다.

```cs
private async Task ValidateSSLRoot()
{
    // Send a get request to Bing
    var httpClient = new HttpClient();
    var bingUri = new Uri("https://www.bing.com");
    HttpResponseMessage response = 
        await httpClient.GetAsync(bingUri);

    // Get the list of certificates that were used to
    // validate the server's identity
    IReadOnlyList<Certificate> serverCertificates = response.RequestMessage.TransportInformation.ServerIntermediateCertificates;
  
    // Perform validation
    if (!ValidateCertificates(serverCertificates))
    {
        // Close connection as chain is not valid
        return;
    }
    // Validation passed, continue with connection to service
}

private bool ValidateCertificates(IReadOnlyList<Certificate> certs)
{
    // In this example, we iterate through the certificates
    // and check that the chain contains
    // one specific certificate we are expecting
    foreach (var cert in certs)
    {
        byte[] thumbprint = cert.GetHashValue();

        // Check if the thumbprint matches whatever you 
        // are expecting
        var expected = new byte[] { 212, 222, 32, 208, 94, 102, 
            252, 83, 254, 26, 80, 136, 44, 120, 219, 40, 82, 202, 
            228, 116 };

        // ThumbprintMatches does the byte[] comparison 
        if (ThumbprintMatches(thumbprint, expected))
        {
            return true;
        }
    }
    return false;
}
```

## <a name="33-publishing-and-securing-access-to-rest-apis"></a>3.3 REST Api에 대 한 액세스 게시 및 보안


웹 서비스에 대 한 인증 된 액세스를 보장 하려면 API 호출을 수행할 때마다 인증을 요구 해야 합니다. 성능 및 확장을 제어 하는 것은 웹 서비스가 웹에서 노출 될 때 고려해 야 할 사항입니다. Azure API Management는 웹을 통해 Api를 노출 하는 데 사용할 수 있는 서비스 이며 세 가지 수준에 기능을 제공 합니다.

API의 게시자 **/관리자** 는 Azure API Management의 게시자 포털을 통해 쉽게 api를 구성할 수 있습니다. 여기에서 API 집합을 만들 수 있으며 해당 집합에 대 한 액세스를 관리 하 여 Api에 액세스할 수 있는 사용자를 제어할 수 있습니다.

이러한 Api에 대 한 액세스를 요청 하는 **개발자** 는 개발자 포털을 통해 요청을 수행할 수 있습니다 .이 포털은 즉시 액세스를 제공 하거나 게시자/관리자가 승인을 요구할 수 있습니다. 개발자는 또한 개발자 포털에서 API 설명서 및 샘플 코드를 보고 웹 서비스에서 제공 하는 Api를 신속 하 게 채택할 수 있습니다.

이러한 개발자가 만드는 **앱** 은 Azure API Management에서 제공 하는 프록시를 통해 API에 액세스 합니다. 프록시는 모두 은둔 계층을 제공 하 고, 게시자/관리자 서버에서 API의 실제 끝점을 숨기고, api 변환과 같은 추가 논리를 포함 하 여 한 API에 대 한 호출이 다른 API에 리디렉션되는 경우 노출 되는 API가 일관 되 게 유지 되도록 할 수도 있습니다. 또한 IP 필터링을 사용 하 여 특정 IP 도메인 또는 도메인 집합에서 시작 되는 API 호출을 차단할 수 있습니다. 또한 Azure API Management는 API 키 라는 공개 키 집합을 사용 하 여 각 API 호출을 인증 하 고 권한을 부여 하 여 웹 서비스를 안전 하 게 유지 합니다. 권한 부여가 실패 하면 API에 대 한 액세스 및 지원 되는 기능이 차단 됩니다.

Azure API Management는 웹 서비스의 성능을 최적화 하기 위해 서비스 (제한 이라는 프로시저)에 대 한 API 호출 수를 줄일 수도 있습니다. 자세히 알아보려면 AzureCon 2015에서 [azure API Management](https://azure.microsoft.com/services/api-management/) 및 [azure API Management를 검토 하세요.](https://channel9.msdn.com/events/Microsoft-Azure/AzureCon-2015/ACON313)

## <a name="4-data-at-rest-security-methods"></a>4 개의 미사용 데이터 보안 방법


데이터가 장치에 도착 하면이를 "미사용 데이터"로 지칭 합니다. 이 데이터는 권한이 없는 사용자 또는 앱에서 액세스할 수 없도록 장치에 안전 하 게 저장 해야 합니다. Windows 10의 앱 모델은 응용 프로그램에 의해 저장 된 데이터에만 액세스할 수 있도록 하는 동시에 필요한 경우 데이터를 공유 하는 Api를 제공 합니다. 추가 Api를 사용 하 여 데이터를 암호화 하 고 자격 증명을 안전 하 게 저장할 수 있는지도 확인할 수 있습니다.

## <a name="41-windows-app-model"></a>4.1 Windows 앱 모델


일반적으로 Windows에는 앱의 정의가 없습니다. 가장 일반적으로 실행 파일 (.exe) 이라고 하며,이는 설치, 상태 저장, 실행 길이, 버전 관리, OS 통합 또는 앱 간 통신을 포함 하지 않습니다. 유니버설 Windows 플랫폼 모델은 설치, 런타임 환경, 리소스 관리, 업데이트, 데이터 모델 및 제거를 포함 하는 앱 모델을 정의 합니다.

Windows 10 앱은 컨테이너에서 실행 됩니다. 즉, 기본적으로 권한이 제한 되어 있습니다 (사용자가 추가 권한을 요청 하 고 부여할 수 있음). 예를 들어 응용 프로그램이 시스템의 파일에 액세스 하려는 경우에는 파일에 대 한 직접 액세스를 사용 하도록 설정 하지 않고, 사용자가 파일을 선택 하는 데 사용 해야 하는 경우에는 [**Windows.**](/uwp/api/Windows.Storage.Pickers) . i m l 네임 스페이스의 파일 선택기를 사용 해야 합니다. 또 다른 예는 응용 프로그램이 사용자의 위치 데이터에 액세스 하려는 경우 위치 장치 기능을 선언 해야 하는 위치를 사용 하도록 설정 해야 하며, 사용자에 게이 앱에서 사용자의 위치에 대 한 액세스를 요청 하는 메시지가 표시 되는 경우입니다. 처음에는 앱에서 사용자의 위치에 처음 액세스 하려고 할 때 사용자에 게 데이터 액세스 권한을 요청 하는 추가 동의 프롬프트가 표시 됩니다.

이 앱 모델은 응용 프로그램에 대 한 "감옥"으로 작동 합니다. 즉,이 모델은 외부에서 연결할 수 없는 "성"은 아닙니다 (관리자 권한이 있는 응용 프로그램은 여전히에 도달할 수 있음). 조직/IT에서 실행할 수 있는 (Win32) 앱을 지정할 수 있도록 하는 Windows 10의 Device Guard는이 액세스를 제한 하는 데 도움이 될 수 있습니다.

앱 모델은 또한 앱 수명 주기를 관리 합니다. 기본적으로 앱의 백그라운드 실행을 제한 합니다. 예를 들어 앱이 백그라운드에 들어가면 프로세스가 일시 중단 되 고, 코드에서 앱 일시 중단을 해결 하는 짧은 기간을 앱에 제공 하 고 해당 메모리가 고정 됩니다. 운영 체제는 앱에서 특정 백그라운드 작업 실행을 요청 하는 메커니즘 (예: 인터넷/Bluetooth 연결, 전원 변경 등의 다양 한 이벤트에 의해 트리거되는 일정에 따라, 음악 재생 또는 GPS 추적 등의 특정 시나리오)을 제공 합니다.

장치의 메모리 리소스가 부족 하 게 되 면 Windows는 앱을 종료 하 여 메모리 공간을 확보 합니다. 이 수명 주기 모델은 일시 중단 및 종료 사이에 사용할 수 있는 추가 시간이 없기 때문에 앱이 일시 중단 될 때마다 데이터를 유지 하도록 합니다.

자세한 내용은 [범용: Windows 10 응용 프로그램의 수명 주기 이해](https://visualstudiomagazine.com/articles/2015/09/01/its-universal.aspx)를 참조 하세요.

## <a name="42-stored-credential-protection"></a>4.2 저장 된 자격 증명 보호


인증 된 서비스에 액세스 하는 Windows 앱은 종종 사용자에 게 로컬 장치에 자격 증명을 저장 하는 옵션을 제공 합니다. 사용자가 편리 하 게 사용할 수 있습니다. 사용자의 사용자 이름과 암호를 제공 하면 앱은 다음에 앱을 시작할 때 자동으로 사용 합니다. 공격자가이 저장 된 데이터에 액세스할 수 있는 경우 보안 문제가 될 수 있으므로 windows 10은 Windows 앱이 보안 자격 증명 보관에 사용자 자격 증명을 저장 하는 기능을 제공 합니다. 앱은 자격 증명 보관 API를 호출 하 여 앱의 저장소 컨테이너에 자격 증명을 저장 하는 대신 보관 하 고 자격 증명을 검색 합니다. 자격 증명 보관은 운영 체제에서 관리 되지만 액세스는 자격 증명 저장소에 안전 하 게 관리 되는 솔루션을 제공 하 여 저장 하는 앱 으로만 제한 됩니다.

사용자가 저장할 자격 증명을 제공 하면 앱은 PasswordVault 네임 스페이스의 [**PasswordVault**](/uwp/api/Windows.Security.Credentials.PasswordVault) 개체를 사용 하 여 자격 증명 보관에 대 한 참조를 [**가져옵니다.**](/uwp/api/Windows.Security.Credentials) 그런 다음 Windows 앱에 대 한 식별자와 사용자 이름 및 암호를 포함 하는 [**Passwordcredential**](/uwp/api/Windows.Security.Credentials.PasswordCredential) 개체를 만듭니다. 이는 자격 증명을 보관에 저장 하기 위해 [**PasswordVault**](/uwp/api/windows.security.credentials.passwordvault.add) 메서드로 전달 됩니다. 다음 c # 코드 예제에서는이 작업을 수행 하는 방법을 보여 줍니다.

```cs
var vault = new PasswordVault();
vault.Add(new PasswordCredential("My App", username, password));
```

다음 c # 코드 예제에서 앱은 [**PasswordVault**](/uwp/api/Windows.Security.Credentials.PasswordVault) 개체의 [**FindAllByResource**](/uwp/api/windows.security.credentials.passwordvault.findallbyresource) 메서드를 호출 하 여 앱에 해당 하는 모든 자격 증명을 요청 합니다. 둘 이상의가 반환 되 면 사용자에 게 사용자 이름을 입력 하 라는 메시지가 표시 됩니다. 자격 증명이 보관에 없으면 앱에서 사용자에 게 해당 자격 증명을 묻는 메시지를 표시 합니다. 그런 다음 사용자는 자격 증명을 사용 하 여 서버에 로그인 합니다.

```cs
private string resourceName = "My App";
private string defaultUserName;

private void Login()
{
    PasswordCredential loginCredential = GetCredentialFromLocker();

    if (loginCredential != null)
    {
        // There is a credential stored in the locker.
        // Populate the Password property of the credential
        // for automatic login.
        loginCredential.RetrievePassword();
    }
    else
    {
        // There is no credential stored in the locker.
        // Display UI to get user credentials.
        loginCredential = GetLoginCredentialUI();
    }
    // Log the user in.
    ServerLogin(loginCredential.UserName, loginCredential.Password);
}

private PasswordCredential GetCredentialFromLocker()
{
    PasswordCredential credential = null;

    var vault = new PasswordVault();
    var credentialList = vault.FindAllByResource(resourceName);

    if (credentialList.Count == 1)
    {
        credential = credentialList[0];
    }
    else if (credentialList.Count > 0)
    {
        // When there are multiple usernames,
        // retrieve the default username. If one doesn't
        // exist, then display UI to have the user select
        // a default username.
        defaultUserName = GetDefaultUserNameUI();

        credential = vault.Retrieve(resourceName, defaultUserName);
    }
    return credential;
}
```

자세한 내용은 [자격 증명](credential-locker.md)보관을 참조 하세요.

## <a name="43-stored-data-protection"></a>4.3 저장 된 데이터 보호


일반적으로 미사용 데이터 라고 하는 저장 된 데이터를 처리 하는 경우 암호화를 사용 하면 권한이 없는 사용자가 저장 된 데이터에 액세스 하지 못할 수 있습니다. 데이터를 암호화 하는 두 가지 일반적인 메커니즘은 대칭 키를 사용 하거나 비대칭 키를 사용 하는 것입니다. 그러나 데이터 암호화는 데이터가 전송 된 시간과 저장 된 시간 사이에 변경 되지 않도록 보장할 수 없습니다. 즉, 데이터 무결성을 보장할 수 없습니다. 메시지 인증 코드, 해시 및 디지털 서명을 사용 하는 것은이 문제를 해결 하는 일반적인 기술입니다.

## <a name="431-data-encryption"></a>4.3.1 데이터 암호화


대칭 암호화를 사용 하 여 보낸 사람과 받는 사람이 모두 동일한 키를 사용 하 여 데이터를 암호화 하 고 암호 해독 하는 데 사용 합니다. 이 방법의 과제는 키를 안전 하 게 공유 하 여 두 당사자가 모두 인식 한다는 것입니다.

이에 대 한 한 가지 대답은 공개/개인 키 쌍이 사용 되는 비대칭 암호화입니다. 공개 키는 메시지를 암호화 하려는 사용자와 자유롭게 공유 됩니다. 개인 키는 데이터를 암호 해독 하는 데만 사용할 수 있도록 항상 암호를 유지 합니다. 공개 키의 검색을 허용 하는 일반적인 방법은 디지털 인증서를 사용 하는 것입니다 (인증서 라고도 함). 인증서는 사용자 또는 서버에 대 한 정보 (예: 이름, 발급자, 전자 메일 주소 및 국가) 뿐만 아니라 공개 키에 대 한 정보를 저장 합니다.

Windows 앱 개발자는 [**SymmetricKeyAlgorithmProvider**](/uwp/api/Windows.Security.Cryptography.Core.SymmetricKeyAlgorithmProvider) 및 [**AsymmetricKeyAlgorithmProvider**](/uwp/api/Windows.Security.Cryptography.Core.AsymmetricKeyAlgorithmProvider) 클래스를 사용 하 여 UWP 앱에서 대칭 및 비대칭 암호화를 구현할 수 있습니다. 또한 [**CryptographicEngine**](/uwp/api/Windows.Security.Cryptography.Core.CryptographicEngine) 클래스를 사용 하 여 데이터를 암호화 및 암호 해독 하 고, 콘텐츠에 서명 하 고, 디지털 서명을 확인할 수 있습니다. 앱은 DataProtectionProvider [**네임 스페이스**](/uwp/api/Windows.Security.Cryptography.DataProtection) 의 [**DataProtectionProvider**](/uwp/api/Windows.Security.Cryptography.DataProtection.DataProtectionProvider) 클래스를 사용 하 여 저장 된 로컬 데이터를 암호화 하 고 해독할 수도 있습니다.

## <a name="432-detecting-message-tampering-macs-hashes-and-signatures"></a>메시지 변조를 검색 하는 4.3.2 (Mac, 해시 및 서명)


MAC은 대칭 키 (비밀 키) 또는 메시지를 MAC 암호화 알고리즘에 대 한 입력으로 사용 하 여 생성 되는 코드 또는 태그입니다. 비밀 키와 알고리즘은 메시지를 전송 하기 전에 발신자와 수신자에 의해 합의 됩니다.

Mac은 다음과 같은 메시지를 확인 합니다.

-   발신자는 비밀 키를 MAC 알고리즘에 대 한 입력으로 사용 하 여 MAC 태그를 파생 합니다.
-   보낸 사람이 MAC 태그와 메시지를 받는 사람에 게 보냅니다.
-   수신기는 비밀 키와 메시지를 MAC 알고리즘에 대 한 입력으로 사용 하 여 MAC 태그를 파생 합니다.
-   수신자는 해당 MAC 태그를 발신자의 MAC 태그와 비교 합니다. 동일한 경우에는 메시지가 변조 되지 않았음을 알 수 있습니다.

![mac 확인](images/secure-macs.png)

Windows 앱은 [**MacAlgorithmProvider**](/uwp/api/Windows.Security.Cryptography.Core.MacAlgorithmProvider) 클래스를 호출 하 여 mac 암호화 알고리즘을 수행 하는 Key 및 [**CryptographicEngine**](/uwp/api/Windows.Security.Cryptography.Core.CryptographicEngine) 클래스를 생성 하 여 mac 메시지 확인을 구현할 수 있습니다.

## <a name="433-using-hashes"></a>해시를 사용 하 여 4.3.3


해시 함수는 임의의 long 데이터 블록을 사용 하 고 해시 값 이라는 고정 크기 비트 문자열을 반환 하는 암호화 알고리즘입니다. 이 작업을 수행할 수 있는 전체 해시 함수 패밀리가 있습니다.

위의 메시지 전송 시나리오에서 MAC 대신 해시 값을 사용할 수 있습니다. 발신자는 해시 값과 메시지를 보내고 수신자는 보낸 사람의 해시 값과 메시지에서 고유한 해시 값을 파생 시키고 두 해시 값을 비교 합니다. Windows 10에서 실행 되는 앱은 [**HashAlgorithmProvider**](/uwp/api/Windows.Security.Cryptography.Core.HashAlgorithmProvider) 클래스를 호출 하 여 사용할 수 있는 해시 알고리즘을 열거 하 고 그 중 하나를 실행할 수 있습니다. [**CryptographicHash**](/uwp/api/Windows.Security.Cryptography.Core.CryptographicHash) 클래스는 해시 값을 나타냅니다. [**CryptographicHash**](/uwp/api/windows.security.cryptography.core.cryptographichash.getvalueandreset) 메서드를 사용 하 여 각 사용에 대 한 개체를 다시 만들지 않고도 다른 데이터를 반복적으로 해시 하는 데 사용할 수 있습니다. **CryptographicHash** 클래스의 Append 메서드는 해시 될 버퍼에 새 데이터를 추가 합니다. 이 전체 프로세스는 다음 c # 코드 예제에 나와 있습니다.

```cs
public void SampleReusableHash()
{
    // Create a string that contains the name of the
    // hashing algorithm to use.
    string strAlgName = HashAlgorithmNames.Sha512;

    // Create a HashAlgorithmProvider object.
    HashAlgorithmProvider objAlgProv = HashAlgorithmProvider.OpenAlgorithm(strAlgName);

    // Create a CryptographicHash object. This object can be reused to continually
    // hash new messages.
    CryptographicHash objHash = objAlgProv.CreateHash();

    // Hash message 1.
    string strMsg1 = "This is message 1";
    IBuffer buffMsg1 = CryptographicBuffer.ConvertStringToBinary(strMsg1, BinaryStringEncoding.Utf16BE);
    objHash.Append(buffMsg1);
    IBuffer buffHash1 = objHash.GetValueAndReset();

    // Hash message 2.
    string strMsg2 = "This is message 2";
    IBuffer buffMsg2 = CryptographicBuffer.ConvertStringToBinary(strMsg2, BinaryStringEncoding.Utf16BE);
    objHash.Append(buffMsg2);
    IBuffer buffHash2 = objHash.GetValueAndReset();

    // Convert the hashes to string values (for display);
    string strHash1 = CryptographicBuffer.EncodeToBase64String(buffHash1);
    string strHash2 = CryptographicBuffer.EncodeToBase64String(buffHash2);
}
```

## <a name="434-digital-signatures"></a>4.3.4 디지털 서명


디지털 서명 된 저장 메시지의 데이터 무결성은 MAC 인증과 비슷한 방식으로 확인 됩니다. 디지털 서명 워크플로의 작동 방식은 다음과 같습니다.

-   보낸 사람은 해시 알고리즘에 대 한 입력으로 메시지를 사용 하 여 해시 값 (다이제스트 라고도 함)을 파생 합니다.
-   발신자는 개인 키를 사용 하 여 다이제스트를 암호화 합니다.
-   보낸 사람은 메시지, 암호화 된 다이제스트 및 사용 된 해시 알고리즘의 이름을 보냅니다.
-   수신자는 공개 키를 사용 하 여 받은 암호화 된 다이제스트의 암호를 해독 합니다. 그런 다음 해시 알고리즘을 사용 하 여 메시지를 해시 하 여 자체의 다이제스트를 만듭니다. 마지막으로 수신기는 두 다이제스트 (수신 하 고 해독 한 다이제스트와 자신이 만든 다이제스트)를 비교 합니다. 두 개의 일치 항목이 수신자가 메시지를 개인 키의 소유자나에서 보냈는지 확인할 수 있는 경우에만이 메시지는 개인 키의 메시지에 의해 전송 되 고 메시지가 전송 중에 변경 되지 않았음을 확인할 수 있습니다.

![디지털 서명](images/secure-digital-signatures.png)

해시 알고리즘은 매우 빠르기 때문에 큰 메시지에서 해시 값을 신속 하 게 파생 시킬 수 있습니다. 결과 해시 값은 임의 길이 이며 전체 메시지 보다 짧을 수 있으므로 공개 키와 개인 키를 사용 하 여 전체 메시지가 아니라 다이제스트만 암호화 하 고 해독 하면 됩니다.

자세한 내용은 [디지털 서명](/windows/desktop/SecCrypto/digital-signatures), [mac, 해시, 서명](macs-hashes-and-signatures.md)및 암호화에 대 한 문서를 참조 [하세요.](cryptography.md)

## <a name="5-summary"></a>5 요약


Windows 10의 유니버설 Windows 플랫폼는 운영 체제 기능을 활용 하 여 더 안전한 앱을 만드는 여러 가지 방법을 제공 합니다. OAuth id 공급자를 사용 하 여 단일 요소, multi-factor 또는 조정 된 인증과 같은 다양 한 인증 시나리오에서 Api는 인증을 사용 하 여 가장 일반적인 문제를 완화 하기 위해 존재 합니다. Windows Hello는 사용자를 인식 하 고 적절 한 식별을 회피 하기 위해 적극적으로 노력 하는 새로운 생체 인식 로그인 시스템을 제공 합니다. 또한 신뢰할 수 있는 플랫폼 모듈 외부에서 표시 하거나 사용할 수 없는 여러 계층의 키 및 인증서를 제공 합니다. 또한 증명 id 키 및 인증서를 선택적으로 사용 하 여 추가 보안 계층을 사용할 수 있습니다.

비행 중인 데이터를 보호 하기 위해 Api는 ssl을 통해 안전 하 게 원격 시스템과 통신 하는 데 사용 되 고, SSL 고정으로 서버 인증의 유효성을 검사할 수 있는 가능성을 제공 합니다. Api를 안전 하 고 제어 된 방식으로 게시 하면 API 끝점의 난독 처리를 추가로 제공 하는 프록시를 사용 하 여 웹에서 Api를 노출 하기 위한 강력한 구성 옵션을 제공 하 여 Azure API Management 지 원하는 것입니다. Api 키를 사용 하 여 이러한 Api에 대 한 액세스를 보호 하 고, API 호출을 제한 하 여 성능을 제어할 수 있습니다.

데이터가 장치에 도착할 때 Windows 앱 모델은 앱을 설치 하 고, 업데이트 하 고, 액세스 하는 방법을 제어 하는 동시에 권한 없는 방식으로 다른 앱의 데이터에 액세스할 수 있도록 합니다. 자격 증명 보관은 운영 체제에서 관리 하는 사용자 자격 증명의 보안 저장소를 제공할 수 있으며, 유니버설 Windows 플랫폼에서 제공 하는 암호화 및 해시 Api를 사용 하 여 장치에서 기타 데이터를 보호할 수 있습니다.

## <a name="6-resources"></a>6 리소스


### <a name="61-how-to-articles"></a>6.1 방법 문서

-   [인증 및 사용자 ID](authentication-and-user-identity.md)
-   [Windows Hello](microsoft-passport.md)
-   [자격 증명 보관](credential-locker.md)
-   [웹 인증 브로커](web-authentication-broker.md)
-   [지문 생체 인식](fingerprint-biometrics.md)
-   [스마트 카드](smart-cards.md)
-   [공유 인증서](share-certificates.md)
-   [암호화](cryptography.md)
-   [인증서](certificates.md)
-   [암호화 키](cryptographic-keys.md)
-   [데이터 보호](data-protection.md)
-   [MAC, 해시, 서명](macs-hashes-and-signatures.md)
-   [암호화에 대한 내보내기 제한](export-restrictions-on-cryptography.md)
-   [일반적인 암호화 작업](common-cryptography-tasks.md)

### <a name="62-code-samples"></a>6.2 코드 샘플

-   [자격 증명 보관](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/PasswordVault)
-   [자격 증명 선택](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CredentialPicker)
-   [Azure 로그인을 사용 하 여 장치 잠금](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/DeviceLockdownAzureLogin)
-   [엔터프라이즈 데이터 보호](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/EnterpriseDataProtection)
-   [KeyCredentialManager](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/KeyCredentialManager)
-   [스마트 카드](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SmartCard)
-   [웹 계정 관리](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/WebAccountManagement)
-   [WebAuthenticationBroker](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/WebAuthenticationBroker)

### <a name="63-api-reference"></a>6.3 API 참조

-   [**Windows.security.authentication.onlineid.**](/uwp/api/Windows.Security.Authentication.OnlineId)
-   [**Windows.Security.Authentication.Web**](/uwp/api/Windows.Security.Authentication.Web)
-   [**Windows.Security.Authentication.Web.Core**](/uwp/api/Windows.Security.Authentication.Web.Core)
-   [**Windows. 웹 공급자**](/uwp/api/Windows.Security.Authentication.Web.Provider)
-   [**Windows. 보안 자격 증명**](/uwp/api/Windows.Security.Credentials)
-   [**Windows. 보안 자격 증명**](/uwp/api/Windows.Security.Credentials)
-   [**Windows.Security.Credentials.UI**](/uwp/api/Windows.Security.Credentials.UI)
-   [**Windows.Security.Cryptography**](/uwp/api/Windows.Security.Cryptography)
-   [**Windows. 보안 인증서**](/uwp/api/Windows.Security.Cryptography.Certificates)
-   [**Windows.Security.Cryptography.Core**](/uwp/api/Windows.Security.Cryptography.Core)
-   [**Windows.Security.Cryptography.DataProtection**](/uwp/api/Windows.Security.Cryptography.DataProtection)
-   [**Windows.Security.ExchangeActiveSyncProvisioning**](/uwp/api/Windows.Security.ExchangeActiveSyncProvisioning)
-   [**창. 보안과 데이터**](/uwp/api/Windows.Security.EnterpriseData)