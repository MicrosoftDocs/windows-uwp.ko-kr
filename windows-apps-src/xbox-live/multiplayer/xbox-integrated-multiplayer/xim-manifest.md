---
title: XIM 프로젝트 구성
author: KevinAsgari
description: UWP 앱 매니페스트에서 사용 하 여 Xbox 통합 멀티 플레이어 (XIM) 작동 하도록 구성 하는 방법을 설명 합니다.
ms.assetid: 5d0ed7bd-9527-42a3-ae09-8cc9012c1111
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 게임, uwp, windows 10, 하나는 xbox, xbox 통합된 멀티 플레이어, 매니페스트
ms.localizationpriority: medium
ms.openlocfilehash: ad509fd41ae1b2a568fcc2e988beec693ebb0b66
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/08/2018
ms.locfileid: "4427071"
---
# <a name="xim-project-configuration"></a>XIM 프로젝트 구성

## <a name="required-appxmanifest-capability-content"></a>필요한 AppXManifest 접근 권한 값은 콘텐츠

기본적으로 XIM을 사용 하 여 응용 프로그램에 연결 하 고 모두 인터넷 및 로컬 네트워크를 통해 네트워크 리소스에서 연결을 수락 해야 합니다. 음성 채팅을 지원 하도록 마이크 장치에 액세스를 해야 합니다. 결과적으로, 앱 "internetClientServer" 및 "privateNetworkClientServer" 기능과 그 AppXManifest에서 "마이크" 장치 접근 권한 값을 선언 해야 합니다. 각 부분에 대 한 자세한 내용은 플랫폼 설명서를 참조 하세요. 다음 코드 조각은 패키지/기능 노드 아래에 있어야 하는 노드를 보여 줍니다. 그렇지 않으면 채팅 및 연결을 차단 됩니다.

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

## <a name="universal-windows-application-xbox-live-multiplayer-invite-protocol-activation-extension"></a>Xbox Live 멀티 플레이 하는 유니버설 Windows 응용 프로그램 프로토콜 활성화 확장 초대

Xbox Live 멀티 플레이 응용 프로그램은 게임 초대를 지원 해야 합니다. 초대를 수락 로컬 사용자가 앱의 프로토콜 활성화의 숫자 형태로 도착 (프로토콜 활성화에 대 한 자세한 정보에 대 한 플랫폼 설명서를 참조). XIM 네트워크의 대역 예약을 통해 관리 호출에 대 한 이외의 XIM을 사용 하는 앱의 `xim::extract_protocol_activation_information()` 런타임, 유니버설 Windows 플랫폼 (UWP) 응용 프로그램에서 프로토콜 활성화를 처리 하는 메서드를 통해 정적으로 선언 해야는 이러한 "ms-xbl-멀티 플레이어" 정품 인증 시스템에 의해 지원 AppXManifest "windows.protocol" 확장 합니다. 다음 코드 조각에서와 같이 수행할 수 있습니다.

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

이만 유니버설 Windows 응용 프로그램에 필요 합니다. Xbox Live 타이틀 ID 확장을 선언 하 여 자동으로 등록 된 대로 Xbox One 단독 리소스 ("XDK") 응용 프로그램 프로토콜 활성화 확장을 명시적으로 선언 하지 않습니다.

## <a name="xbox-one-exclusive-resource-application-network-manifest-templates"></a>단독 리소스 응용 프로그램 네트워크 Xbox One 매니페스트 템플릿

XIM을 사용 하는 모든 Xbox One 단독 리소스 응용 프로그램에 특정 한 소켓 설명 확인 해야 하 고 템플릿 콘텐츠가 AppXManifest 확장명이 "네트워크 매니페스트"에 포함 된 (네트워크 매니페스트 세부 정보에 대 한 플랫폼 설명서 참조). 앱도 다른 템플릿이 있을 수 있지만 다음 코드 조각은 있어야 합니다. 그렇지 실패 XIM 네트워크에 이동:

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

## <a name="universal-windows-application-uwp-network-manifest-templates"></a>유니버설 Windows 응용 프로그램 (UWP) 네트워크 매니페스트 템플릿

XIM을 사용 하는 모든 유니버설 Windows 응용 프로그램에 특정 한 소켓 설명 확인 해야 하 고 템플릿 콘텐츠가 해당 "네트워크 매니페스트" 파일에 포함 된 (패키지 루트 디렉터리에 networkmanifest.xml 파일 참조 세부 정보에 대 한 플랫폼 설명서 네트워크 매니페스트). 앱도 다른 템플릿이 있을 수 있지만 다음 코드 조각은 있어야 합니다. 그렇지 실패 XIM 네트워크에 이동:

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
