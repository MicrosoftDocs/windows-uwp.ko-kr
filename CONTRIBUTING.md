# <a name="contributing-to-uwp-conceptual-documentation"></a>UWP 개념 문서에 기여

UWP(유니버설 Windows 플랫폼) 문서에 대한 관심에 감사 드립니다! 우리의 문서에 대한 피드백, 편집, 및 추가해주셔서 감사합니다.

이 페이지에서는 개발자 문서에 기여하는 기본 단계에 대해 설명합니다.

## <a name="public-and-private-repos"></a>공용 및 비공개 리포지토리

UWP 개념 문서는 두 개의 서로 다른 리포지토리에 호스트된 후 단일 사이트로 병합 및 업데이트됩니다. 한 리포지토리는 누구나 기여할 수 있으며 다른 하나는 Microsoft 직원만 기여할 수 있습니다.

Microsoft 직원이 ***아닌*** 경우 [공개 콘텐츠 리포지토리](https://github.com/MicrosoftDocs/windows-uwp)에서 작성합니다.

Microsoft ***직원***인 경우 공개 리포지토리 또는 [비공개 콘텐츠 리포지토리](https://cpubwin.visualstudio.com/_git/windows-uwp)에서 작성할 수 있습니다. 직원은 비공개 저장소에 기여하여 조금 더 빠르게 변경 사항을 실시간으로 게시할 수 있으며 [특정 분기](https://review.docs.microsoft.com/en-us/windows-authoring-guide/uwp/conceptual/setup-local-repo-for-large-changes#what-branch-should-i-use-for-my-authoring)를 사용하여 변경 사항을 이후 날짜까지 공개하지 않고 보유할 수도 있습니다.

## <a name="editing-topics-on-the-public-repo"></a>공개 리포지토리에서 항목 편집

기존 파일의 편집을 최대한 간단하게 하려고 했습니다. 
- 이미 리포지토리에 있는 경우 파일을 찾아 **편집** 단추를 클릭합니다.  
- 또는 브라우저에서 Docs.microsoft.com 페이지를 보는 경우 페이지 오른쪽 맨 위에 있는 **편집** 단추를 클릭합니다. 리포지토리에 있는 올바른 마크다운 원본 파일로 리디렉션되며 여기에서 **편집** 단추를 클릭합니다. 

GitHub는 변경할 수 있는 개인 GitHub 계정에 공식 리포지토리를 자동으로 포크합니다. 완료되면 끌어오기 요청을 다시 "문서" 분기로 제출합니다. 끌어오기 요청을 생성하면 UWP 설명서 팀이 변경 내용을 검토합니다. 요청이 승인되면 업데이트 내용이 https://docs.microsoft.com/windows/에 게시됩니다.

몇 분 내에 마크다운의 기본 사항을 알아볼 수 있습니다.  시작하려면 [마크다운 마스터하기](https://guides.github.com/features/mastering-markdown/)의 확인란을 누릅니다.

## <a name="making-more-substantial-changes"></a>더 실질적인 변경

기존 문서에 실질적인 변경 작업도 수행하거나,이미지를 추가 또는 변경 거나 새 문서에 기여하려면 비공개 콘텐츠 리포지토리의 로컬 복제본을 생성해야 합니다. [Windows 제작 가이드의 지침](https://review.docs.microsoft.com/en-us/windows-authoring-guide/uwp/conceptual/)을 따르세요. 아직 GitHub 계정을 설정하지 않았고 Microsoft 별칭을 도메인에 가입하지 않았다면 [여기에서 시작](https://review.docs.microsoft.com/en-us/windows-authoring-guide/github-account)합니다.

## <a name="using-issues-to-provide-feedback-on-uwp-conceptual-documentation"></a>UWP 개념 문서에 피드백을 제공하는 이슈 사용

직접 실제 설명서 페이지를 수정하지 않고 단지 피드백만을 제공하려면 [공개 리포지토리에서 이슈를 만들](https://github.com/MicrosoftDocs/windows-uwp/issues) 수 있습니다. "이슈" 탭을 클릭한 다음 **새로운 이슈** 단추를 클릭합니다. 항목의 제목 및 페이지의 URL을 포함하도록 합니다.

UWP 설명서 팀의 구성원들이 이슈를 정기적으로 검토하고 적절하게 심사, 할당 및 해결합니다.

*내부 이슈의 경우 [http://aka.ms/pubrequest](http://aka.ms/pubrequest)에서 WDG 콘텐츠를 요청 도구를 사용하세요. 
