---
author: mcleblanc
description: 이 포팅 가이드를 끝까지 읽어 보시고 프로젝트를 빌드하여 실행하는 단계를 신속하게 진행해 보세요.
title: Windows 런타임 8.x를 UWP로 포팅하는 문제 해결&quot;
ms.assetid: 1882b477-bb5d-4f29-ba99-b61096f45e50
ms.author: markl
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: b30d26041718a74b9e2f3b9b93440e8fdf02b6c5
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.locfileid: "220267"
---
# <a name="troubleshooting-porting-windows-runtime-8x-to-uwp"></a>Windows 런타임 8.x를 UWP로 포팅하는 문제 해결

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

이전 항목에서는 [프로젝트 포팅](w8x-to-uwp-porting-to-a-uwp-project.md)에 대해 살펴봤습니다.

이 포팅 가이드를 끝까지 읽어 보시고 프로젝트를 빌드하여 실행하는 단계를 신속하게 진행해 보세요. 그렇게 하려면 필수가 아닌 코드에 주석이나 설명을 추가하고 나중에 돌아와서 해당 부채를 청산하여 일시적으로 진행할 수 있습니다. 이 항목의 문제 해결 증상 및 해결 방법 표는 다음 몇 개 항목을 대체하지는 않지만 이 단계에 유용한 정보를 제공합니다. 이후 항목을 진행하는 중에 언제든지 이 표를 다시 참조할 수 있습니다.

## <a name="tracking-down-issues"></a>문제 추적

예외 내에서 의미 있는 오류 메시지가 없는 경우에 특히 XAML 구문 분석 예외를 진단하는 것이 어려울 수 있습니다. 초기에 구문 분석 예외를 찾기 위해 첫째 예외를 catch하도록 디버거가 구성되어 있는지 확인하세요. 디버거에서 예외 변수를 검사하여 HRESULT 또는 메시지에 유용한 정보가 있는지 확인할 수 있습니다. XAML 파서로 오류 메시지 출력을 위한 Visual Studio 출력 창을 확인할 수도 있습니다.

앱이 종료되고 XAML 태그 구문 분석 중에 처리되지 않은 예외가 발생했다는 사실만 알고 있는 경우 누락된 리소스를 참조했기 때문일 수 있습니다. 즉, 유니버설 8.1 앱에 대한 키는 있지만 Windows 10 앱에 대한 키(예: 일부 시스템 **TextBlock** 스타일 키)는 없는 리소스를 참조했을 수 있습니다. 또는 **UserControl**, 사용자 지정 컨트롤 또는 사용자 지정 레이아웃 패널에서 예외가 발생했을 수 있습니다.

마지막으로 사용할 수 있는 방법은 이진 분할입니다. 페이지에서 태그의 약 1/2을 제거하고 앱을 다시 실행합니다. 그런 다음 오류가 제거된 부분에서 발생했는지 제거되지 *않은* 부분에서 발생했는지 여부를 확인합니다. 문제가 더 이상 발생하지 않을 때까지 오류가 포함된 부분을 계속해서 분할하면서 프로세스를 반복합니다.

## <a name="targetplatformversion"></a>TargetPlatformVersion

이 섹션에서는 Visual Studio에서 Windows 10 프로젝트를 열었을 때 "Visual Studio 업데이트 필요. 하나 이상의 프로젝트에 설치되지 않았거나 Visual Studio의 향후 업데이트의 일부로 포함되는 플랫폼 SDK <version>이(가) 필요합니다."라는 메시지가 표시될 경우 해야 할 일을 설명합니다.

-   먼저, 설치한 Windows 10용 SDK의 버전 번호를 확인합니다. **C:\\Program Files (x86)\\Windows Kits\\10\\Include\\<versionfoldername>** 로 이동하여 *<versionfoldername>* 을 적어두세요. 4중 표기인 "Major.Minor.Build.Revision"에 있습니다.
-   편집할 프로젝트 파일을 열고 `TargetPlatformVersion` 및 `TargetPlatformMinVersion` 요소를 찾습니다. 해당 요소를 다음과 같이 편집하고 *<versionfoldername>* 을 디스크에 있는 4중 표기 버전 번호로 바꿉니다.

```xml
   <TargetPlatformVersion><versionfoldername></TargetPlatformVersion>
    <TargetPlatformMinVersion><versionfoldername></TargetPlatformMinVersion>
```

## <a name="troubleshooting-symptoms-and-remedies"></a>문제 해결 증상 및 해결 방법

