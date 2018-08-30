---
author: PatrickFarley
ms.assetid: 78D833B9-E528-4BCA-9C48-A757F17E6C22
title: Windows 앱 인증 키트
description: 앱은 Microsoft Store에 게시 될 가능성을 제공 하거나 유효성을 검사 하 고 인증을 위해 제출 하기 전에 로컬로 테스트 Windows 인증을 높이려면 합니다. 이 항목에서는 Windows 앱 인증 키트를 설치하고 실행하는 방법을 보여 줍니다.
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 앱 인증
ms.localizationpriority: medium
ms.openlocfilehash: b7a72a89704aa3768cc43cdfbb75b620bae303e3
ms.sourcegitcommit: 7efffcc715a4be26f0cf7f7e249653d8c356319b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/30/2018
ms.locfileid: "3123250"
---
# <a name="windows-app-certification-kit"></a>Windows 앱 인증 키트



[Windows 인증](https://msdn.microsoft.com/windows/desktop/jj134964.aspx) 앱 또는 [Microsoft Store에 게시](https://msdn.microsoft.com/library/windows/apps/Hh694062)하기 위해 준비 하면 해야 유효성을 검사 하 고 테스트 로컬로 먼저 합니다. 이 항목에서는 설치 하 고 안전 하 고 효율적으로 앱은 [Windows 앱 인증 키트](http://go.microsoft.com/fwlink/p/?LinkID=309666) 실행 하는 방법을 보여 줍니다.

## <a name="prerequisites"></a>필수 조건

유니버설 Windows 앱을 테스트하기 위한 필수 구성 요소:

-   Windows 10을 설치하고 실행해야 합니다.
-   Windows 10용 Windows SDK(소프트웨어 개발 키트)에 포함된 [Windows 앱 인증 키트 버전 10]( http://go.microsoft.com/fwlink/p/?LinkID=309666)을 설치해야 합니다.
-   [개발을 위해 디바이스를 사용](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development)하도록 설정해야 합니다.
-   테스트할 Windows 앱을 컴퓨터에 배포해야 합니다.

**현재 위치 업그레이드에 대한 참고 사항**

최신 [Windows 앱 인증 키트]( http://go.microsoft.com/fwlink/p/?LinkID=309666)를 설치하면 컴퓨터에 설치되어 있는 이전 버전의 키트가 대체됩니다.

## <a name="validate-your-windows-app-using-the-windows-app-certification-kit-interactively"></a>대화식으로 Windows 앱 인증 키트를 사용하여 Windows 앱의 유효성 검사

1.  **시작** 메뉴에서 **앱**을 검색하고 **Windows 키트**를 찾은 다음 **Windows 앱 인증 키트**를 클릭합니다.

2.  Windows 앱 인증 키트에서 수행할 유효성 검사 범주를 선택합니다. 예를 들어 Windows 앱의 유효성을 검사할 경우 **Windows 앱 유효성 검사**를 선택합니다.

    테스트할 앱을 직접 찾아보거나 UI의 목록에서 앱을 선택할 수 있습니다. Windows 앱 인증 키트가 처음으로 실행되면 컴퓨터에 설치된 모든 Windows 앱이 UI에 나열됩니다. 그 이후에 실행되면 UI에 유효성을 검사한 최근 Windows 앱이 표시됩니다. 테스트할 앱이 목록에 표시되지 않는 경우 **내 앱이 나열되지 않음**을 클릭하여 시스템에 설치된 모든 앱의 포괄적인 목록을 가져올 수 있습니다.

3.  테스트할 앱을 입력하거나 선택한 후 **다음**을 클릭합니다.

4.  다음 화면에서 테스트할 앱 유형에 맞게 정렬된 테스트 워크플로가 표시됩니다. 테스트가 목록에서 회색으로 표시되면 테스트가 환경에 적용되지 않습니다. 예를 들어 Windows 7에서 Windows 10 앱을 테스트할 경우 정적 테스트만 워크플로에 적용됩니다. 참고는 Microsoft Store에서이 워크플로의 모든 테스트를 적용할 수 있습니다. 실행할 테스트를 선택하고 **다음**을 클릭합니다.

    Windows 앱 인증 키트에서 앱 유효성 검사를 시작합니다.

5.  테스트 후의 프롬프트에 테스트 보고서를 저장할 폴더 경로를 입력합니다.

    Windows 앱 인증 키트가 XML 보고서와 함께 HTML을 만들고 이 폴더에 저장합니다.

6.  보고서 파일을 열고 테스트 결과를 검토합니다.

**참고** Visual Studio를 사용 중인 경우 앱 패키지를 만들 때 Windows 앱 인증 키트를 실행할 수 있습니다. 방법을 알아보려면 [UWP 앱 패키징](https://msdn.microsoft.com/library/windows/apps/Mt627715)을 참조하세요.

 

## <a name="validate-your-windows-app-using-the-windows-app-certification-kit-from-a-command-line"></a>명령줄에서 Windows 앱 인증 키트를 사용하여 Windows 앱의 유효성 검사

**중요** Windows 앱 인증 키트는 활성 사용자 세션에서 실행되어야 합니다.

1.  명령 창에서 Windows 앱 인증 키트가 들어 있는 디렉터리로 이동합니다.

    **참고** 기본 경로는 C:\\Program Files\\Windows Kits\\10\\App Certification Kit\\입니다.

2.  테스트 컴퓨터에 이미 설치한 앱을 테스트하려면 다음 명령을 순서대로 입력합니다.

    `appcert.exe reset`

    `appcert.exe test -packagefullname [package full name] -reportoutputpath [report file name]`

    또는 앱을 설치하지 않은 경우 다음 명령을 사용할 수 있습니다. Windows 앱 인증 키트는 패키지를 열고 적절한 테스트 워크플로를 적용합니다.

    `appcert.exe reset`

    `appcert.exe test -appxpackagepath [package path] -reportoutputpath [report file name]`

3.  테스트가 완료되면 `[report file name]`이라는 보고서 파일을 열고 테스트 결과를 검토합니다.

**참고** Windows 앱 인증 키트는 서비스에서 실행될 수 있지만, 서비스는 활성 사용자 세션에서 키트 프로세스를 시작해야 하며 Session0에서 실행될 수 없습니다.

**참고** Windows 앱 인증 키트 명령줄에 대한 자세한 내용을 보려면 다음 명령을 입력하세요. `appcert.exe /?`

## <a name="testing-with-a-low-power-computer"></a>절전 컴퓨터를 사용하여 테스트

Windows 앱 인증 키트의 성능 테스트 임계값은 절전 컴퓨터의 성능을 기반으로 합니다.

테스트를 수행하는 컴퓨터의 특성이 테스트 결과에 영향을 줄 수 있습니다. 앱의 성능에 [Microsoft Store 정책](https://msdn.microsoft.com/library/windows/apps/Dn764944)을 충족 하는 경우를 확인 하려면 Intel Atom 프로세서 기반 컴퓨터 화면 해상도 1366x768 (또는 그 이상) 및 회전을 사용 하 여 하드 등의 절전 컴퓨터에서 앱을 테스트 하는 권장 드라이브 (반도체 하드 드라이브) 반대입니다.

저성능 컴퓨터가 발전함에 따라 시간이 지나면 성능 특성이 변할 수도 있습니다. 앱 최신 성능 요구 사항을 준수 하는지 확인 Windows 앱 인증 키트의 최신 버전을 사용 하 여 앱 테스트 및 최신 [Microsoft Store 정책](https://msdn.microsoft.com/library/windows/apps/Dn764944) 참조 하세요.

## <a name="related-topics"></a>관련 항목

* [Windows 앱 인증 키트 테스트](windows-app-certification-kit-tests.md)
* [Microsoft Store 정책](https://msdn.microsoft.com/library/windows/apps/Dn764944)
 

 



