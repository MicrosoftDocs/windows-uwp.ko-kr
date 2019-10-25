---
title: 테스트용으로 로드 된 UWP 앱에 대해 조정 된 Windows 런타임 구성 요소
description: 이 문서에서는 Windows 10에서 지원 되는 엔터프라이즈 대상 기능에 대해 설명 합니다 .이 기능을 사용 하면 터치 친화적인 .NET 앱이 중요 한 비즈니스에 중요 한 작업을 담당 하는 기존 코드를 사용할 수 있습니다.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 81b3930c-6af9-406d-9d1e-8ee6a13ec38a
ms.localizationpriority: medium
ms.openlocfilehash: b28df646bb505889626ced8591c5ef9e6ece3f44
ms.sourcegitcommit: f561efbda5c1d47b85601d91d70d86c5332bbf8c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/21/2019
ms.locfileid: "72690344"
---
# <a name="brokered-windows-runtime-components-for-a-side-loaded-uwp-app"></a>테스트용으로 로드 된 UWP 앱에 대해 조정 된 Windows 런타임 구성 요소

이 문서에서는 Windows 10에서 지원 되는 엔터프라이즈 대상 기능에 대해 설명 합니다 .이 기능을 사용 하면 터치 친화적인 .NET 앱에서 중요 한 비즈니스에 중요 한 작업을 담당 하는 기존 코드를 사용할 수 있습니다.

## <a name="introduction"></a>소개

