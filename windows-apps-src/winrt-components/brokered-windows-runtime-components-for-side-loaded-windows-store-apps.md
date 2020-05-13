---
title: 테스트용으로 로드 된 UWP 앱에 대해 조정 된 Windows 런타임 구성 요소
description: 이 문서에서는 Windows 10에서 지원 되는 엔터프라이즈 대상 기능에 대해 설명 합니다 .이 기능을 사용 하면 터치 친화적인 .NET 앱이 중요 한 비즈니스에 중요 한 작업을 담당 하는 기존 코드를 사용할 수 있습니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 81b3930c-6af9-406d-9d1e-8ee6a13ec38a
ms.localizationpriority: medium
ms.openlocfilehash: a3e95eae10fb06135f0fed1b92f1717f5e5fdf4d
ms.sourcegitcommit: 0f2ae8f97daac440c8e86dc07d11d356de29515c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/13/2020
ms.locfileid: "83280283"
---
# <a name="brokered-windows-runtime-components-for-a-side-loaded-uwp-app"></a>테스트용으로 로드 된 UWP 앱에 대해 조정 된 Windows 런타임 구성 요소

이 문서에서는 Windows 10에서 지원 되는 엔터프라이즈 대상 기능에 대해 설명 합니다 .이 기능을 사용 하면 터치 친화적인 .NET 앱에서 중요 한 비즈니스에 중요 한 작업을 담당 하는 기존 코드를 사용할 수 있습니다.

## <a name="introduction"></a>소개

