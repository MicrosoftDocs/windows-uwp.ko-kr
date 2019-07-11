---
ms.openlocfilehash: f8e74688d0f7048276b12680237b85663d7e2b81
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66214742"
---
# <a name="contributing-to-uwp-conceptual-documentation"></a>UWP 개념 문서에 참여

UWP(유니버설 Windows 플랫폼) 문서에 대한 관심에 감사 드립니다! 여러분이 보내주신 문서 관련 피드백, 편집 내용 및 추가 기능에 감사드립니다.

## <a name="writing-content"></a>콘텐츠 작성

문서는 Markdown(간단한 텍스트 스타일 구문)으로 작성됩니다. Markdown에 대해 잘 모르는 경우 [GitHub에서 기본 개념을 알아볼 수 있습니다](https://guides.github.com/features/mastering-markdown/). 확실하지 않을 때는 언제든지 문서의 다른 페이지에서 서식 스타일을 복사할 수 있습니다.

## <a name="public-contributions"></a>공개 참여

Microsoft 직원이 **아닌** 경우 [공개 콘텐츠 리포지토리](https://github.com/MicrosoftDocs/windows-uwp)를 통해 참여할 수 있습니다. 공개 참여는 기존 페이지를 변경하거나 설명하는 데 적합합니다.

### <a name="editing-a-file"></a>파일 편집

이미 공개 콘텐츠 리포지토리에 있는 경우 먼저 변경하려는 파일로 이동합니다. 여기에서 표시된 콘텐츠 위에 있는 연필 아이콘을 선택하여 편집을 시작합니다.

또는 docs.microsoft.com에서 페이지를 보는 경우 페이지의 오른쪽 위에서 **편집** 단추를 선택할 수 있습니다. 이렇게 하면 리포지토리의 연결된 원본 파일로 리디렉션됩니다.

편집을 시작하면 GitHub는 변경할 수 있는 개인 GitHub 계정에 공식 리포지토리를 자동으로 포크합니다. 완료되면 끌어오기 요청을 다시 **docs** 분기로 제출합니다.

### <a name="pull-requests"></a>끌어오기 요청

끌어오기 요청을 제출한 후에는 콘텐츠 품질 검사 목록에 대해 평가되어 기본적인 기준을 충족하는지 확인합니다. 요청이 전달되면 요청은 추가 검토를 위해 UWP 문서 팀의 구성원에게 할당됩니다. 실패하면 변경된 내용에 대한 알림을 받습니다.

할당된 검토자는 PR를 승인 또는 거부하거나 추가로 변경할 수 있습니다.

## <a name="internal-contributions"></a>내부 참여

Microsoft 직원인 경우 [프라이빗 콘텐츠 리포지토리](https://github.com/microsoftdocs/windows-uwp-pr)를 통해 참여할 수 있습니다. [Windows 제작 가이드](https://review.docs.microsoft.com/windows-authoring-guide/uwp/?branch=master)에서 이 리포지토리를 사용하는 방법에 대한 지침을 볼 수 있습니다. 예정된 기능에 관한 문서의 경우 프라이빗 리포지토리를 통해서만 참여해야 합니다.

### <a name="editing-a-file"></a>파일 편집

공개 리포지토리처럼 로컬 복제본을 만들지 않아도 브라우저에서 프라이빗 리포지토리를 약간씩 변경할 수 있습니다. 올바른 분기에 참여하고 있는지를 **확인해야 합니다**. 개인 분기 만들기에 대한 자세한 내용은 [Windows 제작 가이드의 지침](https://review.docs.microsoft.com/windows-authoring-guide/uwp/conceptual/branches?branch=master)을 참조하세요.

### <a name="making-substantial-changes"></a>큰 폭의 변경

기존 문서에 더 광범위한 변경이 필요하거나, 이미지를 추가 또는 변경하거나, 새 문서에 참여하려는 경우에는 프라이빗 콘텐츠 리포지토리의 로컬 복제본을 만듭니다. 자세한 내용은 [Windows 제작 가이드의 지침](https://review.docs.microsoft.com/windows-authoring-guide/uwp/conceptual/)을 따릅니다.

### <a name="pull-requests"></a>끌어오기 요청

내부 리포지토리에서 끌어오기 요청을 만드는 경우 개인 분기를 생성된 원본 분기로 병합할지 확인합니다.

끌어오기 요청을 제출한 후에는 [콘텐츠 품질 검사 목록](https://review.docs.microsoft.com/windows-authoring-guide/managing-contributions/editorial-checklist?branch=master)에 대해 평가되어 기본적인 기준을 충족하는지 확인합니다. 요청이 전달되면 요청은 추가 검토를 위해 UWP 문서 팀의 구성원에게 할당됩니다. 실패하면 변경된 내용에 대한 알림을 받습니다.

할당된 검토자는 PR를 승인 또는 거부하거나 추가로 변경할 수 있습니다. 사용자가 직접 PR을 승인할 때까지 검토자는 PR를 병합하지 않습니다.

## <a name="using-issues-to-provide-feedback-on-uwp-conceptual-documentation"></a>UWP 개념 문서에 피드백을 제공하는 이슈 사용

직접 편집하는 대신 문서에 대한 피드백을 제공하려면 [공개 리포지토리에서 이슈를 만들 수 있습니다](https://github.com/MicrosoftDocs/windows-uwp/issues). **이슈** 탭을 선택하고 **새 이슈** 단추를 선택합니다. 토픽 제목과 페이지 URL을 꼭 포함해야 합니다. 이슈는 검토를 위해 UWP 문서 팀의 구성원에게 할당됩니다.

* 내부 이슈의 경우 [WDG 콘텐츠 요청 도구](https://aka.ms/pubrequest)를 사용합니다.
