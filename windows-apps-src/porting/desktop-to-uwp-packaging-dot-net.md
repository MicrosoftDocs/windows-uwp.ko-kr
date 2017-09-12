---
author: normesta
Description: "이 가이드에서는 Visual Studio 솔루션을 구성하여 변환된 데스크톱 브리지 패키지 데스크톱 앱을 편집, 디버그 및 패키징하는 방법을 설명합니다."
Search.Product: eADQiWindows 10XVcnh
title: "Visual Studio를 사용한 앱 패키징(데스크톱 브리지)"
ms.author: normesta
ms.date: 07/20/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 807a99a7-d285-46e7-af6a-7214da908907
ms.openlocfilehash: d8919448b965f18ff7f8fdaeda325889e495ef85
ms.sourcegitcommit: f6dd9568eafa10ee5cb2b849c0d82d84a1c5fb93
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/02/2017
---
# <a name="package-an-app-by-using-visual-studio-desktop-bridge"></a>Visual Studio를 사용한 앱 패키징(데스크톱 브리지)

Visual Studio를 사용하여 데스크톱 앱용 패키지를 만들 수 있습니다. 그런 다음 이 패키지를 Windows 스토어에 게시하거나, 하나 이상의 PC에서 테스트 로드 할 수 있습니다.

이 가이드에서는 솔루션을 설정한 후 데스크톱 응용 프로그램 패키지를 만드는 방법을 알아봅니다.

## <a name="first-consider-how-youll-distribute-your-app"></a>먼저, 앱을 배포할 방법을 고려합니다.