이 표의 해결 방법 정보는 차단 해제를 위한 충분한 정보를 제공하기 위한 것입니다. 뒤에 나오는 항목들을 참조하여 각 문제에 대한 세부 정보를 확인합니다.

| 증상 | 해결 방법 |
|---------|--------|
| Visual Studio에서 Windows 10 프로젝트를 열었을 때 "Visual Studio 업데이트 필요. 하나 이상의 프로젝트에 설치되지 않았거나 Visual Studio의 향후 업데이트의 일부로 포함되는 플랫폼 SDK &lt;버전&gt;이(가) 필요합니다."라는 메시지가 표시될 경우 해야 할 일을 설명합니다. | 이 항목의 [TargetPlatformVersion](#targetplatformversion) 섹션을 참조하세요. |
| xaml.cs 파일에서 InitializeComponent가 호출되었을 때 System.InvalidCastException이 발생했습니다.| 이 예외는 동일한 xaml.cs 파일을 공유하는 둘 이상의 xaml 파일(이 중 하나 이상이 MRT 정규화됨)이 있고 요소가 두 xaml 파일 간에 일관되지 않은 x:Name 특성을 갖고 있는 경우 발생할 수 있습니다. 두 xaml 파일의 동일한 요소에 동일한 이름을 추가하거나 이름을 완전히 생략하세요. |
| 앱이 종료되는 디바이스에서 실행했거나 Visual Studio에서 시작한 경우 “Windows 스토어 앱 \[…\]을(를) 활성화할 수 없습니다. 활성화 요청이 실패했습니다. 오류: ‘Windows가 대상 응용 프로그램과 통신할 수 없습니다. 이는 일반적으로 대상 응용 프로그램의 프로세스가 중단되었음을 나타냅니다. \[…\]”. | 초기화 중에 개발자 페이지 또는 바인딩된 속성 또는 다른 형식에서 실행 중인 명령적 코드에 문제가 있을 수 있습니다. 또는 앱이 종료될 때 표시하려는 XAML 파일을 구문 분석 하는 중에 발생할 수 있습니다. Visual Studio에서 시작할 경우에는 시작 페이지에서 발생할 수 있습니다. 잘못된 리소스 키를 찾아서 이 항목의 "문제 추적" 섹션의 몇 가지 지침을 참조하세요.|
| XAML 파서나 컴파일러 또는 런타임 예외에서 "*"<resourcekey>" 리소스를 확인할 수 없습니다.*"라는 오류가 발생합니다. | 리소스 키가 UWP(유니버설 Windows 플랫폼) 앱에 적용되지 않습니다(예: 일부 Windows Phone 리소스의 경우). 올바른 해당 리소스를 찾아서 태그를 업데이트합니다. 즉시 발생할 수 있는 예로는 `PhoneAccentBrush`과(와) 같은 시스템 키가 있습니다. |
| C# 컴파일러에서 "*\[...\]에 '<name>' 형식 또는 네임스페이스 이름이 없습니다.*" 또는 "*\[...\] 네임스페이스에 '<name>' 형식 또는 네임스페이스 이름이 없습니다.*" 또는 "*'&lt;<name>&gt;' 형식 또는 네임스페이스 이름이 현재 컨텍스트에 없습니다.*"라는 오류를 발생합니다. | 해결이 간단하지 않은 경우가 있을 수 있지만 확장 SDK에서 형식이 구현되었다는 의미일 수 있습니다. [Windows API](https://msdn.microsoft.com/library/windows/apps/bg124285) 참조 콘텐츠를 사용하여 API를 구현하는 확장 SDK를 확인한 다음 Visual Studio의 **추가** > **참조** 명령을 사용하여 해당 SDK에 대한 참조를 프로젝트에 추가합니다. 앱에서 범용 디바이스 패밀리라고 알려진 API 집합을 대상으로 하는 경우 [**ApiInformation**](https://msdn.microsoft.com/library/windows/apps/dn949001) 클래스를 호출하기 전에 확장 SDK가 있는지를 런타임으로 테스트하는 것이 중요합니다(적응 코드라고 함). 유니버설 API가 있으면 항상 확장 SDK의 API보다 더 선호됩니다. 자세한 내용은 [확장 SDK](w8x-to-uwp-porting-to-a-uwp-project.md)를 참조하세요. |

다음 항목은 [XAML 및 UI 포팅](w8x-to-uwp-porting-xaml-and-ui.md)입니다.
