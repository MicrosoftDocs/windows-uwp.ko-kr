---
Description: This topic provides answers to frequently-asked questions and issues related to the Multilingual App Toolkit (MAT) 4.0.
title: 다국어 앱 도구 키트 FAQ 및 문제 해결
template: detail.hbs
ms.date: 11/13/2017
ms.topic: article
keywords: windows 10, uwp, 세계화, 현지화, 지역화
ms.localizationpriority: medium
ms.openlocfilehash: a39d2b3133714ab784309e131a71219beae4e3c0
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "8800713"
---
# <a name="multilingual-app-toolkit-40-faq--troubleshooting"></a>다국어 앱 도구 키트 4.0 FAQ 및 문제 해결

이 항목에는 자주 묻는 질문과 대답, 다국어 앱 도구 키트(MAT) 4.0과 관련된 문제가 나와 있습니다.

[다국어 앱 도구 키트 4.0 사용](use-mat.md)도 참조하세요.

**참고** 도구 키트는 .resw (XAML) 및 .resjson (JavaScript) 파일을 모두 지원합니다. 그러나 이 항목에서는 .resw 파일만 언급합니다. .resw 파일은 리소스 파일이라고 합니다. 여기에는 기본 언어로 또는 다른 언어로 번역된 형태의 문자열이 포함됩니다. .resw 파일이 포함된 폴더는 일반적으로 언어 태그 값에 따라 이름이 지정됩니다.

## <a name="do-i-need-resw-files-in-multiple-languages"></a>여러 언어로 .resw 파일이 필요한가요?

아니요. 도구 키트의 주요 이점 중 하나는 여러 언어로 된 .resw 파일이 필요하지 않다는 점입니다. 도구 키트는 .xlf 파일을 사용하여 앱 리소스를 관리하고 동기화합니다. 이를 통해 여러 .resw 파일 간에 콘텐츠 동기화 유지와 관련된 문제를 제거할 수 있습니다.

일치하는 .resw 및 .xlf 파일이 포함된 프로젝트의 경우 .xlf 파일의 번역이 무시됩니다. 이 경우 .xlf 번역이 최종 앱에 포함되어 있지 않음을 알려주는 경고가 빌드하는 동안 표시됩니다. 같은 언어 코드를 사용하는 대상 언어가 있는 경우 .Resw 파일과.xlf 파일이 일치합니다. 일치하는 쌍의 예로는 `Strings\de-DE\Resources.resw` 및 `<project-name>.de-DE.xlf` 파일(`target-language="de-DE"` 포함)이 있습니다.

## <a name="can-i-have-resw-files-in-multiple-languages"></a>여러 언어로 .resw 파일을 가질 수 있나요?

가능하지만 권장하지는 않습니다. 프로젝트에서 여러 언어로 된 .resw 파일을 포함하고 도구 키트를 사용하려면 일치하는 .resw 및 .xlf 파일이 없어야 합니다.

## <a name="i-dont-see-an-option-in-the-tools-menu-to-enable-the-multilingual-app-toolkit"></a>다국어 앱 도구 키트를 사용하도록 설정하기 위한 옵션이 도구 메뉴가 없습니다.

다음 단계를 시도해 보세요.

- **도구** 메뉴를 열기 전에 솔루션 노드가 아닌 프로젝트 노드를 선택해야 합니다.
- 도구 키트 확장 프로그램이 Visual Studio 확장 관리자를 사용하여 설치되었는지 확인합니다.
- UWP 프로젝트인지 확인합니다.

## <a name="when-i-build-my-project-i-dont-see-a-message-saying-that-a-multilingual-app-toolkit-build-has-started"></a>프로젝트를 빌드할 때 다국어 앱 도구 키트 빌드가 시작되었다는 메시지가 표시되지 않습니다.

프로젝트에 MAT를 사용하도록 설정했는지 확인합니다. **도구** 메뉴에서 **다국어 앱 도구 키트** > **선택 사용**을 선택합니다. 이전 버전으로 프로젝트가 활성화된 경우 **도구** 메뉴를 사용하여 MAT를 비활성화한 후 다시 활성화합니다. 이렇게 하면 새로운 도구 키트 버전으로 작동하도록 프로젝트가 업데이트됩니다.

