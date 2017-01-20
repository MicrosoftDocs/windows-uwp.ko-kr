---
author: Mtoepke
title: "Xbox One 개발자 프로그램의 UWP에 대해 알려진 문제"
description: 
translationtype: Human Translation
ms.sourcegitcommit: 3f0647bb76340ccbd538e9e4fefe173924d6baf4
ms.openlocfilehash: 18c8d1fcd696f336601dc6c531424fe8bfb78304

---

# <a name="known-issues-with-uwp-on-xbox-developer-program"></a>Xbox 개발자 프로그램에서 UWP에 대해 알려진 문제

이 항목에서는 Xbox One 개발자 프로그램에서 UWP에 대해 알려진 문제를 설명합니다. 이 프로그램에 대한 자세한 내용은 [Xbox의 UWP](index.md)를 참조하세요. 

\[API 참조 항목 링크를 통해 이 페이지를 방문했으며 유니버설 장치 패밀리 API 정보를 보려는 경우 [Xbox에서 아직 지원되지 않는 UWP 기능](http://go.microsoft.com/fwlink/?LinkID=760755)을 참조하세요.\]

다음 목록에는 발생할 수 있는 몇 가지 알려진 문제가 요약되어 있습니다. 

**피드백을 받고 싶으니**, [유니버설 Windows 플랫폼 앱 개발](https://social.msdn.microsoft.com/forums/windowsapps/home?forum=wpdevelop) 포럼에서 발견된 문제를 모두 신고해 주세요. 

문제가 있으면 이 항목의 내용을 살펴보고, [질문과 대답](frequently-asked-questions.md)을 확인하고, 포럼을 통해 도움을 요청하세요.


<!--## Developing games-->

## <a name="issue-when-leaving-dev-mode"></a>개발자 모드 종료 시의 문제
때때로 개발자 홈 사용 중에 또는 개발자 설정에서 개발자 모드를 종료하지 못하는 상황이 발생할 수도 있습니다.
이 문제에는 두 가지 가능한 해결 방법이 있습니다. 
1. 개발자 모드를 종료할 때 **테스트용으로 로드된 게임 및 앱 삭제** 상자의 선택을 취소합니다.
2. 내 게임 및 앱으로 이동하여 콘솔에 설치된 모든 개발자 앱을 제거합니다.
 
<!--## Memory limits for background apps are partially enforced
 
The maximum memory footprint for apps running in the background is 128 megabytes. In the current version of UWP on Xbox One, your app will be suspended if it is above this limit when it is moved to the background. This limit is not currently enforced if your app exceeds the limit while it is already running in the background—this means that if your app exceeds 128 MB while running in the background, it will still be able to allocate memory.
 
There is currently no workaround for this issue. Apps should govern their memory usage accordingly and continue to stay under the 128 MB limit while running in the background.-->
 
## <a name="deploying-from-vs-fails-with-parental-controls-turned-on"></a>자녀 보호를 켠 상태로 VS에서 배포하지 못함

콘솔의 자녀 보호가 설정에서 켜져 있는 경우 VS에서 앱을 시작하지 못합니다.

이 문제를 해결하려면 자녀 보호를 일시적으로 사용하지 않도록 설정하거나 다음을 수행합니다.
1. 자녀 보호를 끄고 콘솔에 앱을 배포합니다.
2. 자녀 보호를 켭니다.
3. 콘솔에서 앱을 시작합니다.
4. 앱이 시작될 수 있도록 PIN 또는 암호를 입력합니다.
5. 앱이 시작됩니다.
6. 앱을 종료합니다.
7. F5 키를 사용하여 VS에서 시작합니다. 앱이 확인 없이 시작됩니다.

이 시점에서는 앱을 제거하고 다시 설치하는 경우에도 사용자를 로그아웃시킬 때까지 권한은 _고정_되어 있습니다.
 
자녀 계정에만 사용할 수 있는 다른 유형의 예외가 있습니다. 자녀 계정에는 부모가 로그인하여 권한을 부여해야 하지만 권한을 부여할 때 부모는 자녀가 앱을 시작하는 것을 **항상** 허용하도록 선택할 수 있습니다. 이 예외는 클라우드에 저장되고 자녀가 로그아웃하고 다시 로그인하는 경우에도 유지됩니다.   

<!--### x86 vs. x64

By the time we release later this year, we will have great support for both x86 and x64, and we do support x86 in this preview. 
However, x64 has had much more testing to date (the Xbox shell and all of the apps running on the console today are x64), and so we recommend using x64 for your projects. 
This is particularly true for games.

If you decide to use x86, please report any issues you see on the forum.

Also see [Switching build flavors can cause deployment failures](known-issues.md#switching-build-flavors-can-cause-deployment-failures) later on this page.-->

<!--### Game engines

We have tested some popular game engines, but not all of them, and our test coverage for this preview has not been comprehensive. 
Your mileage may vary. 

The following game engines have been confirmed to work:
* [Construct 2](https://www.scirra.com/)

There are likely others that are working too. We would love to get your feedback on what you find. 
Please use the forum to report any issues you see.-->

## <a name="directx-12-support"></a>DirectX 12 지원

Xbox One의 UWP는 DirectX 11 기능 수준 10을 지원합니다. 지금은 DirectX 12가 지원되지 않습니다. 

기존의 모든 게임 콘솔과 마찬가지로, Xbox One은 전체 잠재 기능에 액세스하기 위해 특정 SDK가 필요한 특수 하드웨어입니다. Xbox One 하드웨어의 최대 잠재 기능에 액세스해야 하는 게임을 개발하는 경우 DirectX 12 지원을 포함하는 해당 SDK에 액세스하기 위해 [ID@XBOX](http://www.xbox.com/Developers/id) 프로그램에 등록할 수 있습니다.

<!-- ### Xbox One Developer Preview disables game streaming to Windows 10

Activating the Xbox One Developer Preview on your console will prevent you from streaming games from your Xbox One to the Xbox app on Windows 10, even if your console is set to retail mode. 
To restore the game streaming feature, you must leave the developer preview. -->

<!--## System resources for UWP apps and games on Xbox One

UWP apps and games running on Xbox One share resources with the system and other apps, and so the system governs the resources that are available to any one game or app. 
If you are running into memory or performance issues, this may be why. 
For more details, see [System resources for UWP apps and games on Xbox One](system-resource-allocation.md).-->

<!--
## Networking using traditional sockets

In this developer preview, inbound and outbound network access from the console that uses traditional TCP/UDP sockets (WinSock, Windows.Networking.Sockets) is not available. 
Developers can still use HTTP and WebSockets.
--> 

## <a name="blocked-networking-ports-on-xbox-one"></a>Xbox One에서 차단된 네트워킹 포트

Xbox One 장치의 UWP(유니버설 Windows 플랫폼) 앱은 범위 [57344, 65535]의 모든 포트에 바인딩할 수 없습니다. 이러한 포트에 바인딩하면 런타임 시 성공하는 것처럼 보이지만 앱에 도달하기 전에 네트워크 트래픽이 자동으로 삭제될 수 있습니다. 가능할 때마다 앱을 포트 0에 바인딩해야 시스템이 로컬 포트를 선택할 수 있습니다. 특정 포트를 사용해야 할 경우 포트 번호는 범위 [1025 49151]에 있어야 하고 IANA 레지스트리를 사용하여 충돌을 확인하고 방지해야 합니다. 자세한 내용은 [서비스 이름 및 전송 프로토콜 포트 번호 레지스트리](http://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xhtml)를 참조하세요.

## <a name="uwp-api-coverage"></a>UWP API 검사

일부 UWP API는 Xbox에서 지원되지 않습니다. 작동하지 않는다고 알고 있는 API 목록은 [Xbox에서 아직 지원되지 않는 UWP 기능](http://go.microsoft.com/fwlink/p/?LinkId=760755)을 참조하세요. 다른 API에서 문제가 발견된 경우 포럼에서 신고하세요. 

<!--## XAML controls do not look like or behave like the controls in the Xbox One shell

In this developer preview, the XAML controls are not in their final form. In particular:
* Gamepad X-Y navigation does not work reliably for all controls.
* Controls do not look like controls in the Xbox shell. This includes the control focus rectangle.
* Navigating between controls does not automatically make “navigation sounds.”

These issues will be addressed in a future developer preview.-->

<!--## Visual Studio and deployment issues

### Switching build flavors can cause deployment failures

Switching between Debug and Release builds, or between x86 and x64, or between Managed and .Net Native builds, can cause deployment failures. 

The simplest way to avoid these issues for this preview is to stick to Debug and one architecture. 

If you do hit this issue, uninstalling your app in the Collections app on your Xbox One will typically resolve it.

> ****&nbsp;&nbsp;Uninstalling your app from Windows Device Portal (WDP) will not resolve the issue.

If your issues persist, uninstall your app or game in the Collections app, leave Developer Mode, restart to Retail Mode and then switch back to Developer Mode.
You may also need to restart Visual Studio and clean your solution.

For more information, see the “Fixing deployment failures” section in [Frequently asked questions](frequently-asked-questions.md).

### Uninstalling an app while you are debugging it in Visual Studio will cause it to fail silently

Attempting to uninstall an app that is running under the debugger via the WDP “Installed Apps” tool will cause it to silently fail. 
The workaround is to stop debugging the app in Visual Studio before attempting to remove it via WDP.

### Visual Studio/Xbox PIN pairing failures

It is possible to get into a state where the PIN pairing between Visual Studio and your Xbox One gets out of sync. 
If PIN pairing fails, use the “Remove all pairings” button in Dev Home, restart Xbox One, restart your development PC, and then try again.--> 


<!--## Windows Device Portal (WDP) preview-->

<!--### Starting WDP from Dev Home crashes Dev Home

When you start WDP in Dev Home, it will cause Dev Home to crash after you have entered your user name and password and selected **Save**. 
The credentials are saved but WDP is not started. 
You can start WDP by restarting Xbox One.--> 

<!--### Disabling WDP in Dev Home does not work

If you disable WDP in Dev Home, it will be turned off. 
However, when you restart your Xbox One, WDP will be started again. 
You can work around this issue by using **Reset and keep my games & apps** to delete any stored state on your Xbox One. 
Go to Settings > System > Console info & updates > Reset console, and then select the **Reset and keep my games & apps** button.

> **Caution**&nbsp;&nbsp;Doing this will delete all saved settings on your Xbox One including wireless settings, user accounts and any game progress that has not been saved to cloud storage.

> **Caution**&nbsp;&nbsp;DO NOT select the **Reset and remove everything** button.
This will delete all of your games, apps, settings and content, deactivate Developer Mode, and remove you console from the Developer Preview group.

### The columns in the “Running Apps” table do not update predictably. 

Sometimes this is resolved by sorting a column on the table.-->

## <a name="navigating-to-wdp-causes-a-certificate-warning"></a>WDP로 이동하면 인증서 경고가 표시됨

Xbox One 콘솔에서 서명한 보안 인증서는 신뢰할 수 있는 잘 알려진 게시자로 간주되지 않으므로 제공된 인증서에 대해 다음 스크린샷과 유사한 경고가 표시됩니다. Windows Device Portal에 액세스하려면 **이 웹 사이트를 계속 탐색합니다**를 클릭합니다.

![웹 사이트 보안 인증서 경고](images/security_cert_warning.jpg)

<!--## Dev Home

Occasionally, selecting the “Manage Windows Device Portal” option in Dev Home will cause Dev Home to silently exit to the Home screen. 
This is caused by a failure in the WDP infrastructure on the console and can be resolved by restarting the console.-->

## <a name="see-also"></a>참고 항목
- [질문과 대답](frequently-asked-questions.md)
- [Xbox One의 UWP](index.md)



<!--HONumber=Dec16_HO3-->


