---
title: XIM 프로젝트 구성
description: 사용 하 여 Xbox 통합 멀티 플레이 게임 (XIM) 작업에 UWP 앱 매니페스트를 구성 하는 방법을 설명 합니다.
ms.assetid: 5d0ed7bd-9527-42a3-ae09-8cc9012c1111
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, xbox 통합된 멀티 플레이 게임을 매니페스트
ms.localizationpriority: medium
ms.openlocfilehash: 926cc4495236fc4762f9b3176a5d6a4a579e2e2e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613428"
---
# <a name="xim-project-configuration"></a>XIM 프로젝트 구성

## <a name="required-appxmanifest-capability-content"></a>필요한 AppXManifest 기능 콘텐츠

XIM를 기본적으로 사용 하 여 응용 프로그램에 연결 하 고 네트워크 리소스 모두를 통해 인터넷 및 로컬 네트워크에서에서 연결을 허용 해야 합니다. 음성 채팅 지원 하려면 마이크 장치에 대 한 액세스도 필요 합니다. 결과적으로, 앱 "internetClientServer" 및 "privateNetworkClientServer" 기능과 해당 AppXManifest에서 "마이크" 장치 기능을 선언 해야 합니다. 각각에 대 한 자세한 내용 플랫폼 설명서를 참조 하세요. 연결 및 채팅을 차단 합니다. 그렇지 않으면 다음 코드 조각은 패키지/기능 노드 아래에 존재 해야 하는 노드를 보여 줍니다.

```xml
 <?xml version="1.0" encoding="utf-8"?>
 <Package ...>
   <Identity ... />
   ...
   <Capabilities>
     <Capability Name="internetClientServer" />
     <Capability Name="privateNetworkClientServer" />
     <DeviceCapability Name="microphone" />
   </Capabilities>
 </Package>
```

## <a name="universal-windows-application-xbox-live-multiplayer-invite-protocol-activation-extension"></a>Xbox Live 멀티 플레이 하는 유니버설 Windows 응용 프로그램 초대 프로토콜 활성화 확장

Xbox Live 멀티 플레이어 응용 프로그램은 게임 초대를 지원 해야 합니다. 앱의 프로토콜 활성화 형식의에 도착 하는 로컬 사용자가 동의 하는 초대 (프로토콜 활성화에 대 한 자세한 내용은 플랫폼 설명서 참조). 대역의 예약을 통해 관리 되는 XIM 네트워크 호출에 대 한 이외의 XIM를 사용 하는 앱을 `xim::extract_protocol_activation_information()` 런타임 있지만 유니버설 Windows 플랫폼 (UWP) 응용 프로그램에서 프로토콜을 활성화를 처리 하는 데 도움이 되는 메서드를 통해 또한 정적으로 선언 해야 합니다.는 AppXManifest "windows.protocol" 확장 시스템에서 이러한 "ms-xbl-다중 접속" 정품 인증 지원. 다음 코드 조각에 표시 된 것과 같이 수행할 수 있습니다.

```xml
 <?xml version="1.0" encoding="utf-8"?>
 <Package xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10" xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10" IgnorableNamespaces="uap">
   <Applications>
     <Application ...>
       <Extensions>
         ...
          <uap:Extension Category="windows.protocol">
             <uap:Protocol Name="ms-xbl-multiplayer"/>
          </uap:Extension>
       </Extensions>
     </Application>
   </Applications>
 </Package>
```

만 유니버설 Windows 응용 프로그램에 필요 합니다. 자동으로 등록는 Xbox Live 제목 ID 확장을 선언 하 여 Xbox One 배타 리소스 ("XDK") 응용 프로그램 프로토콜 활성화 확장을 명시적으로 선언 하지 마십시오.

## <a name="xbox-one-exclusive-resource-application-network-manifest-templates"></a>Xbox One 배타 리소스 응용 프로그램 네트워크 매니페스트 템플릿

XIM를 사용 하는 Xbox One 배타 리소스 응용 프로그램 특정 소켓 설명 해야 하 고 템플릿 콘텐츠 AppXManifest의 "네트워크 매니페스트" 확장명이 포함 됩니다 (매니페스트 네트워크에 대 한 자세한 내용 플랫폼 설명서 참조). 앱에 다른 템플릿을 있을 수 있습니다 하지만 다음 코드 조각을 사용할 수 있어야 합니다. 그러지 XIM 네트워크 이동 실패:

```xml

 <?xml version="1.0" encoding="utf-8"?>
 <Package xmlns="http://schemas.microsoft.com/appx/2010/manifest" xmlns:mx="http://schemas.microsoft.com/appx/2013/xbox/manifest" IgnorableNamespaces="mx">
   <Applications>
     <Application ...>
       <Extensions>
         ...
         <mx:Extension Category="windows.xbox.networking">
           <mx:XboxNetworkingManifest>
             <mx:SocketDescriptions>
               <mx:SocketDescription Name="Xim_client_v1" SecureIpProtocol="Udp" BoundPort="17181-17183">
                 <mx:AllowedUsages>
                   <mx:SecureDeviceSocketUsage Type="Initiate" />
                   <mx:SecureDeviceSocketUsage Type="Accept" />
                   <mx:SecureDeviceSocketUsage Type="SendChat" />
                   <mx:SecureDeviceSocketUsage Type="SendGameData" />
                   <mx:SecureDeviceSocketUsage Type="ReceiveChat" />
                   <mx:SecureDeviceSocketUsage Type="ReceiveGameData" />
                 </mx:AllowedUsages>
               </mx:SocketDescription>
               <mx:SocketDescription Name="Xim_relay_acceptor_v1" SecureIpProtocol="Udp" BoundPort="17191-17390">
                 <mx:AllowedUsages>
                   <mx:SecureDeviceSocketUsage Type="Accept" />
                   <mx:SecureDeviceSocketUsage Type="SendChat" />
                   <mx:SecureDeviceSocketUsage Type="SendGameData" />
                   <mx:SecureDeviceSocketUsage Type="ReceiveChat" />
                   <mx:SecureDeviceSocketUsage Type="ReceiveGameData" />
                 </mx:AllowedUsages>
               </mx:SocketDescription>
             </mx:SocketDescriptions>
             <mx:SecureDeviceAssociationTemplates>
               <mx:SecureDeviceAssociationTemplate Name="Xim_relay_v1" InitiatorSocketDescription="Xim_client_v1" AcceptorSocketDescription="Xim_relay_acceptor_v1" MultiplayerSessionRequirement="Required">
                 <mx:AllowedUsages>
                   <mx:SecureDeviceAssociationUsage Type="InitiateFromMicrosoftConsole" />
                   <mx:SecureDeviceAssociationUsage Type="InitiateFromWindowsDesktop" />
                   <mx:SecureDeviceAssociationUsage Type="AcceptOnXboxLiveCompute" />
                 </mx:AllowedUsages>
               </mx:SecureDeviceAssociationTemplate>
               <mx:SecureDeviceAssociationTemplate Name="Xim_peer_v1" InitiatorSocketDescription="Xim_client_v1" AcceptorSocketDescription="Xim_client_v1" MultiplayerSessionRequirement="Required">
                 <mx:AllowedUsages>
                   <mx:SecureDeviceAssociationUsage Type="InitiateFromMicrosoftConsole" />
                   <mx:SecureDeviceAssociationUsage Type="AcceptOnMicrosoftConsole" />
                   <mx:SecureDeviceAssociationUsage Type="InitiateFromWindowsDesktop" />
                   <mx:SecureDeviceAssociationUsage Type="AcceptOnWindowsDesktop" />
                 </mx:AllowedUsages>
               </mx:SecureDeviceAssociationTemplate>
             </mx:SecureDeviceAssociationTemplates>
           </mx:XboxNetworkingManifest>
         </mx:Extension>
       </Extensions>
     </Application>
   </Applications>
 </Package>
```

## <a name="universal-windows-application-uwp-network-manifest-templates"></a>유니버설 Windows (UWP) 응용 프로그램 네트워크 매니페스트 템플릿

XIM를 사용 하는 유니버설 Windows 응용 프로그램 특정 소켓 설명 해야 하 고 템플릿 콘텐츠 "네트워크 매니페스트" 파일에 포함 됩니다 (networkmanifest.xml 파일 패키지 루트 디렉터리에 플랫폼에서 설명서를 참조에 대 한 자세한 정보 네트워크 매니페스트). 앱에 다른 템플릿을 있을 수 있습니다 하지만 다음 코드 조각을 사용할 수 있어야 합니다. 그러지 XIM 네트워크 이동 실패:

```xml
 <?xml version="1.0" encoding="utf-8"?>
 <NetworkManifest xmlns="http://schemas.microsoft.com/xbox/2012/networkmanifest">
   <SocketDescriptions>
     <SocketDescription Name="Xim_client_v1" SecureIpProtocol="Udp" BoundPort="17181-17183">
       <AllowedUsages>
         <SecureDeviceSocketUsage Type="Initiate" />
         <SecureDeviceSocketUsage Type="Accept" />
         <SecureDeviceSocketUsage Type="SendChat" />
         <SecureDeviceSocketUsage Type="SendGameData" />
         <SecureDeviceSocketUsage Type="ReceiveChat" />
         <SecureDeviceSocketUsage Type="ReceiveGameData" />
       </AllowedUsages>
     </SocketDescription>
     <SocketDescription Name="Xim_relay_acceptor_v1" SecureIpProtocol="Udp" BoundPort="17191-17390">
       <AllowedUsages>
         <SecureDeviceSocketUsage Type="Accept" />
         <SecureDeviceSocketUsage Type="SendChat" />
         <SecureDeviceSocketUsage Type="SendGameData" />
         <SecureDeviceSocketUsage Type="ReceiveChat" />
         <SecureDeviceSocketUsage Type="ReceiveGameData" />
       </AllowedUsages>
     </SocketDescription>
   </SocketDescriptions>
   <SecureDeviceAssociationTemplates>
     <SecureDeviceAssociationTemplate Name="Xim_relay_v1" InitiatorSocketDescription="Xim_client_v1" AcceptorSocketDescription="Xim_relay_acceptor_v1" MultiplayerSessionRequirement="Required">
       <AllowedUsages>
         <SecureDeviceAssociationUsage Type="InitiateFromMicrosoftConsole" />
         <SecureDeviceAssociationUsage Type="InitiateFromWindowsDesktop" />
         <SecureDeviceAssociationUsage Type="AcceptOnXboxLiveCompute" />
       </AllowedUsages>
     </SecureDeviceAssociationTemplate>
     <SecureDeviceAssociationTemplate Name="Xim_peer_v1" InitiatorSocketDescription="Xim_client_v1" AcceptorSocketDescription="Xim_client_v1" MultiplayerSessionRequirement="Required">
       <AllowedUsages>
         <SecureDeviceAssociationUsage Type="InitiateFromMicrosoftConsole" />
         <SecureDeviceAssociationUsage Type="AcceptOnMicrosoftConsole" />
         <SecureDeviceAssociationUsage Type="InitiateFromWindowsDesktop" />
         <SecureDeviceAssociationUsage Type="AcceptOnWindowsDesktop" />
       </AllowedUsages>
     </SecureDeviceAssociationTemplate>
   </SecureDeviceAssociationTemplates>
 </NetworkManifest>
```
