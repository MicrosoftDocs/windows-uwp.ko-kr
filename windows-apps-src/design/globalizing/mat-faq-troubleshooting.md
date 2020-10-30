---
description: 이 항목에서는 다국어 앱 도구 키트 (대만) 4.0에 관련 된 질문과 대답을 제공 합니다.
title: 다국어 앱 도구 키트 FAQ & 문제 해결
template: detail.hbs
ms.date: 11/13/2017
ms.topic: article
keywords: windows 10, uwp, 세계화, 지역화 가능성, 지역화
ms.localizationpriority: medium
ms.openlocfilehash: 86c7805f92adf3551729783e2359c85103a0c13e
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93030756"
---
# <a name="multilingual-app-toolkit-40-faq--troubleshooting"></a>다국어 앱 도구 키트 4.0 FAQ 및 문제 해결

이 항목에서는 다국어 앱 도구 키트 (대만) 4.0에 관련 된 질문과 대답을 제공 합니다.

또한 [다국어 앱 도구 키트 4.0 사용을](use-mat.md)참조 하세요.

**참고** 도구 키트는. resw (XAML) 및. resw (JavaScript) 파일을 모두 지원 합니다. 그러나이 항목에서는. resw 파일만을 참조 합니다. . Resw 파일은 리소스 파일 이라고 합니다. 여기에는 기본 언어 또는 다른 언어로 번역 된 문자열이 포함 됩니다. 일반적으로. resw 파일이 포함 된 폴더의 이름은 언어 태그의 값입니다.

## <a name="do-i-need-resw-files-in-multiple-languages"></a>여러 언어로 파일이 필요 한가요?

아니요. Toolkit의 주요 이점 중 하나는 여러 언어의 파일을 사용할 필요가 없다는 것입니다. 도구 키트는. xlf 파일을 사용 하 여 앱의 리소스를 관리 하 고 동기화 합니다. 이렇게 하면 콘텐츠를 여러. resw 파일 간에 동기화 된 상태로 유지 하는 것과 관련 된 문제를 제거 합니다.

일치 하는. resw 및 xlf 파일을 포함 하는 프로젝트를 수행 하면. xlf 파일의 번역이 무시 됩니다. 이 경우에는 빌드 중에 xlf 번역이 최종 응용 프로그램에 포함 되지 않은 것을 알 수 있도록 경고가 표시 됩니다. 동일한 언어 코드를 사용 하는 대상 언어를 사용 하는 경우에는 resw 파일 및 xlf 파일이 일치 합니다. 일치 하는 쌍의 예는 `Strings\de-DE\Resources.resw` 및 `<project-name>.de-DE.xlf` 파일 (포함 `target-language="de-DE"` )입니다.

## <a name="can-i-have-resw-files-in-multiple-languages"></a>여러 언어의 파일을 사용할 수 있나요?

가능 하지만 권장 하지는 않습니다. 프로젝트에서 여러 언어로 resw 파일을 포함 하 고 도구 키트를 사용 하려는 경우에는 일치 하는 resw 및 xlf 파일이 없는지 확인 합니다.

## <a name="i-dont-see-an-option-in-the-tools-menu-to-enable-the-multilingual-app-toolkit"></a>다국어 앱 도구 키트를 사용 하도록 설정 하는 옵션이 도구 메뉴에 표시 되지 않습니다.

이러한 단계를 수행 합니다.

- **도구** 메뉴를 열기 전에 솔루션 노드가 아닌 프로젝트 노드를 선택 했는지 확인 합니다.
- Visual Studio 확장 관리자를 사용 하 여 toolkit 확장을 설치 했는지 확인 합니다.
- 프로젝트가 UWP 프로젝트 인지 확인 합니다.

## <a name="when-i-build-my-project-i-dont-see-a-message-saying-that-a-multilingual-app-toolkit-build-has-started"></a>프로젝트를 빌드할 때 다국어 앱 도구 키트 빌드를 시작 했다는 메시지가 표시 되지 않습니다.

프로젝트에 대 한 대/a를 사용 하도록 설정 했는지 확인 합니다. **도구** 메뉴에서 **다국어 앱 도구 키트**  >  **선택 사용** 을 선택 합니다. 프로젝트가 이전 버전에서 사용 하도록 설정 된 경우 **도구** 메뉴를 사용 하 여 대/소문자를 사용 하지 않도록 설정 하 고 다시 사용 하도록 설정 합니다. 그러면 새 버전의 도구 키트와 함께 작동 하도록 프로젝트가 업데이트 됩니다.