"모든 Visual Studio 버전을 위한 빌드 작업" 구성 요소가 설치되었는지 확인합니다. 이 빌드 구성 요소는 확장 프로그램과 함께 설치되지만 설치하는 동안 수동으로 선택을 해제할 수 있습니다. 이 구성 요소는 .xlf 파일을 업데이트하고 PRI 파일에 번역을 추가할 때 필요합니다. 이 구성 요소가 설치되어 올바르게 작동 중인 경우 다음과 같은 빌드 메시지가 표시됩니다.

```dosbatch
1> Multilingual App Toolkit build started.
1> Multilingual App Toolkit build completed successfully.
```

## <a name="the-toolkit-is-reporting-that-it-didnt-locate-any-xliff-language-files-during-the-build"></a>도구 키트에서 빌드하는 동안 XLIFF 언어 파일을 찾지 못했다고 보고합니다.

```dosbatch
No XLIFF language files were found. The app will not contain any localized resources.
```

이 메시지는 도구 키트에서 .xlf라는 확장자의 파일을 프로젝트에서 찾지 못할 때 표시됩니다. 도구 키트는 기본적으로 이러한 파일을 생성하여 `MultilingualResources`에 저장합니다. 이 파일을 이동할 수 있지만 해당 폴더에 두면 다국어 편집기에서 관련 메타데이터 파일을 찾을 수 있기 때문에 해당 폴더에 두는 것이 가장 좋습니다.

## <a name="my-xlf-file-is-not-included-in-the-list-of-files-processed-by-the-toolkit-during-build"></a>.xlf 파일이 빌드 시 도구 키트에서 처리된 파일 목록에 포함되어 있지 않습니다.

프로젝트에서 .xlf 파일을 수동으로 제외한 후 다시 포함한 경우 파일 형식 요소가 올바르게 설정되지 않을 수 있습니다. Visual Studio에서 파일을 선택하고 속성 창을 확인합니다. 해당 파일의 "빌드 작업"을 "XliffResource"로 설정하고 "출력 디렉터리로 복사"를 "복사 안 함"으로 설정해야 합니다. 프로젝트 파일에 참조가 다음과 같이 표시되어야 합니다.

```xml
<XliffResource Include="MultilingualResources\<project-name>.fr-FR.xlf" />
```

## <a name="ive-added-xlf-based-languages-where-are-my-strings"></a>.Xlf 기반 언어를 추가했습니다. 내 문자열은 어디에 있나요?

기본 언어 리소스 파일(.resw)은 앱에서 사용하는 문자열의 정식 "스키마"입니다. 번역된 리소스 파일에는 이러한 문자열 전체 또는 하위 집합이 포함될 수 있습니다.

프로젝트를 만들 때 리소스 파일 및 .xlf 파일이 동기화됩니다.

- 추가되었거나 제거된 문자열 또는 추가되었거나 제거된 리소스 파일이 반영되도록 .xlf 파일이 업데이트됩니다.
- .xlf 파일에 번역된 문자열이 반영되도록 리소스 파일이 업데이트됩니다.

이러한 내용은 [다국어 앱 도구 키트 4.0 사용](use-mat.md)에 자세히 설명되어 있습니다.

## <a name="when-i-build-my-project-the-xlf-files-remain-empty"></a>프로젝트를 만들 때 .xlf 파일이 계속 비어 있습니다.

MAT를 효과적으로 사용하려면 먼저 앱을 지역화할 수 있어야 합니다. 이러한 내용은 [다국어 앱 도구 키트 4.0 사용](use-mat.md)에 자세히 설명되어 있습니다.

## <a name="what-is-microsoft-translator"></a>Microsoft Translator란 무엇인가요?

