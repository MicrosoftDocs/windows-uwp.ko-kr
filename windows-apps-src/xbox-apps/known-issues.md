---
title: Xbox 개발자 프로그램에서 UWP에 대해 알려진 문제
description: Xbox One Developer Program의 UWP에서 알려진 몇 가지 문제에 대해 알아보고 다른 도움말 리소스에 액세스 하는 방법을 참조 하세요.
ms.date: 03/29/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: a7b82570-1f99-4bc3-ac78-412f6360e936
ms.localizationpriority: medium
ms.openlocfilehash: 52421d17de5f26fe29060c39d33b51cc85fa197a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174557"
---
# <a name="known-issues-with-uwp-on-xbox-developer-program"></a>Xbox 개발자 프로그램에서 UWP에 대해 알려진 문제

이 항목에서는 Xbox One Developer Program의 UWP에서 알려진 문제에 대해 설명 합니다. 이 프로그램에 대 한 자세한 내용은 [Xbox의 UWP](index.md)를 참조 하세요. 

\[API 참조 항목의 링크에서 여기를 제공 하 고 범용 장치 패밀리 API 정보를 찾고 있는 경우 [Xbox에서 아직 지원 되지 않는 UWP 기능](/uwp/extension-sdks/uwp-limitations-on-xbox)을 참조 하세요.\]

다음 목록에서는 발생할 수 있는 몇 가지 알려진 문제를 중점적으로 설명 하지만이 목록은 완전 하지 않습니다. 

