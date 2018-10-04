---
author: andrewleader
Description: Discover the different options desktop Win32 apps have for sending toast notifications
title: 데스크톱 앱에서 알림 메시지
label: Toast notifications from desktop apps
template: detail.hbs
ms.author: mijacobs
ms.date: 05/01/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, win32, 데스크톱, 알림, 데스크톱 브리지, 알림을 보내는 옵션, com 서버, com 활성자, com, 가짜 com, com 없음, com 없이, 알림 보내기
ms.localizationpriority: medium
ms.openlocfilehash: 9f54519fd0ddc975c1e57c2aebde583ef971850d
ms.sourcegitcommit: 5c9a47b135c5f587214675e39c1ac058c0380f4c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/04/2018
ms.locfileid: "4353820"
---
# <a name="toast-notifications-from-desktop-apps"></a>데스크톱 앱에서 알림 메시지

데스크톱 앱(데스크톱 브리지 및 클래식 Win32)은 UWP(유니버설 Windows 플랫폼) 앱과 마찬가지로 대화형 알림 메시지를 보낼 수 있습니다. 그러나 다양한 활성화 스키마로 인해 데스크톱 앱에 대해 몇 가지 다른 옵션이 있습니다.

이 문서에서는 Windows 10에서 알림 메시지 보내기에 대한 옵션을 나열합니다. 모든 옵션을 완벽하게 지원...

* 알림 센터에서 유지
* 팝업 및 알림 센터 내 모두에서 활성화할 수 있음
* EXE가 실행되지 않는 동안 활성화할 수 있음

## <a name="all-options"></a>모든 옵션

아래 표에서 데스크톱 앱 내에서 알림을 지원하는 옵션과 그 옵션에 지원되는 기능을 보여 줍니다. 표를 사용하여 자신의 시나리오에서 가장 좋은 옵션을 선택할 수 있습니다.<br/><br/>

| 옵션 | 화면 효과 | 작업 | 입력 | 프로세스 내 활성화 |
| -- | -- | -- | -- | -- |
| [COM 활성자](#preferred-option---com-activator) | ✔️ | ✔️ | ✔️ | ✔️ |
| [COM 없음 / 스텁 CLSID](#alternative-option---no-com--stub-clsid) | ✔️ | ✔️ | ❌ | ❌ |


## <a name="preferred-option---com-activator"></a>기본 옵션 - COM 활성자

데스크톱 브리지 및 클래식 Win32 모두에서 작동하고 모든 알림 기능을 지원하는 기본 옵션입니다. "COM 활성자"를 두려워 하지 마십시오. COM 서버를 작성해 본 적이 없더라도 이를 매우 단순하게 만드는 [C#](send-local-toast-desktop.md) 및 [C++ 앱](send-local-toast-desktop-cpp-wrl.md)용 라이브러리가 있습니다.<br/><br/>

| 화면 효과 | 작업 | 입력 | 프로세스 내 활성화 |
| -- | -- | -- | -- |
| ✔️ | ✔️ | ✔️ | ✔️ |

COM 활성자 옵션을 사용하면 앱에서 다음 알림 템플릿 및 활성화 유형을 사용할 수 있습니다.<br/><br/>

| 템플릿 및 활성화 유형 | 데스크톱 브리지 | 클래식 Win32 |
| -- | -- | -- |
| ToastGeneric 포그라운드 | ✔️ | ✔️ |
| ToastGeneric 백그라운드 | ✔️ | ✔️ |
| ToastGeneric 프로토콜 | ✔️ | ✔️ |
| 레거시 템플릿 | ✔️ | ❌ |

> [!NOTE]
> 기존 데스크톱 브리지 앱에 COM 활성자를 추가하면 포그라운드/백그라운드 및 레거시 알림이 사용자 명령줄 대신 COM 활성자를 활성화합니다.

이 옵션을 사용하는 방법을 알아보려면 [데스크톱 C # 앱에서 로컬 알림 메시지 보내기](send-local-toast-desktop.md) 또는 [데스크톱 C++ WRL 앱에서 로컬 알림 메시지 보내기](send-local-toast-desktop-cpp-wrl.md)를 참조하세요.


## <a name="alternative-option---no-com--stub-clsid"></a>다른 옵션 - COM 없음 / 스텁 CLSID

COM 활성자를 구현할 수 없는 경우 대체 옵션입니다. 그러나 입력 지원(알림 시 텍스트 상자) 및 프로세스 내 활성화 등의 몇 가지 기능이 저하됩니다.<br/><br/>

| 화면 효과 | 작업 | 입력 | 프로세스 내 활성화 |
| -- | -- | -- | -- |
| ✔️ | ✔️ | ❌ | ❌ |

이 옵션을 사용하면 클래식 Win32를 지원하는 경우 사용할 수 있는 알림 템플릿 및 활성화 유형은 아래와 같이 훨씬 더 제한적입니다.<br/><br/>

| 템플릿 및 활성화 유형 | 데스크톱 브리지 | 클래식 Win32 |
| -- | -- | -- |
| ToastGeneric 포그라운드 | ✔️ | ❌ |
| ToastGeneric 백그라운드 | ✔️ | ❌ |
| ToastGeneric 프로토콜 | ✔️ | ✔️ |
| 레거시 템플릿 | ✔️ | ❌ |

나중에 이 옵션을 사용하는 방법을 보여 주는 문서를 게시할 예정입니다. 기본적으로, 데스크톱 브리지 앱의 경우 UWP 앱과 같이 알림 메시지를 전송합니다. 사용자가 알림을 클릭하면 앱은 알림에서 사용자가 지정한 시작 인수로 명령줄에 의해 시작됩니다.

클래식 Win32 앱의 경우 알림을 전송할 수 있도록 AUMID를 설정한 다음 바로 가기에 CLSID도 지정합니다. 임의의 GUID를 사용할 수 있습니다. COM 서버/활성자를 추가하지 마십시오. 알림 센터에 알림을 유지할 수 있는 "스텁" COM CLSID를 추가합니다. 스텁 CLSID가 모든 다른 알림 활성화를 끊기 때문에 프로토콜 활성화 알림만 사용할 수 있습니다. 따라서 앱이 프로토콜 활성화를 지원하도록 업데이트하고 알림 프로토콜이 앱을 활성화하도록 해야 합니다.


## <a name="resources"></a>리소스

* [데스크톱 C # 앱에서 로컬 알림 메시지 보내기](send-local-toast-desktop.md)
* [데스크톱 C++ WRL 앱에서 로컬 알림 메시지 보내기](send-local-toast-desktop-cpp-wrl.md)
* [알림 콘텐츠 설명서](adaptive-interactive-toasts.md)