---
title: 인증 및 사용자 ID
description: UWP (유니버설 Windows 플랫폼) 앱에는 웹 인증 브로커를 사용 하는 SSO (simple Single Sign-On)에서 매우 안전한 2 단계 인증까지 다양 한 사용자 인증 옵션이 있습니다.
ms.assetid: 53E36DDC-200A-4850-AAF0-07ECA3662BB9
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 보안
ms.localizationpriority: medium
ms.openlocfilehash: a7e13e0488234b47fb49e5ce2a73743718b595e6
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89157898"
---
# <a name="authentication-and-user-identity"></a>인증 및 사용자 ID



UWP (유니버설 Windows 플랫폼) 앱에는 [웹 인증 브로커](web-authentication-broker.md) 를 사용 하는 SSO (simple Single Sign-On)에서 매우 안전한 2 단계 인증까지 다양 한 사용자 인증 옵션이 있습니다.

Facebook, Twitter, Flickr 등의 타사 id 공급자 서비스에 대 한 일반 앱 연결의 경우 [웹 인증 브로커](web-authentication-broker.md)를 사용 합니다. 편의상 [자격 증명](credential-locker.md) 보관을 사용 하 여 사용자의 로그인 정보를 저장 하 고 로밍할 수 있습니다.

Windows 10을 사용 하는 기업은 매우 안전한 2 단계 인증을 가능 하 게 하는 [Microsoft Passport 및 Windows Hello](microsoft-passport.md)를 사용 하는 것이 좋습니다. Microsoft Passport를 사용할 수 없는 경우 [스마트 카드](smart-cards.md) 및 [지문 생체 인식](fingerprint-biometrics.md)을 통해 추가적으로 보안을 강화할 수 있습니다.

<table>
<tr><th>항목</th><th>Description</th></tr>
<tr><td><a href="credential-locker.md">자격 증명 보관</a></td><td>이 문서에서는 앱에서 자격 증명 보관을 사용 하 여 사용자 자격 증명을 안전 하 게 저장 및 검색 하 고 사용자의 Microsoft 계정 있는 장치 간에 로밍 하는 방법에 대해 설명 합니다.</td></tr>