"모든 Visual Studio 버전에 대 한 빌드 작업" 구성 요소가 설치 되어 있는지 확인 합니다. 이 빌드 구성 요소는 확장과 함께 설치 되지만 설치 중에 수동으로 선택 취소할 수 있습니다. 이 구성 요소는. xlf 파일을 업데이트 하 고 해당 번역을 PRI 파일에 추가 하는 데 필요 합니다. 이 구성 요소를 설치 하 고 올바르게 작동 하면 이러한 빌드 메시지가 표시 됩니다.

```dosbatch
1> Multilingual App Toolkit build started.
1> Multilingual App Toolkit build completed successfully.
```

## <a name="the-toolkit-is-reporting-that-it-didnt-locate-any-xliff-language-files-during-the-build"></a>도구 키트는 빌드 중에 XLIFF 언어 파일을 찾지 못한 것으로 보고 합니다.

```dosbatch
No XLIFF language files were found. The app will not contain any localized resources.
```

이 메시지는 도구 키트가 확장명이 xlf 인 프로젝트에서 파일을 찾을 수 없을 때 표시 됩니다. 도구 키트는 기본적으로 이러한 파일을 생성 하 여 폴더에 보관 합니다 `MultilingualResources` . 이동할 수 있습니다. 그러나 다국어 편집기에서 관련 메타 데이터 파일을 찾을 수 있으므로 해당 폴더에 그대로 두는 것이 가장 좋습니다.

## <a name="my-xlf-file-is-not-included-in-the-list-of-files-processed-by-the-toolkit-during-build"></a>빌드 중에 toolkit에서 처리 하는 파일 목록에는 xlf 파일이 포함 되지 않습니다.

프로젝트에서 수동으로 xlf 파일을 제외 하 고 다시 포함 하는 경우 파일 형식 요소가 올바르게 설정 되지 않을 수 있습니다. Visual Studio에서 파일을 선택 하 고 속성 창 확인 합니다. 파일의 빌드 작업을 XliffResource로 설정 하 고 출력 디렉터리로 복사를 복사 안 함으로 설정 해야 합니다. 프로젝트 파일에서 참조를 찾는 방법입니다.

```xml
<XliffResource Include="MultilingualResources\<project-name>.fr-FR.xlf" />
```

## <a name="ive-added-xlf-based-languages-where-are-my-strings"></a>. Xlf 기반 언어를 추가 했습니다. 내 문자열은 어디에 있나요?

기본 언어 리소스 파일 (. resw)은 앱에서 사용 하는 문자열의 정식 "스키마"입니다. 번역 된 리소스 파일은 이러한 문자열의 전체 또는 일부를 포함할 수 있습니다.

프로젝트를 빌드하면 리소스 파일 및. i f f 파일이 동기화 됩니다.

- . Xlf 파일은 추가 되거나 제거 된 문자열이 나 추가 되거나 제거 된 리소스 파일이 반영 되도록 업데이트 됩니다.
- 리소스 파일은. xlf 파일에서 번역 된 문자열을 반영 하도록 업데이트 됩니다.

이에 대해서는 [다국어 앱 도구 키트 4.0 사용](use-mat.md)에 자세히 설명 되어 있습니다.

## <a name="when-i-build-my-project-the-xlf-files-remain-empty"></a>프로젝트를 빌드할 때. xlf 파일이 비어 있는 상태로 유지 됩니다.

이 경우를 효과적으로 사용 하려면 앱을 지역화할 수 있어야 합니다. 이에 대해서는 [다국어 앱 도구 키트 4.0 사용](use-mat.md)에 자세히 설명 되어 있습니다.

## <a name="what-is-microsoft-translator"></a>Microsoft Translator란?