>**참조**  이 문서와 함께제공 되는 샘플 코드는  [VisualStudio2015&2017](https://github.com/Microsoft/Brokered-WinRT-Components)에서다운로드할수있습니다. 조정 된 Windows 런타임 구성 요소를 빌드하기 위한 Microsoft Visual Studio 템플릿은 [windows 10 용 유니버설 Windows 앱을 대상으로 하는 Visual Studio 2015 템플릿](https://marketplace.visualstudio.com/items?itemName=vs-publisher-713547.VS2015TemplateBrokeredComponents) 에서 다운로드할 수 있습니다.

Windows에는 *테스트용으로 로드 된 응용 프로그램을 위해 조정 된 Windows 런타임 구성 요소*라는 새로운 기능이 포함 되어 있습니다. Microsoft는 하나의 프로세스에서 기존 데스크톱 소프트웨어 자산(데스크톱 구성 요소)을 실행하는 한편, UWP 앱에서 이 코드를 조작하는 기능을 설명하는 데 IPC(프로세스 간 통신)라는 용어를 사용합니다. 데이터베이스 애플리케이션과 Windows에서 NT 서비스를 활용하는 애플리케이션이 유사한 다중 프로세스 아키텍처를 공유하므로, 이것은 기업 개발자에게 친숙한 모델입니다.

앱의 테스트용 로드는 이 기능을 구성하는 중요한 요소입니다.
기업용 애플리케이션은 일반 소비자를 대상으로 하는 Microsoft Store에 포함되지 않으며 기업은 보안, 개인 정보, 배포, 설정 및 서비스와 관련하여 매우 구체적인 요구 사항을 가지고 있습니다. 따라서, 테스트용 로드 모델은 이 기능을 사용하는 사용자의 요구 사항인 동시에 중요한 구현 정보입니다.

데이터 위주의 앱은 이 애플리케이션 아키텍처의 주요 대상입니다. 예를 들어 SQL Server 등에 확립되어 있는 기존 비즈니스 규칙이 데스크톱 구성 요소의 공통 부분이 될 것으로 보입니다. 이 기능은 데스크톱 구성 요소에서 제공할 수 있는 유일한 종류의 기능이 아니며, 이 기능에 대한 수요의 대부분은 기존 데이터 및 비즈니스 논리와 관련이 있습니다.

마지막으로, 엔터프라이즈 개발의 .NET 런타임 및 C 언어의 과도 한 침투를 감안 \# 하 여이 기능은 UWP 앱과 데스크톱 구성 요소 쪽 모두에 .net을 사용 하는 것에 중점을 두어 개발 되었습니다. UWP 앱에서 사용할 수 있는 다른 언어 및 런타임이 있지만 함께 제공 되는 샘플은 C만을 보여주고 \# .net 런타임으로만 제한 됩니다.

## <a name="application-components"></a>응용 프로그램 구성 요소

>**참고**   이 기능은 .NET의 용도로만 사용 됩니다. 클라이언트 앱과 데스크톱 구성 요소는 둘 다 .NET을 사용해서 작성해야 합니다.

**애플리케이션 모델**

이 기능은 MVVM(모델 뷰 뷰-모델)이라는 일반적인 애플리케이션 아키텍처를 기반으로 구축되었습니다. 따라서 "모델"이 데스크톱 구성 요소에 완전히 하우징되어 있는 것으로 간주되며 데스크톱 구성 요소가 "비입력 시스템"(예제: UI가 포함되지 않음)이라는 것을 즉시 명확히 알 수 있습니다. 뷰는 테스트용으로 로드하는 엔터프라이즈 애플리케이션에 완전히 포함됩니다. "뷰-모델" 구조를 사용하여 이 애플리케이션을 구축해야 한다는 요구 사항은 없지만, Microsoft는 이 패턴이 일반적으로 사용되기를 기대합니다.

**데스크톱 구성 요소**

이 기능의 데스크톱 구성 요소는 이 기능의 일부로 도입되는 새로운 애플리케이션 형식입니다. 이 데스크톱 구성 요소는 C로만 작성할 수 \# 있으며 Windows 10의 경우 .net 4.6 이상을 대상으로 해야 합니다. 해당 프로젝트는 프로세스 간 통신 형식이 UWP 형식과 클래스로 구성되는 반면 데스크톱 구성 요소는 .NET 런타임 클래스 라이브러리의 모든 부분을 호출할 수 있으므로 UWP를 대상으로 하는 CLR 간의 하이브리드 형식입니다. Visual Studio 프로젝트에 미치는 영향에 대해서는 나중에 자세히 설명하겠습니다. 이 하이브리드 구성 덕분에 데스크톱 구성 요소를 기반으로 구축된 애플리케이션 간에 UWP 형식을 마샬링할 수 있을 뿐만 아니라 데스크톱 구성 요소 구현 내부에서 데스크톱 CLR 코드를 호출할 수도 있습니다.

**계약**

테스트용으로 로드하는 애플리케이션과 데스크톱 구성 요소 간의 계약은 UWP 형식 시스템의 사용 조건에 설명되어 있습니다. 여기 \# 에는 UWP를 나타낼 수 있는 하나 이상의 C 클래스를 선언 하는 작업이 포함 됩니다. C를 사용 하 여 Windows 런타임 클래스를 만들기 위한 특정 요구 사항은 MSDN 항목 [c \# 및 Visual Basic에서 Windows 런타임 구성 요소 만들기](https://docs.microsoft.com/previous-versions/windows/apps/br230301(v=vs.140)) 를 참조 하세요 \# .

>**참고**   이 시점에서 데스크톱 구성 요소와 테스트용으로 로드 된 응용 프로그램 간의 Windows 런타임 구성 요소 계약에서 열거형은 지원 되지 않습니다.

**테스트용으로 로드 된 응용 프로그램**

테스트용으로 로드하는 애플리케이션은 Microsoft Store를 통해 설치되는 대신 테스트용으로 로드된다는 한 가지 측면을 제외한 모든 측면에서 일반적인 UWP 앱입니다. 대부분의 설치 메커니즘은 동일하며, 매니페스트 및 애플리케이션 패키징이 비슷합니다(매니페스트에 추가된 한 가지 사항에 대해서는 나중에 자세히 설명). 테스트용 로드를 사용하도록 설정하면 간단한 PowerShell 스크립트를 통해 필요한 인증서와 애플리케이션 자체를 설치할 수 있습니다. 테스트용으로 로드하는 애플리케이션이 Visual Studio의 프로젝트/스토어 메뉴에 포함된 WACK 인증 테스트를 통과하는 것이 일반적인 모범 사례입니다.

>**참고** 테스트용 로드는 설정- &gt; 업데이트 & 보안-개발자 용으로 설정할 수 있습니다 &gt; .

한 가지 중요한 점은 Windows 10의 일부로 함께 제공되는 앱 브로커 메커니즘은 32비트에만 해당된다는 것입니다. 데스크톱 구성 요소는 32비트여야 합니다.
테스트용으로 로드하는 애플리케이션이 64비트일 수 있지만(64비트 및 32비트 프록시가 모두 등록되어 있는 경우), 이것은 일반적이지 않습니다. 일반적인 "중립" 구성을 사용 하 여 C에 테스트용으로 로드 된 응용 프로그램을 빌드하고 \# "32 비트를 선호" 하는 기본은 본질적으로 32 비트 테스트용으로 로드 된 응용 프로그램을 만드는 것입니다.

**서버 인스턴스 및 Appdomain**

테스트용으로 로드하는 각각의 애플리케이션은 고유한 앱 브로커 서버 인스턴스를 수신합니다("다중 인스턴싱"이라고 함). 서버 코드는 단일 AppDomain 내에서 실행됩니다. 이에 따라, 여러 버전의 라이브러리를 개별 인스턴스에서 실행할 수 있습니다. 예를 들어 A 애플리케이션에는 V1.1의 구성 요소가 필요하고 B 애플리케이션에는 V2가 필요한 경우, 개별 서버 디렉터리에서 V1.1 및 V2 구성 요소를 사용하고 애플리케이션이 올바른 버전을 지원하는 서버를 가리키게 하면 명확히 분리될 수 있습니다.

여러 애플리케이션이 동일한 서버 디렉터리를 가리키게 함으로써 여러 앱 브로커 서버 인스턴스 간에 서버 코드 구현을 공유할 수 있습니다. 앱 브로커 서버 인스턴스가 여러 개 있지만 동일한 코드를 실행합니다. 단일 애플리케이션에서 사용되는 모든 구현 구성 요소는 동일한 경로에 있어야 합니다.

## <a name="defining-the-contract"></a>계약 정의

이 기능을 사용하여 애플리케이션을 만드는 첫 단계는 테스트용으로 로드하는 애플리케이션과 데스크톱 구성 요소 간에 계약을 만드는 것입니다. 이 작업은 Windows 런타임 형식을 독점적으로 사용하여 수행해야 합니다.
다행히 C 클래스를 사용 하 여 쉽게 선언할 수 있습니다 \# . 하지만 이 대화를 정의할 때 중요한 성능 고려 사항이 있으며 이후 섹션에서 설명합니다.

계약을 정의하는 순서는 다음과 같이 소개됩니다.

**1 단계:** Visual Studio에서 새 클래스 라이브러리를 만듭니다. **Windows 런타임 구성 요소** 템플릿이 아닌 **클래스 라이브러리** 템플릿을 사용 하 여 프로젝트를 만들어야 합니다.

당연히 구현이 따르지만, 이 섹션에서는 프로세스 간 계약의 정의에 대해서만 다룹니다. 함께 제공되는 샘플에는 다음과 같은 클래스(EnterpriseServer.cs)가 포함되며, 시작 부분 모양은 다음과 같습니다.

```csharp
namespace Fabrikam
{
    public sealed class EnterpriseServer
    {

        public ILis<String> TestMethod(String input)
        {
            throw new NotImplementedException();
        }
        
        public IAsyncOperation<int> FindElementAsync(int input)
        {
            throw new NotImplementedException();
        }
        
        public string[] RetrieveData()
        {
            throw new NotImplementedException();
        }
        
        public event EventHandler<string> PeriodicEvent;
    }
}
```

이를 통해 테스트용으로 로드하는 애플리케이션에서 인스턴스화할 수 있는 "EnterpriseServer" 클래스가 정의됩니다. 이 클래스는 RuntimeClass에서 약속된 기능을 제공합니다. RuntimeClass를 사용하여 테스트용으로 로드하는 애플리케이션에 포함될 참조 winmd를 생성할 수 있습니다.

**2 단계:** 프로젝트 파일을 수동으로 편집 하 여 프로젝트의 출력 형식을 **Windows 런타임 구성 요소로**변경 합니다.

Visual Studio에서 이 작업을 수행하려면 새로 만든 프로젝트를 마우스 오른쪽 단추로 클릭하고 "프로젝트 언로드"를 선택한 다음 마우스 오른쪽 단추를 다시 클릭하고 "EnterpriseServer.csproj 편집"을 선택하여 편집을 위해 프로젝트 파일인 XML 파일을 엽니다.

열려 있는 파일에서 OutputType 태그를 검색 \< 하 \> 고 해당 값을 "winmdobj"로 변경 합니다.

**3 단계:** "참조" Windows 메타 데이터 파일 (winmd 파일)을 만드는 빌드 규칙을 만듭니다. 즉 구현이 없는 빌드 규칙을 만듭니다.

**4 단계:** "구현" Windows 메타 데이터 파일을 만드는 빌드 규칙을 만듭니다. 즉, 동일한 메타 데이터 정보를 갖지만 구현도 포함 합니다.

이 작업은 다음 스크립트에 의해 수행됩니다. 빌드 후 이벤트 명령줄의 프로젝트 **속성**  >  **빌드 이벤트**에 스크립트를 추가 합니다.

> **참고** 스크립트는 대상 Windows 버전(Windows 10) 및 사용 중인 Visual Studio 버전에 따라 다릅니다.

**Visual Studio 2015**
```cmd
    call "$(DevEnvDir)..\..\vc\vcvarsall.bat" x86 10.0.14393.0

    md "$(TargetDir)"\impl    md "$(TargetDir)"\reference

    erase "$(TargetDir)\impl\*.winmd"
    erase "$(TargetDir)\impl\*.pdb"
    rem erase "$(TargetDir)\reference\*.winmd"

    xcopy /y "$(TargetPath)" "$(TargetDir)impl"
    xcopy /y "$(TargetDir)*.pdb" "$(TargetDir)impl"

    winmdidl /nosystemdeclares /metadata_dir:C:\Windows\System32\Winmetadata "$(TargetPath)"

    midl /metadata_dir "%WindowsSdkDir%UnionMetadata" /iid "$(SolutionDir)BrokeredProxyStub\$(TargetName)_i.c" /env win32 /x86 /h   "$(SolutionDir)BrokeredProxyStub\$(TargetName).h" /winmd "$(TargetName).winmd" /W1 /char signed /nologo /winrt /dlldata "$(SolutionDir)BrokeredProxyStub\dlldata.c" /proxy "$(SolutionDir)BrokeredProxyStub\$(TargetName)_p.c"  "$(TargetName).idl"
    mdmerge -n 1 -i "$(ProjectDir)bin\$(ConfigurationName)" -o "$(TargetDir)reference" -metadata_dir "%WindowsSdkDir%UnionMetadata" -partial

    rem erase "$(TargetPath)"

```


**Visual Studio 2017**
```cmd
    call "$(DevEnvDir)..\..\vc\auxiliary\build\vcvarsall.bat" x86 10.0.16299.0

    md "$(TargetDir)"\impl
    md "$(TargetDir)"\reference

    erase "$(TargetDir)\impl\*.winmd"
    erase "$(TargetDir)\impl\*.pdb"
    rem erase "$(TargetDir)\reference\*.winmd"

    xcopy /y "$(TargetPath)" "$(TargetDir)impl"
    xcopy /y "$(TargetDir)*.pdb" "$(TargetDir)impl"

    winmdidl /nosystemdeclares /metadata_dir:C:\Windows\System32\Winmetadata "$(TargetPath)"

    midl /metadata_dir "%WindowsSdkDir%UnionMetadata" /iid "$(SolutionDir)BrokeredProxyStub\$(TargetName)_i.c" /env win32 /x86 /h "$(SolutionDir)BrokeredProxyStub\$(TargetName).h" /winmd "$(TargetName).winmd" /W1 /char signed /nologo /winrt /dlldata "$(SolutionDir)BrokeredProxyStub\dlldata.c" /proxy "$(SolutionDir)BrokeredProxyStub\$(TargetName)_p.c"  "$(TargetName).idl"
    mdmerge -n 1 -i "$(ProjectDir)bin\$(ConfigurationName)" -o "$(TargetDir)reference" -metadata_dir "%WindowsSdkDir%UnionMetadata" -partial

    rem erase "$(TargetPath)"
```

프로젝트 Target 폴더 아래의 “reference” 폴더에 참조 **winmd**가 만들어지면 이 참조를 사용하는 테스트용으로 로드하는 각 응용 프로그램 프로젝트에 직접 옮겨져(복사) 참조됩니다. 이 내용은 다음 섹션에서 자세히 설명하겠습니다. 위의 빌드 규칙에 포함된 프로젝트 구조에서는 구현 및 참조 **winmd**가 빌드 계층 구조에서 명확하게 분리된 디렉터리에 위치하여 혼동을 피할 수 있습니다.

## <a name="side-loaded-applications-in-detail"></a>테스트용으로 로드하는 애플리케이션 세부 정보
앞에서 설명했듯이, 테스트용으로 로드하는 애플리케이션은 다른 UWP 앱처럼 빌드됩니다. 하지만 한 가지 추가 사항이 있습니다. 즉, 테스트용으로 로드하는 애플리케이션의 매니페스트에서 RuntimeClass의 가용성을 선언합니다. 그러면 애플리케이션이 간단하게 새 값을 작성하여 데스크톱 구성 요소의 해당 기능에 액세스할 수 있습니다. <Extension> 섹션의 새 매니페스트 항목은 데스크톱 구성 요소에서 구현된 RuntimeClass 및 해당 위치에 대한 정보를 설명합니다. 애플리케이션 매니페스트의 이러한 선언 콘텐츠는 Windows 10을 대상으로 하는 앱에서도 동일합니다. 예:

```XML
<Extension Category="windows.activatableClass.inProcessServer">
    <InProcessServer>
        <Path>clrhost.dll</Path>
        <ActivatableClass ActivatableClassId="Fabrikam.EnterpriseServer" ThreadingModel="both">
            <ActivatableClassAttribute Name="DesktopApplicationPath" Type="string" Value="c:\test" />
        </ActivatableClass>
    </InProcessServer>
</Extension>
```

이 애플리케이션 구성에 적용되지 않는 outOfProcessServer 범주에 항목이 여러 개 있기 때문에, 범주는 inProcessServer입니다. <Path>구성 요소는 항상 clrhost .dll을 포함 해야 합니다. 그러나이는 적용 **되지** 않으며 다른 값 지정은 정의 되지 않은 방식으로 실패 합니다.

<ActivatableClass> 섹션은 앱 패키지의 Windows 런타임 구성 요소에서 기본으로 사용되는 실제 In-Process RuntimeClass와 동일합니다. <ActivatableClassAttribute>는 새 요소 이며 Name = "DesktopApplicationPath" 및 Type = "string"은 필수 및 고정입니다. Value 특성은 데스크톱 구성 요소의 구현 winmd가 상주하는 위치를 가리킵니다(자세한 내용은 다음 섹션 참고). 데스크톱 구성 요소에서 기본으로 사용하는 각 RuntimeClass에는 고유한 <ActivatableClass> 요소 트리가 있어야 합니다. ActivatableClassId는 RuntimeClass의 네임스페이스로 정규화된 이름과 일치해야 합니다.

"계약 정의" 섹션의 설명대로, 데스크톱 구성 요소의 참조 winmd를 프로젝트에서 참조해야 합니다. Visual Studio 프로젝트 시스템은 일반적으로 이름이 같은 2개 수준의 디렉터리 구조를 만듭니다. 이 샘플에서는 EnterpriseIPCApplication \\ enterpriseipcapplication입니다. 참조 **winmd** 는이 두 번째 수준 디렉터리에 수동으로 복사 된 후 프로젝트 참조 대화 상자가 사용 됩니다 **. (찾아보기** 를 클릭 합니다. 단추)를 클릭 하 여이 **winmd**를 찾고 참조 합니다. 그런 다음 데스크톱 구성 요소의 최상위 네임 스페이스 (예: Fabrikam)는 프로젝트의 참조 부분에서 최상위 노드로 표시 됩니다.

>**참고** 테스트용으로 로드 된 응용 프로그램의 **참조** 를 사용 하는 것이 매우 중요 합니다. 의도 하지 않은 인스턴스를 테스트용으로 로드 된 응용 프로그램 **디렉터리에 전달 하 여 참조** 하면 "IStringable를 찾을 수 없습니다."와 관련 된 오류가 발생할 수 있습니다. 이는 잘못 된 **winmd** 가 참조 되었음을 나타내는 것입니다. IPC 서버 앱의 빌드 후 규칙 (다음 섹션에 자세히 설명)은 이러한 두 개의 **winmd** 를 별도의 디렉터리로 신중 하 게 분리 합니다.

환경 변수 (특히% ProgramFiles%) 에서 사용할 수 있습니다 <ActivatableClassAttribute Value="path"> . 앞에서 설명한 것 처럼 App Broker는 32 비트만 지원 하므로 \\ 응용 프로그램이 64 비트 OS에서 실행 되는 경우% ProgramFiles%는 C: Program Files (x86)로 확인 됩니다.

## <a name="desktop-ipc-server-detail"></a>데스크톱 IPC 서버 세부 정보

이전 두 단원에서는 클래스의 선언 및 파생 로드 된 응용 프로그램 프로젝트에 참조 **winmd** 를 전송 하는 메커니즘을 설명 합니다. 데스크톱 구성 요소에서 남은 작업의 상당 부분이 구현과 관련됩니다. 데스크톱 구성 요소에서 중요한 점은 데스크톱 코드를 호출할 수 있는(일반적으로 기존 코드 자산을 재활용하기 위해) 기능이므로, 프로젝트를 특별한 방식으로 구성해야 합니다.
일반적으로 .NET을 사용하는 Visual Studio 프로젝트는 두 "프로필" 중 하나를 사용합니다.
하나는 데스크톱(".NetFramework")용이고, 하나는 CLR(".NetCore")의 UWP 앱 부분을 대상으로 합니다. 이 기능의 데스크톱 구성 요소는 이 두 프로필 사이의 하이브리드입니다. 결과적으로, 이 두 프로필을 혼합하도록 참조 섹션이 세심하게 구성됩니다.

일반적인 UWP 앱 프로젝트에는 명시적인 프로젝트 참조가 포함되지 않습니다. Windows 런타임 API 서피스 전체가 암시적으로 포함되어 있기 때문입니다.
일반적으로 다른 프로젝트 간 참조만 만들어집니다. 하지만 데스크톱 구성 요소 프로젝트에는 매우 특별한 참조 집합이 있습니다. "클래식 데스크톱 \\ 클래스 라이브러리" 프로젝트로 수명이 시작 되므로 데스크톱 프로젝트가 됩니다. 따라서 **winmd** 파일에 대 한 참조를 통해 Windows 런타임 API에 대 한 명시적 참조를 만들어야 합니다. 아래와 같이 적절한 참조를 추가합니다.

```XML
<ItemGroup>
    <!-- These reference are added by VS automatically when you create a Class Library project-->
    <Reference Include="System" />
    <Reference Include="System.Core" />
    <Reference Include="System.Xml.Linq" />
    <Reference Include="System.Data.DataSetExtensions" />
    <Reference Include="Microsoft.CSharp" />
    <Reference Include="System.Data" />
    <Reference Include="System.Net.Http" />
<Reference Include="System.Xml" />
    <!-- These reference should be added manually by editing .csproj file-->

    <Reference Include="System.Runtime.WindowsRuntime, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089, processorArchitecture=MSIL">
      <HintPath>$(MSBuildProgramFiles32)\Microsoft SDKs\NETCoreSDK\System.Runtime.WindowsRuntime\4.0.10\lib\netcore50\System.Runtime.WindowsRuntime.dll</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows">
      <HintPath>$(MSBuildProgramFiles32)\Windows Kits\10\UnionMetadata\Facade\Windows.WinMD</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Foundation.FoundationContract">
      <HintPath>$(MSBuildProgramFiles32)\Windows Kits\10\References\Windows.Foundation.FoundationContract\1.0.0.0\Windows.Foundation.FoundationContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Foundation.UniversalApiContract">
      <HintPath>$(MSBuildProgramFiles32)\Windows Kits\10\References\Windows.Foundation.UniversalApiContract\1.0.0.0\Windows.Foundation.UniversalApiContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Networking.Connectivity.WwanContract">
      <HintPath>$(MSBuildProgramFiles32)\Windows Kits\10\References\Windows.Networking.Connectivity.WwanContract\1.0.0.0\Windows.Networking.Connectivity.WwanContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Activation.ActivatedEventsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Activation.ActivatedEventsContract\1.0.0.0\Windows.ApplicationModel.Activation.ActivatedEventsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Activation.ActivationCameraSettingsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Activation.ActivationCameraSettingsContract\1.0.0.0\Windows.ApplicationModel.Activation.ActivationCameraSettingsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Activation.ContactActivatedEventsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Activation.ContactActivatedEventsContract\1.0.0.0\Windows.ApplicationModel.Activation.ContactActivatedEventsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Activation.WebUISearchActivatedEventsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Activation.WebUISearchActivatedEventsContract\1.0.0.0\Windows.ApplicationModel.Activation.WebUISearchActivatedEventsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Background.BackgroundAlarmApplicationContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Background.BackgroundAlarmApplicationContract\1.0.0.0\Windows.ApplicationModel.Background.BackgroundAlarmApplicationContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Calls.LockScreenCallContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Calls.LockScreenCallContract\1.0.0.0\Windows.ApplicationModel.Calls.LockScreenCallContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Resources.Management.ResourceIndexerContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Resources.Management.ResourceIndexerContract\1.0.0.0\Windows.ApplicationModel.Resources.Management.ResourceIndexerContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Search.Core.SearchCoreContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Search.Core.SearchCoreContract\1.0.0.0\Windows.ApplicationModel.Search.Core.SearchCoreContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Search.SearchContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Search.SearchContract\1.0.0.0\Windows.ApplicationModel.Search.SearchContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.ApplicationModel.Wallet.WalletContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.ApplicationModel.Wallet.WalletContract\1.0.0.0\Windows.ApplicationModel.Wallet.WalletContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Devices.Custom.CustomDeviceContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Devices.Custom.CustomDeviceContract\1.0.0.0\Windows.Devices.Custom.CustomDeviceContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Devices.Portable.PortableDeviceContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Devices.Portable.PortableDeviceContract\1.0.0.0\Windows.Devices.Portable.PortableDeviceContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Devices.Printers.Extensions.ExtensionsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Devices.Printers.Extensions.ExtensionsContract\1.0.0.0\Windows.Devices.Printers.Extensions.ExtensionsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Devices.Printers.PrintersContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Devices.Printers.PrintersContract\1.0.0.0\Windows.Devices.Printers.PrintersContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Devices.Scanners.ScannerDeviceContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Devices.Scanners.ScannerDeviceContract\1.0.0.0\Windows.Devices.Scanners.ScannerDeviceContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Devices.Sms.LegacySmsApiContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Devices.Sms.LegacySmsApiContract\1.0.0.0\Windows.Devices.Sms.LegacySmsApiContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Gaming.Preview.GamesEnumerationContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Gaming.Preview.GamesEnumerationContract\1.0.0.0\Windows.Gaming.Preview.GamesEnumerationContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Globalization.GlobalizationJapanesePhoneticAnalyzerContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Globalization.GlobalizationJapanesePhoneticAnalyzerContract\1.0.0.0\Windows.Globalization.GlobalizationJapanesePhoneticAnalyzerContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Graphics.Printing3D.Printing3DContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Graphics.Printing3D.Printing3DContract\1.0.0.0\Windows.Graphics.Printing3D.Printing3DContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Management.Deployment.Preview.DeploymentPreviewContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Management.Deployment.Preview.DeploymentPreviewContract\1.0.0.0\Windows.Management.Deployment.Preview.DeploymentPreviewContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Management.Workplace.WorkplaceSettingsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Management.Workplace.WorkplaceSettingsContract\1.0.0.0\Windows.Management.Workplace.WorkplaceSettingsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Media.Capture.AppCaptureContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Media.Capture.AppCaptureContract\1.0.0.0\Windows.Media.Capture.AppCaptureContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Media.Capture.CameraCaptureUIContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Media.Capture.CameraCaptureUIContract\1.0.0.0\Windows.Media.Capture.CameraCaptureUIContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Media.Devices.CallControlContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Media.Devices.CallControlContract\1.0.0.0\Windows.Media.Devices.CallControlContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Media.MediaControlContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Media.MediaControlContract\1.0.0.0\Windows.Media.MediaControlContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Media.Playlists.PlaylistsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Media.Playlists.PlaylistsContract\1.0.0.0\Windows.Media.Playlists.PlaylistsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Media.Protection.ProtectionRenewalContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Media.Protection.ProtectionRenewalContract\1.0.0.0\Windows.Media.Protection.ProtectionRenewalContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Networking.NetworkOperators.LegacyNetworkOperatorsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Networking.NetworkOperators.LegacyNetworkOperatorsContract\1.0.0.0\Windows.Networking.NetworkOperators.LegacyNetworkOperatorsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Networking.Sockets.ControlChannelTriggerContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Networking.Sockets.ControlChannelTriggerContract\1.0.0.0\Windows.Networking.Sockets.ControlChannelTriggerContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Security.EnterpriseData.EnterpriseDataContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Security.EnterpriseData.EnterpriseDataContract\1.0.0.0\Windows.Security.EnterpriseData.EnterpriseDataContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Security.ExchangeActiveSyncProvisioning.EasContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Security.ExchangeActiveSyncProvisioning.EasContract\1.0.0.0\Windows.Security.ExchangeActiveSyncProvisioning.EasContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Services.Maps.GuidanceContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Services.Maps.GuidanceContract\1.0.0.0\Windows.Services.Maps.GuidanceContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Services.Maps.LocalSearchContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Services.Maps.LocalSearchContract\1.0.0.0\Windows.Services.Maps.LocalSearchContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.System.Profile.SystemManufacturers.SystemManufacturersContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.System.Profile.SystemManufacturers.SystemManufacturersContract\1.0.0.0\Windows.System.Profile.SystemManufacturers.SystemManufacturersContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.System.Profile.ProfileHardwareTokenContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.System.Profile.ProfileHardwareTokenContract\1.0.0.0\Windows.System.Profile.ProfileHardwareTokenContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.System.Profile.ProfileRetailInfoContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.System.Profile.ProfileRetailInfoContract\1.0.0.0\Windows.System.Profile.ProfileRetailInfoContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.System.UserProfile.UserProfileContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.System.UserProfile.UserProfileContract\1.0.0.0\Windows.System.UserProfile.UserProfileContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.System.UserProfile.UserProfileLockScreenContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.System.UserProfile.UserProfileLockScreenContract\1.0.0.0\Windows.System.UserProfile.UserProfileLockScreenContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.UI.ApplicationSettings.ApplicationsSettingsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.UI.ApplicationSettings.ApplicationsSettingsContract\1.0.0.0\Windows.UI.ApplicationSettings.ApplicationsSettingsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.UI.Core.AnimationMetrics.AnimationMetricsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.UI.Core.AnimationMetrics.AnimationMetricsContract\1.0.0.0\Windows.UI.Core.AnimationMetrics.AnimationMetricsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.UI.Core.CoreWindowDialogsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.UI.Core.CoreWindowDialogsContract\1.0.0.0\Windows.UI.Core.CoreWindowDialogsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.UI.Xaml.Hosting.HostingContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.UI.Xaml.Hosting.HostingContract\1.0.0.0\Windows.UI.Xaml.Hosting.HostingContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="Windows.Web.Http.Diagnostics.HttpDiagnosticsContract">
      <HintPath>$(MsBuildProgramFiles32)\Windows Kits\10\References\Windows.Web.Http.Diagnostics.HttpDiagnosticsContract\1.0.0.0\Windows.Web.Http.Diagnostics.HttpDiagnosticsContract.winmd</HintPath>
      <Private>False</Private>
    </Reference>
</ItemGroup>
```

위의 참조는 이 하이브리드 서버의 정상적인 작동에 중요한 참조가 세심하게 혼합된 것입니다. 프로토콜은 프로젝트 OutputType을 편집하는 방법에서 설명한 대로 .csproj 파일을 열고 필요에 따라 이러한 참조를 추가하는 것입니다.

참조가 제대로 구성되면 다음 작업은 서버의 기능을 구현하는 것입니다.  [Windows 런타임 구성 요소와의 상호 운용성에 대 한 모범 사례 (C \# /Vb/c + + 및 XAML을 사용 하는 UWP 앱)](https://docs.microsoft.com/previous-versions/windows/apps/hh750311(v=win.10))항목을 참조 하세요.
구현의 일환으로 데스크톱 코드를 호출할 수 있는 Windows 런타임 구성 요소 dll을 만드는 작업입니다. 함께 제공되는 샘플에는 Windows 런타임에서 사용되는 다음과 같은 주요 패턴이 있습니다.

-   메서드 호출

-   데스크톱 구성 요소의 Windows 런타임 이벤트 원본

-   Windows 런타임 비동기 작업

-   기본 형식의 배열 반환

**설치**

앱을 설치 하려면 연결 된 테스트용 로드 된 응용 프로그램의 매니페스트에 지정 된 올바른 디렉터리 (' path ')에 구현 **winmd** 를 복사 합니다 <ActivatableClassAttribute> . 관련된 지원 파일 및 프록시/스텁 dll도 복사합니다(이에 대한 자세한 내용은 아래에서 설명). 구현 **winmd** 를 서버 디렉터리 위치로 복사 하지 못하면 RuntimeClass에서 모든 테스트용으로 로드 된 응용 프로그램의 호출이 "클래스 등록 되지 않음" 오류를 throw 합니다. 프록시/스텁을 설치하지 않거나 등록하지 않으면 모든 호출이 실패하고 값이 반환되지 않습니다. 이 두 번째 오류는 표시 되는 예외와 연결 **되지 않는** 경우가 많습니다.
이 구성 오류로 인해 예외가 관찰되는 경우, 이는 "잘못된 캐스팅"과 관련 있을 수 있습니다.

**서버 구현 고려 사항**

데스크톱 Windows 런타임 서버는 "작업자" 또는 "작업"을 기준으로 한다고 생각할 수 있습니다. 모든 서버 호출은 비 UI 스레드에서 작동하며, 모든 코드는 다중 스레드를 인식하고 안전해야 합니다. 테스트용으로 로드하는 애플리케이션 중 서버의 기능을 호출하는 것이 어느 부분인지도 중요합니다. 테스트용으로 로드하는 애플리케이션의 UI 스레드에서 장기 실행 코드를 호출하지 않는 것이 항상 중요합니다. 이 작업을 수행하는 두 가지 주요 방법이 있습니다.

1.  UI 스레드에서 서버 기능을 호출하는 경우 항상 서버 공개 노출 영역 및 구현에서 비동기 패턴을 사용합니다.

2.  테스트용으로 로드하는 애플리케이션의 백그라운드 스레드에서 서버의 기능을 호출합니다.

**서버에서 async를 Windows 런타임 합니다.**

애플리케이션 모델의 크로스 프로세스 특성에 의해, 서버 호출은 In-Process를 독점 실행하는 코드에 비해 오버헤드가 더 많습니다. 메모리 내 값을 반환하는 단순 속성은 실행 속도가 빨라서 UI 스레드 차단이 문제가 되지 않기 때문에 일반적으로 이 속성을 호출해도 좋습니다. 하지만 임의의 종류의 I/O가 사용되는 호출은(모든 파일 처리 및 데이터베이스 검색 포함) 잠재적으로 호출 UI 스레드를 차단하고 애플리케이션이 무응답으로 인해 종료되도록 할 수 있습니다. 또한 성능상의 이유로 이 애플리케이션 아키텍처에서 개체에 대한 속성 호출은 권장되지 않습니다.
자세한 내용은 다음 섹션을 참조하세요.

올바르게 구현된 서버는 Windows 런타임 비동기 패턴을 통해 UI 스레드에서 직접 수행되는 호출을 정상적으로 구현합니다. 다음 패턴에 따라 구현할 수 있습니다. 첫째, (함께 제공된 샘플에서) 선언:

```csharp
public IAsyncOperation<int> FindElementAsync(int input)
```

정수를 반환하는 Windows 런타임 비동기 작업을 선언합니다.
비동기 작업을 구현하면 일반적으로 다음 형태를 가집니다.

```csharp
return Task<int>.Run( () =>
{
    int retval = ...
    // execute some potentially long-running code here 
}).AsAsyncOperation<int>();

```

>**참고** 구현을 작성하는 동안 잠재적으로 장기적으로 실행되는 다른 작업을 대기하는 것이 일반적입니다. 이 경우에는 **작업 실행** 코드를 선언 해야 합니다.

```csharp
return Task<int>.Run(async () =>
{
    int retval = ...
    // execute some potentially long-running code here 
    await ... // some other WinRT async operation or Task
}).AsAsyncOperation<int>();
```

이 비동기 메서드의 클라이언트는 다른 Windows 런타임 비동기 작업과 같이 이 작업을 대기할 수 있습니다.

**응용 프로그램 백그라운드 스레드에서 서버 기능 호출**

클라이언트와 서버 모두를 동일한 조직이 작성하는 것이 일반적이므로, 모든 서버 호출이 테스트용으로 로드하는 애플리케이션의 백그라운드 스레드에 의해 수행되는 프로그래밍 규칙을 채택할 수 있습니다. 서버에서 하나 이상의 데이터 배치를 수집하는 직접 호출을 백그라운드 스레드에서 수행할 수 있습니다. 결과가 완전히 검색되면 애플리케이션 프로세스의 메모리 내 데이터 배치는 일반적으로 UI 스레드에서 직접 검색할 수 있습니다. C \# 개체는 백그라운드 스레드와 UI 스레드 사이에서 자연스럽 게 민첩 하므로 이러한 종류의 호출 패턴에 특히 유용 합니다.

## <a name="creating-and-deploying-the-windows-runtime-proxy"></a>Windows 런타임 프록시 만들기 및 배포

IPC 방법에서는 두 프로세스 사이에 Windows 런타임 인터페이스를 마샬링해야 하므로, 전역으로 등록된 Windows 런타임 프록시 및 스텁을 사용해야 합니다.

**Visual Studio에서 프록시 만들기**

일반 UWP 앱 패키지 내에서 사용할 프록시 및 스텁을 만들고 등록 하는 프로세스는 [Windows 런타임 구성 요소에서 이벤트 발생](https://docs.microsoft.com/previous-versions/windows/apps/dn169426(v=vs.140))항목에 설명 되어 있습니다.
이 문서에서 설명하는 단계는 아래에서 설명하는 프로세스보다 더 복잡합니다. 전역으로 등록하는 것이 아니라, 애플리케이션 패키지 내부에서 프록시/스텁을 등록해야 하기 때문입니다.

**1 단계:** 데스크톱 구성 요소 프로젝트에 대 한 솔루션을 사용 하 여 Visual Studio에서 프록시/스텁 프로젝트를 만듭니다.

**솔루션 > > 프로젝트 > 추가 Visual C++ > Win32 콘솔에서 DLL 옵션을 선택 합니다.**

아래 단계에서는 서버 구성 요소를 **Mywinrtcomponent**라고 가정 합니다.

**3 단계:** 프로젝트에서 모든 CPP/H 파일을 삭제 합니다.

**4 단계:** 이전 "계약 정의" 섹션에는 **winmdidl**, **midl**, **mdmerge .exe**등을 실행 하는 빌드 후 명령이 포함 되어 있습니다. 이 Post-Build 명령의 midl 단계 출력 중 하나는 다음 네 가지 중요한 출력을 생성합니다.

a) Dlldata.c

b) 헤더 파일 (예: MyWinRTComponent .h)

c) A \* \_ .c 파일 (예: MyWinRTComponent \_ i. c)

d) \* \_ p. c 파일 (예: MyWinRTComponent \_ p .c)

