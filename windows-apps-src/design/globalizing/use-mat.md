---
Description: 다국어 앱 도구 키트 (대만) 4.0는 Microsoft Visual Studio 2019과 통합 되어 Windows 앱에 번역 지원, 번역 파일 관리 및 편집기 도구를 제공 합니다.
title: 다국어 앱 도구 키트 사용
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp, 세계화, 지역화 가능성, 지역화
ms.localizationpriority: medium
ms.openlocfilehash: 4759be8b4e386620243cd587df1ac0bd3e6b0033
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/24/2020
ms.locfileid: "91217106"
---
# <a name="use-the-multilingual-app-toolkit-40"></a>다국어 앱 도구 키트 4.0 사용

다국어 앱 도구 키트 (대만) 4.0는 Microsoft Visual Studio 2019과 통합 되어 Windows 앱에 번역 지원, 번역 파일 관리 및 편집기 도구를 제공 합니다. 다음은 toolkit의 가치 제안을 위한 것입니다.

- 개발 중에 리소스 변경 및 변환 상태를 관리 하는 데 도움이 됩니다.
- 구성 된 번역 공급자에 따라 언어를 선택 하기 위한 UI를 제공 합니다.
- 지역화 업계 표준 XLIFF 파일 형식을 지원 합니다.
- 는 개발 중에 변환 문제를 식별 하는 데 도움이 되는 의사 언어 엔진을 제공 합니다.
- Microsoft 언어 포털과 연결 하 여 번역 된 문자열과 용어에 쉽게 액세스할 수 있습니다.
- 빠른 번역 제안을 위해 Microsoft Translator에 연결 합니다.

## <a name="how-to-use-the-toolkit"></a>도구 키트를 사용 하는 방법

### <a name="step-1-design-your-app-for-globalization-and-localization"></a>1단계: 세계화 및 지역화를 위한 앱 디자인

이 경우를 효과적으로 사용 하려면 앱을 지역화할 수 있어야 합니다. 특히, 프로젝트에는 앱의 문자열을 기본 언어로 포함 하는 하나 이상의 리소스 파일 (. resw)이 포함 되어야 합니다. 자세한 내용은 [UI 및 응용 프로그램 패키지 매니페스트에서 문자열 지역화](../../app-resources/localize-strings-ui-manifest.md)를 참조 하세요. 이 작업을 완료 한 후에는 도구 키트를 사용 하 여 더 빠르고 쉽게 언어를 추가할 수 있습니다.

세계화 및 지역화에 대 한 가치를 &mdash; 비롯 하 **여 세계화**, 지역화 **가능성**및 **지역화**용어에 대 한 정의는 &mdash; [세계화 및 지역화](globalizing-portal.md)를 참조 하세요.

또한 [세계화에 대 한 지침](guidelines-and-checklist-for-globalizing-your-app.md) 을 참조 하 여 [앱을 지역화할 수 있도록](prepare-your-app-for-localization.md)합니다.

### <a name="step-2-download-and-install-the-multilingual-app-toolkit-40"></a>2단계. 다국어 앱 도구 키트 4.0 다운로드 및 설치

다국어 앱 도구 키트 4.0 4.0 (대만)에는 두 부분이 있습니다. 각 구성 요소에는 고유한 설치 관리자가 있습니다.

