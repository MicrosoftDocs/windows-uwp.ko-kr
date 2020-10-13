---
Description: 바탕 화면에서 알림 메시지를 보내기 위해 사용할 수 있는 다양 한 옵션 검색
title: Win32 앱의 알림 메시지
label: Toast notifications from Win32 apps
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp, win32, 데스크톱, 알림 메시지, 데스크톱 브리지, msix, 스파스 패키지, 알림을 전송 옵션, com 서버, com 활성기, com, 가짜 com, com 없음, com 없음, 알림 전송
ms.localizationpriority: medium
ms.openlocfilehash: 6f02bc7c615643ba0d2ca0ed1b43ecf13641c1c5
ms.sourcegitcommit: 140bbbab0f863a7a1febee85f736b0412bff1ae7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/13/2020
ms.locfileid: "91984549"
---
# <a name="toast-notifications-from-win32-apps"></a>Win32 앱의 알림 메시지

Win32 앱 (패키지 된 [Msix](/windows/msix/desktop/source-code-overview) 앱, 패키지 id를 가져오기 위해 [스파스 패키지](/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps) 를 사용 하는 앱 및 클래식 패키지 되지 않은 Win32 앱 포함)은 Windows 앱과 마찬가지로 대화형 알림 메시지를 보낼 수 있습니다. 그러나 다른 활성화 스키마로 인해 Win32 앱에 대 한 몇 가지 옵션이 있습니다.

이 문서에서는 Windows 10에서 알림 메시지를 보내기 위한 옵션을 나열 합니다. 모든 옵션은 다음을 완벽 하 게 지원 합니다.

* 작업 센터에 유지
* 팝업 및 내부 작업 센터 모두에서 활성화
* EXE가 실행 되 고 있지 않을 때 활성화 됩니다.

## <a name="all-options"></a>모든 옵션

다음 표에서는 Win32 앱 내에서 알림을를 지 원하는 옵션과 해당 하는 지원 되는 기능을 보여 줍니다. 표를 사용 하 여 시나리오에 가장 적합 한 옵션을 선택할 수 있습니다.<br/><br/>

| 옵션 | 시각적 개체 | 동작 | 입력 | In-process 활성화 |
| -- | -- | -- | -- | -- |
| [COM 활성기](#preferred-option---com-activator) | ✔️ | ✔️ | ✔️ | ✔️ |
| [COM/스텁 CLSID 없음](#alternative-option---no-com--stub-clsid) | ✔️ | ✔️ | ❌ | ❌ |


## <a name="preferred-option---com-activator"></a>기본 설정 옵션-COM 활성기

이 옵션은 Win32 앱에 사용할 수 있는 기본 옵션으로, 모든 알림 기능을 지원 합니다. "COM 활성기"에는 아무런 걱정 하지 마십시오. 이전에 COM 서버를 작성 하지 않은 경우에도이를 매우 간단 하 게 만드는 c # 및 [c + + 응용 프로그램](send-local-toast-desktop-cpp-wrl.md) [에 대 한](send-local-toast-desktop.md) 라이브러리가 있습니다.<br/><br/>

| 시각적 개체 | 동작 | 입력 | In-process 활성화 |
| -- | -- | -- | -- |
| ✔️ | ✔️ | ✔️ | ✔️ |

COM 활성기 옵션을 사용 하면 앱에서 다음과 같은 알림 템플릿 및 활성화 유형을 사용할 수 있습니다.<br/><br/>

| 템플릿 및 활성화 형식 | MSIX/sparse 패키지 | 클래식 Win32 |
| -- | -- | -- |
| To Generic 전경 | ✔️ | ✔️ |
| To Generic 배경 | ✔️ | ✔️ |
| To Generic 프로토콜 | ✔️ | ✔️ |
| 레거시 템플릿 | ✔️ | ❌ |

> [!NOTE]
> 기존 MSIX/sparse 패키지 앱에 COM 활성기를 추가 하는 경우 포그라운드/Background 및 레거시 알림 활성화는 이제 명령줄 대신 COM 활성기를 활성화 합니다.

이 옵션을 사용 하는 방법에 대 한 자세한 내용은 [win32 c # 앱에서 로컬 알림 메시지 보내기](send-local-toast-desktop.md) 또는 [Win32 c + + WRL apps에서 로컬](send-local-toast-desktop-cpp-wrl.md)알림 메시지 보내기를 참조 하세요.


## <a name="alternative-option---no-com--stub-clsid"></a>대체 옵션-COM/스텁 CLSID 없음

이는 COM 활성기를 구현할 수 없는 경우의 대체 옵션입니다. 그러나 입력 지원 (알림을의 텍스트 상자) 및 in-process 활성화와 같은 몇 가지 기능을 더 이상 사용할 수 없습니다.<br/><br/>

| 시각적 개체 | 동작 | 입력 | In-process 활성화 |
| -- | -- | -- | -- |
| ✔️ | ✔️ | ❌ | ❌ |

이 옵션을 사용 하는 경우 클래식 Win32를 지 원하는 경우 아래와 같이 사용할 수 있는 알림 템플릿 및 활성화 유형을 훨씬 더 제한적으로 사용할 수 있습니다.<br/><br/>

| 템플릿 및 활성화 형식 | MSIX/sparse 패키지 | 클래식 Win32 |
| -- | -- | -- |
| To Generic 전경 | ✔️ | ❌ |
| To Generic 배경 | ✔️ | ❌ |
| To Generic 프로토콜 | ✔️ | ✔️ |
| 레거시 템플릿 | ✔️ | ❌ |

패키지 된 [Msix](/windows/msix/desktop/source-code-overview) 앱 및 [스파스 패키지](/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps)를 사용 하는 앱의 경우 UWP 앱 처럼 알림 메시지를 보냅니다. 사용자가 알림을 클릭 하면 앱이 알림에서 지정한 시작 인수를 사용 하 여 시작 하는 명령줄이 됩니다.

클래식 Win32 앱의 경우 AUMID를 설정 하 여 알림을를 보낸 다음 바로 가기에서 CLSID를 지정 하면 됩니다. 임의의 GUID 일 수 있습니다. COM 서버/활성기를 추가 하지 마세요. "스텁" COM CLSID를 추가 하 여 동작 센터에서 알림을 유지 하 게 됩니다. 스텁 CLSID는 다른 알림 활성화의 활성화를 중단 하므로 프로토콜 활성화 알림을만 사용할 수 있습니다. 따라서 프로토콜 활성화를 지원 하도록 앱을 업데이트 하 고 알림을 프로토콜이 자신의 앱을 활성화 하도록 해야 합니다.


## <a name="resources"></a>리소스

* [Win32 c # 앱에서 로컬 알림 메시지 보내기](send-local-toast-desktop.md)
* [Win32 c + + WRL apps에서 로컬 알림 메시지 보내기](send-local-toast-desktop-cpp-wrl.md)
* [알림 콘텐츠 설명서](adaptive-interactive-toasts.md)
