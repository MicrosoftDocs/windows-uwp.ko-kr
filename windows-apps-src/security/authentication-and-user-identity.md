---
title: 인증 및 사용자 ID
description: UWP(유니버설 Windows 플랫폼) 앱에는 웹 인증 브로커를 사용하는 간단한 SSO(single sign-on)부터 높은 보안 수준의 2단계 인증에 이르기까지 사용자 인증을 위한 여러 옵션이 있습니다.
ms.assetid: 53E36DDC-200A-4850-AAF0-07ECA3662BB9
author: PatrickFarley
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 보안
ms.localizationpriority: medium
ms.openlocfilehash: 6f446299dcf1a0bcf93d483d13c926c6e4cd230f
ms.sourcegitcommit: f2f4820dd2026f1b47a2b1bf2bc89d7220a79c1a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/22/2018
ms.locfileid: "2795376"
---
# <a name="authentication-and-user-identity"></a>인증 및 사용자 ID



UWP(유니버설 Windows 플랫폼) 앱에는 [웹 인증 브로커](web-authentication-broker.md)를 사용하는 간단한 SSO(single sign-on)부터 높은 보안 수준의 2단계 인증에 이르기까지 사용자 인증을 위한 여러 옵션이 있습니다.

Facebook, Twitter, Flickr 등과 같이 타사 ID 공급자 서비스에 대한 일반적인 앱 연결의 경우에는 [웹 인증 브로커](web-authentication-broker.md)를 사용합니다. 더 편리하게 사용하려면 [자격 증명 보관](credential-locker.md)을 사용하여 사용자의 로그인 정보를 저장하고 로밍합니다.

Windows 10을 사용하는 기업에서는 수준 높은 2단계 인증을 제공하는 [Microsoft Passport 및 Windows Hello](microsoft-passport.md)를 사용할 것을 고려해야 합니다. Microsoft Passport를 사용할 수 없는 경우 [스마트 카드](smart-cards.md) 및 [지문 생체 인식](fingerprint-biometrics.md)을 통해 추가적으로 보안을 강화할 수 있습니다.

<table>
<tr><th>항목</th><th>설명</th></tr>
<tr><td><a href="credential-locker.md">자격 증명 보관</a></td><td>이 문서에서는 앱에서 자격 증명 보관을 사용하여 사용자 자격 증명을 안전하게 저장 및 검색하고 사용자의 Microsoft 계정을 사용하여 디바이스 간에 로밍하는 방법을 설명합니다.</td></tr>