사용자 **의견**을 보내 주시기 바랍니다. [유니버설 Windows 플랫폼 apps 개발](https://social.msdn.microsoft.com/forums/windowsapps/home?forum=wpdevelop) 포럼에서 발견 한 문제를 보고 하세요. 

문제가 발생 하는 경우이 항목의 정보를 읽고 질문과 [대답](frequently-asked-questions.md)을 확인 하 고 포럼을 사용 하 여 도움을 요청 하세요.

 
## <a name="deploying-from-vs-fails-with-parental-controls-turned-on"></a>부모 컨트롤이 설정 된 상태에서 VS에서 배포가 실패 함

콘솔에 설정에서 자녀 보호 기능이 설정 되어 있으면 VS에서 앱을 시작 하는 작업이 실패 합니다.

이 문제를 해결 하려면 보호자 제어를 일시적으로 해제 하거나 다음을 수행 합니다.
1. 자녀 보호 기능이 해제 된 상태에서 콘솔에 앱을 배포 합니다.
2. 자녀 보호를 설정 합니다.
3. 콘솔에서 앱을 시작 합니다.
4. 앱을 시작할 수 있도록 PIN 또는 암호를 입력 합니다.
5. 앱이 시작 됩니다.
6. 앱을 닫습니다.
7. F5 키를 사용 하 여 VS에서 시작 하 고, 앱이 메시지를 표시 하지 않고 시작 합니다.

이 시점에서 앱을 제거 하 고 다시 설치 하는 경우에도 사용자가 로그인 할 때까지 사용 권한이 _고정_ 됩니다.
 
자식 계정에만 사용할 수 있는 다른 유형의 예외가 있습니다. 자식 계정에는 사용 권한을 부여 하기 위해 부모가 필요 하지만, 부모에는 **항상** 자녀가 앱을 시작 하도록 허용할 수 있는 옵션이 있습니다. 해당 예외는 클라우드에 저장 되며 자녀가 로그 아웃 했다가 다시 로그인 하는 경우에도 유지 됩니다.

## <a name="storagefilecopyasync-fails-to-copy-encrypted-files-to-unencrypted-destination"></a>암호화 된 파일을 암호화 되지 않은 대상으로 복사 하는 StorageFile 

암호화 되지 않은 대상에 암호화 된 파일을 복사 하는 데 StorageFile Async를 사용 하면 다음 예외가 발생 하 여 호출이 실패 합니다.

```
System.UnauthorizedAccessException: Access is denied. (Excep_FromHResult 0x80070005)
```

이는 앱 패키지의 일부로 배포 되는 파일을 다른 위치에 복사 하려는 Xbox 개발자에 게 영향을 줄 수 있습니다. 이는 패키지 내용이 Xbox에서 일반 정품 모드로 암호화 되지만 개발 모드에서는 암호화 되지 않았기 때문입니다. 따라서 개발 및 테스트 중에 앱이 예상 대로 작동 하는 것 처럼 보일 수 있지만, 게시 된 후에는 소매 Xbox에 설치 된 후에 오류가 발생 합니다.
 

## <a name="blocked-networking-ports-on-xbox-one"></a>Xbox One의 차단 된 네트워킹 포트

유니버설 Windows 플랫폼 (UWP) Xbox의 앱은 [57344, 65535] 범위의 포트에 대 한 바인딩 (포함)으로 제한 됩니다. 이러한 포트에 대 한 바인딩이 런타임에 성공 하는 것 처럼 보일 수 있지만, 네트워크 트래픽은 앱에 도달 하기 전에 자동으로 삭제 될 수 있습니다. 앱은 가능 하면 항상 포트 0에 바인딩되어야 합니다. 그러면 시스템에서 로컬 포트를 선택할 수 있습니다. 특정 포트를 사용 해야 하는 경우 포트 번호는 [1025, 49151] 범위 내에 있어야 하 고 IANA 레지스트리와 충돌을 방지 해야 합니다. 자세한 내용은 [서비스 이름 및 전송 프로토콜 포트 번호 레지스트리](https://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xhtml)를 참조 하세요.

## <a name="windows-runtime-api-coverage"></a>Windows 런타임 API 범위

모든 Windows 런타임 Api가 Xbox에서 지원 되는 것은 아닙니다. 작동 하지 않는 Api 목록은 [Xbox에서 아직 지원 되지 않는 UWP 기능](/uwp/extension-sdks/uwp-limitations-on-xbox)을 참조 하세요. 다른 Api와 관련 된 문제를 발견 한 경우 포럼에 보고 해 주세요. 


## <a name="navigating-to-wdp-causes-a-certificate-warning"></a>WDP로 이동 하면 인증서 경고가 발생 합니다.

Xbox One 콘솔에서 서명 된 보안 인증서는 잘 알려진 신뢰할 수 있는 게시자로 간주 되지 않으므로 다음 스크린샷에와 같이 제공 된 인증서에 대 한 경고가 표시 됩니다. Windows 장치 포털에 액세스 하려면 **이 웹 사이트를 계속**탐색 합니다 .를 클릭 합니다.

![웹 사이트 보안 인증서 경고](images/security_cert_warning.jpg)


## <a name="knownfoldersmediaserverdevices-caveat-on-xbox"></a>KnownFolders. Xbox의 MediaServerDevices 주의 사항

데스크톱에서 미디어 서버는 PC와 "페어링" 되며, 장치 연결 서비스는 현재 온라인 상태인 서버를 지속적으로 추적 하므로 초기 파일 시스템 쿼리는 현재 온라인 상태인 페어링 된 서버 목록을 즉시 반환할 수 있습니다.

Xbox에는 서버를 추가 하거나 제거 하는 UI가 없으므로 초기 파일 시스템 쿼리가 항상 빈 상태로 반환 됩니다. 알림을 받을 때마다 쿼리를 만들고 내용 변경 이벤트를 구독 하 고 쿼리를 새로 고쳐야 합니다. 서버는 trickle 3 초 이내에 검색 됩니다.

간단한 예제 코드:

```
namespace TestDNLA {

    public sealed partial class MainPage : Page {
        public MainPage() {
            this.InitializeComponent();
        }

        private async void FindFiles_Click(object sender, RoutedEventArgs e) {
            try {
                StorageFolder library = KnownFolders.MediaServerDevices;
                var folderQuery = library.CreateFolderQuery();
                folderQuery.ContentsChanged += FolderQuery_ContentsChanged;
                IReadOnlyList<StorageFolder> rootFolders = await folderQuery.GetFoldersAsync();
                if (rootFolders.Count == 0) {
                    Debug.WriteLine("No Folders found");
                } else {
                    Debug.WriteLine("Folders found");
                }
            } catch (Exception ex) {
                Debug.WriteLine("Error: " + ex.Message);
            } finally {
                Debug.WriteLine("Done");
            }
        }

        private async void FolderQuery_ContentsChanged(Windows.Storage.Search.IStorageQueryResultBase sender, object args) {
            Debug.WriteLine("Folder added " + sender.Folder.Name);
            IReadOnlyList<StorageFolder> topLevelFolders = await sender.Folder.GetFoldersAsync();
            foreach (StorageFolder topLevelFolder in topLevelFolders) {
                Debug.WriteLine(topLevelFolder.Name);
            }
        }
    }
}
```

## <a name="see-also"></a>참고 항목
- [자주 묻는 질문](frequently-asked-questions.md)
- [Xbox One의 UWP](index.md)