- [Visual Studio 2017 이상용 다국어 앱 도구 키트 4.0 확장](https://marketplace.visualstudio.com/items?itemName=MultilingualAppToolkit.MultilingualAppToolkit-18308) 여기에는 .vsix 설치 관리자의 형태로 Visual Studio 2019의 대/a 4.0 확장이 포함 됩니다.
- [다국어 앱 도구 키트 4.0 편집기](https://developer.microsoft.com/windows/develop/multilingual-app-toolkit). 여기에는 .msi 설치 관리자 형식의 대/a 4.0 독립 실행형 다국어 편집기 도구가 포함 됩니다. 또한 Visual Studio 2015 및 Visual Studio 2013에 대 한 대//도 4.0 확장이 포함 되어 있습니다.

Visual Studio 2017 또는 Visual Studio 2019를 사용 하는 경우 두 설치 관리자를 모두 다운로드 하 여 실행 합니다. Visual Studio 2015 또는 Visual Studio 2013를 사용 하는 경우 .msi 설치 관리자를 다운로드 하 여 실행 합니다.

### <a name="step-3-enable-the-multilingual-app-toolkit-for-your-project"></a>3단계. 프로젝트에 다국어 앱 도구 키트 사용

응용 프로그램 지역화를 시작 하기 전에 프로젝트에 대 한 대/면을 활성화 해야 합니다. 도구 키트를 사용 하도록 설정 하는 방법은 다음과 같습니다.

- Visual Studio에서 프로젝트 솔루션을 엽니다.
- 솔루션 탐색기에서 원하는 프로젝트를 선택 합니다.
- **도구** 메뉴에서 **다국어 앱 도구 키트**  >  **선택 사용**을 선택 합니다. 

출력 창 (다국어 앱 도구 키트의 출력 표시)에서 메시지를 시청 `Project '<project-name>' was enabled. The project's source culture is '<language-tag>' <language-name>` 합니다. 이 메시지가 표시 되는 경우에는 대/면을 사용할 준비가 된 것입니다.

### <a name="step-4-add-languages-to-your-project"></a>4단계. 프로젝트에 언어 추가

프로젝트에 언어를 추가 하려면 다음 단계를 수행 합니다.

1. 솔루션 탐색기에서 프로젝트 노드를 마우스 오른쪽 단추로 클릭합니다.
2. **다국어 앱 도구 키트**  >  **번역 언어 추가**...를 클릭 합니다.
3. 번역 언어 대화 상자에서 지원 하려는 언어를 선택 하 고 확인을 클릭 합니다.

도구 키트는 이러한 항목을 응답으로 수행 합니다.

- 추가한 각 언어에 대해 해당 언어의 [BCP-47 언어 태그](https://tools.ietf.org/html/bcp47) 에 대해 라는 새 폴더가 만들어집니다. 이 폴더 내에는 기본 언어 문자열을 포함 하는 것과 일치 하는 새 리소스 파일 (. resw)이 생성 됩니다.
- 언어를 처음 추가 하는 경우에는 라는 새 폴더가 `MultilingualResources` 프로젝트에 추가 됩니다. 이 폴더 내에는 각 언어에 대 한. xlf 파일이 추가 됩니다. . Xlf 파일에는 프로젝트의 각 리소스 파일 (. resw)의 각 문자열에 대 한 변환 단위가 포함 되어 있습니다.
- 출력 창에는 추가한 언어가 추가 됩니다.

기본 언어 리소스 파일 (.resw)을 추가/제거 하거나 기본 언어 리소스 파일 (. resw) 내에서 문자열을 추가/제거할 때마다 프로젝트를 다시 빌드하여 xlf 파일을 다시 동기화 합니다. 이렇게 하면. xlf 파일에는 기본 언어로 된 문자열의 합집합이 포함 됩니다.

&mdash; [Microsoft 언어 포털](https://www.microsoft.com/Language/) 및 [microsoft Translator](https://www.microsofttranslator.com/)와 같은 설치 된 번역 공급자 &mdash; 를 사용 하 여 앱의 리소스를 번역할 수 있습니다. 공급자가 특정 언어를 지 원하는 경우 번역 언어 대화 상자에서 언어 이름 옆에 공급자의 아이콘이 표시 됩니다.

번역 언어 대화 상자에서 도구 키트에 의해 검색 된 기존의 xlf 기반 언어는 해당 언어에 이미 프로젝트가 포함 되어 있음을 나타내기 위해 미리 선택 상자를 미리 선택 했습니다.

프로젝트에 언어가 추가 되 면 번역 언어 대화 상자에서 확인란을 선택 취소 하 여 제거할 수 없습니다. 언어를 제거 하려면 언어별. xlf 파일을 마우스 오른쪽 단추로 클릭 하 고 **삭제**를 선택 합니다. 확인 하는 경우 해당 리소스 파일 (. resw)도 삭제 됩니다.

### <a name="step-5-test-your-app-using-pseudo-language"></a>5단계. 의사 언어를 사용 하 여 앱 테스트

의사 (Pseudo) 언어는 실제 언어 지역화를 시뮬레이트하는 데 사용 되는 소프트웨어 제품을 인공으로 수정 하는 것으로, 네이티브 스피커에서 읽을 수 있습니다. 의사 번역은 문자를 대체 하 고 리소스 문자열 길이를 확장 하 여 프로젝트 주기 초기에 잠재적 지역화 가능성 문제 또는 버그를 검색 하 고 earnest에서 지역화를 시작 합니다.

프로젝트를 의사 지역화 하 고 테스트 하려면 다음 단계를 수행 합니다.

1. 번역 언어 대화 상자를 사용 하 여 의사 (pseudo) [qps-ploc]를 프로젝트에 추가 합니다.
2. 솔루션 탐색기에서 파일을 마우스 오른쪽 단추로 클릭 `<project-name>.qps-ploc.xlf` 하 고 **다국어 앱 도구 키트**  >  **컴퓨터 번역 생성**을 클릭 합니다.
3. **설정**  >  **시간 &** 언어  >  **지역 &** 언어 언어에서  >  **Languages** **언어 추가**를 클릭 합니다.
5. 검색 상자에서 `qps-ploc`를 입력합니다.
6. 클릭 `English (qps-ploc)` 하 여 추가 합니다.
7. 언어 목록에서을 선택 `English (qps-ploc)` 하 고 **기본값으로 설정**을 클릭 합니다.
8. 의사 지역화 된 앱을 테스트 합니다. 예를 들어 문자열이 모두 표시 되지 않는 (문자열이 잘린 경우) 또는 변환 되지 않은 문자열 (대신 하드 코드 됨) 인 UI 레이아웃 문제를 찾습니다.

의사 (pseudo) 엔진은 문자 교체 및 확장 외에도 각 리소스에 대해 고유한 추적 식별자를 제공 합니다. 이 추적기는 모든 문자열의 시작 부분 앞에 괄호 안에 포함 되어 `[xxxxx]` 있습니다. 시각적 UI 검사 테스트 중에 이러한 추적기를 사용할 수 있습니다. 특히 여러 리소스에 비슷하거나 중복 된 텍스트가 있는 경우 제품의 특정 리소스를 추적 하는 데 도움이 될 수 있습니다.

이 "Hello, 세계!"에서 텍스트 예를 들어 의사 번역이 확장 되어 약 30% 더 많은 화면 공간을 차지 하 고 리소스 추적기를 적용 합니다.

`"Hello World" -> "Ĥèĺļõ Ŵòŗłđ" -> "[!!_Ĥèĺļõ Ŵòŗłđ_!!]" -> "[hJ8s1][!!_Ĥèĺļõ Ŵòŗłđ_!!]"`

### <a name="step-6-translate-your-app-into-selected-languages"></a>6단계. 앱을 선택한 언어로 번역

다국어 앱 도구 키트는 빌드 프로세스에 통합 되어 있습니다. 빌드 중에 업데이트 된 문자열이 각 언어의 xlf 파일에 자동으로 추가 됩니다.
의사 언어를 사용 하 여 앱을 테스트 한 후에는 릴리스를 위해 앱을 다른 언어로 변환 하는 세 가지 옵션이 있습니다.

#### <a name="option-1-translate-the-strings-yourself"></a>옵션 1. 직접 문자열 변환

다국어 편집기를 사용 하 여 문자열을 개별적으로 변환할 수 있습니다. 이미 언급 했 듯이이는 [.msi 설치 관리자](https://developer.microsoft.com/windows/develop/multilingual-app-toolkit)에 포함 되어 있습니다.

- 변환할 xlf 파일을 마우스 오른쪽 단추로 클릭 합니다.
- **연결 프로그램** ...을 클릭 하 고 다국어 편집기를 선택 합니다. 필요에 따라 **기본값으로 설정**을 클릭할 수 있습니다.
- **원본** 에서 각 문자열에 대해 원본 문자열을 기본 언어로 표시 합니다. **변환**에서 편집 중인. xlf 파일의 해당 언어로 번역 된 문자열을 입력 합니다.
- 완료 되 면 파일을 저장 하 고 닫습니다.

프로젝트를 다시 빌드하여 변환 된 문자열이 방금 편집한. a. i f 파일에 해당 하는 리소스 파일 (. resw)에 복사 됩니다.

이와 같은 다국어 편집기를 시작할 수도 있습니다. 시작, 모든 앱 표시로 이동 하 고 다국어 앱 도구 키트 폴더를 연 다음 다국어 편집기를 클릭 하 여 시작 합니다.

#### <a name="option-2-send-the-xlf-files-to-a-third-party-for-translation"></a>옵션 2. 변환을 위해. a 3 파티에 xlf 파일 보내기

번역을 아웃소싱 하 고 지역화 작업을 편집 하려면 솔루션 탐색기에서 원하는 xlf 파일을 선택 하 고 마우스 오른쪽 단추를 클릭 한 다음 **다국어 앱 도구 키트**  >  **내보내기 번역 ...** 을 클릭 합니다.

문자열 리소스 내보내기 대화 상자에서 **출력: 메일 받는 사람** 을 선택 하 고 확인을 클릭 하면 파일이 압축 되어 새 전자 메일에 첨부 됩니다. **출력: 파일 폴더 위치**, 폴더에 대 한 브라우저를 선택 하 고 확인을 클릭 한 다음 원하는 파일을 선택 합니다 .를 선택 하 고 확인을 다시 클릭 합니다. 그러면 사용자가 선택한 위치에서 프로젝트에 대해 이름이 지정 된 새 폴더 안에 파일 (zip 및)이 저장 됩니다.

지역화 담당자가 번역 작업을 완료 하 고 번역 된 xlf 파일을 보내면 프로젝트로 가져올 수 있습니다. 솔루션 탐색기에서 원하는 xlf 파일을 선택 하 고 마우스 오른쪽 단추를 클릭 한 다음 **다국어 앱 도구 키트**  >  **가져오기/재생 번역 ...** 을 클릭 합니다. **추가**를 클릭 하 고. xlf 또는 .zip 파일로 이동 하 고 **가져오기**를 클릭 합니다.

**참고** 가져오기 프로세스는 가져오기 전에 기본 유효성 검사를 수행 합니다. 이렇게 하면 가져오는 파일의 대상 문화권 정보가 기존 xlf 파일의 대상 문화권 정보와 일치 하 게 됩니다.

번역 된 문자열을 방금 가져온 xlf 파일에 해당 하는 리소스 파일 (. resw)에 복사 하도록 프로젝트를 다시 빌드합니다.

이러한 타사 공급자는 지역화 서비스를 제공 하며이를 지원할 수 있습니다.

- [Elanex](https://www.strakertranslations.com/)
- [스튜디오 키워드](https://www.keywordsstudios.com/)
- [Lionbridge](https://www.lionbridge.com)
- [Moravia](https://www.rws.com/what-we-do/rws-moravia/)
- [SDL](https://www.sdl.com/translate/get-started/instant-quote.html)
- [Welocalize](https://www.welocalize.com/)

> [!NOTE]
> 위의 목록은 정보 제공 용 으로만 제공 되며 인증 되지 않습니다. Microsoft는 이러한 공급 업체 또는 서비스와 관련 하 여 어떠한 표현 또는 보증도 하지 않으며, 어떤 경우에도 Microsoft는 이러한 공급 업체 또는 서비스 사용에 대 한 책임을 지지 않습니다. 이러한 공급 업체 또는 서비스와 관련 된 질문, 불만 또는 클레임은 적절 한 공급 업체에 문의 해야 합니다.

#### <a name="option-3-use-the-integrated-translation-services"></a>옵션 3. 통합 번역 서비스 사용

번역 서비스는 다국어 편집기 뿐만 아니라 Visual Studio IDE에 통합 됩니다. 이를 통해 제품을 개발 하 고 리소스를 지역화 하는 동안 번역 서비스에 쉽게 액세스할 수 있습니다. 이 서비스의 경우 [Microsoft Translator에서 Azure Portal로 이동](https://multilingualapptoolkit.uservoice.com/knowledgebase/articles/1167898-microsoft-translator-moves-to-the-azure-portal)에 설명 된 대로 Azure 계정 구독이 필요 합니다.

Visual Studio 내에서 변환 서비스에 액세스 하려면 솔루션 탐색기에서 하나 이상의. xlf 파일을 선택 하 고 마우스 오른쪽 단추로 클릭 한 다음 **컴퓨터 번역 생성**을 클릭 합니다.

다국어 편집기는 동일한 번역 지원을 제공 하 고 대화형 번역 제안을 추가 하 여 리소스 문자열에 가장 적합 한 번역을 선택할 수 있습니다. 번역 제안이 제공 된 후 번역 스타일에 대 한 문자열을 세밀 하 게 조정할 수 있습니다.

두 공급자가 다국어 앱 도구 키트와 함께 제공 됩니다.

- Microsoft [언어 포털](https://www.microsoft.com/Language/) 공급자는 microsoft 제품 및 서비스에 대 한 사용자 인터페이스 텍스트의 번역에 따라 번역 재생 및 용어 일치 지원을 사용할 수 있도록 합니다.
- [Microsoft Translator](https://www.microsofttranslator.com/) 공급자는 주문형 기계 번역 서비스를 사용 하도록 설정 합니다.

사용자 및 번역기는 다국어 편집기에서 번역 상태를 관리 하 여 나중에 불확실 한 번역을 검토할 수 있습니다. **속성** 탭에서 각 문자열의 상태를 설정할 수 있습니다. 상태 값은 **신규**, 검토, **번역**, **최종**및 **로그 오프** **필요**입니다. 행의 왼쪽에 있는 표시기는 상태를 표시 합니다. 다국어 편집기에서 모든 행이 녹색으로 표시 되 면 변환 작업이 완료 됩니다.

번역 된 문자열이 방금 편집한. a s f 파일에 해당 하는 리소스 파일 (. resw)에 복사 되도록 프로젝트를 다시 빌드합니다.

### <a name="step-7-upload-your-app-to-the-microsoft-store"></a>7단계. Microsoft Store에 앱 업로드

Microsoft Store 인증 프로세스를 시작 하기 전에 프로젝트에서 파일을 제외 해야 합니다 `<project-name>.qps-ploc.xlf` . 의사 (Pseudo) 언어는 잠재적 지역화 가능성 문제 또는 버그를 검색 하는 데 사용 되지만 올바른 Microsoft Store 언어가 아닙니다. 제거 되지 않은 경우 Microsoft Store 인증 프로세스 중에 앱이 실패 합니다.

## <a name="related-topics"></a>관련 항목

* [UI 및 앱 패키지 매니페스트의 문자열 지역화](../../app-resources/localize-strings-ui-manifest.md)
* [세계화 및 지역화](globalizing-portal.md)
* [세계화 지침](guidelines-and-checklist-for-globalizing-your-app.md)
* [앱을 현지화 가능하게 만들기](prepare-your-app-for-localization.md)
* [BCP-47 언어 태그](https://tools.ietf.org/html/bcp47)

## <a name="downloads"></a>다운로드

* [다국어 앱 도구 키트 4.0. vsix 설치 관리자](https://marketplace.visualstudio.com/items?itemName=MultilingualAppToolkit.MultilingualAppToolkit-18308)
* [다국어 앱 도구 키트 4.0 .msi 설치 관리자](https://developer.microsoft.com/windows/develop/multilingual-app-toolkit)

## <a name="translation-services"></a>번역 서비스

* [Microsoft 언어 포털](https://www.microsoft.com/Language/)
* [Microsoft Translator](https://www.microsofttranslator.com/)