>** 이** 문서와 함께 제공 되는 샘플 코드는 [Visual Studio 2015 & 2017](https://aka.ms/brokeredsample)에 다운로드 될 수 있습니다. 조정 된 Windows 런타임 구성 요소를 빌드하기 위한 Microsoft Visual Studio 템플릿은 [windows 10 용 유니버설 Windows 앱을 대상으로 하는 Visual Studio 2015 템플릿](https://marketplace.visualstudio.com/items?itemName=vs-publisher-713547.VS2015TemplateBrokeredComponents) 에서 다운로드할 수 있습니다.

Windows에는 *테스트용으로 로드 된 응용 프로그램을 위해 조정 된 Windows 런타임 구성 요소*라는 새로운 기능이 포함 되어 있습니다. IPC (프로세스 간 통신) 이라는 용어를 사용 하 여 UWP 앱에서이 코드와 상호 작용 하는 동안 기존 데스크톱 소프트웨어 자산을 한 프로세스 (데스크톱 구성 요소)에서 실행 하는 기능을 설명 합니다. 이는 Windows에서 NT 서비스를 활용 하는 데이터베이스 응용 프로그램 및 응용 프로그램은 비슷한 다중 프로세스 아키텍처를 활용 하는 엔터프라이즈 개발자에 게 친숙 한 모델입니다.

앱을 테스트용으로 로드 하는 것은이 기능의 중요 한 구성 요소입니다.
엔터프라이즈급 응용 프로그램은 일반 소비자 Microsoft Store에는 없으며, 기업에는 보안, 개인 정보, 배포, 설정 및 서비스와 관련 된 매우 구체적인 요구 사항이 있습니다. 따라서 테스트용 로드 모델은이 기능과 중요 한 구현 세부 정보를 사용 하는 사용자의 요구 사항입니다.

데이터 중심 응용 프로그램은이 응용 프로그램 아키텍처의 핵심 대상입니다. 예를 들어 SQL Server와 같은 기존 비즈니스 규칙은 데스크톱 구성 요소에서 공통적으로 ensconced 됩니다. 이것은 데스크톱 구성 요소에 의해 제공 되 수 있는 유일한 기능 유형이 아닙니다. 하지만이 기능에 대 한 수요의 상당 부분은 기존 데이터 및 비즈니스 논리와 관련이 있습니다.

마지막으로, 엔터프라이즈 개발의 .NET 런타임 및 C\# 언어의 과도 한 침투를 감안 하 여이 기능은 UWP 앱과 데스크톱 구성 요소 쪽 모두에 .NET을 사용 하는 것에 중점을 두어 개발 되었습니다. UWP 앱에서 사용할 수 있는 다른 언어 및 런타임이 있지만 함께 제공 되는 샘플은 C\#만 보여 주며 .NET 런타임으로만 제한 됩니다.

## <a name="application-components"></a>응용 프로그램 구성 요소

>**참고**  이 기능은 .net의 용도로만 사용 됩니다. 클라이언트 앱과 데스크톱 구성 요소는 모두 .NET을 사용 하 여 작성 해야 합니다.

**애플리케이션 모델**

이 기능은 MVVM (모델 뷰 뷰 모델) 라는 일반적인 응용 프로그램 아키텍처를 중심으로 작성 되었습니다. 따라서 "모델"은 전적으로 데스크톱 구성 요소에 있는 것으로 간주 됩니다. 따라서 바탕 화면 구성 요소가 "헤드리스" (즉, UI를 포함 하지 않음) 임을 명확 하 게 알 수 있습니다. 뷰는 테스트용으로 로드 된 엔터프라이즈 응용 프로그램에 완전히 포함 됩니다. "뷰 모델" 구문을 사용 하 여이 응용 프로그램을 빌드할 필요는 없지만이 패턴의 사용이 일반적인 것으로 예상 됩니다.

**데스크톱 구성 요소**

이 기능의 데스크톱 구성 요소는이 기능의 일부로 도입 되는 새로운 응용 프로그램 유형입니다. 이 데스크톱 구성 요소는 C\# 으로만 작성할 수 있으며 Windows 10의 경우 .NET 4.6 이상을 대상으로 해야 합니다. 프로세스 간 통신 형식은 UWP 형식 및 클래스를 구성 하는 반면, 데스크톱 구성 요소는 .NET 런타임 클래스 라이브러리의 모든 부분을 호출할 수 있으므로 프로젝트 형식은 UWP를 대상으로 하는 CLR 간의 하이브리드입니다. Visual Studio 프로젝트에 미치는 영향은 나중에 자세히 설명 합니다. 이 하이브리드 구성을 사용 하면 데스크톱 구성 요소에서 빌드된 응용 프로그램 간에 UWP 형식을 마샬링할 수 있으며 데스크톱 구성 요소 구현 내에서 데스크톱 CLR 코드를 호출할 수 있습니다.

**내용의**

파생 로드 된 응용 프로그램과 데스크톱 구성 요소 간의 계약은 UWP 형식 시스템 측면에서 설명 됩니다. 여기에는 UWP를 나타낼 수 있는 하나 이상의 C\# 클래스를 선언 하는 작업이 포함 됩니다. C [\#에서 Windows 런타임 구성 요소 만들기 및 Visual Basic](https://docs.microsoft.com/previous-versions/windows/apps/br230301(v=vs.140)) c\#를 사용 하 여 Windows 런타임 클래스를 만드는 특정 요구 사항은 MSDN 항목을 참조 하세요.

>**참고**  열거형은 데스크톱 구성 요소와 테스트용으로 로드 된 응용 프로그램 사이에서 Windows 런타임 구성 요소 계약에서 지원 되지 않습니다.

**테스트용으로 로드 된 응용 프로그램**

테스트용으로 로드 된 응용 프로그램은 하나를 제외 하 고는 일반적인 UWP 앱입니다 .이 앱은 Microsoft Store를 통해 설치 되는 대신 테스트용으로 로드 됩니다. 대부분의 설치 메커니즘은 동일 합니다. 매니페스트와 응용 프로그램 패키지는 유사 합니다. 매니페스트에 추가 하는 방법에 대해서는 뒷부분에서 자세히 설명 합니다. Side-by-side 로드를 사용 하도록 설정 하면 간단한 PowerShell 스크립트를 통해 필요한 인증서와 응용 프로그램 자체를 설치할 수 있습니다. 테스트용으로 로드 된 응용 프로그램이 Visual Studio의 프로젝트/스토어 메뉴에 포함 된 WACK 인증 테스트를 통과 하는 것이 가장 좋은 방법입니다.

>**참고** 테스트용 로드는 개발자를 위한 설정-&gt; 업데이트 & 보안&gt;에서 설정할 수 있습니다.

한 가지 중요 한 점은 Windows 10의 일부로 제공 되는 App Broker 메커니즘이 32 비트 전용입니다. 데스크톱 구성 요소는 32 비트 여야 합니다.
테스트용으로 로드 된 응용 프로그램은 64 비트 (64 비트 및 32 비트 프록시가 등록 되어 있음) 일 수 있지만이는 불규칙 합니다. 일반적인 "중립" 구성을 사용 하 여 C\#에 테스트용으로 로드 된 응용 프로그램을 빌드하고 "32 비트 선호" 기본값은 자연스럽 게 32 비트 테스트용으로 로드 된 응용 프로그램을 만듭니다.

**서버 인스턴스 및 Appdomain**

각 면 로드 응용 프로그램은 App Broker 서버 ("다중 인스턴스")의 자체 인스턴스를 수신 합니다. 서버 코드는 단일 AppDomain 내부에서 실행 됩니다. 이렇게 하면 여러 버전의 라이브러리가 별도의 인스턴스에서 실행 될 수 있습니다. 예를 들어 응용 프로그램 A에는 구성 요소의 V 1.1이 필요 하 고 응용 프로그램 B에는 V2가 필요 합니다. 이러한 구성 요소는 별도의 서버 디렉터리에 V 1.1 및 V2 구성 요소를 포함 하 고 응용 프로그램에서 원하는 올바른 버전을 지 원하는 서버를 가리키도록 하 여 완전히 분리 됩니다.

서버 코드 구현은 여러 응용 프로그램을 동일한 서버 디렉터리로 가리켜 여러 App Broker 서버 인스턴스 간에 공유할 수 있습니다. 여전히 App Broker 서버의 여러 인스턴스가 있지만 동일한 코드를 실행 하 게 됩니다. 단일 응용 프로그램에 사용 되는 모든 구현 구성 요소가 동일한 경로에 있어야 합니다.

## <a name="defining-the-contract"></a>계약 정의

이 기능을 사용 하 여 응용 프로그램을 만드는 첫 번째 단계는 테스트용으로 로드 된 응용 프로그램과 데스크톱 구성 요소 간의 계약을 만드는 것입니다. Windows 런타임 형식을 사용 하 여이 작업을 수행 해야 합니다.
다행히 이러한 클래스는 C\# 클래스를 사용 하 여 쉽게 선언할 수 있습니다. 그러나 이후 섹션에서 설명 하는 이러한 대화를 정의할 때 중요 한 성능 고려 사항이 있습니다.

계약을 정의 하는 순서는 다음과 같이 정의 됩니다.

**1 단계:** Visual Studio에서 새 클래스 라이브러리를 만듭니다. **Windows 런타임 구성 요소** 템플릿이 아닌 **클래스 라이브러리** 템플릿을 사용 하 여 프로젝트를 만들어야 합니다.

구현은 분명히 다르지만이 섹션에서는 프로세스 간 계약의 정의만 다룹니다. 함께 제공 되는 샘플에는 다음 클래스 (EnterpriseServer.cs)가 포함 됩니다. 시작 셰이프는 다음과 같습니다.

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

이 클래스는 테스트용으로 로드 된 응용 프로그램에서 인스턴스화할 수 있는 "EnterpriseServer" 클래스를 정의 합니다. 이 클래스는 RuntimeClass에 약속 된 기능을 제공 합니다. RuntimeClass은 테스트용으로 로드 된 응용 프로그램에 포함 될 참조 winmd를 생성 하는 데 사용할 수 있습니다.

**2 단계:** 프로젝트 파일을 수동으로 편집 하 여 프로젝트의 출력 형식을 **Windows 런타임 구성 요소로**변경 합니다.

Visual Studio에서이 작업을 수행 하려면 새로 만든 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 "프로젝트 언로드"를 선택한 다음 다시 마우스 오른쪽 단추로 클릭 하 고 "EnterpriseServer. .csproj 편집"을 선택 하 여 프로젝트 파일 (XML 파일)을 편집용으로 엽니다.

열려 있는 파일에서 \<OutputType\> 태그를 검색 하 고 해당 값을 "winmdobj"로 변경 합니다.

**3 단계:** "참조" Windows 메타 데이터 파일 (winmd 파일)을 만드는 빌드 규칙을 만듭니다. 즉,는 구현 하지 않습니다.

**4 단계:** "구현" Windows 메타 데이터 파일을 만드는 빌드 규칙을 만듭니다. 즉, 동일한 메타 데이터 정보를 갖지만 구현도 포함 합니다.

이 작업은 다음 스크립트에 의해 수행 됩니다. 빌드 후 이벤트 명령줄에 스크립트를 추가 하 고 프로젝트 **속성** 에서 **빌드 이벤트** > 합니다.

> 스크립트 **는 대상으로 하는 windows** 버전 (windows 10)과 사용 중인 Visual Studio 버전에 따라 다릅니다.

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

참조 **winmd** 가 생성 되 면 (프로젝트의 대상 폴더 아래에 있는 "참조" 폴더에서) 사용 하는 로드 된 응용 프로그램 프로젝트와 참조 된 각 응용 프로그램에 전달 (복사) 됩니다. 이에 대해서는 다음 섹션에서 자세히 설명 합니다. 위의 빌드 규칙에서 합의서 등 프로젝트 구조는 구현 및 참조 **winmd** 가 혼동을 피하기 위해 빌드 계층 구조에서 명확 하 게 분리 된 디렉터리에 있는지 확인 합니다.

## <a name="side-loaded-applications-in-detail"></a>테스트용으로 로드 된 응용 프로그램 세부 정보
앞에서 설명한 것 처럼, 테스트용으로 로드 된 응용 프로그램은 다른 UWP 앱과 마찬가지로 빌드 되지만, 테스트용으로 로드 된 응용 프로그램의 매니페스트에서 RuntimeClass의 가용성을 선언 하는 추가 세부 정보는 하나 있습니다. 이를 통해 응용 프로그램은 단순히 새를 작성 하 여 데스크톱 구성 요소의 기능에 액세스할 수 있습니다. <Extension> 섹션의 새 매니페스트 항목에서는 데스크톱 구성 요소에서 구현 되는 RuntimeClass의 위치에 대 한 정보를 설명 합니다. 응용 프로그램 매니페스트의 이러한 선언 콘텐츠는 Windows 10을 대상으로 하는 앱에 대해 동일 합니다. 예:

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

OutOfProcessServer 범주에이 응용 프로그램 구성에 적용할 수 없는 항목이 여러 개 있으므로 범주가 inProcessServer입니다. <Path> 구성 요소는 항상 clrhost .dll을 포함 해야 합니다. 그러나이는 적용 **되지** 않으며 다른 값 지정은 정의 되지 않은 방식으로 실패 합니다.

<ActivatableClass> 섹션은 응용 프로그램 패키지의 Windows 런타임 구성 요소에서 선호 하는 진정한 in-process RuntimeClass와 동일 합니다. <ActivatableClassAttribute>는 새 요소 이며 Name = "DesktopApplicationPath" 및 Type = "string"은 필수 및 고정입니다. Value 특성은 데스크톱 구성 요소의 구현 winmd가 있는 위치를 가리킵니다 (다음 섹션에서 자세히 설명). 데스크톱 구성 요소에서 선호 하는 각 RuntimeClass에는 고유한 <ActivatableClass> 요소 트리가 있어야 합니다. ActivatableClassId는 RuntimeClass의 정규화 된 네임 스페이스로 정규화 된 이름과 일치 해야 합니다.

"계약 정의" 섹션에서 설명한 것 처럼 데스크톱 구성 요소의 참조 winmd에 대 한 프로젝트 참조를 만들어야 합니다. Visual Studio 프로젝트 시스템은 일반적으로 같은 이름의 두 수준 디렉터리 구조를 만듭니다. 이 샘플에서는 EnterpriseIPCApplication\\EnterpriseIPCApplication입니다. 참조 **winmd** 는이 두 번째 수준 디렉터리에 수동으로 복사 된 후 프로젝트 참조 대화 상자가 사용 됩니다 **. (찾아보기** 를 클릭 합니다. 단추)를 클릭 하 여이 **winmd**를 찾고 참조 합니다. 그런 다음 데스크톱 구성 요소의 최상위 네임 스페이스 (예: Fabrikam)는 프로젝트의 참조 부분에서 최상위 노드로 표시 됩니다.

>**참고** 테스트용으로 로드 된 응용 프로그램의 **참조** 를 사용 하는 것이 매우 중요 합니다. 의도 하지 않은 인스턴스를 테스트용으로 로드 된 응용 프로그램 **디렉터리에 전달 하 여 참조** 하면 "IStringable를 찾을 수 없습니다."와 관련 된 오류가 발생할 수 있습니다. 이는 잘못 된 **winmd** 가 참조 되었음을 나타내는 것입니다. IPC 서버 앱의 빌드 후 규칙 (다음 섹션에 자세히 설명)은 이러한 두 개의 **winmd** 를 별도의 디렉터리로 신중 하 게 분리 합니다.

환경 변수 (특히% ProgramFiles%) <ActivatableClassAttribute Value="path">에서 사용할 수 있습니다. 앞에서 설명한 것 처럼 App Broker는 32 비트만 지원 하므로% ProgramFiles%는 64 비트 OS에서 응용 프로그램을 실행 하는 경우 C:\\Program Files (x86)로 확인 됩니다.

## <a name="desktop-ipc-server-detail"></a>데스크톱 IPC 서버 세부 정보

이전 두 단원에서는 클래스의 선언 및 파생 로드 된 응용 프로그램 프로젝트에 참조 **winmd** 를 전송 하는 메커니즘을 설명 합니다. 데스크톱 구성 요소의 남은 작업 중에는 구현이 포함 됩니다. 데스크톱 구성 요소의 전체 지점은 일반적으로 기존 코드 자산을 다시 활용 하기 위해 데스크톱 코드를 호출할 수 있기 때문에 프로젝트를 특별 한 방식으로 구성 해야 합니다.
일반적으로 .NET을 사용 하는 Visual Studio 프로젝트는 두 가지 "프로필" 중 하나를 사용 합니다.
하나는 데스크톱용입니다 ("). NetFramework ") 및 하나는 CLR의 UWP 앱 부분을 대상으로 합니다. NetCore "). 이 기능의 데스크톱 구성 요소는 이러한 두 가지를 혼합 하는 것입니다. 따라서 참조 섹션은 이러한 두 프로필을 혼합 하기 위해 매우 신중 하 게 구성 됩니다.

일반적인 UWP 앱 프로젝트에는 Windows 런타임 API 화면 전체가 암시적으로 포함 되므로 명시적인 프로젝트 참조가 포함 되지 않습니다.
일반적으로 다른 프로젝트 간 참조만 생성 됩니다. 그러나 데스크톱 구성 요소 프로젝트에는 매우 특수 한 참조 집합이 있습니다. "클래식 데스크톱\\클래스 라이브러리" 프로젝트로 수명이 시작 되므로 데스크톱 프로젝트가 됩니다. 따라서 **winmd** 파일에 대 한 참조를 통해 Windows 런타임 API에 대 한 명시적 참조를 만들어야 합니다. 아래와 같이 적절 한 참조를 추가 합니다.

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

위의 참조는이 하이브리드 서버를 적절 하 게 작동 하는 데 중요 한 eferences의 신중한 조합입니다. 이 프로토콜은 프로젝트 OutputType을 편집 하는 방법에 설명 된 .csproj 파일을 열고 필요에 따라 이러한 참조를 추가 하는 것입니다.

참조가 제대로 구성 되 면 다음 태스크는 서버 기능을 구현 하는 것입니다.  [Windows 런타임 구성 요소와의 상호 운용성에 대 한 모범 사례 (C\#/VB/C++ 및 XAML을 사용 하는 UWP 앱)](https://docs.microsoft.com/previous-versions/windows/apps/hh750311(v=win.10))항목을 참조 하세요.
작업은 해당 구현의 일부로 데스크톱 코드를 호출할 수 있는 Windows 런타임 구성 요소 dll을 만드는 것입니다. 함께 제공 되는 샘플에는 Windows 런타임에서 사용 되는 주요 패턴이 포함 됩니다.

-   메서드 호출

-   데스크톱 구성 요소에의 한 이벤트 원본 Windows 런타임

-   비동기 작업 Windows 런타임

-   기본 형식의 배열 반환

**설치**

앱을 설치 하려면 연결 된 ' 연결 된 ' 응용 프로그램의 매니페스트에 지정 된 올바른 디렉터리 (<ActivatableClassAttribute>의 값 = "경로")에 구현 **winmd** 를 복사 합니다. 또한 연결 된 지원 파일 및 프록시/스텁 dll을 복사 합니다 (이에 대 한 자세한 내용은 아래에 설명 되어 있음). 구현 **winmd** 를 서버 디렉터리 위치로 복사 하지 못하면 RuntimeClass에서 모든 테스트용으로 로드 된 응용 프로그램의 호출이 "클래스 등록 되지 않음" 오류를 throw 합니다. 프록시/스텁 (또는 등록 실패)을 설치 하지 못하면 모든 호출이 실패 하 고 반환 값이 발생 하지 않습니다. 이 두 번째 오류는 표시 되는 예외와 연결 **되지 않는** 경우가 많습니다.
이 구성 오류로 인해 예외가 관찰 되는 경우 "잘못 된 캐스트"를 참조할 수 있습니다.

**서버 구현 고려 사항**

데스크톱 Windows 런타임 서버는 "작업자" 또는 "작업" 기반으로 간주할 수 있습니다. 서버에 대 한 모든 호출은 비 UI 스레드에서 작동 하며 모든 코드는 다중 스레드 인식 및 안전 해야 합니다. 또한 서버측에서 로드 한 응용 프로그램에서 서버 기능을 호출 하는 부분은 중요 합니다. 테스트용으로 로드 된 응용 프로그램의 UI 스레드에서 긴 실행 코드를 호출 하지 않는 것이 중요 합니다. 이 작업을 수행 하는 두 가지 주요 방법이 있습니다.

1.  UI 스레드에서 서버 기능을 호출 하는 경우 항상 서버의 공용 노출 영역 및 구현에서 비동기 패턴을 사용 합니다.

2.  테스트용으로 로드 된 응용 프로그램의 백그라운드 스레드에서 서버 기능을 호출 합니다.

**서버에서 async를 Windows 런타임 합니다.**

응용 프로그램 모델의 프로세스 간 특성을 고려 하 여 서버에 대 한 호출에는 독점적으로 실행 되는 코드 보다 많은 오버 헤드가 있습니다. 일반적으로 메모리 내 값을 반환 하는 간단한 속성을 호출 하는 것이 안전 합니다 .이 속성은 UI 스레드를 차단 하는 것이 중요 하지 않을 만큼 신속 하 게 실행 되기 때문입니다. 그러나 모든 정렬의 i/o를 포함 하는 모든 호출 (이 경우 모든 파일 처리 및 데이터베이스를 포함 하는 호출)은 잠재적으로 호출 UI 스레드를 차단 하 고 응답 하지 않아 응용 프로그램을 종료 시킬 수 있습니다. 또한 개체에 대 한 속성 호출은 성능상의 이유로이 응용 프로그램 아키텍처에서 권장 되지 않습니다.
이에 대해서는 다음 섹션에서 자세히 설명 합니다.

정상적으로 구현 된 서버는 일반적으로 Windows 런타임 비동기 패턴을 통해 UI 스레드에서 직접 호출을 구현 합니다. 이 패턴을 따라 구현할 수 있습니다. 먼저 선언 (함께 제공 된 샘플에서 다시)입니다.

```csharp
public IAsyncOperation<int> FindElementAsync(int input)
```

이는 정수를 반환 하는 Windows 런타임 비동기 작업을 선언 합니다.
비동기 작업의 구현은 일반적으로 다음 형식을 사용 합니다.

```csharp
return Task<int>.Run( () =>
{
    int retval = ...
    // execute some potentially long-running code here 
}).AsAsyncOperation<int>();

```

>**참고** 구현을 작성 하는 동안 잠재적으로 시간이 오래 걸리는 다른 작업을 대기 하는 것이 일반적입니다. 이 경우에는 **작업 실행** 코드를 선언 해야 합니다.

```csharp
return Task<int>.Run(async () =>
{
    int retval = ...
    // execute some potentially long-running code here 
    await ... // some other WinRT async operation or Task
}).AsAsyncOperation<int>();
```

이 비동기 메서드의 클라이언트는 다른 Windows 런타임 aysnc 작업 처럼이 작업을 기다릴 수 있습니다.

**응용 프로그램 백그라운드 스레드에서 서버 기능 호출**

클라이언트와 서버는 모두 동일한 조직에서 작성 되기 때문에 서버에 대 한 모든 호출이 테스트용으로 로드 된 응용 프로그램의 백그라운드 스레드에서 수행 되도록 프로그래밍 방법을 채택할 수 있습니다. 서버에서 하나 이상의 데이터 일괄 처리를 수집 하는 직접 호출은 백그라운드 스레드에서 수행할 수 있습니다. 결과를 완전히 검색할 때 응용 프로그램 프로세스의 메모리 내 데이터 일괄 처리는 일반적으로 UI 스레드에서 직접 검색할 수 있습니다. C @ no__t_0_ 개체는 백그라운드 스레드와 UI 스레드 사이에서 자연스럽 게 민첩 하므로 이러한 종류의 호출 패턴에 특히 유용 합니다.\#

## <a name="creating-and-deploying-the-windows-runtime-proxy"></a>Windows 런타임 프록시 만들기 및 배포

IPC 접근 방식에는 두 프로세스 간의 인터페이스 Windows 런타임 마샬링을 포함 하므로 전역적으로 등록 된 Windows 런타임 프록시와 스텁을 사용 해야 합니다.

**Visual Studio에서 프록시 만들기**

일반 UWP 앱 패키지 내에서 사용할 프록시 및 스텁을 만들고 등록 하는 프로세스는 [Windows 런타임 구성 요소에서 이벤트 발생](https://docs.microsoft.com/previous-versions/windows/apps/dn169426(v=vs.140))항목에 설명 되어 있습니다.
이 문서에서 설명 하는 단계는 전역으로 등록 하는 것과는 반대로 응용 프로그램 패키지 내에 프록시/스텁을 등록 하는 작업을 포함 하기 때문에 아래에 설명 된 프로세스 보다 더 복잡 합니다.

**1 단계:** 데스크톱 구성 요소 프로젝트에 대 한 솔루션을 사용 하 여 Visual Studio에서 프록시/스텁 프로젝트를 만듭니다.

**솔루션 > > 프로젝트 > Visual C++ > Win32 콘솔을 추가 합니다. DLL 옵션을 선택 합니다.**

아래 단계에서는 서버 구성 요소를 **Mywinrtcomponent**라고 가정 합니다.

**3 단계:** 프로젝트에서 모든 CPP/H 파일을 삭제 합니다.

**4 단계:** 이전 "계약 정의" 섹션에는 **winmdidl**, **midl**, **mdmerge .exe**등을 실행 하는 빌드 후 명령이 포함 되어 있습니다. 이 빌드 후 명령의 midl 단계 출력 중 하나는 네 가지 중요 한 출력을 생성 합니다.

a) Dlldata.c

b) 헤더 파일 (예: MyWinRTComponent .h)

c) \*\_.c 파일 (예: MyWinRTComponent\_i. c)

d) \*\_.c 파일 (예: MyWinRTComponent\_p. c)

