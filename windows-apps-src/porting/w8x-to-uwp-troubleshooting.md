---
description: 이 포팅 가이드의 끝 부분에 대 한 읽기를 권장 하지만, 프로젝트를 빌드하고 실행 하는 단계를 계속 해 서 확인 하는 것도 이해 하 고 있습니다.
title: Windows 런타임 .x에서 UWP로 포팅 문제 해결
ms.assetid: 1882b477-bb5d-4f29-ba99-b61096f45e50
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e9d2ba97ece396cbec3c0b1f1cf9941b91f261aa
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162197"
---
# <a name="troubleshooting-porting-windows-runtime-8x-to-uwp"></a>4.x에서 UWP로의 포팅 Windows 런타임 문제 해결


이전 항목에서는 [프로젝트를 포팅](w8x-to-uwp-porting-to-a-uwp-project.md)했습니다.

이 포팅 가이드의 끝 부분에 대 한 읽기를 권장 하지만, 프로젝트를 빌드하고 실행 하는 단계를 계속 해 서 확인 하는 것도 이해 하 고 있습니다. 이렇게 하려면 중요 하지 않은 코드를 주석 처리 하거나 구체적인 다음을 반환 하 여 나중에 해당 부채를 지불 하는 방식으로 임시 진행률을 만들 수 있습니다. 이 항목의 문제 해결 및 해결 방법 표는이 단계에서 도움이 될 수 있지만 다음 몇 가지 항목을 읽는 것은 아닙니다. 이후 항목을 진행 하면서 언제 든 지 테이블을 다시 참조할 수 있습니다.

## <a name="tracking-down-issues"></a>문제 추적

XAML 구문 분석 예외는 특히 예외 내에 의미 없는 오류 메시지가 없는 경우 진단 하기 어려울 수 있습니다. 첫 번째 예외를 catch 하도록 디버거가 구성 되어 있는지 확인 합니다 (조기에 구문 분석 예외를 시도 하 고 catch 하기 위해). 디버거에서 예외 변수를 검사 하 여 HRESULT 또는 메시지에 유용한 정보가 있는지 여부를 확인할 수 있습니다. 또한 Visual Studio의 출력 창에서 XAML 파서에서 출력 하는 오류 메시지를 확인 합니다.

앱이 종료 되 고 모든 것이 XAML 태그 구문 분석 중에 처리 되지 않은 예외가 throw 된 경우에는 누락 된 리소스에 대 한 참조의 결과일 수 있습니다. 즉, 일부 시스템 **TextBlock** 스타일 키와 같은 Windows 10 앱에 대해서는 그렇지 않고 유니버설 8.1 앱에 대 한 키가 존재 하는 리소스입니다. 또는 **UserControl**, 사용자 지정 컨트롤 또는 사용자 지정 레이아웃 패널 내에서 예외가 throw 될 수 있습니다.

마지막 수단은 이진 분할입니다. 페이지에서 태그의 절반을 제거 하 고 앱을 다시 실행 합니다. 그런 다음 오류가 제거 된 절반 (어떤 경우에는 어떤 경우에만 복원 해야 함) 내에 있는지 또는 제거 *하지* 않은 절반에 있는지를 확인 합니다. 오류를 포함 하는 절반을 분할 하 여 프로세스를 반복 합니다.

## <a name="targetplatformversion"></a>TargetPlatformVersion

이 섹션에서는 Visual Studio에서 Windows 10 프로젝트를 열 때 "Visual Studio 업데이트 필요" 메시지가 표시 되는 경우 수행할 작업에 대해 설명 합니다. 하나 이상의 프로젝트에 \<version\> 설치 되지 않았거나 Visual Studio에 대 한 향후 업데이트의 일부로 포함 되는 플랫폼 SDK가 필요 합니다. "

-   먼저 설치한 Windows 10 용 SDK의 버전 번호를 확인 합니다. **C: \\ Program Files (x86) \\ Windows 키트 \\ 10 \\ \\ \<versionfoldername\> ** 로 이동 하 여 *\<versionfoldername\>* 쿼드 표기법 "Major. 개정판"으로 표시 되는을 기록해 둡니다.
-   편집을 위해 프로젝트 파일을 열고 `TargetPlatformVersion` 및 요소를 찾습니다 `TargetPlatformMinVersion` . 다음과 같이 편집 하 여 *\<versionfoldername\>* 디스크에 있는 쿼드 표기법 버전 번호로 바꿉니다.

```xml
   <TargetPlatformVersion><versionfoldername></TargetPlatformVersion>
    <TargetPlatformMinVersion><versionfoldername></TargetPlatformMinVersion>
```