<tr><td><a href="fingerprint-biometrics.md">지문 생체 인식</a> </td><td>이 문서에서는 앱에 지문 생체 인식을 추가하는 방법을 설명합니다. 사용자가 특정 작업에 동의해야 하는 경우 지문 인증 요청을 포함하면 앱의 보안이 향상됩니다. 예를 들어 제한된 리소스 액세스 또는 앱에서 바로 구매 권한을 부여하기 전에 지문 인증을 요구할 수 있습니다. 지문 인증은 <a href="https://msdn.microsoft.com/library/windows/apps/hh701356">Windows.Security.Credentials.UI</a> 네임스페이스의 <a href="https://msdn.microsoft.com/library/windows/apps/dn279134">UserConsentVerifier</a> 클래스를 사용하여 관리됩니다.</td></tr>
<tr><td><a href="microsoft-passport.md">Microsoft Passport 및 Windows Hello</a></td><td>이 문서에서는 새로운 Windows 10 Microsoft Passport 기술에 대해 설명하고 개발자가 이 기술을 구현함으로써 해당 앱 및 백 엔드 서비스를 보호할 수 있는 방법을 살펴봅니다. 여기서는 기존 자격 증명의 위협 요소를 완화할 수 있는 이러한 기술의 특징에 대해 중점적으로 설명하고 Windows 10 롤아웃의 일부로 이러한 기술을 디자인 및 배포하는 방법에 대한 지침을 제공합니다. </td></tr>
<tr><td><a href="microsoft-passport-login.md">Microsoft Passport 로그인 앱 만들기</a></td><td>전체 연습의 1부에는 기존의 사용자 이름 및 암호 인증 시스템에 대한 대안으로 Microsoft Passport를 사용하는 Windows 10 UWP(유니버설 Windows 플랫폼) 앱을 만드는 방법이 포함되어 있습니다.</td></tr>
<tr><td><a href="microsoft-passport-login-auth-service.md">Microsoft Passport 로그인 서비스 만들기</a></td><td>전체 연습의 2부에는 Windows 10 UWP(유니버설 Windows 플랫폼) 앱에서 기존의 사용자 이름 및 암호 인증 시스템에 대한 대안으로 Microsoft Passport를 사용하는 방법이 포함되어 있습니다.</td></tr>
<tr><td><a href="smart-cards.md">스마트 카드</a></td><td>이 항목에서는 물리적 스마트 카드 판독기에 액세스하고, 가상 스마트 카드를 만들고, 스마트 카드와 통신하고, 사용자를 인증하고, 사용자 PIN을 다시 설정하고 스마트 카드를 제거하거나 분리하는 방법을 포함하여 앱에서 스마트 카드를 사용하여 사용자를 보안 네트워크 서비스에 연결할 수 있는 방법을 설명합니다.</td></tr>
<tr><td><a href="share-certificates.md">앱 간에 공유 인증서</a></td><td>사용자 ID 및 암호 조합 이상의 보안 인증이 필요한 UWP 앱은 인증을 위해 인증서를 사용할 수 있습니다. 인증서 인증은 사용자를 인증할 때 높은 수준의 신뢰를 제공합니다. 서비스 그룹에서 여러 개의 앱에 대해 사용자를 인증하려는 경우도 있습니다. 이 문서에서는 동일한 인증서를 사용하여 여러 개의 앱을 인증하는 방법 및 보안 웹 서비스 액세스를 위해 제공된 인증서를 사용자가 가져오는 데 편리한 코드를 제공하는 방법을 보여 줍니다.</td></tr>
<tr><td><a href="companion-device-unlock.md">도우미 IoT 디바이스에서 Windows 잠금 해제</a></td><td>도우미 디바이스는 사용자 인증 환경을 향상하기 위해 Windows 10 데스크톱과 함께 실행할 수 있습니다. 도우미 디바이스 프레임워크를 사용하면 Windows Hello를 사용할 수 없는 경우(예: Windows 10 데스크톱에 안면 인증용 카메라 또는 지문 판독기를 사용할 수 없는 경우)에도 Microsoft Passport에 풍부한 환경을 제공할 수 있습니다.</td></tr>
<tr><td><a href="web-account-manager.md">웹 계정 관리자</a></td><td>이 문서는 새로운 Windows 10 웹 계정 관리자를 사용하여 AccountsSettingsPane을 표시하고 UWP(유니버설 Windows 플랫폼) 앱을 외부 ID 공급자(예: Microsoft, Facebook)에 연결하는 방법에 대해 설명합니다. 사용자의 사용 권한에서 Microsoft 계정을 사용하도록 요청하고, 액세스 토큰을 받고, 이를 사용하여 기본 작업(프로필 데이터 가져오기, OneDrive에 파일 업로드)을 수행하는 방법에 대해 살펴보겠습니다. </td></tr>
<tr><td><a href="web-authentication-broker.md">웹 인증 브로커</a></td><td>이 문서에서는 앱을 OpenID 또는 OAuth 인증 프로토콜을 사용하는 온라인 ID 공급자(예: Facebook, Twitter, Flickr, Instagram 등)에 연결하는 방법을 설명합니다. <a href="https://msdn.microsoft.com/library/windows/apps/br212066">AuthenticateAsync</a> 메서드는 온라인 ID 공급자로 요청을 보내고 앱이 액세스할 수 있는 공급자 리소스에 대해 설명하는 액세스 토큰을 다시 가져옵니다.</td></tr>
</table>