<tr><td><a href="fingerprint-biometrics.md">지문 생체 인식</a> </td><td>이 문서에서는 앱에 지문 생체 인식을 추가 하는 방법을 설명 합니다. 사용자가 특정 작업에 동의 해야 하는 경우 지문 인증에 대 한 요청을 포함 하 여 앱의 보안을 강화 합니다. 예를 들어 앱 내 구매에 권한을 부여 하기 전에 또는 제한 된 리소스에 액세스 하기 전에 지문 인증을 요구할 수 있습니다. 지문 인증은 <a href="/uwp/api/Windows.Security.Credentials.UI.UserConsentVerifier">Windows.Security.Credentials.UI</a> 네임스페이스의 <a href="https://docs.microsoft.com/uwp/api/Windows.Security.Credentials.UI">UserConsentVerifier</a> 클래스를 사용하여 관리됩니다.</td></tr>
<tr><td><a href="microsoft-passport.md">Microsoft Passport 및 Windows Hello</a></td><td>이 문서에서는 새로운 Windows 10 Microsoft Passport 기술에 대해 설명 하 고 개발자가이 기술을 구현 하 여 앱 및 백 엔드 서비스를 보호 하는 방법을 설명 합니다. 기본 자격 증명의 위협을 완화 하는 데 도움이 되는 이러한 기술의 특정 기능을 강조 하 고 Windows 10 롤아웃의 일부로 이러한 기술을 디자인 및 배포 하는 방법에 대 한 지침을 제공 합니다. </td></tr>
<tr><td><a href="microsoft-passport-login.md">Microsoft Passport 로그인 앱 만들기</a></td><td>기존 사용자 이름 및 암호 인증 시스템의 대 안으로 Microsoft Passport를 사용 하는 Windows 10 UWP (유니버설 Windows 플랫폼) 앱을 만드는 방법에 대 한 전체 연습 1 부.</td></tr>
<tr><td><a href="microsoft-passport-login-auth-service.md">Microsoft Passport 로그인 서비스 만들기</a></td><td>Windows 10 UWP (유니버설 Windows 플랫폼) 앱에서 기존 사용자 이름 및 암호 인증 시스템 대신 Microsoft Passport를 사용 하는 방법에 대 한 전체 연습 과정 2 부</td></tr>
<tr><td><a href="smart-cards.md">스마트 카드</a></td><td>이 항목에서는 물리적 스마트 카드 판독기에 액세스하고, 가상 스마트 카드를 만들고, 스마트 카드와 통신하고, 사용자를 인증하고, 사용자 PIN을 다시 설정하고 스마트 카드를 제거하거나 분리하는 방법을 포함하여 앱에서 스마트 카드를 사용하여 사용자를 보안 네트워크 서비스에 연결할 수 있는 방법을 설명합니다.</td></tr>
<tr><td><a href="share-certificates.md">앱 간에 인증서 공유</a></td><td>사용자 Id 및 암호 조합 이외의 보안 인증을 필요로 하는 UWP 앱은 인증을 위해 인증서를 사용할 수 있습니다. 인증서 인증은 사용자를 인증할 때 높은 수준의 신뢰를 제공 합니다. 경우에 따라 서비스 그룹은 여러 앱에 대해 사용자를 인증 하려고 합니다. 이 문서에서는 동일한 인증서를 사용 하 여 여러 앱을 인증 하는 방법과 사용자가 보안 웹 서비스에 액세스 하기 위해 제공 된 인증서를 가져오는 편리한 코드를 제공할 수 있는 방법을 보여 줍니다.</td></tr>
<tr><td><a href="companion-device-unlock.md">부록 IoT 장치를 사용 하 여 Windows 잠금 해제</a></td><td>도우미 디바이스는 사용자 인증 환경을 향상하기 위해 Windows 10 데스크톱과 함께 실행할 수 있습니다. 도우미 디바이스 프레임워크를 사용하면 Windows Hello를 사용할 수 없는 경우(예: Windows 10 데스크톱에 안면 인증용 카메라 또는 지문 판독기 디바이스를 사용할 수 없는 경우)에도 도우미 디바이스에서 풍부한 환경을 Microsoft Passport에 제공할 수 있습니다.</td></tr>
<tr><td><a href="web-account-manager.md">웹 계정 관리자</a></td><td>이 문서에서는 새 Windows 10 웹 계정 관리자 Api를 사용 하 여 AccountsSettingsPane를 표시 하 고 UWP (유니버설 Windows 플랫폼) 앱을 Microsoft 또는 Facebook과 같은 외부 id 공급자에 연결 하는 방법을 설명 합니다. 사용자의 권한을 요청 하 여 Microsoft 계정 사용 권한을 요청 하 고, 액세스 토큰을 가져오고,이를 사용 하 여 프로필 데이터 가져오기 또는 OneDrive에 파일 업로드와 같은 기본 작업을 수행 하는 방법을 알아봅니다. </td></tr>
<tr><td><a href="web-authentication-broker.md">웹 인증 브로커</a></td><td>이 문서에서는 Openid connect 또는 OAuth와 같은 인증 프로토콜을 사용 하는 온라인 id 공급자 (예: Facebook, Twitter, Flickr, Instagram 등)에 앱을 연결 하는 방법을 설명 합니다. <a href="/uwp/api/windows.security.authentication.web.webauthenticationbroker.authenticateasync">AuthenticateAsync</a> 메서드는 온라인 id 공급자에 게 요청을 보내고 앱이 액세스할 수 있는 공급자 리소스를 설명 하는 액세스 토큰을 다시 가져옵니다.</td></tr>
</table>