## <a name="troubleshooting-symptoms-and-remedies"></a>증상 및 해결 방법

테이블의 보상 정보는 자신을 차단 해제 하는 데 충분 한 정보를 제공 하기 위한 것입니다. 이러한 각 문제에 대 한 자세한 내용은 이후 항목을 참조 하세요.

| 증상 | 해결책 |
|---------|--------|
| Visual Studio에서 Windows 10 프로젝트를 열면 "Visual Studio 업데이트 필요" 메시지가 표시 됩니다. 하나 이상의 프로젝트에 &lt; &gt; 설치 되지 않았거나 Visual Studio에 대 한 향후 업데이트의 일부로 포함 된 platform SDK 버전이 필요 합니다. " | 이 항목의 [Targetplatformversion](#targetplatformversion) 섹션을 참조 하세요. |
| Xaml.cs 파일에서 InitializeComponent을 호출 하면 InvalidCastException이 throw 됩니다.| 이 문제는 동일한 xaml.cs 파일을 공유 하는 하나 이상의 xaml 파일이 있는 경우, 그리고 요소에 두 xaml 파일 간에 일치 하지 않는 x:Name 특성이 있는 경우에 발생할 수 있습니다. 두 xaml 파일의 동일한 요소에 동일한 이름을 추가 하거나 이름을 모두 생략 합니다. |
| 앱이 장치에서 실행 되는 경우 또는 Visual Studio에서 시작 될 때 "Windows 런타임 .x 앱을 활성화할 수 없습니다 \[ .... \] ' Windows가 대상 응용 프로그램과 통신할 수 없습니다. ' 오류로 인해 활성화 요청이 실패 했습니다. 이는 일반적으로 대상 응용 프로그램의 프로세스가 중단 되었음을 나타냅니다. \[…\]”. | 문제는 초기화 하는 동안 사용자의 페이지 또는 바인딩된 속성 (또는 기타 형식)에서 실행 되는 명령적 코드 일 수 있습니다. 또는 앱이 종료 될 때 표시 될 XAML 파일을 구문 분석 하는 동안 발생할 수 있습니다 (Visual Studio에서 시작 하는 경우 시작 페이지가 됨). 잘못 된 리소스 키를 확인 하 고이 항목의 "문제 추적" 섹션에서 지침 중 일부를 시도 합니다.|
| XAML 파서 또는 컴파일러 또는 런타임 예외는 "*리소스" "을 (를) \<resourcekey\> 확인할*수 없습니다." 라는 오류를 제공 합니다. | 리소스 키는 유니버설 Windows 플랫폼 (UWP) 앱에 적용 되지 않습니다. 예를 들어 일부 Windows Phone 리소스가 있는 경우에 해당 합니다. 올바른 해당 리소스를 찾고 태그를 업데이트 합니다. 바로 발생할 수 있는 예는와 같은 시스템 키 `PhoneAccentBrush` 입니다. |
| C # 컴파일러는 "'*' 형식 또는 네임 스페이스 이름을 \<name\> 찾을 수 없습니다 \[ \] .*" 또는 "형식 또는 네임 스페이스 이름 ' '이 (가)* \<name\> 네임 스페이스 \[ \] 에*없습니다." 또는 "*형식 또는 네임 스페이스 이름 ' '이 (가) \<name\> 현재 컨텍스트에*없습니다." 오류를 제공 합니다. | 이는 형식이 확장 SDK에서 구현 될 수 있다는 것을 의미할 수 있습니다 (해결 방법이 명확 하지 않은 경우도 있음). [Windows api](/uwp/) 참조 콘텐츠를 사용 하 여 API를 구현 하는 확장 sdk를 확인 하 고 Visual Studio의 참조 **추가**명령을 사용 하 여  >  **Reference** 해당 sdk에 대 한 참조를 프로젝트에 추가 합니다. 앱이 유니버설 장치 제품군 이라는 Api 집합을 대상으로 하는 경우,이를 호출 하기 전에 [**Apiinformation**](/uwp/api/Windows.Foundation.Metadata.ApiInformation) 클래스를 사용 하 여 런타임 시 확장 SDK가 있는지 테스트 하는 것이 중요 합니다 (적응 코드 라고 함). 범용 API가 있는 경우에는 항상 확장 SDK의 API에 대 한 것이 좋습니다. 자세한 내용은 [확장 sdk](w8x-to-uwp-porting-to-a-uwp-project.md)를 참조 하세요. |

다음 항목에서는 [XAML 및 UI 포팅](w8x-to-uwp-porting-xaml-and-ui.md)에 대해 설명 합니다.