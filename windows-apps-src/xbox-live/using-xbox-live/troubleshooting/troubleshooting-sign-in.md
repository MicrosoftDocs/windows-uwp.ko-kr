---
title: Xbox Live에 로그인 문제 해결
description: Xbox Live에 로그인을 사용 하 여 문제를 해결 하는 방법을 알아봅니다.
ms.assetid: 87b70b4c-c9c1-48ba-bdea-b922b0236da4
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, xbox 로그인 하나, 문제 해결
ms.localizationpriority: medium
ms.openlocfilehash: ff8d66105d8a1a44708bf23a681767a3044cb654
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57630328"
---
# <a name="troubleshooting-xbox-live-sign-in"></a>Xbox Live에 로그인 문제 해결

어려움 로그인 일으킬 수 있는 몇 가지 문제가 있습니다.  단계를 수행 하는 경우 [UWP 게임을 위한 Visual Studio를 사용 하 여 시작](../../get-started-with-partner/get-started-with-visual-studio-and-uwp.md) 예기치 않은 오류를 최소화할 수 있습니다.

## <a name="common-issues"></a>일반적인 문제

### <a name="sandbox-problems"></a>샌드박스 문제

일반적으로 잘 이해 해야 샌드박스 및 Xbox Live에 관련 된 개념입니다.  자세한 정보를 볼 수 있습니다 합니다 [Xbox Live 샌드박스](../../xbox-live-sandboxes.md) 가이드입니다.

요약 하자면, 샌드박스 소매 릴리스 전에 콘텐츠 격리 및 액세스 제어를 적용 합니다.  개발 샌드박스에 액세스 하지 않고 사용자가 쓰기 작업을 제목에 속하는 또는 읽기를 수행할 수 없습니다.  또한 테스트에 대 한 다양 한 샌드박스에 서비스 구성의 변형에 게시할 수 있습니다.

샌드박스와 관련 하 여에 대 한 주의 해야 할 몇 가지 작업은 아래 설명 되어 있습니다.

#### <a name="developer-account-doesnt-have-access-to-the-right-sandbox-for-run-time-access"></a>개발자 계정에는 런타임 액세스에 대 한 올바른 샌드박스에 액세스할 수 없습니다.

