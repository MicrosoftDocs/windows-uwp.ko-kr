---
author: stevewhims
Description: The Multilingual App Toolkit (MAT) 4.0 integrates with Microsoft Visual Studio 2017 to provide UWP apps with translation support, translation file management, and editor tools.
title: 다국어 앱 도구 키트 사용
template: detail.hbs
ms.author: stwhi
ms.date: 01/23/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 세계화, 현지화, 지역화
ms.localizationpriority: medium
ms.openlocfilehash: 39e002247cabb6389ddf23860499ebf1f166b03a
ms.sourcegitcommit: 1938851dc132c60348f9722daf994b86f2ead09e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/03/2018
ms.locfileid: "4267214"
---
# <a name="use-the-multilingual-app-toolkit-40"></a>다국어 앱 도구 키트 4.0 사용

MAT(다국어 앱 도구 키트) 4.0은 UWP 앱에 번역 지원, 번역 파일 관리 및 편집기 도구를 제공하도록 Microsoft Visual Studio 2017과 통합되어 있습니다. 도구 키트에 대해 제안하는 몇 가지 값은 다음과 같습니다.

- 개발 중에 리소스 변경 사항 및 변환 상태를 관리할 수 있습니다.
- 구성된 번역 제공자를 기반으로 언어를 선택할 UI를 제공합니다.
- 지역화 산업 표준 XLIFF 파일 형식을 지원합니다.
- 개발 중에 번역 문제를 확인하는 데 도움이 되도록 의사 언어 엔진을 제공합니다.
- Microsoft 언어 포털에 연결하여 번역된 문자열 및 용어에 쉽게 액세스합니다.
- Microsoft Translator에 연결하여 빠르게 번역 제안을 제공 받습니다.

## <a name="how-to-use-the-toolkit"></a>도구 키트를 사용하는 방법

### <a name="step-1-design-your-app-for-globalization-and-localization"></a>1단계. 세계화 및 지역화를 위한 앱 디자인

MAT를 효과적으로 사용하려면 먼저 앱을 지역화할 수 있어야 합니다. 특히, 프로젝트에 앱 문자열이 기본 언어로 포함된 하나 이상의 리소스 파일이 들어 있어야 합니다. 자세한 내용은 [UI와 앱 패키지 매니페스트에 문자열 지역화](../../app-resources/localize-strings-ui-manifest.md)를 참조하세요. 작업을 수행하고 나면 도구 키트를 통해 추가 언어를 쉽고 빠르게 추가할 수 있습니다.

**세계화**, **현지화** 및 **지역화**라는 용어의 정의뿐만 아니라 세계화 및 지역화에 대한 가치 제안에 대한 자세한 내용은 [세계화 및 지역화](globalizing-portal.md)를 참조하세요.

[세계화 지침](guidelines-and-checklist-for-globalizing-your-app.md) 및 [자신의 앱을 현지화 가능하도록 만들기](prepare-your-app-for-localization.md)를 참조하세요.

### <a name="step-2-download-and-install-the-multilingual-app-toolkit-40"></a>2단계. 다국어 앱 도구 키트 4.0을 다운로드하여 설치합니다.

MAT 4.0(다국어 앱 도구 키트 4.0)에는 두 가지 부분이 있으며 각 부분에는 고유한 설치 프로그램이 포함되어 있습니다.