앱을 [Windows 스토어](https://www.microsoft.com/store/apps)에 게시하려는 경우에는 [이 양식](https://developer.microsoft.com/windows/projects/campaigns/desktop-bridge)을 작성하는 것부터 시작합니다. 그러면 온보딩 프로세스를 시작하라는 메시지가 표시됩니다. 이 과정의 일환으로, 스토어에서 이름을 예약하면 앱을 패키징 하기 위해 필요한 정보를 얻게 됩니다.

## <a name="add-a-packaging-project-to-your-solution"></a>솔루션에 패키징 프로젝트 추가

1. Visual Studio에서 데스크톱 응용 프로그램 프로젝트가 포함된 솔루션을 엽니다.

2. 솔루션에 JavaScript **빈 앱(유니버설 Windows)** 프로젝트를 추가합니다.

   여기에 코드를 추가할 필요가 없습니다. 패키지를 생성할 코드가 있습니다. 저희는 이 프로젝트를 '패키징 프로젝트'로 부를 것입니다.

   ![javascript UWP 프로젝트](images/desktop-to-uwp/javascript-uwp-project.png)

   >[!IMPORTANT]
   >일반적으로 이 프로젝트에 JavaScript 버전을 사용해야 합니다.  C#, VB.NET, C++ 버전은 몇 가지 문제가 있습니다. 이를 사용하고 싶다면 [알려진 문제](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-known-issues#known-issues-anchor) 가이드를 참조하십시오.

## <a name="add-the-desktop-application-binaries-to-the-packaging-project"></a>패키징 프로젝트에 데스크톱 응용 프로그램 바이너리 추가

패키징 프로젝트에 직접 바이너리를 추가합니다.

1. **솔루션 탐색기**에서 패키징 프로젝트 폴더를 확장하고, 하위 폴더를 생성하고, 원하는 이름을 지정합니다(예,**win32**).

2. 하위 폴더를 마우스 오른쪽 단추로 클릭한 후 **기존 항목 추가**를 선택합니다.

3. **기존 항목 추가** 대화 상자에서 데스크톱 응용 프로그램 출력 폴더 파일을 찾아 추가합니다. 이 폴더에는 실행 파일과 모든 dll 또는.config 파일이 들어있습니다.

   ![실행 파일 참조](images/desktop-to-uwp/cpp-exe-reference.png)

   데스크톱 응용 프로그램 프로젝트를 변경할 때마다, 파일의 새 버전을 패키징 프로젝트에 복사해야 합니다. 패키징 프로젝트의 프로젝트 파일에 사후 빌드 이벤트를 추가하면 이를 자동화 할 수 있습니다. 예를 들면 다음과 같습니다.

   ```XML
   <Target Name="PostBuildEvent">
     <Copy SourceFiles="..\MyWindowsFormsApplication\bin\Debug\MyWindowsFormsApplication.exe"
       DestinationFolder="win32" />
     <Copy SourceFiles="..\MyWindowsFormsApplication\bin\Debug\MyWindowsFormsApplication.exe.config"
       DestinationFolder="win32" />
     <Copy SourceFiles="..\MyWindowsFormsApplication\bin\Debug\MyWindowsFormsApplication.pdb"
       DestinationFolder="win32" />
     <Copy SourceFiles="..\MyWindowsFormsApplication\bin\Debug\MyBusinessLogicLibrary.dll"
       DestinationFolder="win32" />
     <Copy SourceFiles="..\MyWindowsFormsApplication\bin\Debug\MyBusinessLogicLibrary.pdb"
       DestinationFolder="win32" />
   </Target>
   ```

## <a name="modify-the-package-manifest"></a>패키지 매니페스트 수정

패키징 프로젝트에는 패키지 설정에 대해 설명하는 파일이 포함되어 있습니다. 기본적으로 이 파일은 UWP 앱을 설명합니다. 따라서 패키지에 완전 신뢰 상태에서 실행되는 데스크톱 응용 프로그램이 포함되어 있다는 점을 시스템이 이해하도록 수정을 해야 합니다.  

1. **솔루션 탐색기**에서 패키징 프로젝트를 확장하고, 마우스 오른쪽 단추로 **package.appxmanifest** 파일을 클릭한 다음 **코드 보기**를 선택합니다.

   ![dotnet 프로젝트 참조](images/desktop-to-uwp/reference-dotnet-project.png)

2. 파일 맨 위에 이 네임스페이스를 추가하고, 네임스페이스 접두사를 ``IgnorableNamespaces`` 목록에 추가합니다.

   ```XML
   xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
   ```
   완료되었을 때 네임스페이스 선언은 다음과 같이 표시됩니다.

   ```XML
   <Package
     xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
     xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
     xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
     xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
     IgnorableNamespaces="uap mp rescap">
   ```

3. ``TargetDeviceFamily`` 요소를 찾고, ``Name`` 속성을 **Windows.Desktop**, ``MinVersion`` 속성을 패키징 프로젝트의 최소 버전, ``MaxVersionTested``는 대상 패키징 프로젝트 버전으로 설정합니다.

   ```XML
   <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.10586.0" MaxVersionTested="10.0.15063.0" />
   ```

   최소 버전과 대상 버전은 패키징 프로젝트 속성 페이지에서 찾을 수 있습니다.

   ![최소 및 대상 버전 설정](images/desktop-to-uwp/min-target-version-settings.png)


4. ``Application`` 요소에서 ``StartPage`` 속성을 제거하세요. 그런 다음 ``Executable`` 및 ``EntryPoint`` 속성을 추가합니다.

   ``Application`` 요소는 이렇게 표시됩니다.

   ```XML
   <Application Id="App"  Executable=" " EntryPoint=" ">
   ```

5. ``Executable`` 속성을 데스크톱 응용 프로그램 실행 파일 이름으로 설정합니다. 그런 다음 ``EntryPoint`` 속성을 **Windows.FullTrustApplication**로 설정합니다.

   ``Application`` 요소는 이렇게 표시됩니다.

   ```XML
   <Application Id="App"  Executable="win32\MyWindowsFormsApplication.exe" EntryPoint="Windows.FullTrustApplication">
   ```
6. ``runFullTrust`` 기능을 ``Capabilities`` 요소에 추가합니다.

   ```XML
     <rescap:Capability Name="runFullTrust"/>
   ```
   이 선언 아래 푸른 물결 표시가 나타나지만, 무시해도 안전합니다.

   >[!IMPORTANT]
   C++ 데스크톱 응용 프로그램용 패키지를 만드는 경우, 앱과 함께 Visual C++ 런타임이 배포될 수 있도록 매니페스트 파일을 추가 변경해야 합니다. [데스크톱 브리지 프로젝트에 Visual C++ 사용하기](https://blogs.msdn.microsoft.com/vcblog/2016/07/07/using-visual-c-runtime-in-centennial-project/)를 참조합니다.

7. 오류가 표시되지 않도록 패키징 프로젝트를 빌드합니다.

8. 패키지 테스트는 [패키지로 만든 데스크톱 앱 실행, 디버그, 테스트(데스크톱 브리지)](desktop-to-uwp-debug.md)를 참조합니다.

   그런 다음 이 가이드로 돌아와, 패키지 생성을 위한 다음 섹션을 참조합니다.

## <a name="generate-a-package"></a>패키지 생성

앱 패키지를 생성하려면 이 주제를 설명한 가이드([UWP 앱 패키징](..\packaging\packaging-uwp-apps.md))을 참조합니다.

**패키지 선택 및 구성** 화면이 표시되면, 패키지에 포함시킬 바이너리 종류를 잠시 생각한 후 확인란을 선택합니다.

* C#, C++, VB.NET 기반 유니버설 Windows 플랫폼 프로젝트를 솔루션에 추가해 데스크톱 응용 프로그램을 [확장](desktop-to-uwp-extend.md)했다면, **x86** 및 **x64** 확인란을 선택합니다.  

* 그렇지 않으면 **Neutral** 확인란을 선택합니다.

>[!NOTE]
각 지원 플랫폼을 선택해야 하는 이유는 확장한 솔루션에 두 종류의 바이너리가 포함되어 있기 때문입니다. 하나는 UWP 프로젝트용, 다른 하나는 데스크톱 프로젝트용입니다. 이들은 종류가 다른 바이너리이기 때문에, .NET 네이티브가 플랫폼별로 기본 바이너리를 생성해야 합니다.

패키지를 생성하려 시도할 때 오류가 발생한 경우, [알려진 문제](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-known-issues#known-issues-anchor) 가이드를 참조하세요. 해당 문제가 목록에 없는 경우, [여기 링크](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)를 통해 저희에게 문제를 알려주세요.

## <a name="next-steps"></a>다음 단계

**앱 실행 / 문제 찾기 및 해결**

 [패키지 데스크톱 앱 실행, 디버그, 테스트(데스크톱 브리지)](desktop-to-uwp-debug.md)를 참조하세요.

**UWP API를 추가해 데스크톱 앱 향상**

[Windows 10용 데스크톱 응용 프로그램 향상](desktop-to-uwp-enhance.md) 참조

**UWP 구성 요소를 추가해 데스크톱 앱 확장**

[최신 UWP 구성 요소로 데스크톱 응용 프로그램 확장](desktop-to-uwp-extend.md) 참조.

**앱 배포**

[패키지 데스크톱 앱 배포(데스크톱 브리지)](desktop-to-uwp-distribute.md)를 참조하세요.

**특정 질문에 대한 답변 찾기**

저희 팀은 이러한 [StackOverflow 태그](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)를 모니터링합니다.

**이 문서에 대한 피드백 제공**

아래의 설명 섹션을 사용합니다.