**5 단계:** 이러한 4 개의 생성 된 파일을 "MyWinRTProxy" 프로젝트에 추가 합니다.

**6 단계:** Def 파일을 "MyWinRTProxy" 프로젝트에 추가 하 고 **(프로젝트 > 새 항목 추가 > 코드 > 모듈 정의 파일**) 콘텐츠를 업데이트 합니다.

LIBRARY MyWinRTComponent.Proxies.dll

EXPORTS

DllCanUnloadNow PRIVATE

DllGetClassObject PRIVATE

DllRegisterServer PRIVATE

DllUnregisterServer PRIVATE

**7 단계:** "MyWinRTProxy" 프로젝트에 대 한 속성을 엽니다.

**Comfiguration 속성 > 일반 > 대상 이름:**

MyWinRTComponent.Proxies

**C/c + + > 전처리기 정의 > 추가**

"WIN32; \_ WINDOWS \_프록시 \_ DLL 등록 "

**C/c + + > 미리 컴파일된 헤더: "미리 컴파일된 헤더 사용 안 함"을 선택 합니다.**

**링커 > 일반 > 가져오기 라이브러리 무시: "예"를 선택 합니다.**

**링커는 추가 종속성 > 입력을 > 합니다. rpcrt4; runtimeobject .lib를 추가 합니다.**