**5 단계:** 이러한 4 개의 생성 된 파일을 "MyWinRTProxy" 프로젝트에 추가 합니다.

**6 단계:** Def 파일을 "MyWinRTProxy" 프로젝트에 추가 하 고 **(프로젝트 > 새 항목 추가 > 코드 > 모듈 정의 파일**) 콘텐츠를 업데이트 합니다.

라이브러리 MyWinRTComponent. 프록시 .dll

내보내도록

DllCanUnloadNow 개인

DllGetClassObject 개인

DllRegisterServer 개인

DllUnregisterServer 개인

**7 단계:** "MyWinRTProxy" 프로젝트에 대 한 속성을 엽니다.

**Comfiguration 속성 > 일반 > 대상 이름:**

MyWinRTComponent. 프록시

**C/C++ > 전처리기 정의 > 추가**

(WIN32\_WINDOWS\_프록시\_DLL "등록

**C/C++ > 미리 컴파일된 헤더: "미리 컴파일된 헤더 사용 안 함"을 선택 합니다.**

**링커 > 일반 > 가져오기 라이브러리 무시: "예"를 선택 합니다.**

**링커는 추가 종속성 > 입력을 > 합니다. rpcrt4; runtimeobject .lib를 추가 합니다.**

**Windows 메타 데이터를 생성 하 > 링커 > "아니요"를 선택 합니다.**

**8 단계:** "MyWinRTProxy" 프로젝트를 빌드합니다.

**프록시 배포**

프록시는 전역적으로 등록 되어야 합니다. 이 작업을 수행 하는 가장 간단한 방법은 설치 프로세스에서 프록시 dll에 대해 DllRegisterServer를 호출 하는 것입니다. 이 기능은 x 86 용으로 빌드된 서버 (즉, 64 비트 지원 없음)만 지원 하므로 가장 간단한 구성은 32 비트 서버, 32 비트 프록시 및 32 비트의 테스트용 로드 응용 프로그램을 사용 하는 것입니다. 프록시는 일반적으로 데스크톱 구성 요소에 대 한 **winmd** 구현과 함께 있습니다.

하나의 추가 구성 단계를 수행 해야 합니다. 테스트용으로 로드 된 프로세스에서 프록시를 로드 하 고 실행 하려면 디렉터리를 ALL_APPLICATION_PACKAGES의 "읽기/실행"으로 표시 해야 합니다. 이 작업은 **icacls** 명령줄 도구를 통해 수행 됩니다. 이 명령은 **winmd** 및 프록시/스텁 dll 구현이 있는 디렉터리에서 실행 해야 합니다.

*icacls. /T/grant \*S-1-15-2-1: RX*

## <a name="patterns-and-performance"></a>패턴 및 성능

프로세스 간 전송의 성능을 신중 하 게 모니터링 하는 것이 매우 중요 합니다. 크로스 프로세스 호출은 in-process 호출 보다 두 배 이상 비쌉니다. "번잡" 대화를 만들거나 비트맵 이미지와 같은 커다란 개체를 반복적으로 전송 하는 작업을 수행 하면 예기치 않은 응용 프로그램 성능이 저하 될 수 있습니다.

다음은 고려해 야 할 사항에 대 한 완전 하지 않은 목록입니다.

-   응용 프로그램의 UI 스레드에서 서버에 동기 메서드를 호출 하는 것은 항상 피해 야 합니다. 응용 프로그램의 백그라운드 스레드에서 메서드를 호출한 다음 CoreWindowDispatcher를 사용 하 여 필요한 경우 UI 스레드에 결과를 가져옵니다.

-   응용 프로그램 UI 스레드에서 비동기 작업을 호출 하는 것은 안전 하지만 아래에서 설명 하는 성능 문제를 고려해 야 합니다.

-   결과를 대량으로 전송 하면 프로세스 간 데이터 전송량 줄어듭니다. 이는 일반적으로 Windows 런타임 배열 구문을 사용 하 여 수행 됩니다.

-   *목록<T>* 반환. 여기서 *t* 는 비동기 작업 또는 속성 페치의 개체 이므로 많은 프로세스 간 데이터 전송량이 발생 합니다. 예를 들어*사용자가 개체&gt;&lt;목록을* 반환 한다고 가정 합니다. 각 반복 패스는 크로스 프로세스 호출입니다. 반환 되는 각 *사용자* 개체는 프록시로 표시 되 고 개별 개체의 메서드 또는 속성에 대 한 각 호출은 크로스 프로세스 호출을 발생 시킵니다. 따라서 "가 중" 목록에는 *개수가* 클 때 많은 수의 저속 호출이 발생 하는 *사람&gt;* 개체를&lt;합니다. 배열에 있는 콘텐츠의 구조체를 대량으로 전송 하는 경우 성능 결과가 향상 됩니다. 예:

```csharp
struct PersonStruct
{
    String LastName;
    String FirstName;
    int Age;
   // etc.
}
```

그런 다음 *목록&lt;PersonObject&gt;* 대신 * PersonStruct\[\]*를 반환 합니다.
이를 통해 모든 데이터를 하나의 크로스 프로세스 "홉"으로 가져옵니다.

모든 성능 고려 사항과 마찬가지로 측정 및 테스트는 중요 합니다. 이상적인 원격 분석은 다양 한 작업에 삽입 하 여 소요 시간을 확인 해야 합니다. 모든 범위에서 측정 하는 것이 중요 합니다. 예를 들어, 테스트용으로 로드 된 응용 프로그램에서 특정 쿼리에 대 한 모든 *사람* 개체를 사용 하는 데 걸리는 시간입니다.

또 다른 방법은 가변 부하 테스트입니다. 이 작업은 서버 처리에 변수 지연을 로드 하는 응용 프로그램에 성능 테스트 후크를 추가 하 여 수행할 수 있습니다. 이를 통해 다양 한 종류의 부하와 다양 한 서버 성능에 대 한 응용 프로그램의 반응을 시뮬레이션할 수 있습니다.
이 샘플에서는 적절 한 비동기 기술을 사용 하 여 시간 지연을 코드에 넣는 방법을 보여 줍니다. 삽입할 지연 양과 인공 로드 범위는 응용 프로그램 디자인 및 응용 프로그램이 실행 되는 예상 환경에 따라 달라 집니다.

## <a name="development-process"></a>개발 프로세스

서버를 변경할 때에는 이전에 실행 중인 인스턴스가 더 이상 실행 되지 않는지 확인 해야 합니다. COM은 궁극적으로 프로세스를 청소 하지만, 런다운 타이머가 반복적인 개발에 효율적이 지는 것 보다 시간이 오래 걸립니다. 따라서 이전에 실행 중인 인스턴스를 중지 하는 것은 개발 중에 일반적인 단계입니다. 이를 위해서는 개발자가 서버를 호스팅하는 dllhost 인스턴스를 추적 해야 합니다.

작업 관리자 또는 타사 앱을 사용 하 여 서버 프로세스를 찾아 중지할 수 있습니다. 명령줄 도구 * * TaskList * *도 포함 되어 있으며 다음과 같은 유연한 구문을 포함 합니다.

  
 | **명령** | **작업** |
 | ------------| ---------- |
 | tasklist | 가장 최근에 만든 프로세스를 사용 하 여 실행 중인 모든 프로세스를 생성 시간의 대략적인 순서로 나열 합니다. |
 | tasklist/FI "IMAGENAME eq dllhost.exe"/M | 모든 dllhost.exe 인스턴스의 정보를 나열 합니다. /M 스위치는 로드 된 모듈을 나열 합니다. |
 | tasklist/FI "PID eq 12564"/M | PID를 알고 있는 경우이 옵션을 사용 하 여 dllhost.exe를 쿼리할 수 있습니다. |

브로커 서버에 대 한 모듈 목록은 로드 된 모듈 목록에서 *clrhost .dll* 을 나열 해야 합니다.

## <a name="resources"></a>리소스

-   [Windows 10 및 VS 2015에 대 한 조정 된 WinRT 구성 요소 프로젝트 템플릿](https://marketplace.visualstudio.com/items?itemName=vs-publisher-713547.VS2015TemplateBrokeredComponents)

-   [NorthwindRT 조정 된 WinRT 구성 요소 샘플](https://go.microsoft.com/fwlink/p/?LinkID=397349)

-   [안정적이 고 신뢰할 수 있는 Microsoft Store 앱 제공](https://go.microsoft.com/fwlink/p/?LinkID=393644)

-   [앱 계약 및 확장 (Windows 스토어 앱)](https://docs.microsoft.com/previous-versions/windows/apps/hh464906(v=win.10))

-   [Windows 10에서 앱을 테스트용으로 로드 하는 방법](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development)

-   [비즈니스에 UWP 앱 배포](https://go.microsoft.com/fwlink/p/?LinkID=264770)

