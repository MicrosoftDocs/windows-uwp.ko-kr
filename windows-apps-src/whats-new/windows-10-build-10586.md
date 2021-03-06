---
title: Windows 10 빌드 10586 - 2015년 11월의 새로운 기능
description: Windows 10 빌드 10586 및 새로운 개발자 도구는 새로운 유니버설 Windows 플랫폼을 기반으로 하는 도구, 기능 및 환경을 제공합니다.
keywords: 기능, Windows 10, 1511, 10586
ms.date: 11/02/2017
ms.topic: article
ms.assetid: 0d6c65c5-2ad5-46c7-964e-a3a9833c94ce
ms.localizationpriority: medium
ms.openlocfilehash: 34db0115e25cb9d54ac44e3703d23edf56276264
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172797"
---
# <a name="whats-new-in-windows-10-for-developers-build-10586"></a>개발자용 Windows 10 빌드 10586의 새로운 기능

Visual Studio 2019 및 업데이트된 SDK와 함께 Windows 10 빌드 10586(11월 업데이트 또는 버전 1511이라고도 함)은 놀라운 유니버설 Windows 플랫폼 앱을 만드는 도구, 기능 및 환경을 제공합니다. Windows 10에 [도구 및 SDK를 설치](https://developer.microsoft.com/windows/downloads#_blank)하면 [새로운 유니버설 Windows 앱을 생성](../get-started/create-uwp-apps.md)하거나 [Windows의 기존 앱 코드](../porting/index.md)를 사용하는 방법을 살펴볼 수 있습니다.

## <a name="windows-10-build-10586---november-2015"></a>Windows 10 빌드 10586 - 2015년 11월

기능 | 설명
 :---- | :----
 사용자 환경 | 새 [Windows.UI.StartScreen.JumpList](/uwp/api/windows.ui.startscreen) 및 [Windows.UI.StartScreen.JumpListItem](/uwp/api/windows.ui.startscreen) 클래스는 앱에 사용하려는 시스템 관리된 점프 목록 유형을 프로그래밍 방식으로 선택하는 기능, 점프 목록에 사용자 지정 작업 진입점을 추가하는 기능 및 점프 목록에 사용자 지정 그룹을 추가하는 기능을 제공합니다.
 입력 | [키보드 전달 인터셉터](/uwp/api/windows.ui.input.keyboarddeliveryinterceptor) SAS(Secure Attention Sequence) 키 조합을 제외하고 앱에서 바로 가기 키, 선택키(또는 핫 키), 액셀러레이터 키, 애플리케이션 키 등 원시 키보드 입력의 시스템 처리를 재정의할 수 있습니다. Ctrl-Alt-Del, Windows-L 등의 SAS(Secure Attention Sequence) 키 조합은 계속 시스템에서 처리합니다. <br /><br />[UWP 앱](/uwp/api/windows.ui.core.corewindow) 및 [클래식 Windows 앱](/previous-versions/windows/desktop/inputmsg/messages) 둘 다에 대해 포인터 입력의 크로스 프로세스 연결 입력의 크로스 프로세스 연결을 가능하게 하는 새 포인터 이벤트입니다. <br /><br />[클래식 데스크톱 앱에 대한 Ink Presenter](/previous-versions/windows/desktop/input_ink/ink-presenter) Ink Presenter API는 Microsoft Win32 앱이 앱의 [DirectComposition](/windows/desktop/directcomp/directcomposition-portal) 시각적 트리에 삽입된 [InkPresenter](/uwp/api/Windows.UI.Input.Inking.InkPresenter) 개체를 통해 잉크 입력(표준 및 수정됨)의 입력, 처리 및 렌더링을 관리할 수 있도록 합니다.
네트워킹 | WebSockets 사용자의 경우: [MessageWebSocket.OutputStream.FlushAsync](/uwp/api/windows.storage.streams.datawriter.flushasync) 및 [StreamWebSocket.OutputStream.FlushAsync](/uwp/api/windows.storage.streams.datawriter.flushasync)가 완전히 구현되었으며, 이전에 실행한 WriteAsync 호출이 완료될 때까지 기다립니다. [FlushAsync](/uwp/api/windows.storage.streams.datawriter.flushasync)를 호출할 때 WebSocket이 잘못된 상태인 경우 기존 코드에서 예외를 일으킬 수 있습니다. <br /><br />새 속성 [CookieUsageBehavior](/uwp/api/windows.web.http.filters.httpbaseprotocolfilter)가 기존 [Windows.Web.Http.Filters.HttpBaseProtocolFilter 클래스](/uwp/api/windows.web.http.filters.httpbaseprotocolfilter)에 추가되었습니다. 이를 통해 개발자가 시스템에서 쿠키를 처리하는 방식을 제어할 수 있습니다.
ORTC | Microsoft Edge가 이제 네이티브 Javascript API를 통해 브라우저, 모바일 디바이스 및 서버 간에 웹에서 음성/화상 통화를 직접 실시간으로 가능하도록 하는 [ORTC(개체 실시간 통신)](/previous-versions//mt433097(v=vs.85))를 구현합니다. 개발자는 이제 ORTC API를 사용하여 Microsoft Edge 브라우저에 고급 실시간 오디오/비디오 통신 애플리케이션을 빌드할 수 있으며, 그룹 화상 통화, 동시 방송, SVC(스케일러블 비디오 코딩) 등에 대한 지원을 받습니다. Microsoft Edge 브라우저 간 ORTC API를 통한 1:1 음성/화상 통화 데모는 [시험 사용 사이트 및 데모](https://developer.microsoft.com/microsoft-edge/testdrive/demos/ortcdemo)에서 확인할 수 있습니다.
Microsoft Edge F12 개발자 도구 | Microsoft Edge에는 [UserVoice](https://wpdev.uservoice.com/forums/257854-microsoft-edge-developer)에서 가장 많이 요청한 기능을 비롯하여 F12 개발자 도구에 대한 새로운 개선 기능이 도입되었습니다. 완료되기 전에 강력한 새 기능을 사용해 볼 수 있는 DOM 탐색기, 콘솔, 디버거, 네트워크, 성능, 메모리, 에뮬레이션의 새로운 기능 및 새로운 시험 도구를 살펴보세요. 새 도구는 TypeScript로 작성되었으며 항상 실행되므로 다시 로드할 필요가 없습니다. 또한 F12 개발자 도구 설명서가 이제 [Microsoft Edge 개발 사이트](https://developer.microsoft.com/microsoft-edge)의 일부이며 [GitHub](https://github.com/MicrosoftEdge/MicrosoftEdge-Documentation)에서 모두 제공됩니다. 이제부터 사용자의 피드백에 따라 설명서를 작성할 뿐만 아니라 사용자가 설명서 작성에 기여하고 도움을 줄 수 있습니다. F12 개발자 도구에 대한 간략한 소개 동영상을 보려면 [Channel9의 One Dev Minute](https://channel9.msdn.com/Blogs/One-Dev-Minute/Microsoft-Edge-F12-tools)를 방문하세요.
Windows Hello | Windows Hello에서는 얼굴 또는 지문 인식을 사용하여 Windows 시스템 또는 디바이스에 로그온할 수 있는 기능을 앱에 제공합니다. 공급자 API를 사용하면 IHV 및 OEM이 컴퓨터 비전용 깊이, 적외선 및 컬러 카메라(및 관련 메타데이터)를 UWP에 표시하고 Windows Hello 안면 인증에 사용할 카메라를 지정할 수 있습니다. [Windows.Devices.Perception](/uwp/api/windows.devices.perception) 네임스페이스에는 UWP 응용 프로그램에서 컴퓨터 비전 카메라의 컬러, 깊이 또는 적외선 데이터에 액세스할 수 있도록 클라이언트 API가 포함됩니다.
새로운 게임 API | 게임 바가 표시되거나 해제될 때 새 Windows.Gaming.UI.GameBar 클래스를 사용하여 알림을 받을 수 있습니다.
Bluetooth API | Bluetooth LE, 디바이스 열거 및 Bluetooth의 기타 기능에 대한 지원을 확장하도록 여러 API가 추가되고 업데이트되었습니다. [Windows.Devices.Bluetooth](/uwp/api/windows.devices.bluetooth) 네임스페이스를 참조하세요.
스마트 카드 API | 보안 암호 지급 프로토콜을 지원하기 위해 여러 SmartCardCryptogram API가 [Windows.Devices.SmartCards](/uwp/api/windows.devices.smartcards) 네임스페이스에 추가되었습니다. 탭하여 결제하는 기능을 지원하기 위해 호스트 카드 에뮬레이션을 사용하는 지급 앱이 추가 보안 및 성능에 대해 이러한 API를 사용할 수 있습니다. 앱은 TPM을 사용하여 키를 생성하고 제한된 용도의 트랜잭션 키를 보호할 수 있습니다. 또한 앱은 NGC(차세대 자격 증명) 프레임워크를 사용하여 사용자의 PIN과 함께 키를 보호할 수 있습니다. 이러한 API는 향상된 성능을 위해 시스템에 암호 생성을 위임합니다. 또한 다른 앱에서 키 및 암호에 액세스하는 것을 막습니다.
업데이트된 스토리지 API | [Windows.Storage.DownloadsFolder 클래스](/uwp/api/windows.storage.downloadsfolder)에서 이제 앱은 특정 [사용자](/uwp/api/windows.system.user)에 대해 다운로드 폴더 내에 [파일을 생성](/uwp/api/windows.storage.downloadsfolder.createfileforuserasync)하거나 [폴더를 생성](/uwp/api/windows.storage.downloadsfolder.createfolderforuserasync)할 수 있습니다. [Windows.Storage.StorageLibrary 클래스](/uwp/api/windows.storage.storagelibrary)에서 이제 앱은 특정 [사용자](/uwp/api/windows.system.user)에 대해 [지정된 라이브러리를 가져올 수 있습니다](/uwp/api/windows.storage.storagelibrary.getlibraryforuserasync).
Windows 앱 인증 키트 | Windows 앱 인증 키트가 향상된 테스트로 업데이트되었습니다. 전체 업데이트 목록은 [Windows 앱 인증 키트](https://developer.microsoft.com/windows/develop/app-certification-kit) 페이지에서 확인할 수 있습니다.
디자인 다운로드 | Adobe Photoshop용 새 UWP 앱 디자인 템플릿을 확인하세요. Microsoft PowerPoint 및 Adobe Illustrator 템플릿도 업데이트되었으며 PDF 버전의 지침이 제공됩니다. [디자인 다운로드 페이지를 방문하세요](../design/downloads/index.md).