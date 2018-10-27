---
title: Xbox Live 로그인 문제 해결
author: KevinAsgari
description: Xbox Live 로그인을 사용 하 여 문제를 해결 하는 방법을 알아봅니다.
ms.assetid: 87b70b4c-c9c1-48ba-bdea-b922b0236da4
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox 로그인 하나, 문제 해결
ms.localizationpriority: medium
ms.openlocfilehash: ca291312345646bb48691615854503f5121c8888
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2018
ms.locfileid: "5697926"
---
# <a name="troubleshooting-xbox-live-sign-in"></a>Xbox Live 로그인 문제 해결

어려움을 겪습니다 로그인 초래할 수 있는 몇 가지 문제가 있습니다.  [UWP 게임용 Visual Studio를 사용 하 여 시작](../../get-started-with-partner/get-started-with-visual-studio-and-uwp.md) 의 단계를 수행 하는 경우 예기치 않은 오류 수를 최소화할 수 있습니다.

## <a name="common-issues"></a>일반적인 문제

### <a name="sandbox-problems"></a>샌드박스 문제

일반적으로 잘 이해 해야 샌드박스 및 Xbox Live 관련 어떻게의 개념입니다.  [Xbox Live 샌드박스](../../xbox-live-sandboxes.md) 가이드에서 자세한 내용을 볼 수 있습니다.

간단 하 게 말해 샌드박스 소매 릴리스 전에 콘텐츠 격리 및 액세스 제어를 적용 합니다.  개발 샌드박스에 액세스 하지 않고 사용자가 모든 읽기를 수행 하거나 쓰기 타이틀에 관한 작업 수 없습니다.  서비스 구성의 변형을 테스트를 위해 다양 한 샌드박스에 게시할 수도 있습니다.

샌드박스 관련해에 주의 해야 할 몇 가지는 아래에서 설명 합니다.

#### <a name="developer-account-doesnt-have-access-to-the-right-sandbox-for-run-time-access"></a>개발자 계정 런타임 액세스에 대 한 올바른 샌드박스를 액세스할 수 없는