**Windows 메타 데이터를 생성 하 > 링커 > "아니요"를 선택 합니다.**

**8 단계:** "MyWinRTProxy" 프로젝트를 빌드합니다.

**프록시 배포**

프록시를 전역으로 등록해야 합니다. 이를 위한 가장 간단한 방법은 설치 프로세스에서 프록시 dll의 DllRegisterServer를 호출하도록 만드는 것입니다. 이 기능은 x86용으로 빌드된 서버만 지원하므로(즉, 64비트 지원 없음), 가장 간단한 구성은 32비트 서버, 32비트 프록시 및 32비트 테스트용으로 로드하는 애플리케이션을 사용하는 것입니다. 프록시는 일반적으로 데스크톱 구성 요소의 구현 **winmd**와 함께 배치됩니다.

한 가지 추가 구성 단계를 수행해야 합니다. 테스트용 로드 프로세스에서 프록시를 로드하고 실행할 수 있게 하려면 디렉터리에 ALL_APPLICATION_PACKAGES에 대한 "읽기 / 실행"을 표시해야 합니다. **icacls.exe**명령줄 도구를 통해 이를 수행합니다. 이 명령은 구현 **winmd** 및 프록시/스텁 dll이 상주하는 디렉터리에서 실행됩니다.

*icacls. /T/grant \* S-1-15-2-1: RX*