- [Visual Studio 2017용 다국어 앱 도구 키트 4.0 확장 프로그램](https://marketplace.visualstudio.com/items?itemName=MultilingualAppToolkit.MultilingualAppToolkit-18308). 여기에는 .vsix 형식의 설치 프로그램으로 Visual Studio 2017용 MAT 4.0 확장 프로그램이 포함됩니다.
- [다국어 앱 도구 키트 4.0 편집기](https://developer.microsoft.com/en-us/windows/develop/multilingual-app-toolkit). 여기에는 .msi 설치 프로그램 형식의 MAT 4.0 독립 실행형 다국어 편집기 도구가 포함됩니다. Visual Studio 2015 및 Visual Studio 2013용 MAT 4.0 확장 프로그램도 포함됩니다.

Visual Studio 2017을 사용하는 경우 두 설치 프로그램을 차례로 다운로드하여 실행합니다. Visual Studio 2015 또는 Visual Studio 2013을 사용하는 경우 .msi 설치 프로그램을 다운로드하여 실행합니다.

### <a name="step-3-enable-the-multilingual-app-toolkit-for-your-project"></a>3단계. 프로젝트에 다국어 앱 도구 키트를 사용하도록 설정

앱 지역화를 시작하려면 먼저 프로젝트에 MAT를 사용하도록 설정해야 합니다. 도구 키트를 사용하도록 설정하는 방법은 다음과 같습니다.

- Visual Studio에서 프로젝트 솔루션을 엽니다.
- 솔루션 탐색기에서 원하는 프로젝트를 선택합니다.
- **도구** 메뉴에서 **다국어 앱 도구 키트** > **선택 사용**을 선택합니다. 

다국어 앱 도구 키트에서 출력을 표시하는 출력 창에서 `Project '<project-name>' was enabled. The project's source culture is '<language-tag>' <language-name>`라는 메시지가 나타나는지 봅니다. 이 메시지가 표시되면 MAT를 사용할 준비가 된 것입니다.

### <a name="step-4-add-languages-to-your-project"></a>4단계. 프로젝트에 언어 추가

다음 단계에 따라 프로젝트에 언어를 추가합니다.

1. 솔루션 탐색기에서 프로젝트 노드를 마우스 오른쪽 단추로 클릭합니다.
2. **다국어 앱 도구 키트** > **번역 언어 추가...** 를 클릭합니다.
3. 번역 언어 대화 상자에서 지원할 언어를 선택하고 확인을 클릭합니다.

이에 대한 응답으로 도구 키트에서는 다음과 같은 작업을 수행합니다.

- 추가한 각 언어에 대해 해당 언어의 [BCP-47 언어 태그](http://go.microsoft.com/fwlink/p/?linkid=227302) 이름의 새로운 폴더가 생성됩니다. 해당 폴더 내에 기본 언어 문자열을 포함하는 파일과 일치하도록 새 리소스 파일(.resw)이 생성됩니다.
- 언어를 처음 추가한 경우 `MultilingualResources`라는 이름의 새 폴더가 프로젝트에 추가됩니다. 해당 폴더 내에 각 언어마다 .xlf 파일이 추가됩니다. .Xlf 파일은 프로젝트의 각 리소스 파일(.resw)에 포함된 각 문자열의 번역 단위를 포함합니다.
- 출력 창에서는 추가한 언어가 추가되었음을 확인할 수 있습니다.

기본 언어 리소스 파일(.resw)을 추가/제거하거나 기본 언어 리소스 파일(.resw) 내에서 문자를 추가/제거할 때마다 .xlf 파일을 다시 동기화하도록 프로젝트를 다시 생성합니다. 이렇게 하면 .xlf 파일에 문자열 집합이 기본 언어로 포함됩니다.

[Microsoft 언어 포털](http://go.microsoft.com/fwlink/p/?LinkId=330295) 및 [Microsoft Translator](http://go.microsoft.com/fwlink/p/?LinkId=258220)와 같은 설치된 번역 제공자를 사용하여 앱 리소스를 번역할 수 있습니다. 공급자가 특정 언어를 지원하는 경우 공급장의 아이콘이 번역 언어 대화 상자의 언어 이름 옆에 표시됩니다.

번역 언어 대화 상자에서 도구 키트가 발견한 기존 .xlf 기반 언어는 프로젝트에 이미 포함되어 있음을 나타내도록 선택 상자가 미리 체크됩니다.

언어가 프로젝트에 추가되고 나면 번역 언어 대화 상자에서 상자의 표시를 해제하는 방식으로 언어를 제거할 수 없습니다. 언어를 제거하려면 언어별 .xlf 파일을 마우스 오른쪽 단추로 클릭하고 **삭제**를 선택합니다. 확인하고 나면 해당 리소스 파일(.resw) 도 삭제됩니다.

### <a name="step-5-test-your-app-using-pseudo-language"></a>5단계. 의사 언어를 사용하여 앱 테스트

의사 언어는 실제 언어 지역화를 시뮬레이션하기 위해 인공적으로 소프트웨어 제품을 수정하지만 모국어 사용자도 여전히 읽을 수 있습니다. 의사 번역은 문자를 교체하고 리소스 문자열 길이를 확장하여 프로젝트 주기 초반 및 지역화를 시작하기 전에 잠재적인 지역화 문제 또는 버그를 감지합니다.

다음 단계에 따라 프로젝트를 의사 지역화하고 테스트합니다.

1. 번역 언어 대화 상자를 사용하여 프로젝트에 의사 언어(Pseudo) [qps-ploc]를 추가합니다.
2. 솔루션 탐색기에서 `<project-name>.qps-ploc.xlf` 파일을 마우스 오른쪽 단추로 클릭하고 **다국어 앱 도구 키트** > **기계 번역 생성**을 클릭합니다.
3. **설정** > **시간 및 언어** > **지역 및 언어** > **언어**에서 **언어 추가**를 클릭합니다.
5. 검색 상자에 `qps-ploc`을 입력합니다.
6. `English (qps-ploc)`을 클릭하여 추가합니다.
7. 언어 목록에서 `English (qps-ploc)`을 선택하고 **기본으로 설정**을 클릭합니다.
8. 의사 지역화된 앱을 테스트합니다. 예를 들어, 문자열의 일부가 표시되지 않거나(문자열이 잘림) 문자열이 번역되지 않은 경우(대신 하드 코딩됨)와 같은 레이아웃 문제를 찾습니다.

문자 교체 및 확장과 함께 의사 엔진은 각 리소스에 고유한 추적 ID를 제공합니다. 이 추적기는 모든 문자열 시작 부분 앞에 추가되며 괄호 `[xxxxx]` 안에 들어갑니다. 시각적 UI 검사 테스트를 위해 이러한 추적기를 사용할 수 있습니다. 특히 여러 리소스에 유사한 텍스트 및 중복 텍스트가 들어 있는 경우 제품에서 특정 리소스를 추적할 수도 있습니다.

이번 "Hello, World!" 텍스트 예제에서는 의사 번역이 화면 공간보다 30% 더 많이 차지하도록 확장되고 리소스 추적기가 적용됩니다.

```
"Hello World" -> "Ĥèĺļõ Ŵòŗłđ" -> "[!!_Ĥèĺļõ Ŵòŗłđ_!!]" -> "[hJ8s1][!!_Ĥèĺļõ Ŵòŗłđ_!!]"
```

### <a name="step-6-translate-your-app-into-selected-languages"></a>6단계. 앱을 선택한 언어로 번역

다국어 앱 도구 키트는 빌드 과정에서 통합됩니다. 빌드 과정에서 업데이트된 문자열은 각 언어의 .xlf 파일에 자동으로 추가됩니다.
의사 언어를 사용하여 앱을 테스트하고 나면 출시를 위해 앱을 다른 언어로 번역하는 3가지 옵션이 있습니다.

#### <a name="option-1-translate-the-strings-yourself"></a>옵션 1. 문자열을 스스로 번역

다국어 편집기를 사용하여 문자열을 각각 번역할 수 있습니다. 이미 언급했듯이 편집기는 [.msi 설치 프로그램](https://developer.microsoft.com/en-us/windows/develop/multilingual-app-toolkit)에 포함됩니다.

- 번역하려는 .xlf 파일을 마우스 오른쪽 단추로 클릭합니다.
- **다른 프로그램으로 열기...** 를 클릭한 후 다국어 편집기를 선택합니다. 선택적으로 **기본으로 설정**을 클릭할 수 있습니다.
- 각 문자열에 대해 **소스**는 기존 문자열을 기본 언어로 표시합니다. **Translation**에 편집 중인 .xlf 파일에 대한 적절한 언어로 번역된 문자열을 입력합니다.
- 완료하면 파일을 저장하고 닫습니다.

방금 편집한 .xlf 파일에 상응하는 번역된 문자열이 리소스 파일(.resw)에 복사되도록 프로젝트를 다시 빌드합니다.

다음과 같이 다국어 편집기를 시작할 수도 있습니다. 시작으로 이동하여 모든 앱을 표시하고 다국어 앱 도구 키트 폴더를 열고 다국어 편집을 클릭하여 시작합니다.

#### <a name="option-2-send-the-xlf-files-to-a-third-party-for-translation"></a>옵션 2. 번역을 위해 .xlf 파일을 타사로 전송

번역 및 편집 작업을 지역화 업체로 아웃소싱하려면 솔루션 탐색기에서 .xlf 파일을 선택하고 마우스 오른쪽 단추로 클릭한 후 **다국어 앱 도구 키트** > **번역 내보내기...** 를 클릭합니다.

기존 문자열 리소스 대화 상자에서 **출력: 메일 수신자**를 선택하고 확인을 클릭하면 파일이 압축되어 새 이메일에 첨부됩니다. 폴더에 사용할 **출력: 파일 폴더 위치** 브라우저를 택하고 확인을 클릭하고 선택적으로 압축할 파일에 대해 선택한 후 다시 확인을 클릭합니다. 그러면 파일이 압축되고 선택한 위치(프로젝트에 대해 이름이 지정된 새 폴더 내부)에 저장됩니다.

지역화 업체에서 번역 작업을 완료하고 번역된 .xlf 파일을 사용자에게 전송하면 사용자는 이 파일을 프로젝트에 가져올 수 있습니다. 솔루션 탐색기에서 원하는 .xlf 파일을 선택하고 마우스 오른쪽 단추로 이 파일을 클릭한 다음 **다국어 앱 도구 키트** > **번역 가져오기/재사용...** 을 클릭합니다. **추가**를 클릭하고 .xlf 또는 .zip 파일을 탐색한 후 **가져오기**를 클릭합니다.

**참고** 가져오기 과정에서 가져오기 전에 기본적인 유효성 검사가 실행됩니다. 이렇게 하면 기존 .xlf 파일에서와 일치하도록 파일의 대상 문화권 정보를 가져올 수 있습니다.

방금 가져온 .xlf 파일에 상응하는 번역된 문자열이 리소스 파일(.resw)에 복사되도록 프로젝트를 다시 빌드합니다.

다음 타사 공급자는 지역화 서비스를 제공하며 도움을 제공할 수 있습니다.

- [Elanex](https://www.elanex.com/)
- [Keywords Studios](https://www.keywordsstudios.com/)
- [Lionbridge](https://www.lionbridge.com)
- [Moravia](https://www.moravia.com/)
- [SDL](https://www.sdl.com/languagecloud/managed-translation/ilp/instantquote)
- [Welocalize](https://www.welocalize.com/)

> [!NOTE]
> 위 목록은 정보 제공의 목적으로만 제공되며 홍보가 아닙니다. Microsoft는 이러한 공급업체 또는 해당 서비스와 관련하여 어떠한 진술이나 보증도 하지 않으며, 어떤 상황에서도 Microsoft는 이러한 공급업체 또는 서비스의 사용에 대해 책임지지 않습니다. 이러한 공급업체 또는 해당 서비스와 관련한 모든 질문, 불만 또는 청구는 해당 공급업체에 문의해야 합니다.

#### <a name="option-3-use-the-integrated-translation-services"></a>옵션 3. 통합된 번역 서비스 사용

번역 서비스는 다국어 편집기뿐만 아니라 Visual Studio IDE에도 통합되어 있습니다. 이를 통해 제품을 개발하고 리소스를 지역화하는 동안 번역 서비스에 쉽게 액세스할 수 있습니다. 이 서비스를 사용하려면 [Microsoft Translator가 Azure Portal로 이동됨](https://multilingualapptoolkit.uservoice.com/knowledgebase/articles/1167898-microsoft-translator-moves-to-the-azure-portal)에 설명된 대로 Zure 계정 구독이 필요하게 됩니다.

Visual Studio 내의 번역 서비스에 액세스하려면 솔루션 탐색기에서 하나 이상의 .xlf 파일을 선택하고 마우스 오른쪽 단추로 클릭한 후 **기계 번역 생성**을 클릭합니다.

다국어 편집기는 대화형 번역 제안을 추가할 뿐만 아니라 동일한 번역 지원을 제공합니다. 이를 통해 리소스 문자열에 가장 적합한 번역을 선택할 수 있도록 해줍니다. 번역 제안이 제공되면 번역 스타일에 맞게 문자열을 미세하게 조정할 수 있습니다.

2개의 공급자가 다국어 앱 도구 키트에 함께 제공됩니다.

- [Microsoft 언어 포털](http://go.microsoft.com/fwlink/p/?LinkId=330295) 공급자를 통해 Microsoft 제품 및 서비스의 사용자 인터페이스 텍스트 번역을 기반으로 번역을 재사용하고 용어와 일치하는 지원을 받을 수 있습니다.
- [Microsoft Translator](http://go.microsoft.com/fwlink/p/?LinkId=258220) 공급자를 통해 주문형 기계 번역 서비스를 사용할 수 있습니다.

사용자 및 사용자의 번역자는 다국어 편집기에서 번역 상태를 관리하여 불확실한 번역을 나중에 검토할 수 있습니다. 각 문자열을 상태를 **속성** 탭에서 설정할 수 있습니다. 상태 값으로는 **새 문자열**, **검토 필요**, **번역됨**, **최종** 및 **로그오프됨**이 있습니다. 행 왼쪽의 표시기는 상태를 나타냅니다. 다국어 편집기에서 모든 행이 녹색으로 표시되면 번역 작업이 완료된 것입니다.

방금 편집한 .xlf 파일에 상응하는 번역된 문자열이 리소스 파일(.resw)에 복사되도록 프로젝트를 다시 빌드합니다.

### <a name="step-7-upload-your-app-to-the-microsoft-store"></a>7단계. Microsoft Store에 앱 업로드

Microsoft Store 인증 과정을 시작하기 전에 프로젝트에서 `<project-name>.qps-ploc.xlf` 파일을 제외해야 합니다. 잠재적인 지역화 문제 또는 버그를 감지하기 위해 의사 언어가 사용되지만 이 언어는 유효한 Microsoft Store 언어가 아닙니다. 이 언어를 제거하지 않으면 Microsoft Store 인증 과정에서 앱이 인증을 통과하지 못하게 됩니다.

## <a name="related-topics"></a>관련 항목

* [UI와 앱 패키지 매니페스트에 문자열 지역화](../../app-resources/localize-strings-ui-manifest.md)
* [세계화 및 지역화](globalizing-portal.md)
* [세계화 지침](guidelines-and-checklist-for-globalizing-your-app.md)
* [자신의 앱을 현지화 가능하도록 만들 수 있습니다.](prepare-your-app-for-localization.md)
* [BCP-47 언어 태그](http://go.microsoft.com/fwlink/p/?linkid=227302)

## <a name="downloads"></a>다운로드

* [다국어 앱 도구 키트 4.0 .vsix 설치 프로그램](https://marketplace.visualstudio.com/items?itemName=MultilingualAppToolkit.MultilingualAppToolkit-18308)
* [다국어 앱 도구 키트 4.0 .msi 설치 프로그램](https://developer.microsoft.com/en-us/windows/develop/multilingual-app-toolkit)

## <a name="translation-services"></a>번역 서비스

* [Microsoft 언어 포털](http://go.microsoft.com/fwlink/p/?LinkId=330295)
* [Microsoft Translator](http://go.microsoft.com/fwlink/p/?LinkId=258220)
