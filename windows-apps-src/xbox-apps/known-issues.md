---
author: Mtoepke
title: "Xbox One Developer Preview의 UWP에 대해 알려진 문제"
description: 
area: Xbox
translationtype: Human Translation
ms.sourcegitcommit: 3de603aec1dd4d4e716acbbb3daa52a306dfa403
ms.openlocfilehash: e016be20af9a0d7a67fa383cbdc93083d12a1113

---

# Xbox Developer Preview에서 UWP에 대해 알려진 문제

이 항목에서는 Xbox Developer Preview에서 UWP에 대해 알려진 문제를 설명합니다. 이 개발자 미리 보기에 대한 자세한 내용은 [Xbox에서 UWP](index.md)를 참조하세요. 

\[API 참조 항목 링크를 통해 이 페이지를 방문하고 유니버설 디바이스 패밀리 API 정보를 찾는 경우 [Xbox에서 아직 지원되지 않는 UWP 기능](http://go.microsoft.com/fwlink/?LinkID=760755)을 참조하세요.\]

Xbox Developer Preview 시스템 업데이트에는 실험용 및 초기 시험판 소프트웨어가 포함되어 있습니다. 즉, 일부 인기 있는 게임 및 앱이 예상대로 작동하지 않고 가끔 크래시 및 데이터 손실을 경험할 수 있습니다. Developer Preview를 종료하면 콘솔이 초기화되며 모든 게임, 앱 및 콘텐츠를 다시 설치해야 합니다.

개발자의 경우 이는 일부 개발자 도구와 API가 예상대로 작동하지 않음을 의미합니다. 최종 릴리스용 기능 중 일부가 포함되지 않았거나 릴리스 품질이 아닙니다. 
**특히, 이 Preview의 시스템 성능은 최종 릴리스의 시스템 성능을 반영하지 않습니다.**

다음 목록에는 이 릴리스에서 발생할 수 있는 몇 가지 알려진 문제가 요약되어 있습니다. 

**피드백을 받고 싶으니**, [유니버설 Windows 앱 개발](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/home?forum=wpdevelop) 포럼에서 발견된 문제를 모두 신고해 주세요. 

문제가 있으면 이 항목의 내용을 살펴보고, [질문과 대답](frequently-asked-questions.md)을 확인하고, 포럼을 통해 도움을 요청하세요.


<!--## Developing games-->

## 마우스 모드 지원

이 미리 보기부터 _마우스 모드_는 XAML 및 호스트된 웹앱에 기본적으로 사용하도록 설정됩니다. 옵트아웃(opt out)하지 않은 모든 응용 프로그램은 Xbox Edge 브라우저에 있는 것과 유사한 마우스 포인터를 받게 됩니다.

**개발자는 마우스 모드를 끄고 컨트롤러(X-Y) 탐색에 최적화하는 것이 좋습니다.**

XAML에서 마우스 모드를 끄려면 다음 예제를 따릅니다.

```code
public App() {
    this.InitializeComponent();
    this.RequiresPointerMode = Windows.UI.Xaml.ApplicationRequiresPointerMode.WhenRequested;
    this.Suspending += OnSuspending;
}
```

HTML/JavaScript 앱에서 마우스 모드를 끄려면 다음 예제를 따릅니다.

```code
// Turn off mouse mode
navigator.gamepadInputEmulation = "keyboard";
```

HTML/JavaScript 앱에서 방향 탐색을 켜는 방법을 포함하여 자세한 내용은 [마우스 모드를 사용하지 않도록 설정하는 방법](how-to-disable-mouse-mode.md#html) 항목을 참조하세요.

> **참고**&nbsp;&nbsp;이 개발자 미리 보기에서 마우스 모드가 켜져 있는 경우 컨트롤러의 오른쪽 조이스틱을 사용하여 이동하면 콘솔이 응답하지 않을 수 있습니다. 이 문제가 발생하면 콘솔을 다시 부팅해야 합니다.

마우스 모드 지원에 대한 자세한 내용은 [Xbox 및 TV용 디자인](https://msdn.microsoft.com/windows/uwp/input-and-devices/designing-for-tv?f=255&MSPPError=-2147217396#mouse-mode) 항목을 참조하세요. 이 항목에는 마우스 모드를 사용하거나 사용하지 않도록 설정하는 방법에 대한 정보가 포함되어 있으므로 앱에 올바른 동작을 선택할 수 있습니다.

## 앱을 배포하려면 사용자가 로그인되어 있어야 함(오류 0x87e10008)

이제 앱이 시작되려면 먼저 사용자가 로그인되어 있어야 합니다(VS 2015에서 디버깅을 시작(F5)하려면 먼저 사용자가 로그인되어 있어야 함). Visual Studio에서 받은 현재 오류 메시지는 직관적이지 않습니다.
 
![Windows 스토어 앱을 활성화할 수 없습니다.](images/windows-store-app-activation-error.jpg)
 
이 문제를 해결하려면 앱을 배포하기 전에 Xbox 셸 또는 DevHome에서 사용자로 로그인합니다.
 
## 백그라운드 앱에 대한 메모리 제한이 아직 적용되지 않음
 
백그라운드에서 실행되는 앱에 대한 128MB 제한이 이 미리 보기에서 적용되지 않습니다. 즉, 앱이 백그라운드에서 실행될 때 128MB를 초과하는 경우 계속 메모리를 할당할 수 있습니다.
 
현재 이 문제에 대한 해결 방법은 없습니다. 메모리 사용을 적절하게 관리해야 합니다. 이후 미리 보기에서는 앱이 128MB 제한을 초과하는 경우 메모리 할당 오류가 발생합니다.
 
## 자녀 보호를 켠 상태로 VS에서 배포하지 못함

콘솔의 자녀 보호가 설정에서 켜져 있는 경우 VS에서 앱을 시작하지 못합니다.

이 문제를 해결하려면 자녀 보호를 일시적으로 사용하지 않도록 설정하거나 다음을 수행합니다.
1. 자녀 보호를 끄고 콘솔에 앱을 배포합니다.
2. 자녀 보호를 켭니다.
3. 콘솔에서 앱을 시작합니다.
4. 앱이 시작될 수 있도록 PIN 또는 암호를 입력합니다.
5. 앱이 시작됩니다.
6. 앱을 닫습니다.
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

## DirectX 12 지원

Xbox One의 UWP는 DirectX 11 기능 수준 10을 지원합니다. 지금은 DirectX 12가 지원되지 않습니다. 기존의 모든 게임 콘솔과 마찬가지로, Xbox One은 전체 잠재 기능에 액세스하기 위해 특정 SDK가 필요한 특수 하드웨어입니다. Xbox One 하드웨어의 최대 잠재 기능에 액세스해야 하는 게임을 개발하는 경우 DirectX 12 지원을 포함하는 해당 SDK에 액세스하기 위해 [ID@XBOX](http://www.xbox.com/Developers/id) 프로그램에 등록할 수 있습니다.

<!-- ### Xbox One Developer Preview disables game streaming to Windows 10

Activating the Xbox One Developer Preview on your console will prevent you from streaming games from your Xbox One to the Xbox app on Windows 10, even if your console is set to retail mode. 
To restore the game streaming feature, you must leave the developer preview. -->

## TV에 적합한 영역의 알려진 문제

기본적으로 Xbox에서 UWP 앱의 디스플레이 영역은 TV에 적합한 영역으로 음각 처리되어야 합니다. 그러나 Xbox One Developer Preview에는 알려진 버그가 있어 TV에 적합한 영역이 [_오프셋_, _오프셋_]이 아니라 [0, 0]에서 시작됩니다.

> **참고**&nbsp;&nbsp;이는 JavaScript를 사용하는 UWP 앱에만 적용됩니다.

이 문제를 해결하는 가장 쉬운 방법은 다음 JavaScript 예제와 같이 TV에 적합한 영역을 사용하지 않도록 설정하는 것입니다.

    var applicationView = Windows.UI.ViewManagement.ApplicationView.getForCurrentView();

    applicationView.setDesiredBoundsMode(Windows.UI.ViewManagement.ApplicationViewBoundsMode.useCoreWindow);

TV 안전 영역에 대한 자세한 내용은 [Xbox 및 TV용 디자인](https://msdn.microsoft.com/windows/uwp/input-and-devices/designing-for-tv)을 참조하세요.

<!--## System resources for UWP apps and games on Xbox One

UWP apps and games running on Xbox One share resources with the system and other apps, and so the system governs the resources that are available to any one game or app. 
If you are running into memory or performance issues, this may be why. 
For more details, see [System resources for UWP apps and games on Xbox One](system-resource-allocation.md).-->

<!--
## Networking using traditional sockets

In this developer preview, inbound and outbound network access from the console that uses traditional TCP/UDP sockets (WinSock, Windows.Networking.Sockets) is not available. 
Developers can still use HTTP and WebSockets.
--> 


## UWP API 검사

일부 UWP API는 Xbox에서 지원되지 않습니다. 작동하지 않는다고 나타나는 API 목록은 [Xbox에서 아직 지원되지 않는 UWP 기능](http://go.microsoft.com/fwlink/p/?LinkId=760755)을 참조하세요. 다른 API에서 문제가 발견된 경우 포럼에서 신고하세요. 

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


## WDP(Windows Device Portal) Preview

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

### Internet Explorer 7에서 WDP UI가 제대로 표시되지 않음 

Internet Explorer 7을 사용할 경우 기본적으로 WDP UI가 브라우저에서 제대로 표시되지 않습니다. WDP에 대해 Internet Explorer 7의 호환성 보기를 끄면 이 문제를 해결할 수 있습니다.

### WDP로 이동하면 인증서 경고가 표시됨

Xbox One 콘솔에서 서명한 보안 인증서는 신뢰할 수 있는 잘 알려진 게시자로 간주되지 않으므로 제공된 인증서에 대해 다음 스크린샷과 유사한 경고가 표시됩니다. "이 웹 사이트를 계속 탐색합니다"를 클릭하여 Windows Device Portal에 액세스합니다.

![웹 사이트 보안 인증서 경고](images/security_cert_warning.jpg)

<!--## Dev Home

Occasionally, selecting the “Manage Windows Device Portal” option in Dev Home will cause Dev Home to silently exit to the Home screen. 
This is caused by a failure in the WDP infrastructure on the console and can be resolved by restarting the console.-->

## 참고 항목
- [질문과 대답](frequently-asked-questions.md)
- [Xbox One의 UWP](index.md)



<!--HONumber=Jul16_HO2-->