Microsoft Translator는 컴퓨터 기반 번역을 제공 하는 클라우드 기반 서비스입니다. 기계 번역은 사용자 변환이 적절 하지 않을 때 번역에 액세스 하는 데 적합 합니다. [Microsoft Translator](https://www.microsofttranslator.com/)에서 자세히 알아볼 수 있습니다.

도구 키트는 Microsoft Translator 서비스를 사용 하 여 번역 제안을 제공 합니다. Microsoft translator 아이콘이 번역 언어 대화 상자에 있는 경우 Microsoft Translator에서 지원 되는 언어를 확인할 수 있습니다.

다국어 편집기 내에서 문자열을 선택 하 고 **번역** 을 클릭 하 여 Microsoft Translator를 사용 하 여 앱을 신속 하 게 변환할 수 있습니다.

## <a name="what-is-pseudo-language-and-what-are-pseudo-resource-trackers"></a>의사 (pseudo) 언어는 무엇 인가요? 의사 (pseudo) 리소스 추적기는 무엇 인가요?

의사 (Pseudo) 언어는 실제 언어 지역화를 시뮬레이션 하기 위한 소프트웨어 제품의 인공 수정입니다. [다국어 앱 도구 키트 4.0를 사용](use-mat.md)하 여 의사 언어 및 의사 리소스 추적기에 대 한 자세한 정보를 찾을 수 있습니다.

## <a name="how-do-i-set-my-language-preference-to-pseudo-language-so-that-i-can-test-my-pseudo-locd-strings"></a>의사 (pseudo) 기능을 의사 (pseudo) 언어로 설정 어떻게 할까요? 의사 (pseudo) loc 문자열을 테스트할 수 있나요?

이에 대 [한 설명은 다국어 앱 도구 키트 4.0을 사용](use-mat.md)합니다.

## <a name="what-kind-of-localizability-issues-can-i-find-using-pseudo-language"></a>의사 언어를 사용 하 여 어떤 종류의 지역화 가능성 문제를 찾을 수 있나요?

이에 대 [한 설명은 다국어 앱 도구 키트 4.0을 사용](use-mat.md)합니다.

## <a name="im-not-seeing-any-translations-when-i-launch-my-app-or-my-app-is-only-partially-translated"></a>앱을 시작할 때 번역이 표시 되지 않거나 앱이 부분적 으로만 변환 됨

다국어 편집기에서. xlf 파일을 열어 번역이 있는지 확인 합니다. 기본 언어. resw 파일의 문자열을 명시적으로 변경 하는 경우 해당 번역은. xlf 파일에서 제거 됩니다. 이는 번역이 원본 문자열과 일치 하는지 확인 하기 위한 것입니다. . Xlf 파일의 문자열을 변환 하 고 다시 빌드하여 기본이 아닌 언어. resw 파일을 업데이트 합니다.

문자열이 xlf 파일에서 변환 되었지만 앱에 표시 되지 않으면 프로젝트를 다시 빌드하여 기본이 아닌 언어. resw 파일을 업데이트 합니다. Visual Studio는 빌드 명령을 최적화 하 여 마지막 빌드 이후 변경 된 파일만 빌드합니다.

언어 기본 설정 순서를 확인 합니다. 테스트 하려는 언어가 **설정** 의 언어 기본 설정 목록 맨 위에 나열 되는지 확인 합니다.

## <a name="the-toolkit-is-reporting-error--0x80004004-in-the-build-output"></a>도구 키트가 빌드 출력에 오류 0x80004004 보고 하 고 있습니다.

```dosbatch
Merge of Loc PRI file failed calling makepri.exe: "0x80004004"
```

이 메시지는 지역 형식이 toolkit 빌드 작업과 충돌 하는 경우에 표시 될 수 있습니다. 해결 방법은 빌드하는 동안 **설정** 에서 언어를 en-us로 변경 하는 것입니다.


## <a name="the-toolkit-is-reporting-error--0x80004005-in-the-build-output"></a>도구 키트는 빌드 출력에 오류 0x80004005를 보고 합니다.

```dosbatch
Merge of Loc PRI file failed calling makepri.exe: "0x80004005"
```

이 메시지는. xlf 파일에 지원 되지 않는 대상 언어가 포함 된 경우에 표시 될 수 있습니다. 예를 들어, "zh-cn-cht"는 잘못 되었으며 ("zh-cn-zh-hant"로 변경) "zh-cn-chs"는 올바르지 않습니다 ("zh-cn-hans"로 변경).

## <a name="is-there-a-way-to-find-out-more-information-about-the-errors-im-seeing"></a>표시 되는 오류에 대 한 자세한 정보를 확인할 수 있는 방법이 있나요?

예, Visual Studio에서 자세한 정보 로깅을 켤 수 있습니다. **도구**  >  **옵션**  >  **프로젝트 및 솔루션**  >  **빌드 및 실행** 을 클릭 합니다. **MSBuild 프로젝트 빌드 출력의 자세한 정도** 를 최소에서 보통 이상으로 변경 합니다.

명령줄에서 MSBuild를 실행 하면 추가 메시지가 생성 될 수도 있습니다.

```dosbatch
msbuild /t:rebuild <project-name>
```

## <a name="import-translation-failed"></a>변환 가져오기 실패

가져오기 프로세스는 가져오기 전에 기본 유효성 검사를 수행 합니다. 이렇게 하면 가져오는 파일의 대상 문화권 정보가 기존 xlf 파일의 대상 문화권 정보와 일치 하 게 됩니다. 다국어 편집기에서. xlf 파일을 열고 culture 정보가 일치 하는지 확인 합니다.

## <a name="what-if-my-translator-doesnt-have-windows-10-andor-visual-studio-andor-the-multilingual-app-toolkit-installed"></a>Translator에 Windows 10,/또는 Visual Studio 및/또는 다국어 앱 도구 키트가 설치 되어 있지 않으면 어떻게 되나요?

문자열 리소스 내보내기 대화 상자에서 **출력: 메일 받는 사람** 을 선택 하면 다국어 앱 도구 키트 (대만) 4.0을 다운로드 하 여 설치할 수 있는 링크가 전자 메일에 포함 됩니다. 변환기는 Windows 10 또는 Visual Studio 없이도 4.0의 독립 실행형 다국어 편집기 도구를 설치할 수 있습니다.

자세한 내용은 [다국어 앱 도구 키트 4.0 사용](use-mat.md)을 참조 하세요.

## <a name="what-happened-to-the-markuprulesxml-and-resourceslocksxml-files"></a>및 파일은 어떻게 `MarkupRules.xml` 되었습니까 `ResourcesLocks.xml` ?

다국어 앱 도구 키트 4.0는 소유 리소스 잠금 파일을 사용 하지 않습니다. 대신, `<mrk>` 컴퓨터 변환 중에 수정 되지 않은 문자열을 식별 하기 위해 XLIFF 1.2 태그가. xlf 파일에 직접 추가 됩니다. 이렇게 하면 XLIFF 파일이 자체 포함 될 수 있고 파일당 기반 리소스 잠금을 허용 합니다.

이러한 추가 지원 파일은 더 이상 필요 하지 않으며, 있는 경우 안전 하 게 삭제할 수 있습니다.

## <a name="what-happened-to-the-tpx-file"></a>Tpx 파일의 변경 내용

.Tpx 파일은 `MarkupRules.xml` `ResourcesLocks.xml` 변환을 위해. xlf 파일이 전송 될 때 및 파일을 포함 하는 쉬운 방법을 제공 합니다. 이 기능은 더 이상 필요 하지 않습니다.

검색 해야 하는 .ttpx 파일의 번역이 있는 경우 .ttpx 파일 확장명의 이름을 .zip으로 변경 하면 됩니다. 이렇게 하면 파일 탐색기나 .zip 호환 도구를 사용 하 여 콘텐츠를 열고 추출할 수 있습니다.

## <a name="i-think-ive-done-everything-right-but-it-still-isnt-working"></a>모든 것이 제대로 수행 되었다고 생각 하지만 여전히 작동 하지 않습니다.

이러한 단계를 수행 합니다.

1. 이미 설명한 방법 중 하나를 사용 하 여 번역을 추가 합니다.
2. 번역이 파일에 있는지를 확인 하려면 .pri 파일 ( [MakePri.exe 명령줄 옵션](../../app-resources/makepri-exe-command-options.md)참조)을 덤프 합니다. 번역은 다음과 같이 언어 코드 및 번역 된 값과 함께 표시 됩니다.
   ```xml
   <Candidate qualifiers="Language-QPS-PLOC" type="String">
       <Value>[!!_Ŝéãřćĥ_!!]</Value>
   </Candidate>
   ```
3. 명령 프롬프트에서 빌드합니다. 결과 오류는 빌드 출력에 보고 된 것 보다 더 자세한 정보를 포함할 수 있습니다.

## <a name="my-app-failed-certification-to-the-microsoft-store"></a>앱이 Microsoft Store에 대 한 인증에 실패 했습니다.

Microsoft Store 인증 프로세스를 시작 하기 전에 프로젝트에서 파일을 제외 해야 합니다 `<project-name>.qps-ploc.xlf` . 의사 (Pseudo) 언어는 잠재적 지역화 가능성 문제 또는 버그를 검색 하는 데 사용 되지만 올바른 Microsoft Store 언어가 아닙니다. 제거 되지 않은 경우 Microsoft Store 인증 프로세스 중에 앱이 실패 합니다.

## <a name="related-topics"></a>관련 항목

* [다국어 앱 도구 키트 4.0 사용](use-mat.md)
* [Microsoft Translator](https://www.microsofttranslator.com/)
* [MakePri.exe 명령줄 옵션](../../app-resources/makepri-exe-command-options.md)