## <a name="patterns-and-performance"></a>패턴 및 성능

크로스 프로세스 전송의 성능을 주의 깊게 모니터링해야 합니다. 크로스 프로세스 호출은 In-Process 호출에 비해 최소 2배의 비용이 듭니다. 프로세스 간에 "활성" 대화를 만들거나 비트맵 이미지와 같은 큰 개체를 반복적으로 이전하면 애플리케이션 성능이 예기치 않게 악화될 수 있습니다.

고려해야 할 부분 목록은 다음과 같습니다.

-   애플리케이션의 UI 스레드에서 수행하는 동기 메서드 호출은 항상 피해야 합니다. 애플리케이션의 백그라운드 스레드에서 메서드를 호출한 후 필요한 경우 CoreWindowDispatcher를 사용하여 결과를 UI 스레드로 가져옵니다.

-   애플리케이션 UI 스레드에서 비동기 작업을 호출해도 좋지만, 아래에서 설명하는 성능 문제를 고려해야 합니다.

-   결과를 대량 전송하면 크로스 프로세스 활성도가 줄어듭니다. 일반적으로 Windows 런타임 배열 구문을 사용하여 수행됩니다.

-   반환 되는 *목록을 <T> * 반환 합니다. 여기서 *T* 는 비동기 작업 또는 속성 인출의 개체 이므로 많은 프로세스 간 데이터 전송량이 발생 합니다. 예를 들어* &lt; &gt; 사용자 목록* 개체를 반환 한다고 가정 합니다. 각 반복 단계가 크로스 프로세스 호출이 됩니다. 반환 되는 각 *사용자* 개체는 프록시로 표시 되 고 개별 개체의 메서드 또는 속성에 대 한 각 호출은 크로스 프로세스 호출을 발생 시킵니다. 따라서 *Count* 가 클 수 *있는 &lt; 사용자 &gt; * 개체를 나열 하면 많은 수의 저속 호출이 발생 합니다. 배열의 구조체를 대량으로 전송하면 더 높은 성능을 보입니다. 예:

```csharp
struct PersonStruct
{
    String LastName;
    String FirstName;
    int Age;
   // etc.
}
```

그런 다음 *List &lt; PersonObject &gt; *대신 *PersonStruct \[ \] * 를 반환 합니다.
그러면 한 번의 크로스 프로세스 "홉"에서 모든 데이터를 전달합니다.

모든 성능 고려 사항의 경우처럼, 측정과 테스트가 중요합니다. 원칙적으로, 걸리는 시간을 결정하기 위해 다양한 작업에 원격 분석을 삽입해야 합니다. 범위 전체에서 측정하는 것이 중요합니다. 예를 들어 테스트용으로 로드하는 응용 프로그램의 특정 쿼리가 모든 *피플* 개체를 사용하는 데 실제로 걸리는 시간을 측정합니다.

또 하나의 기술은 가변 부하 테스트입니다. 가변 지연 로드를 서버 처리에 도입하는 애플리케이션에 성능 테스트 후크를 배치하여 수행할 수 있습니다. 그러면 다양한 종류의 부하와 다양한 서버 성능에 대한 애플리케이션의 반응을 시뮬레이션할 수 있습니다.
샘플은 적합한 비동기 기술을 사용하여 시간 지연을 코드에 배치하는 방법을 보여 줍니다. 주입할 정확한 지연 시간 및 인위적인 부하에 배치할 불규칙화 범위는 애플리케이션 디자인 및 애플리케이션 실행 예상 환경에 따라 달라집니다.