* 테스트 계정 (개발 계정 라고도 함) 또는 권한 있는 개발자 계정에 로그인 하는 개발 중인 제목에 대 한 사용 되어야 합니다.  로그인 중 하나를 사용 하 여를 시도 중인 또는 다른 테스트 계정에 XDP 만들어집니다 있는지 확인 [https://xdp.xboxlive.com/User/Contact/MyAccess?selectedMenu=devaccounts](https://xdp.xboxlive.com/User/Contact/MyAccess?selectedMenu=devaccounts). Windows 개발자 센터에서 xbox live 관련된 개발자 계정에 권한을 부여할 수 있습니다.[https://partner.microsoft.com/en-us/xboxconfig/TestAccounts/Creator](https://partner.microsoft.com/en-us/xboxconfig/TestAccounts/Creator)
* 계정에 대 한 액세스 타이틀에 게시 된 샌드박스를 확인 합니다.  XDP에서 만든 테스트 계정을 만든 XDP 계정의 권한을 상속 받습니다.

#### <a name="your-device-is-not-on-the-correct-sandbox"></a>올바른 샌드박스 없는 장치

개발 샌드박스를 개발 하는 장치를 설정 해야 합니다.  Xbox One에서 *Xbox One 관리자*를 사용 하 여 샌드박스를 설정할 수 있습니다.  Windows 10 데스크톱, Xbox Live SDK 설치의 도구 디렉터리에 있는 SwitchSandbox.cmd 스크립트를 사용할 수 있습니다.

#### <a name="your-titles-service-configuration-is-not-published-to-the-correct-development-sandbox"></a>올바른 개발 샌드박스를 게시 되지 타이틀의 서비스 구성

타이틀의 서비스 구성 개발 샌드박스에 게시 되어 있는지 확인 합니다.  있습니다 수 없습니다 사이 니 지에 Xbox Live에 지정 된 개발 샌드박스 제목에 대 한 하지 않는 한 제목 동일한 샌드박스에 게시 된 합니다.  이 작업을 수행 하는 방법에 대해서는 [XDP 설명서](https://developer.xboxlive.com/en-us/xdphelp/development/xdpdocs/Pages/setting_up_service_configuration_03_31_16.aspx#PublishServiceConfig) 를 참조 하십시오. Windows 개발자 센터 구성 게시 하는 방법은 [개발자 센터 문서](../../get-started-with-creators/xbox-live-service-configuration-creators.md#publish-your-xbox-live-service-configuration) 를 읽을 수 있습니다.

### <a name="ids-configured-incorrectly"></a>잘못 구성 된 Id

ID 게임을 구성 하는 데 필요한 몇 가지 있습니다.  만들려는 어떤 유형의 제목에 따라 [UWP 게임에 대 한 Visual Studio 시작](../../get-started-with-partner/get-started-with-visual-studio-and-uwp.md) 및 [크로스 플레이 게임 시작](../../get-started-with-partner/get-started-with-cross-play-games.md) 에 자세한 정보를 볼 수 있습니다.

주의 사항은 다음과 같습니다.

* 앱 ID XDP 또는 개발자 센터에 올바르게 입력 확인
* 사용자 PFN XDP 또는 개발자 센터에 올바르게 입력 확인
* 다시 확인 만든는 xboxservices.config Visual Studio 프로젝트와 동일한 디렉터리에 [새로운 또는 기존 UWP 프로젝트에 Xbox Live 추가](../../get-started-with-partner/get-started-with-visual-studio-and-uwp.md) 가이드에 설명 된 대로 합니다.
* 사용자 appxmanifest에서 "패키지 Id" 올바른지 확인 합니다.  이 "패키지/Id/이름" 앱 Id 섹션에서 Windows 개발자 센터에서으로 Windows 개발자 센터에 표시 됩니다.

### <a name="title-id-or-scid-not-configured-correctly"></a>제목 ID 또는 서비스 안내 올바르게 구성 되지 않음

* UWP 제목 xboxservices.config 파일의 ID 및 서비스 안내 타이틀 올바른 값으로 설정 해야 합니다.  또한이 파일은 u t f 8로 올바르게 형식화 되어 있는지 확인 합니다.  [UWP 게임용 Visual Studio를 사용 하 여 시작](../../get-started-with-partner/get-started-with-visual-studio-and-uwp.md)의 자세한 정보를 볼 수 있습니다. Xboxservices.config 파일은 대/소문자 구분 합니다.
* XDK 제목에 이러한 값에 package.appxmanifest에서 설정 됩니다.
* Xbox Live sdk 샘플 디렉터리에 있는 UWP 및 XDK 모두 제목 구성에 대 한 예제를 볼 수 있습니다.

## <a name="test-using-the-xbox-app"></a>Xbox 앱을 사용 하 여 테스트

UWP 응용 프로그램을 개발 하는 경우에 Xbox 앱을 사용 하 여 몇 가지 문제를 디버깅할 수 있습니다.

1. 디바이스의 샌드박스 SwitchSandbox.cmd 스크립트를 사용 하 여 개발 샌드박스 설정
2. Xbox 앱을 열고 테스트 계정을 사용 하 여 동일한 샌드박스에 대 한 액세스를 사용 하 여 로그인 하려고 시도 합니다.

하지 못하는 경우 성공적으로 로그인 한 다음이 확인 개발 샌드박스 장치에서 올바르게 설정 되었는지와 테스트 계정에 액세스할 수 있습니다.

로그인 오류 여전히 액세스 하는 하는 경우 서비스 구성이 샌드박스, 게시 되지 않은 또는 xboxservices.config가 제대로 설치 되지 않은 가능성이 높습니다. Xboxservices.config 파일은 대/소문자 구분 합니다.

## <a name="debug-based-on-error-code"></a>오류 코드를 기준으로 디버그

다음은 몇 가지 오류 코드에 로그인 하 고이 디버그 하는 단계에 표시 될 수 있습니다.  오류 코드와 같이 표시 됩니다는 아래 스크린샷.

![0x8015DC12 Sign-in Error 스크린샷](../../images/troubleshooting/sign_in_error.png)

### <a name="0x80860003-the-application-is-either-disabled-or-incorrectly-configured"></a>0x80860003는 응용 프로그램이 비활성화 되거나 잘못 구성

1. PFX 파일을 삭제 하십시오입니다.

![솔루션 탐색기에서 pfx 파일](../../images/troubleshooting/pfx_file.png)

Visual Studio는 자동으로 하면 하지 않았던 로그인 경우 Windows 개발자 센터에 앱 프로 비전에 사용 되는 Microsoft 계정을 사용 하 여 Visual Studio로, 개인 Microsoft 계정 또는 도메인 계정에 따라 서명 pfx 파일을 생성 합니다. Appx 패키지를 빌드할 때 자동으로 생성 된 pfx 패키지에 서명 및 "publisher" 부분 package.appxmanifest에서 패키지 id 변경 하려면 Visual Studio 사용 합니다. 결과적으로, 생성 된 비트 (특히 appxmanifest.xml)을 사용 하려는 보다 다른 패키지 id를 갖게 됩니다. 

2. 개발자 센터에서 사용자 package.appxmanifest 타이틀 같은 응용 프로그램 id로 설정 되어 있는지 확인 합니다. 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 저장소에 표시 된 대로... 앱 스토어와 연결할->는 아래 스크린샷. 또는 수동으로 편집 하 여 package.appxmanifest 합니다. 자세한 내용은 [UWP 게임에 대 한 Visual Studio 시작](../../get-started-with-partner/get-started-with-visual-studio-and-uwp.md) 을 참조 하세요.

![스토어 연결](../../images/troubleshooting/appxmanifest_binding.png)

### <a name="0x8015dc12-content-isolation-error"></a>0x8015DC12 콘텐츠 격리 오류

요약 하자면, 장치 또는 사용자 지정된 제목 액세스 하지 않는 권한이 없음을 의미 합니다.

1. 이 테스트 계정 로그인을 시도를 사용 하지 않는 또는 테스트 계정으로 로그인 하는 동일한 샌드박스를 액세스할 수 없는 의미할 수 있습니다. [XDP 설명서](https://developer.xboxlive.com/en-us/xdphelp/development/xdpdocs/Pages/creating_development_accounts_03_31_16.aspx) 에 테스트 계정을 만드는 싶으시면 다시 확인 하 고 [개발자 센터 설명서.](../../xbox-live-test-accounts.md) 필요한 경우에 적절 한 샌드박스에 액세스할 수 있는 새 테스트 계정을 만듭니다.

시작 메뉴에서 설정으로 이동 하 고 계정에 진행 하 여이 수행할 수 있는, Windows 10에서 기존 사용자 계정을 제거 해야 할 수 있습니다.

2. 타이틀은 게시 되지 않았다는 점에 샌드박스를 사용 하 여 시도 하는 두 번 클릭 합니다. 이 작업을 수행 하는 방법에 대 한 내용은 [XDP 설명서](https://developer.xboxlive.com/en-us/xdphelp/development/xdpdocs/Pages/setting_up_service_configuration_03_31_16.aspx#PublishServiceConfig) 또는 [개발자 센터 설명서](../../xbox-live-service-configuration.md#sandbox-ids) 를 참조 하세요.

### <a name="0x87dd0005-unexpected-or-unknown-title"></a>0x87DD0005 알 수 없는 또는 알 수 없는 제목

응용 프로그램 ID와 XDP 바인딩 개발자 센터에 다시 확인 합니다. [새로운 또는 기존 Visual Studio UWP에 Xbox Live 지원 추가](https://docs.microsoft.com/windows-hardware/drivers/devapps/step-1--create-a-uwp-device-app#span-idassociateyourappwiththewindowsstorespanspan-idassociateyourappwiththewindowsstorespanspan-idassociateyourappwiththewindowsstorespanassociate-your-app-with-the-microsoft-store) 지침을 볼 수 있습니다.

### <a name="0x87dd000e-title-not-authorized"></a>0x87DD000E 제목 권한이 없음

장치가 적절 한 개발 샌드박스 설정 되어 있는지 및 사용자에 게 샌드박스에 액세스할 수 있는지 다시 확인 합니다. Xbox 앱을 사용 하 여이 확인 하는 방법에 대 한 자세한 내용은 [Xbox 앱을 사용 하 여 테스트](#test-xbox-app) 섹션을 참조 합니다.

문제가 해결 되지 않으면, 다음 또한 개발자 센터 바인딩 및 앱 ID 설정을 확인 위에서 설명한 대로 합니다.

여기에서 설명 하지 않은 오류를 발생 오류 목록의 오류 코드에 대 한 자세한 내용을 보려면 xbox::services::xbox_live_error_code 설명서를 참조 하십시오. 에 XSAPI errors.h 나타낼 수도 있습니다 포함 됩니다.

이러한 모든 단계 후 경우 여전히 수 없는-로그인 하기 타이틀을 사용 하 여, 지원 스레드에 [포럼](http://forums.xboxlive.com) 에 게시 하거나 하십시오 도달 하 여 댐을.