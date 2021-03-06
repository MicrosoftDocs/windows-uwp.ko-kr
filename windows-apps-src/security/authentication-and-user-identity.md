---
title: 인증 및 사용자 ID
description: UWP (유니버설 Windows 플랫폼) 앱에는 웹 인증 브로커를 사용 하는 SSO (simple Single Sign-On)에서 매우 안전한 2 단계 인증까지 다양 한 사용자 인증 옵션이 있습니다.
ms.assetid: 53E36DDC-200A-4850-AAF0-07ECA3662BB9
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 보안
ms.localizationpriority: medium
ms.openlocfilehash: b2711cfdf418edacb5cb4e9316949f9aeb2e5661
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220186"
---
# <a name="authentication-and-user-identity"></a>인증 및 사용자 ID



UWP (유니버설 Windows 플랫폼) 앱에는 [웹 인증 브로커](web-authentication-broker.md) 를 사용 하는 SSO (simple Single Sign-On)에서 매우 안전한 2 단계 인증까지 다양 한 사용자 인증 옵션이 있습니다.

Facebook, Twitter, Flickr 등의 타사 id 공급자 서비스에 대 한 일반 앱 연결의 경우 [웹 인증 브로커](web-authentication-broker.md)를 사용 합니다. 편의상 [자격 증명](credential-locker.md) 보관을 사용 하 여 사용자의 로그인 정보를 저장 하 고 로밍할 수 있습니다.

Windows 10을 사용 하는 기업은 매우 안전한 2 단계 인증을 가능 하 게 하는 [Microsoft Passport 및 Windows Hello](microsoft-passport.md)를 사용 하는 것이 좋습니다. Microsoft Passport를 사용할 수 없는 경우 [스마트 카드](smart-cards.md) 및 [지문 생체 인식](fingerprint-biometrics.md)을 통해 추가적으로 보안을 강화할 수 있습니다.

<table>
<tr><th>항목</th><th>설명</th></tr>
<tr><td><a href="credential-locker.md">자격 증명 보관</a></td><td>이 문서에서는 앱에서 자격 증명 보관을 사용하여 사용자 자격 증명을 안전하게 저장 및 검색하고 사용자의 Microsoft 계정을 사용하여 디바이스 간에 로밍하는 방법을 설명합니다.</td></tr>

