---
title: 보안
description: 이 섹션에 Windows10 보안 유니버설 Windows 플랫폼 (UWP) 앱을 빌드에 대 한 문서가 포함 되어 있습니다.
ms.assetid: 41E2EEFB-E8A9-4592-814C-72B703CD952C
author: msatranjr
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 보안
ms.localizationpriority: medium
ms.openlocfilehash: a415c456d5743a694c998e7463a70d3c53a5bb75
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5832630"
---
# <a name="security"></a>보안



이 섹션에 Windows10 보안 유니버설 Windows 플랫폼 (UWP) 앱을 빌드에 대 한 문서가 포함 되어 있습니다.

## <a name="introduction"></a>소개 

Windows 또는 UWP 개발을 처음 접하는 경우에는 [보안 Windows 앱 개발 소개](intro-to-secure-windows-app-development.md)부터 시작합니다. 소개 수준의 이 문서는 앱 및 Windows 10에서 사용할 수 있는 다양한 기능의 보안 고려 사항에 대한 개요를 제공합니다.

## <a name="authentication-and-user-identity"></a>인증 및 사용자 ID

[인증 및 사용자 ID 섹션](authentication-and-user-identity.md)에는 사용자 로그인 및 ID와 관련된 시나리오에 대한 연습이 포함되어 있습니다. 앱에는 [웹 인증 브로커](web-authentication-broker.md)를 사용하는 간단한 SSO(single sign-on)부터 높은 보안 수준의 2단계 인증에 이르기까지 사용자 인증을 위한 여러 옵션이 있습니다.

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
<tr><td><a href="web-account-manager.md">웹 계정 관리자</a></td><td>이 문서는 Windows10 웹 계정 관리자 API를 사용하여 AccountsSettingsPane을 표시하고 UWP(유니버설 Windows 플랫폼) 앱을 외부 ID 공급자(예: Microsoft, Facebook)에 연결하는 방법에 대해 설명합니다. 사용자의 사용 권한에서 Microsoft 계정을 사용하도록 요청하고, 액세스 토큰을 받고, 이를 사용하여 기본 작업(프로필 데이터 가져오기, OneDrive에 파일 업로드)을 수행하는 방법에 대해 살펴보겠습니다. </td></tr>
<tr><td><a href="web-authentication-broker.md">웹 인증 브로커</a></td><td>이 문서에서는 앱을 OpenID 또는 OAuth 인증 프로토콜을 사용하는 온라인 ID 공급자(예: Facebook, Twitter, Flickr, Instagram 등)에 연결하는 방법을 설명합니다. <a href="https://msdn.microsoft.com/library/windows/apps/br212066">AuthenticateAsync</a> 메서드는 온라인 ID 공급자로 요청을 보내고 앱이 액세스할 수 있는 공급자 리소스에 대해 설명하는 액세스 토큰을 다시 가져옵니다.</td></tr>
</table>

## <a name="cryptography"></a>암호화 

암호화 섹션에는 더 복잡한, 암호화 관련 항목에 대한 정보가 포함되어 있습니다. 

| 항목                                                                         | 설명                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|-------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [인증서 소개](certificates.md)                                      | 이 문서에서는 앱의 인증서 사용에 대해 설명합니다. 디지털 인증서는 공개 키를 개인, 컴퓨터 또는 조직에 바인딩하는 공개 키 암호화에 사용됩니다. 바인딩 ID는 한 대상을 다른 대상에 인증하는 데 가장 많이 사용됩니다. 예를 들면, 웹 서버를 사용자에게 인증하거나 사용자를 웹 서버에 인증하는 데 사용됩니다. 인증서 요청을 만들고 발급된 인증서를 설치하거나 가져올 수 있습니다. 또한 인증서 계층에 인증서를 등록할 수도 있습니다. |
| [암호화 키](cryptographic-keys.md)                                   | 이 문서에서는 표준 키 파생 함수를 사용하여 키를 파생하는 방법과 대칭 및 비대칭 키를 사용하여 콘텐츠를 암호화하는 방법을 보여 줍니다.                                                                                                                                                                                                                                                                                                                                                                         |
| [데이터 보호](data-protection.md)                                         | 이 문서에서는 [Windows.Security.Cryptography.DataProtection](https://msdn.microsoft.com/library/windows/apps/br241585) 네임스페이스의 [DataProtectionProvider](https://msdn.microsoft.com/library/windows/apps/br241559) 클래스를 사용하여 UWP 앱에서 디지털 데이터를 암호화 및 해독하는 방법을 설명합니다.                                                                                                                                                                                                              |
| [MAC, 해시, 서명](macs-hashes-and-signatures.md)               | 이 문서에서는 앱에서 MAC(메시지 인증 코드), 해시 및 서명을 사용하여 메시지 변조를 감지하는 방법을 설명합니다.                                                                                                                                                                                                                                                                                                                                                                                |
| [암호화에 대한 내보내기 제한](export-restrictions-on-cryptography.md) | 이 정보를 사용하여 앱이 Windows 스토어에 나열될 수 없는 방식으로 암호화를 사용하는지 확인할 수 있습니다.                                                                                                                                                                                                                                                                                                                                                                                                     |
| [일반적인 암호화 작업](common-cryptography-tasks.md)                     | 이러한 문서에서는 난수 만들기, 버퍼 비교, 문자열과 이진 데이터 간 변환, 바이트 배열에서 복사 및 데이터 인코딩 및 디코딩 등의 일반적인 암호화 작업에 대한 예제 코드를 제공합니다.                                                                                                                                                                                                                                                                                    |
