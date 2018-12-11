---
title: Xbox 개발자 프로그램에서 UWP에 대해 알려진 문제
description: Xbox 개발자 프로그램에서 UWP에 대해 알려진 문제를 나열합니다.
ms.date: 03/29/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: a7b82570-1f99-4bc3-ac78-412f6360e936
ms.localizationpriority: medium
ms.openlocfilehash: 55068ef3f0a0a0d01c61746bde02ddb7aa4ef885
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8892939"
---
# <a name="known-issues-with-uwp-on-xbox-developer-program"></a>Xbox 개발자 프로그램에서 UWP에 대해 알려진 문제

이 항목에서는 Xbox One 개발자 프로그램에서 UWP에 대해 알려진 문제를 설명합니다. 이 프로그램에 대한 자세한 내용은 [Xbox의 UWP](index.md)를 참조하세요. 

\[API 참조 항목 링크를 통해 이 페이지를 방문했으며 유니버설 장치 패밀리 API 정보를 보려는 경우 [Xbox에서 아직 지원되지 않는 UWP 기능](http://go.microsoft.com/fwlink/?LinkID=760755)을 참조하세요.\]

다음 목록에는 발생할 수 있는 몇 가지 알려진 문제가 요약되어 있습니다. 

**피드백을 받고 싶으니**, [유니버설 Windows 플랫폼 앱 개발](https://social.msdn.microsoft.com/forums/windowsapps/home?forum=wpdevelop) 포럼에서 발견된 문제를 모두 신고해 주세요. 

문제가 있으면 이 항목의 내용을 살펴보고, [질문과 대답](frequently-asked-questions.md)을 확인하고, 포럼을 통해 도움을 요청하세요.

 
## <a name="deploying-from-vs-fails-with-parental-controls-turned-on"></a>자녀 보호를 켠 상태로 VS에서 배포하지 못함

콘솔의 자녀 보호가 설정에서 켜져 있는 경우 VS에서 앱을 시작하지 못합니다.

이 문제를 해결하려면 자녀 보호를 일시적으로 사용하지 않도록 설정하거나 다음을 수행합니다.
1. 자녀 보호를 끄고 콘솔에 앱을 배포합니다.
2. 자녀 보호를 켭니다.
3. 콘솔에서 앱을 시작합니다.
4. 앱이 시작될 수 있도록 PIN 또는 암호를 입력합니다.
5. 앱이 시작됩니다.
6. 앱을 종료합니다.
7. F5 키를 사용하여 VS에서 시작합니다. 앱이 확인 없이 시작됩니다.

이 시점에서는 앱을 제거하고 다시 설치하는 경우에도 사용자를 로그아웃시킬 때까지 권한은 _고정_되어 있습니다.
 
자녀 계정에만 사용할 수 있는 다른 유형의 예외가 있습니다. 자녀 계정에는 부모가 로그인하여 권한을 부여해야 하지만 권한을 부여할 때 부모는 자녀가 앱을 시작하는 것을 **항상** 허용하도록 선택할 수 있습니다. 이 예외는 클라우드에 저장되고 자녀가 로그아웃하고 다시 로그인하는 경우에도 유지됩니다.

## <a name="storagefilecopyasync-fails-to-copy-encrypted-files-to-unencrypted-destination"></a>암호화된 파일을 암호화되지 않은 대상으로 복사할 때 StorageFile.CopyAsync 실패 

암호화된 파일을 암호화되지 않은 대상으로 복사하는 데 StorageFile.CopyAsync를 사용하면 다음 예외와 함께 호출이 실패합니다.

```
System.UnauthorizedAccessException: Access is denied. (Excep_FromHResult 0x80070005)
```

이는 앱 패키지의 일부로 배포된 파일을 다른 위치로 복사하려는 Xbox 개발자에게 영향을 줄 수 있습니다. 이 문제의 이유는 정품 모드에서는 콘텐츠가 Xbox에서 암호화되지만, 개발자 모드에서는 그렇지 않기 때문입니다. 그 결과로, 개발과 테스트 동안에는 앱이 기대한 대로 작동하는 것처럼 보이지만, 게시되어 Xbox에 설치되면 오류가 발생하게 됩니다.
 

## <a name="blocked-networking-ports-on-xbox-one"></a>Xbox One에서 차단된 네트워킹 포트

Xbox One 장치의 UWP(유니버설 Windows 플랫폼) 앱은 범위 [57344, 65535]의 모든 포트에 바인딩할 수 없습니다. 이러한 포트에 바인딩하면 런타임 시 성공하는 것처럼 보이지만 앱에 도달하기 전에 네트워크 트래픽이 자동으로 삭제될 수 있습니다. 가능할 때마다 앱을 포트 0에 바인딩해야 시스템이 로컬 포트를 선택할 수 있습니다. 특정 포트를 사용해야 할 경우 포트 번호는 범위 [1025 49151]에 있어야 하고 IANA 레지스트리를 사용하여 충돌을 확인하고 방지해야 합니다. 자세한 내용은 [서비스 이름 및 전송 프로토콜 포트 번호 레지스트리](http://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xhtml)를 참조하세요.

## <a name="uwp-api-coverage"></a>UWP API 검사

일부 UWP API는 Xbox에서 지원되지 않습니다. 작동하지 않는다고 알고 있는 API 목록은 [Xbox에서 아직 지원되지 않는 UWP 기능](http://go.microsoft.com/fwlink/p/?LinkId=760755)을 참조하세요. 다른 API에서 문제가 발견된 경우 포럼에서 신고하세요. 


## <a name="navigating-to-wdp-causes-a-certificate-warning"></a>WDP로 이동하면 인증서 경고가 표시됨

Xbox One 본체에서 서명한 보안 인증서는 신뢰할 수 있는 잘 알려진 게시자로 간주되지 않으므로 제공된 인증서에 대해 다음 스크린샷과 유사한 경고가 표시됩니다. Windows 장치 포털에 액세스하려면 **이 웹 사이트를 계속 탐색합니다**를 클릭합니다.

![웹 사이트 보안 인증서 경고](images/security_cert_warning.jpg)


## <a name="knownfoldersmediaserverdevices-caveat-on-xbox"></a>Xbox에서 KnownFolders.MediaServerDevices 주의

데스크톱에서 미디어 서버는 PC와 "페어링"되고 장치 연결 서비스는 현재 어떤 서버가 온라인 상태인지 계속 추적하므로 초기 파일 시스템 쿼리가 즉시 현재 온라인 상태의 페어링된 서버 목록을 반환할 수 있습니다.

Xbox에서는 서버를 추가하거나 제거할 UI가 없어 초기 파일 시스템 쿼리가 항상 빈 값을 반환합니다. 쿼리를 작성하고 ContentsChanged 이벤트를 구독하고 알림을 받을 때마다 쿼리를 새로 고쳐야 합니다. 서버가 점차 연결되고 대부분은 3초 이내에 발견됩니다.

단순한 예제 코드:

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
- [질문과 대답](frequently-asked-questions.md)
- [Xbox One의 UWP](index.md)