<tr><td><a href="fingerprint-biometrics.md">지문 생체 인식</a> </td><td>이 문서에서는 앱에 지문 생체 인식을 추가하는 방법을 설명합니다. 사용자가 특정 작업에 동의해야 하는 경우 지문 인증 요청을 포함하면 앱의 보안이 향상됩니다. 예를 들어 제한된 리소스 액세스 또는 앱에서 바로 구매 권한을 부여하기 전에 지문 인증을 요구할 수 있습니다. 지문 인증은 <a href="/uwp/api/Windows.Security.Credentials.UI.UserConsentVerifier">Windows.Security.Credentials.UI</a> 네임스페이스의 <a href="/uwp/api/Windows.Security.Credentials.UI">UserConsentVerifier</a> 클래스를 사용하여 관리됩니다.</td></tr>
<tr><td><a href="microsoft-passport.md">Microsoft Passport 및 Windows Hello</a></td><td>이 문서에서는 새로운 Windows 10 Microsoft Passport 기술에 대해 설명하고 개발자가 이 기술을 구현함으로써 해당 앱 및 백 엔드 서비스를 보호할 수 있는 방법을 살펴봅니다. 여기서는 기존 자격 증명의 위협 요소를 완화할 수 있는 이러한 기술의 특징에 대해 중점적으로 설명하고 Windows 10 롤아웃의 일부로 이러한 기술을 디자인 및 배포하는 방법에 대한 지침을 제공합니다. </td></tr>
<tr><td><a href="microsoft-passport-login.md">Microsoft Passport 로그인 앱 만들기</a></td><td>전체 연습의 1부에는 기존의 사용자 이름 및 암호 인증 시스템에 대한 대안으로 Microsoft Passport를 사용하는 Windows 10 UWP(유니버설 Windows 플랫폼) 앱을 만드는 방법이 포함되어 있습니다.</td></tr>
<tr><td><a href="microsoft-passport-login-auth-service.md">Microsoft Passport 로그인 서비스 만들기</a></td><td>전체 연습의 2부에는 Windows 10 UWP(유니버설 Windows 플랫폼) 앱에서 기존의 사용자 이름 및 암호 인증 시스템에 대한 대안으로 Microsoft Passport를 사용하는 방법이 포함되어 있습니다.</td></tr>
<tr><td><a href="smart-cards.md">스마트 카드</a></td><td>이 항목에서는 물리적 스마트 카드 판독기에 액세스하고, 가상 스마트 카드를 만들고, 스마트 카드와 통신하고, 사용자를 인증하고, 사용자 PIN을 다시 설정하고 스마트 카드를 제거하거나 분리하는 방법을 포함하여 앱에서 스마트 카드를 사용하여 사용자를 보안 네트워크 서비스에 연결할 수 있는 방법을 설명합니다.</td></tr>
<tr><td><a href="share-certificates.md">앱 간에 인증서 공유</a></td><td>사용자 ID 및 암호 조합 이상의 보안 인증이 필요한 UWP 앱은 인증을 위해 인증서를 사용할 수 있습니다. 인증서 인증은 사용자를 인증할 때 높은 수준의 신뢰를 제공합니다. 서비스 그룹에서 여러 개의 앱에 대해 사용자를 인증하려는 경우도 있습니다. 이 문서에서는 동일한 인증서를 사용하여 여러 개의 앱을 인증하는 방법 및 보안 웹 서비스 액세스를 위해 제공된 인증서를 사용자가 가져오는 데 편리한 코드를 제공하는 방법을 보여 줍니다.</td></tr>
<tr><td><a href="companion-device-unlock.md">도우미(IoT) 디바이스에서 Windows 잠금 해제</a></td><td>도우미 디바이스는 사용자 인증 환경을 향상하기 위해 Windows 10 데스크톱과 함께 실행할 수 있습니다. 도우미 디바이스 프레임워크를 사용하면 Windows Hello를 사용할 수 없는 경우(예: Windows 10 데스크톱에 안면 인증용 카메라 또는 지문 판독기 디바이스를 사용할 수 없는 경우)에도 도우미 디바이스에서 풍부한 환경을 Microsoft Passport에 제공할 수 있습니다.</td></tr>
<tr><td><a href="web-account-manager.md">웹 계정 관리자</a></td><td>이 문서에서는 새 Windows 10 웹 계정 관리자 Api를 사용 하 여 AccountsSettingsPane를 표시 하 고 UWP (유니버설 Windows 플랫폼) 앱을 Microsoft 또는 Facebook과 같은 외부 id 공급자에 연결 하는 방법을 설명 합니다. 사용자의 사용 권한에서 Microsoft 계정을 사용하도록 요청하고, 액세스 토큰을 받고, 이를 사용하여 기본 작업(프로필 데이터 가져오기, OneDrive에 파일 업로드)을 수행하는 방법에 대해 살펴보겠습니다. </td></tr>
<tr><td><a href="web-authentication-broker.md">웹 인증 브로커</a></td><td>이 문서에서는 앱을 OpenID 또는 OAuth 인증 프로토콜을 사용하는 온라인 ID 공급자(예: Facebook, Twitter, Flickr, Instagram 등)에 연결하는 방법을 설명합니다. <a href="/uwp/api/windows.security.authentication.web.webauthenticationbroker.authenticateasync">AuthenticateAsync</a> 메서드는 온라인 ID 공급자로 요청을 보내고 앱이 액세스할 수 있는 공급자 리소스에 대해 설명하는 액세스 토큰을 다시 가져옵니다.</td></tr>
</table>