Microsoft Translator는 기계 번역을 제공하는 클라우드 기반 서비스입니다. 기계 번역은 합리적인 방식으로 사람의 번역을 얻지 못할 경우 번역을 제공받고자 할 때 이상적입니다. [Microsoft Translator](http://go.microsoft.com/fwlink/p/?LinkId=258220)에서 더 자세히 알아볼 수 있습니다.

도구 키트는 Microsoft Translator 서비스를 사용하여 사용자에게 번역 제안을 제공합니다. Microsoft Translator 아이콘이 번역 언어 대화 상자에 나타날 때 Microsoft Translator에서 지원하는 언어를 확인할 수 있습니다.

다국어 편집기에서 Microsoft Translator를 사용하여 문자열을 선택하고 **번역**을 클릭하여 앱을 빠르게 번역할 수 있습니다.

## <a name="what-is-pseudo-language-and-what-are-pseudo-resource-trackers"></a>의사 언어란 무엇이고, 의사 리소스 추적기란 무엇인가요?

의사 언어는 실제 언어 지역화를 시뮬레이션하기 위해 인공적으로 소프트웨어 제품을 수정하는 것입니다. 의사 언어 및 의사 리소스 추적기에 대한 자세한 내용은 [다국어 앱 도구 키트 4.0 사용](use-mat.md)을 참조하세요.

## <a name="how-do-i-set-my-language-preference-to-pseudo-language-so-that-i-can-test-my-pseudo-locd-strings"></a>의사 지역화된 문자열을 테스트하기 위해 의사 언어를 기본 언어로 설정하려면 어떻게 하나요?

이와 관련된 내용은 [다국어 앱 도구 키트 4.0 사용](use-mat.md)에 설명되어 있습니다.

## <a name="what-kind-of-localizability-issues-can-i-find-using-pseudo-language"></a>의사 언어를 사용하는 동안 발견할 수 있는 지역화 문제에는 무엇이 있나요?

이와 관련된 내용은 [다국어 앱 도구 키트 4.0 사용](use-mat.md)에 설명되어 있습니다.

## <a name="im-not-seeing-any-translations-when-i-launch-my-app-or-my-app-is-only-partially-translated"></a>앱을 시작할 때 번역이 표시되지 않거나 앱의 일부만 번역된 것으로 나타납니다.

다국어 편집기에서 .xlf 파일을 열어 번역이 있는지 확인합니다. 기본 언어 .resw 파일의 문자열이 명시적으로 변경된 경우 해당 번역이 .xlf 파일에서 제거됩니다. 이는 번역이 해당 소스 문자열과 일치하는지 확인하기 위한 것입니다. .xlf 파일의 문자열을 번역하고 기본 언어가 아닌 다른 언어로 된 .resw 파일을 업데이트하도록 다시 생성합니다.

.xlf 파일에서 문자열이 번역되어 있지만 앱에 표시되지 않는 경우 기본 언어가 아닌 다른 언어로 된 .resw 파일을 업데이트하도록 프로젝트를 다시 빌드합니다. Visual Studio는 마지막 빌드 이후에 변경된 파일만 생성하도록 빌드 명령을 최적화합니다.

언어 기본 설정 순서를 확인합니다. 테스트하려는 언어가 **설정**의 언어 기본 설정 목록 맨 위에 있어야 합니다.

## <a name="the-toolkit-is-reporting-error--0x80004004-in-the-build-output"></a>도구 키트에서 빌드 출력에 오류 0x80004004가 보고됩니다.

```dosbatch
Merge of Loc PRI file failed calling makepri.exe: "0x80004004"
```

지역 형식이 도구 키트 빌드 작동과 충돌하는 경우 이 메시지가 표시될 수 있습니다. 이 문제에 대한 해결 방법은 빌드 시 **설정**에서 언어를 en-US로 변경하는 것입니다.


## <a name="the-toolkit-is-reporting-error--0x80004005-in-the-build-output"></a>도구 키트에서 빌드 출력에 오류 0x80004005가 보고됩니다.

```dosbatch
Merge of Loc PRI file failed calling makepri.exe: "0x80004005"
```

.xlf 파일에 지원되지 않는 대상 언어가 포함된 경우 이 메시지가 표시될 수 있습니다. 예를 들어, "zh-cht"는 올바르지 않으며("zh-hant"로 변경), "zh-chs"가 올바릅니다("zh-hans"로 변경).

## <a name="is-there-a-way-to-find-out-more-information-about-the-errors-im-seeing"></a>표시되는 오류에 대한 자세한 내용을 찾아볼 수 있는 방법이 있나요?

예. Visual Studio에서 자세한 로깅을 켤 수 있습니다. **도구** > **옵션** > **프로젝트 및 솔루션** > **빌드 및 실행**을 클릭합니다. **MSBuild 프로젝트 빌드 출력 세부 정보 표시**를 최소에서 보통 이상으로 변경합니다.

명령줄에서 MSBuild를 실행해도 추가 메시지를 생성할 수 있습니다.

```dosbatch
msbuild /t:rebuild <project-name>
```

## <a name="import-translation-failed"></a>번역 가져오기 실패

가져오기 과정에서 가져오기 전에 기본적인 유효성 검사가 실행됩니다. 이렇게 하면 기존 .xlf 파일에서와 일치하도록 파일의 대상 문화권 정보를 가져올 수 있습니다. 다국어 편집기에서 .xlf 파일을 열고 문화권 정보가 일치하는지 확인합니다.

## <a name="what-if-my-translator-doesnt-have-windows-10-andor-visual-studio-andor-the-multilingual-app-toolkit-installed"></a>번역자의 시스템에 Windows 10 및/또는 Visual Studio 및/또는 다국어 앱 도구 키트가 설치되어 있지 않은 경우 어떻게 하나요?

문자열 리소스 내보내기 대화 상자에서 **출력: 메일 수신자**를 선택하면 MAT(다국어 앱 도구 키트) 4.0을 다운로드하고 설치할 수 있는 링크가 이메일에 포함됩니다. 번역자는 시스템에 Windows 10 또는 Visual Studio가 설치되어 있지 않아도 MAT 4.0 독립 실행형 다국어 편집기 도구를 설치할 수 있습니다.

자세한 내용은 [다국어 앱 도구 키트 4.0 사용](use-mat.md)을 참조하세요.

## <a name="what-happened-to-the-markuprulesxml-and-resourceslocksxml-files"></a>`MarkupRules.xml` 및 `ResourcesLocks.xml` 파일은 어떻게 되나요?

다국어 앱 도구 키트 4.0은 비공개 리소스 잠금 파일을 사용하지 않습니다. 대신 XLIFF 1.2 태그 `<mrk>`가 .xlf 파일에 직접 추가되어 기계 번역 시 수정되는 문자열을 식별합니다. 이를 통해 XLIFF 파일이 자체 포함되고 파일 기반 리소스 잠금이 가능합니다.

추가 지원 파일이 필요하지 않으며 파일을 안전하게 삭제할 수 있습니다.

## <a name="what-happened-to-the-tpx-file"></a>.tpx 파일은 어떻게 되나요?

번역을 위해 .xlf 파일을 전송할 때 .tpx 파일은 `MarkupRules.xml` 및 `ResourcesLocks.xml` 파일이 포함되는 쉬운 방법을 제공했습니다. 이 기능을 더 이상 필요하지 않습니다.

가져와야 하는 번역이 .tpx 파일에 있는 경우 .tpx 파일 확장자를 .zip으로 변경하기만 하면 됩니다. 이렇게 하면 파일 탐색기 또는 .zip 호환 도구를 사용하여 콘텐츠를 열고 추출할 수 있습니다.

## <a name="i-think-ive-done-everything-right-but-it-still-isnt-working"></a>전부 올바르게 수행한 것 같은데 여전히 작동하지 않아요.

다음 단계를 시도해 보세요.

1. 다음 방법 중 하나를 사용하여 번역을 추가하는 것은 이미 설명했습니다.
2. .pri 파일을 덤프하여([MakePri.exe 명령줄 옵션](../../app-resources/makepri-exe-command-options.md) 참조) 번역이 .pri 파일에 있는지 확인합니다. 번역이 다음과 같이 언어 코드 및 번역된 값과 함께 나타납니다.
   ```xml
   <Candidate qualifiers="Language-QPS-PLOC" type="String">
       <Value>[!!_Ŝéãřćĥ_!!]</Value>
   </Candidate>
   ```
3. 명령 프롬프트에서 빌드합니다. 표시되는 오류에는 빌드 출력에서 보고되는 것보다 자세한 정보가 나타날 수 있습니다.

## <a name="my-app-failed-certification-to-the-microsoft-store"></a>앱이 Microsoft Store 인증에 실패했습니다.

Microsoft Store 인증 과정을 시작하기 전에 프로젝트에서 `<project-name>.qps-ploc.xlf` 파일을 제외해야 합니다. 잠재적인 지역화 문제 또는 버그를 감지하기 위해 의사 언어가 사용되지만 이 언어는 유효한 Microsoft Store 언어가 아닙니다. 이 언어를 제거하지 않으면 Microsoft Store 인증 과정에서 앱이 인증을 통과하지 못하게 됩니다.

## <a name="related-topics"></a>관련 항목

* [다국어 앱 도구 키트 4.0 사용](use-mat.md)
* [Microsoft Translator](http://go.microsoft.com/fwlink/p/?LinkId=258220)
* [MakePri.exe 명령줄 옵션](../../app-resources/makepri-exe-command-options.md)