## <a name="development-process"></a>개발 프로세스

서버를 변경하는 경우 이전에 실행 중인 인스턴스가 더 이상 실행되지 않는지 확인해야 합니다. 궁극적으로 COM에서 프로세스를 청소하지만 런다운 타이머는 반복적인 개발에 효율적인 것보다 더 오래 걸립니다. 따라서 개발 중에는 이전에 실행 중인 인스턴스를 중단하는 것이 일반적인 단계입니다. 이렇게 하려면 개발자가 서버를 호스트하는 dllhost 인스턴스를 추적해야 합니다.

작업 관리자 또는 다른 타사 앱을 사용하여 서버 프로세스를 찾아서 중단할 수 있습니다. 명령줄 도구 **TaskList .exe** 도 포함 되어 있으며 다음과 같은 유연한 구문을 포함 합니다.

  
 | **명령** | **작업** |
 | ------------| ---------- |
 | tasklist | 실행 중인 모든 프로세스를 대략적인 생성 시간 순으로 나열합니다. 가장 최근에 만든 프로세스가 아래쪽에 나열됩니다. |
 | tasklist /FI "IMAGENAME eq dllhost.exe" /M | 모든 dllhost.exe 인스턴스에 대한 정보를 나열합니다. /M 스위치는 로드한 모듈을 나열합니다. |
 | tasklist /FI "PID eq 12564" /M | 해당 PID를 알고 있는 경우 이 옵션을 사용하여 dllhost.exe를 쿼리할 수 있습니다. |

브로커 서버에 대 한 모듈 목록은 로드 된 모듈 목록에서 *clrhost .dll* 을 나열 해야 합니다.

## <a name="resources"></a>리소스

-   [Windows 10 및 VS 2015에 대 한 조정 된 WinRT 구성 요소 프로젝트 템플릿](https://marketplace.visualstudio.com/items?itemName=vs-publisher-713547.VS2015TemplateBrokeredComponents)

-   [안정적이 고 신뢰할 수 있는 Microsoft Store 앱 제공](https://blogs.msdn.com/b/b8/archive/2012/05/17/delivering-reliable-and-trustworthy-metro-style-apps.aspx)

-   [앱 계약 및 확장(Windows 스토어 앱)](https://docs.microsoft.com/previous-versions/windows/apps/hh464906(v=win.10))

-   [Windows 10에서 앱을 테스트용으로 로드하는 방법](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development)

-   [비즈니스에 UWP 앱 배포](https://blogs.msdn.com/b/windowsstore/archive/2012/04/25/deploying-metro-style-apps-to-businesses.aspx)