* 테스트 계정을 (개발 계정 라고도 함) 또는 권한이 부여 된 개발자 계정에 로그인 하는 개발 하는 제목에 대 한 사용 되어야 합니다.  있는지 확인 하려고 하나으로 로그인 하거나 다른 테스트 계정에서 XDP에 생성 됩니다 [ https://xdp.xboxlive.com/User/Contact/MyAccess?selectedMenu=devaccounts ](https://xdp.xboxlive.com/User/Contact/MyAccess?selectedMenu=devaccounts)합니다. 파트너 센터의 xbox live 연결 된 개발자 계정 권한을 부여할 수 있습니다. [https://partner.microsoft.com/en-us/xboxconfig/TestAccounts/Creator](https://partner.microsoft.com/en-us/xboxconfig/TestAccounts/Creator)
* 계정에 게시 된 프로그램 제목 샌드박스에 액세스 권한을 갖는지 확인 합니다.  XDP에서 만든 테스트 계정을 만들 XDP 계정의 사용 권한을 상속합니다

#### <a name="your-device-is-not-on-the-correct-sandbox"></a>장치의 올바른 샌드박스의 아닙니다.

개발 하는 장치는 개발 샌드박스가로 설정 되어야 합니다.  Xbox One에 설정할 수 있습니다 사용 하 여 샌드박스 *Xbox One Manager*합니다.  Windows 10 데스크톱, Xbox Live SDK 설치의 도구 디렉터리에 있는 SwitchSandbox.cmd 스크립트를 사용할 수 있습니다.

#### <a name="your-titles-service-configuration-is-not-published-to-the-correct-development-sandbox"></a>타이틀의 서비스 구성의 올바른 개발 샌드박스에 게시 되지 않았습니다.

개발 샌드박스에서 타이틀의 서비스 구성이 게시 됨을 확인 합니다.  없습니다 로그인 Xbox Live에 지정 된 개발 샌드박스에서 제목에 대 한 경우가 아니면 제목 동일한 샌드박스에서에 게시 되는.  참조 하십시오 합니다 [XDP 설명서](https://developer.xboxlive.com/en-us/xdphelp/development/xdpdocs/Pages/setting_up_service_configuration_03_31_16.aspx#PublishServiceConfig) 이 작업을 수행 하는 방법에 대 한 정보에 대 한 합니다. 읽을 수는 [파트너 센터 설명서](../../get-started-with-creators/xbox-live-service-configuration-creators.md#publish-your-xbox-live-service-configuration) 파트너 센터 구성을 게시 하는 방법을 알아보려면.

### <a name="ids-configured-incorrectly"></a>올바르게 구성 된 Id

게임을 구성 하는 데 필요한 ID의 몇 가지 있습니다.  자세한 정보를 볼 수 있습니다 [UWP 게임을 위한 Visual Studio를 사용 하 여 시작](../../get-started-with-partner/get-started-with-visual-studio-and-uwp.md) 및 [간 play 게임 시작](../../get-started-with-partner/get-started-with-cross-play-games.md) 타이틀의 유형에 따라 만들어집니다.

주의 해야 할 몇 가지 사항은 다음과 같습니다.

* 앱 ID XDP 또는 파트너 센터를 올바르게 입력 했는지 확인
* 프로그램 PFN XDP 또는 파트너 센터를 올바르게 입력 했는지 확인
* 다시 확인는 xboxservices.config에서에서 만든 Visual Studio 프로젝트와 동일한 디렉터리에 설명 된 대로 합니다 [신규 또는 기존 UWP 프로젝트에 추가 Xbox Live](../../get-started-with-partner/get-started-with-visual-studio-and-uwp.md) 가이드입니다.
* "패키지 Id" 사용자 appxmanifest에 올바른지 확인 합니다.  "패키지/Id/이름" 앱 Id 섹션에서으로 파트너 센터에 표시 됩니다.

### <a name="title-id-or-scid-not-configured-correctly"></a>제목 ID 또는 올바르게 구성 되지 않은 서비스 안내

* UWP 타이틀에 대 한 xboxservices.config 파일에서 ID 및 서비스 안내를 제목 올바른 값으로 설정 되어야 합니다.  또한이 파일이 UTF8으로 올바른 형식 인지 확인 합니다.  자세한 정보를 볼 수 있습니다 [UWP 게임을 위한 Visual Studio를 사용 하 여 시작](../../get-started-with-partner/get-started-with-visual-studio-and-uwp.md)합니다. Xboxservices.config 파일은 대/소문자 구분입니다.
* XDK 타이틀에 대 한 이러한 값에 package.appxmanifest에서 설정 됩니다.
* Xbox Live SDK의 샘플 디렉터리에 있는 UWP와 XDK 제목 구성에 대 한 예제를 볼 수 있습니다.

## <a name="test-using-the-xbox-app"></a>Xbox 앱을 사용 하 여 테스트

UWP 응용 프로그램을 개발 하는 경우에 Xbox 앱을 사용 하 여 몇 가지 문제를 디버깅할 수 있습니다.

1. 장치의 샌드박스 SwitchSandbox.cmd 스크립트를 사용 하는 개발 샌드박스가로 설정
2. Xbox 앱을 열고 동일한 샌드박스에서에 대 한 액세스를 사용 하 여 테스트 계정을 사용 하 여 로그인을 시도 합니다.

할 경우 성공적으로 로그인 한 다음이 확인 개발 샌드박스에 장치에서 올바르게 설정 되어 있는지를 테스트 계정에 액세스할 수 있습니다.

로그인 오류가 여전히 발생 하는 경우는 서비스 구성에 샌드박스에 게시 되지 않거나 사용자 xboxservices.config가 제대로 설치 되지 않은 것일 수 있습니다. Xboxservices.config 파일은 대/소문자 구분입니다.

## <a name="debug-based-on-error-code"></a>오류 코드를 기반으로 디버그

다음은 로그인 및 이러한 디버깅을 수행할 수 있는 단계에 따라 표시 될 수 있습니다 하는 오류 코드 중 일부입니다.  오류 코드와 같이 표시 됩니다는 아래 스크린샷.

![0x8015DC12 sign In 오류 스크린샷](../../images/troubleshooting/sign_in_error.png)

### <a name="0x80860003-the-application-is-either-disabled-or-incorrectly-configured"></a>응용 프로그램 사용 하지 않도록 설정 또는 잘못 구성 된 0x80860003

1. PFX 파일을 삭제 해 보십시오.

![솔루션 탐색기에서 pfx 파일](../../images/troubleshooting/pfx_file.png)

Visual Studio는 자동 경우 하지 않은 로그인 파트너 센터에서 앱을 프로 비전에 사용 되는 Microsoft 계정을 사용 하 여 Visual studio를 개인 Microsoft 계정 또는 도메인 계정을 기반으로 서명 pfx 파일을 생성 합니다. Appx 패키지를 빌드할 때 자동으로 생성 된 pfx 하 고 패키지에 서명 및 package.appxmanifest에서 패키지 id의 "게시자" 부분을 변경 하는 Visual Studio 사용 합니다. 결과적으로 생성 된 비트 (특히 appxmanifest.xml)를 사용 하려는 보다 다양 한 패키지 id를 갖습니다. 

2. 파트너 센터에서 package.appxmanifest에 제목으로 동일한 응용 프로그램 id로 설정 되어 있는지 다시 확인 합니다. 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 저장소를 선택 합니다. 저장소와 앱 연결->... 에 표시 된 대로 아래 스크린샷. 또는 수동으로 편집 하 여 package.appxmanifest입니다. 참조 [UWP 게임을 위한 Visual Studio를 사용 하 여 시작](../../get-started-with-partner/get-started-with-visual-studio-and-uwp.md) 자세한 내용은 합니다.

![스토어에 연결](../../images/troubleshooting/appxmanifest_binding.png)

### <a name="0x8015dc12-content-isolation-error"></a>콘텐츠 격리 0x8015DC12 오류

요약 하자면, 즉, 장치 또는 사용자로 지정 된 제목에 액세스할 수 없습니다.

1. 이 테스트 계정으로 로그인 하는 동일한 샌드박스를 액세스할 수 없는 또는 로그인을 시도 하려면 테스트 계정을 사용 하지 않는 의미할 수 있습니다. 테스트 계정 만들기에 대 한 지침을 확인 한 다음 합니다 [XDP 설명서](https://developer.xboxlive.com/en-us/xdphelp/development/xdpdocs/Pages/creating_development_accounts_03_31_16.aspx) 고 [파트너 센터 설명서.](../../xbox-live-test-accounts.md) 필요한 경우 적절 한 샌드박스에 대 한 액세스를 사용 하 여 새 테스트 계정을 만듭니다.

Windows 10에서 이전 계정을 제거 해야 하는 시작 메뉴에서 설정으로 이동 하 고 계정으로 이동 하 여 수행할 수 있습니다,

2. 제목에 게시 된 샌드박스 사용 하려고 하는 다시 확인 합니다. 참조 하십시오 합니다 [XDP 설명서](https://developer.xboxlive.com/en-us/xdphelp/development/xdpdocs/Pages/setting_up_service_configuration_03_31_16.aspx#PublishServiceConfig) 또는 [파트너 센터 설명서](../../xbox-live-service-configuration.md#sandbox-ids) 이 작업을 수행 하는 방법에 대 한 정보에 대 한 합니다.

### <a name="0x87dd0005-unexpected-or-unknown-title"></a>0x87DD0005 예기치 않은 또는 알 수 없는 제목

응용 프로그램 ID 설치 및 바인딩 XDP 개발자 센터에 다시 확인 합니다. 지침을 볼 수 있습니다 [신규 또는 기존 Visual Studio UWP에 Xbox Live 지원 추가](https://docs.microsoft.com/windows-hardware/drivers/devapps/step-1--create-a-uwp-device-app#span-idassociateyourappwiththewindowsstorespanspan-idassociateyourappwiththewindowsstorespanspan-idassociateyourappwiththewindowsstorespanassociate-your-app-with-the-microsoft-store)

### <a name="0x87dd000e-title-not-authorized"></a>권한이 없는 0x87DD000E 제목

장치의 적절 한 개발 샌드박스가로 설정 되어 있는지와 사용자 샌드박스에 대 한 액세스 권한이 있는지 다시 확인 합니다. 참조 된 [Xbox 앱을 사용 하 여 테스트할](#test-using-the-xbox-app) Xbox 앱을 사용 하 여이 확인 하는 방법에 대 한 자세한 내용은 섹션입니다.

문제를 해결 되지 않으면는 또한 확인 Dev Center 바인딩 및 앱 ID 설치 위에 설명 된 대로.

위에서 설명 하지 않은 오류가 발생 하는 경우 오류 코드에 대 한 자세한 정보를 보려면 xbox::services::xbox_live_error_code 설명서에서 오류 목록을 참조 하세요. XSAPI에서 errors.h를 참조할 수도 있습니다 포함 되어 있습니다.

이러한 모든 단계 후 계속 수 없습니다. 로그인을 제목, 경우에 게시 하세요 지원 스레드는 [포럼](https://forums.xboxlive.com) 하거나 프로그램 파악 합니다.
