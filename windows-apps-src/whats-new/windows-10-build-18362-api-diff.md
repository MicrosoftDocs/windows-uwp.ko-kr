---
title: Windows 10 빌드 18362 API 변경 사항
description: 개발자는 다음 목록을 사용하여 Windows 10 빌드 18362에서 새롭게 추가되었거나 변경된 네임스페이스를 식별할 수 있습니다.
keywords: 새로운 기능, 새로운 기능, 업데이트, Windows 10, 최신, apis, 18362, 5월
ms.date: 04/19/2019
ms.topic: article
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: e230ac2716fc79449197a2f777b9262b2c237fe3
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/13/2019
ms.locfileid: "63780360"
---
# <a name="new-apis-in-windows-10-build-18362"></a>Windows 10 빌드 18362의 새로운 API

신규 및 업데이트 API 네임스페이스가 Windows 10 빌드 18362(SDK 버전 1903이라고도 함)의 개발자들에게 제공되었습니다. 아래는 이 릴리스에서 추가 또는 수정된 네임스페이스에 게시된 설명서의 전체 목록입니다.

이전의 공개 릴리스에 추가된 API에 대한 정보는 [Windows 10 10월 업데이트의 신규 API](windows-10-build-17763-api-diff.md)를 참조하세요.

## <a name="windowsai"></a>Windows.AI

### <a name="windowsaimachinelearninghttpsdocsmicrosoftcomuwpapiwindowsaimachinelearning"></a>[Windows.AI.MachineLearning](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning)

#### <a name="learningmodelsessionoptionshttpsdocsmicrosoftcomuwpapiwindowsaimachinelearninglearningmodelsessionoptions"></a>[LearningModelSessionOptions](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning.learningmodelsessionoptions)

LearningModelSessionOptions <br> LearningModelSessionOptions.BatchSizeOverride <br> LearningModelSessionOptions.#ctor

#### <a name="learningmodelsessionhttpsdocsmicrosoftcomuwpapiwindowsaimachinelearninglearningmodelsession"></a>[LearningModelSession](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning.learningmodelsession)

LearningModelSession.#ctor

#### <a name="tensorbooleanhttpsdocsmicrosoftcomuwpapiwindowsaimachinelearningtensorboolean"></a>[TensorBoolean](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning.tensorboolean)

TensorBoolean.Close <br> TensorBoolean.CreateFromBuffer <br> TensorBoolean.CreateFromShapeArrayAndDataArray <br> TensorBoolean.CreateReference

#### <a name="tensordoublehttpsdocsmicrosoftcomuwpapiwindowsaimachinelearningtensordouble"></a>[TensorDouble](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning.tensordouble)

TensorDouble.Close <br> TensorDouble.CreateFromBuffer <br> TensorDouble.CreateFromShapeArrayAndDataArray <br> TensorDouble.CreateReference

#### <a name="tensorfloat16bithttpsdocsmicrosoftcomuwpapiwindowsaimachinelearningtensorfloat16bit"></a>[TensorFloat16Bit](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning.tensorfloat16bit)

TensorFloat16Bit.Close <br> TensorFloat16Bit.CreateFromBuffer <br> TensorFloat16Bit.CreateFromShapeArrayAndDataArray <br> TensorFloat16Bit.CreateReference

#### <a name="tensorfloathttpsdocsmicrosoftcomuwpapiwindowsaimachinelearningtensorfloat"></a>[TensorFloat](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning.tensorfloat)

TensorFloat.Close <br> TensorFloat.CreateFromBuffer <br> TensorFloat.CreateFromShapeArrayAndDataArray <br> TensorFloat.CreateReference

#### <a name="tensorint16bithttpsdocsmicrosoftcomuwpapiwindowsaimachinelearningtensorint16bit"></a>[TensorInt16Bit](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning.tensorint16bit)

TensorInt16Bit.Close <br> TensorInt16Bit.CreateFromBuffer <br> TensorInt16Bit.CreateFromShapeArrayAndDataArray <br> TensorInt16Bit.CreateReference

#### <a name="tensorint32bithttpsdocsmicrosoftcomuwpapiwindowsaimachinelearningtensorint32bit"></a>[TensorInt32Bit](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning.tensorint32bit)

TensorInt32Bit.Close <br> TensorInt32Bit.CreateFromBuffer <br> TensorInt32Bit.CreateFromShapeArrayAndDataArray <br> TensorInt32Bit.CreateReference

#### <a name="tensorint64bithttpsdocsmicrosoftcomuwpapiwindowsaimachinelearningtensorint64bit"></a>[TensorInt64Bit](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning.tensorint64bit)

TensorInt64Bit.Close <br> TensorInt64Bit.CreateFromBuffer <br> TensorInt64Bit.CreateFromShapeArrayAndDataArray <br> TensorInt64Bit.CreateReference

#### <a name="tensorint8bithttpsdocsmicrosoftcomuwpapiwindowsaimachinelearningtensorint8bit"></a>[TensorInt8Bit](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning.tensorint8bit)

TensorInt8Bit.Close <br> TensorInt8Bit.CreateFromBuffer <br> TensorInt8Bit.CreateFromShapeArrayAndDataArray <br> TensorInt8Bit.CreateReference

#### <a name="tensorstringhttpsdocsmicrosoftcomuwpapiwindowsaimachinelearningtensorstring"></a>[TensorString](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning.tensorstring)

TensorString.Close <br> TensorString.CreateFromShapeArrayAndDataArray <br> TensorString.CreateReference

#### <a name="tensoruint16bithttpsdocsmicrosoftcomuwpapiwindowsaimachinelearningtensoruint16bit"></a>[TensorUInt16Bit](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning.tensoruint16bit)

TensorUInt16Bit.Close <br> TensorUInt16Bit.CreateFromBuffer <br> TensorUInt16Bit.CreateFromShapeArrayAndDataArray <br> TensorUInt16Bit.CreateReference

#### <a name="tensoruint32bithttpsdocsmicrosoftcomuwpapiwindowsaimachinelearningtensoruint32bit"></a>[TensorUInt32Bit](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning.tensoruint32bit)

TensorUInt32Bit.Close <br> TensorUInt32Bit.CreateFromBuffer <br> TensorUInt32Bit.CreateFromShapeArrayAndDataArray <br> TensorUInt32Bit.CreateReference

#### <a name="tensoruint64bithttpsdocsmicrosoftcomuwpapiwindowsaimachinelearningtensoruint64bit"></a>[TensorUInt64Bit](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning.tensoruint64bit)

TensorUInt64Bit.Close <br> TensorUInt64Bit.CreateFromBuffer <br> TensorUInt64Bit.CreateFromShapeArrayAndDataArray <br> TensorUInt64Bit.CreateReference

#### <a name="tensoruint8bithttpsdocsmicrosoftcomuwpapiwindowsaimachinelearningtensoruint8bit"></a>[TensorUInt8Bit](https://docs.microsoft.com/uwp/api/windows.ai.machinelearning.tensoruint8bit)

TensorUInt8Bit.Close <br> TensorUInt8Bit.CreateFromBuffer <br> TensorUInt8Bit.CreateFromShapeArrayAndDataArray <br> TensorUInt8Bit.CreateReference

## <a name="windowsapplicationmodel"></a>Windows.ApplicationModel

### <a name="windowsapplicationmodelhttpsdocsmicrosoftcomuwpapiwindowsapplicationmodel"></a>[Windows.ApplicationModel](https://docs.microsoft.com/uwp/api/windows.applicationmodel)

#### <a name="packagehttpsdocsmicrosoftcomuwpapiwindowsapplicationmodelpackage"></a>[Package](https://docs.microsoft.com/uwp/api/windows.applicationmodel.package)

Package.EffectiveLocation <br> Package.MutableLocation

### <a name="windowsapplicationmodelappservicehttpsdocsmicrosoftcomuwpapiwindowsapplicationmodelappservice"></a>[Windows.ApplicationModel.AppService](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appservice)

#### <a name="appserviceconnectionhttpsdocsmicrosoftcomuwpapiwindowsapplicationmodelappserviceappserviceconnection"></a>[AppServiceConnection](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appservice.appserviceconnection)

AppServiceConnection.SendStatelessMessageAsync

#### <a name="appservicetriggerdetailshttpsdocsmicrosoftcomuwpapiwindowsapplicationmodelappserviceappservicetriggerdetails"></a>[AppServiceTriggerDetails](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appservice.appservicetriggerdetails)

AppServiceTriggerDetails.CallerRemoteConnectionToken

#### <a name="statelessappserviceresponsehttpsdocsmicrosoftcomuwpapiwindowsapplicationmodelappservicestatelessappserviceresponse"></a>[StatelessAppServiceResponse](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appservice.statelessappserviceresponse)

StatelessAppServiceResponse

#### <a name="statelessappserviceresponsestatushttpsdocsmicrosoftcomuwpapiwindowsapplicationmodelappservicestatelessappserviceresponsestatus"></a>[StatelessAppServiceResponseStatus](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appservice.statelessappserviceresponsestatus)

StatelessAppServiceResponseStatus

#### <a name="statelessappserviceresponsehttpsdocsmicrosoftcomuwpapiwindowsapplicationmodelappservicestatelessappserviceresponse"></a>[StatelessAppServiceResponse](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appservice.statelessappserviceresponse)

StatelessAppServiceResponse.Message <br> StatelessAppServiceResponse.Status

### <a name="windowsapplicationmodelbackgroundhttpsdocsmicrosoftcomuwpapiwindowsapplicationmodelbackground"></a>[Windows.ApplicationModel.Background](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background)

#### <a name="conversationalagenttriggerhttpsdocsmicrosoftcomuwpapiwindowsapplicationmodelbackgroundconversationalagenttrigger"></a>[ConversationalAgentTrigger](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.conversationalagenttrigger)

ConversationalAgentTrigger <br> ConversationalAgentTrigger.#ctor

### <a name="windowsapplicationmodelcallshttpsdocsmicrosoftcomuwpapiwindowsapplicationmodelcalls"></a>[Windows.ApplicationModel.Calls](https://docs.microsoft.com/uwp/api/windows.applicationmodel.calls)

#### <a name="phonelinetransportdevicehttpsdocsmicrosoftcomuwpapiwindowsapplicationmodelcallsphonelinetransportdevice"></a>[PhoneLineTransportDevice](https://docs.microsoft.com/uwp/api/windows.applicationmodel.calls.phonelinetransportdevice)

PhoneLineTransportDevice <br> PhoneLineTransportDevice.ConnectAsync <br> PhoneLineTransportDevice.Connect <br> PhoneLineTransportDevice.DeviceId <br> PhoneLineTransportDevice.FromId <br> PhoneLineTransportDevice.GetDeviceSelector <br> PhoneLineTransportDevice.GetDeviceSelector <br> PhoneLineTransportDevice.IsRegistered <br> PhoneLineTransportDevice.RegisterAppForUser <br> PhoneLineTransportDevice.RegisterApp <br> PhoneLineTransportDevice.RequestAccessAsync <br> PhoneLineTransportDevice.Transport <br> PhoneLineTransportDevice.UnregisterAppForUser <br> PhoneLineTransportDevice.UnregisterApp

#### <a name="phonelinehttpsdocsmicrosoftcomuwpapiwindowsapplicationmodelcallsphoneline"></a>[PhoneLine](https://docs.microsoft.com/uwp/api/windows.applicationmodel.calls.phoneline)

PhoneLine.EnableTextReply <br> PhoneLine.TransportDeviceId

### <a name="windowsapplicationmodelcallsbackgroundhttpsdocsmicrosoftcomuwpapiwindowsapplicationmodelcallsbackground"></a>[Windows.ApplicationModel.Calls.Background](https://docs.microsoft.com/uwp/api/windows.applicationmodel.calls.background)

#### <a name="phoneincomingcalldismissedreasonhttpsdocsmicrosoftcomuwpapiwindowsapplicationmodelcallsbackgroundphoneincomingcalldismissedreason"></a>[PhoneIncomingCallDismissedReason](https://docs.microsoft.com/uwp/api/windows.applicationmodel.calls.background.phoneincomingcalldismissedreason)

PhoneIncomingCallDismissedReason

#### <a name="phoneincomingcalldismissedtriggerdetailshttpsdocsmicrosoftcomuwpapiwindowsapplicationmodelcallsbackgroundphoneincomingcalldismissedtriggerdetails"></a>[PhoneIncomingCallDismissedTriggerDetails](https://docs.microsoft.com/uwp/api/windows.applicationmodel.calls.background.phoneincomingcalldismissedtriggerdetails)

PhoneIncomingCallDismissedTriggerDetails <br> PhoneIncomingCallDismissedTriggerDetails.DismissalTime <br> PhoneIncomingCallDismissedTriggerDetails.DisplayName <br> PhoneIncomingCallDismissedTriggerDetails.LineId <br> PhoneIncomingCallDismissedTriggerDetails.PhoneNumber <br> PhoneIncomingCallDismissedTriggerDetails.Reason <br> PhoneIncomingCallDismissedTriggerDetails.TextReplyMessage

### <a name="windowsapplicationmodelcallsproviderhttpsdocsmicrosoftcomuwpapiwindowsapplicationmodelcallsprovider"></a>[Windows.ApplicationModel.Calls.Provider](https://docs.microsoft.com/uwp/api/windows.applicationmodel.calls.provider)

#### <a name="phonecalloriginmanagerhttpsdocsmicrosoftcomuwpapiwindowsapplicationmodelcallsproviderphonecalloriginmanager"></a>[PhoneCallOriginManager](https://docs.microsoft.com/uwp/api/windows.applicationmodel.calls.provider.phonecalloriginmanager)

PhoneCallOriginManager.IsSupported

### <a name="windowsapplicationmodelconversationalagenthttpsdocsmicrosoftcomuwpapiwindowsapplicationmodelconversationalagent"></a>[Windows.ApplicationModel.ConversationalAgent](https://docs.microsoft.com/uwp/api/windows.applicationmodel.conversationalagent)

#### <a name="conversationalagentsessionhttpsdocsmicrosoftcomuwpapiwindowsapplicationmodelconversationalagentconversationalagentsession"></a>[ConversationalAgentSession](https://docs.microsoft.com/uwp/api/windows.applicationmodel.conversationalagent.conversationalagentsession)

ConversationalAgentSession

#### <a name="conversationalagentsessioninterruptedeventargshttpsdocsmicrosoftcomuwpapiwindowsapplicationmodelconversationalagentconversationalagentsessioninterruptedeventargs"></a>[ConversationalAgentSessionInterruptedEventArgs](https://docs.microsoft.com/uwp/api/windows.applicationmodel.conversationalagent.conversationalagentsessioninterruptedeventargs)

ConversationalAgentSessionInterruptedEventArgs

#### <a name="conversationalagentsessionupdateresponsehttpsdocsmicrosoftcomuwpapiwindowsapplicationmodelconversationalagentconversationalagentsessionupdateresponse"></a>[ConversationalAgentSessionUpdateResponse](https://docs.microsoft.com/uwp/api/windows.applicationmodel.conversationalagent.conversationalagentsessionupdateresponse)

ConversationalAgentSessionUpdateResponse

#### <a name="conversationalagentsessionhttpsdocsmicrosoftcomuwpapiwindowsapplicationmodelconversationalagentconversationalagentsession"></a>[ConversationalAgentSession](https://docs.microsoft.com/uwp/api/windows.applicationmodel.conversationalagent.conversationalagentsession)

ConversationalAgentSession.AgentState <br> ConversationalAgentSession.Close <br> ConversationalAgentSession.CreateAudioDeviceInputNodeAsync <br> ConversationalAgentSession.CreateAudioDeviceInputNode <br> ConversationalAgentSession.GetAudioCaptureDeviceIdAsync <br> ConversationalAgentSession.GetAudioCaptureDeviceId <br> ConversationalAgentSession.GetAudioClientAsync <br> ConversationalAgentSession.GetAudioClient <br> ConversationalAgentSession.GetAudioRenderDeviceIdAsync <br> ConversationalAgentSession.GetAudioRenderDeviceId <br> ConversationalAgentSession.GetCurrentSessionAsync <br> ConversationalAgentSession.GetCurrentSessionSync <br> ConversationalAgentSession.GetSignalModelIdAsync <br> ConversationalAgentSession.GetSignalModelId <br> ConversationalAgentSession.GetSupportedSignalModelIdsAsync <br> ConversationalAgentSession.GetSupportedSignalModelIds <br> ConversationalAgentSession.IsIndicatorLightAvailable <br> ConversationalAgentSession.IsInterrupted <br> ConversationalAgentSession.IsInterruptible <br> ConversationalAgentSession.IsScreenAvailable <br> ConversationalAgentSession.IsUserAuthenticated <br> ConversationalAgentSession.IsVoiceActivationAvailable <br> ConversationalAgentSession.RequestAgentStateChangeAsync <br> ConversationalAgentSession.RequestAgentStateChange <br> ConversationalAgentSession.RequestForegroundActivationAsync <br> ConversationalAgentSession.RequestForegroundActivation <br> ConversationalAgentSession.RequestInterruptibleAsync <br> ConversationalAgentSession.RequestInterruptible <br> ConversationalAgentSession.SessionInterrupted <br> ConversationalAgentSession.SetSignalModelIdAsync <br> ConversationalAgentSession.SetSignalModelId <br> ConversationalAgentSession.Signal <br> ConversationalAgentSession.SignalDetected <br> ConversationalAgentSession.SystemStateChanged

#### <a name="conversationalagentsignalhttpsdocsmicrosoftcomuwpapiwindowsapplicationmodelconversationalagentconversationalagentsignal"></a>[ConversationalAgentSignal](https://docs.microsoft.com/uwp/api/windows.applicationmodel.conversationalagent.conversationalagentsignal)

ConversationalAgentSignal

#### <a name="conversationalagentsignaldetectedeventargshttpsdocsmicrosoftcomuwpapiwindowsapplicationmodelconversationalagentconversationalagentsignaldetectedeventargs"></a>[ConversationalAgentSignalDetectedEventArgs](https://docs.microsoft.com/uwp/api/windows.applicationmodel.conversationalagent.conversationalagentsignaldetectedeventargs)

ConversationalAgentSignalDetectedEventArgs

#### <a name="conversationalagentsignalhttpsdocsmicrosoftcomuwpapiwindowsapplicationmodelconversationalagentconversationalagentsignal"></a>[ConversationalAgentSignal](https://docs.microsoft.com/uwp/api/windows.applicationmodel.conversationalagent.conversationalagentsignal)

ConversationalAgentSignal.IsSignalVerificationRequired <br> ConversationalAgentSignal.SignalContext <br> ConversationalAgentSignal.SignalEnd <br> ConversationalAgentSignal.SignalId <br> ConversationalAgentSignal.SignalName <br> ConversationalAgentSignal.SignalStart

#### <a name="conversationalagentstatehttpsdocsmicrosoftcomuwpapiwindowsapplicationmodelconversationalagentconversationalagentstate"></a>[ConversationalAgentState](https://docs.microsoft.com/uwp/api/windows.applicationmodel.conversationalagent.conversationalagentstate)

ConversationalAgentState

#### <a name="conversationalagentsystemstatechangedeventargshttpsdocsmicrosoftcomuwpapiwindowsapplicationmodelconversationalagentconversationalagentsystemstatechangedeventargs"></a>[ConversationalAgentSystemStateChangedEventArgs](https://docs.microsoft.com/uwp/api/windows.applicationmodel.conversationalagent.conversationalagentsystemstatechangedeventargs)

ConversationalAgentSystemStateChangedEventArgs <br> ConversationalAgentSystemStateChangedEventArgs.SystemStateChangeType

#### <a name="conversationalagentsystemstatechangetypehttpsdocsmicrosoftcomuwpapiwindowsapplicationmodelconversationalagentconversationalagentsystemstatechangetype"></a>[ConversationalAgentSystemStateChangeType](https://docs.microsoft.com/uwp/api/windows.applicationmodel.conversationalagent.conversationalagentsystemstatechangetype)

ConversationalAgentSystemStateChangeType

### <a name="windowsapplicationmodelpreviewholographichttpsdocsmicrosoftcomuwpapiwindowsapplicationmodelpreviewholographic"></a>[Windows.ApplicationModel.Preview.Holographic](https://docs.microsoft.com/uwp/api/windows.applicationmodel.preview.holographic)

#### <a name="holographickeyboardplacementoverridepreviewhttpsdocsmicrosoftcomuwpapiwindowsapplicationmodelpreviewholographicholographickeyboardplacementoverridepreview"></a>[HolographicKeyboardPlacementOverridePreview](https://docs.microsoft.com/uwp/api/windows.applicationmodel.preview.holographic.holographickeyboardplacementoverridepreview)

HolographicKeyboardPlacementOverridePreview <br> HolographicKeyboardPlacementOverridePreview.GetForCurrentView <br> HolographicKeyboardPlacementOverridePreview.ResetPlacementOverride <br> HolographicKeyboardPlacementOverridePreview.SetPlacementOverride <br> HolographicKeyboardPlacementOverridePreview.SetPlacementOverride

### <a name="windowsapplicationmodelresourceshttpsdocsmicrosoftcomuwpapiwindowsapplicationmodelresources"></a>[Windows.ApplicationModel.Resources](https://docs.microsoft.com/uwp/api/windows.applicationmodel.resources)

#### <a name="resourceloaderhttpsdocsmicrosoftcomuwpapiwindowsapplicationmodelresourcesresourceloader"></a>[ResourceLoader](https://docs.microsoft.com/uwp/api/windows.applicationmodel.resources.resourceloader)

ResourceLoader.GetForUIContext

### <a name="windowsapplicationmodelresourcescorehttpsdocsmicrosoftcomuwpapiwindowsapplicationmodelresourcescore"></a>[Windows.ApplicationModel.Resources.Core](https://docs.microsoft.com/uwp/api/windows.applicationmodel.resources.core)

#### <a name="resourcecandidatekindhttpsdocsmicrosoftcomuwpapiwindowsapplicationmodelresourcescoreresourcecandidatekind"></a>[ResourceCandidateKind](https://docs.microsoft.com/uwp/api/windows.applicationmodel.resources.core.resourcecandidatekind)

ResourceCandidateKind

#### <a name="resourcecandidatehttpsdocsmicrosoftcomuwpapiwindowsapplicationmodelresourcescoreresourcecandidate"></a>[ResourceCandidate](https://docs.microsoft.com/uwp/api/windows.applicationmodel.resources.core.resourcecandidate)

ResourceCandidate.Kind

#### <a name="resourcecontexthttpsdocsmicrosoftcomuwpapiwindowsapplicationmodelresourcescoreresourcecontext"></a>[ResourceContext](https://docs.microsoft.com/uwp/api/windows.applicationmodel.resources.core.resourcecontext)

ResourceContext.GetForUIContext

### <a name="windowsapplicationmodeluseractivitieshttpsdocsmicrosoftcomuwpapiwindowsapplicationmodeluseractivities"></a>[Windows.ApplicationModel.UserActivities](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities)

#### <a name="useractivitychannelhttpsdocsmicrosoftcomuwpapiwindowsapplicationmodeluseractivitiesuseractivitychannel"></a>[UserActivityChannel](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivitychannel)

UserActivityChannel.GetForUser

## <a name="windowsdevices"></a>Windows.Devices

### <a name="windowsdevicesbluetoothgenericattributeprofilehttpsdocsmicrosoftcomuwpapiwindowsdevicesbluetoothgenericattributeprofile"></a>[Windows.Devices.Bluetooth.GenericAttributeProfile](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.genericattributeprofile)

#### <a name="gattserviceprovideradvertisingparametershttpsdocsmicrosoftcomuwpapiwindowsdevicesbluetoothgenericattributeprofilegattserviceprovideradvertisingparameters"></a>[GattServiceProviderAdvertisingParameters](https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.genericattributeprofile.gattserviceprovideradvertisingparameters)

GattServiceProviderAdvertisingParameters.ServiceData

### <a name="windowsdevicesenumerationhttpsdocsmicrosoftcomuwpapiwindowsdevicesenumeration"></a>[Windows.Devices.Enumeration](https://docs.microsoft.com/uwp/api/windows.devices.enumeration)

#### <a name="devicepairingrequestedeventargshttpsdocsmicrosoftcomuwpapiwindowsdevicesenumerationdevicepairingrequestedeventargs"></a>[DevicePairingRequestedEventArgs](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.devicepairingrequestedeventargs)

DevicePairingRequestedEventArgs.AcceptWithPasswordCredential

### <a name="windowsdevicesinputhttpsdocsmicrosoftcomuwpapiwindowsdevicesinput"></a>[Windows.Devices.Input](https://docs.microsoft.com/uwp/api/windows.devices.input)

#### <a name="pendevicehttpsdocsmicrosoftcomuwpapiwindowsdevicesinputpendevice"></a>[PenDevice](https://docs.microsoft.com/uwp/api/windows.devices.input.pendevice)

PenDevice <br> PenDevice.GetFromPointerId <br> PenDevice.PenId

### <a name="windowsdevicespointofservicehttpsdocsmicrosoftcomuwpapiwindowsdevicespointofservice"></a>[Windows.Devices.PointOfService](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice)

#### <a name="journalprintercapabilitieshttpsdocsmicrosoftcomuwpapiwindowsdevicespointofservicejournalprintercapabilities"></a>[JournalPrinterCapabilities](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.journalprintercapabilities)

JournalPrinterCapabilities.IsReversePaperFeedByLineSupported <br> JournalPrinterCapabilities.IsReversePaperFeedByMapModeUnitSupported <br> JournalPrinterCapabilities.IsReverseVideoSupported <br> JournalPrinterCapabilities.IsStrikethroughSupported <br> JournalPrinterCapabilities.IsSubscriptSupported <br> JournalPrinterCapabilities.IsSuperscriptSupported

#### <a name="journalprintjobhttpsdocsmicrosoftcomuwpapiwindowsdevicespointofservicejournalprintjob"></a>[JournalPrintJob](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.journalprintjob)

JournalPrintJob.FeedPaperByLine <br> JournalPrintJob.FeedPaperByMapModeUnit <br> JournalPrintJob.Print

#### <a name="posprinterfontpropertyhttpsdocsmicrosoftcomuwpapiwindowsdevicespointofserviceposprinterfontproperty"></a>[PosPrinterFontProperty](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinterfontproperty)

PosPrinterFontProperty <br> PosPrinterFontProperty.CharacterSizes <br> PosPrinterFontProperty.IsScalableToAnySize <br> PosPrinterFontProperty.TypeFace

#### <a name="posprinterprintoptionshttpsdocsmicrosoftcomuwpapiwindowsdevicespointofserviceposprinterprintoptions"></a>[PosPrinterPrintOptions](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinterprintoptions)

PosPrinterPrintOptions <br> PosPrinterPrintOptions.Alignment <br> PosPrinterPrintOptions.Bold <br> PosPrinterPrintOptions.CharacterHeight <br> PosPrinterPrintOptions.CharacterSet <br> PosPrinterPrintOptions.DoubleHigh <br> PosPrinterPrintOptions.DoubleWide <br> PosPrinterPrintOptions.Italic <br> PosPrinterPrintOptions.#ctor <br> PosPrinterPrintOptions.ReverseVideo <br> PosPrinterPrintOptions.Strikethrough <br> PosPrinterPrintOptions.Subscript <br> PosPrinterPrintOptions.Superscript <br> PosPrinterPrintOptions.TypeFace <br> PosPrinterPrintOptions.Underline

#### <a name="posprinterhttpsdocsmicrosoftcomuwpapiwindowsdevicespointofserviceposprinter"></a>[PosPrinter](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.posprinter)

PosPrinter.GetFontProperty <br> PosPrinter.SupportedBarcodeSymbologies

#### <a name="receiptprintercapabilitieshttpsdocsmicrosoftcomuwpapiwindowsdevicespointofservicereceiptprintercapabilities"></a>[ReceiptPrinterCapabilities](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.receiptprintercapabilities)

ReceiptPrinterCapabilities.IsReversePaperFeedByLineSupported <br> ReceiptPrinterCapabilities.IsReversePaperFeedByMapModeUnitSupported <br> ReceiptPrinterCapabilities.IsReverseVideoSupported <br> ReceiptPrinterCapabilities.IsStrikethroughSupported <br> ReceiptPrinterCapabilities.IsSubscriptSupported <br> ReceiptPrinterCapabilities.IsSuperscriptSupported

#### <a name="receiptprintjobhttpsdocsmicrosoftcomuwpapiwindowsdevicespointofservicereceiptprintjob"></a>[ReceiptPrintJob](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.receiptprintjob)

ReceiptPrintJob.FeedPaperByLine <br> ReceiptPrintJob.FeedPaperByMapModeUnit <br> ReceiptPrintJob.Print <br> ReceiptPrintJob.StampPaper

#### <a name="sizeuint32httpsdocsmicrosoftcomuwpapiwindowsdevicespointofservicesizeuint32"></a>[SizeUInt32](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.sizeuint32)

SizeUInt32

#### <a name="slipprintercapabilitieshttpsdocsmicrosoftcomuwpapiwindowsdevicespointofserviceslipprintercapabilities"></a>[SlipPrinterCapabilities](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.slipprintercapabilities)

SlipPrinterCapabilities.IsReversePaperFeedByLineSupported <br> SlipPrinterCapabilities.IsReversePaperFeedByMapModeUnitSupported <br> SlipPrinterCapabilities.IsReverseVideoSupported <br> SlipPrinterCapabilities.IsStrikethroughSupported <br> SlipPrinterCapabilities.IsSubscriptSupported <br> SlipPrinterCapabilities.IsSuperscriptSupported

#### <a name="slipprintjobhttpsdocsmicrosoftcomuwpapiwindowsdevicespointofserviceslipprintjob"></a>[SlipPrintJob](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.slipprintjob)

SlipPrintJob.FeedPaperByLine <br> SlipPrintJob.FeedPaperByMapModeUnit <br> SlipPrintJob.Print

## <a name="windowsglobalization"></a>Windows.Globalization

### <a name="windowsglobalizationhttpsdocsmicrosoftcomuwpapiwindowsglobalization"></a>[Windows.Globalization](https://docs.microsoft.com/uwp/api/windows.globalization)

#### <a name="currencyamounthttpsdocsmicrosoftcomuwpapiwindowsglobalizationcurrencyamount"></a>[CurrencyAmount](https://docs.microsoft.com/uwp/api/windows.globalization.currencyamount)

CurrencyAmount <br> CurrencyAmount.Amount <br> CurrencyAmount.Currency <br> CurrencyAmount.#ctor

## <a name="windowsgraphics"></a>Windows.Graphics

### <a name="windowsgraphicsdirectxhttpsdocsmicrosoftcomuwpapiwindowsgraphicsdirectx"></a>[Windows.Graphics.DirectX](https://docs.microsoft.com/uwp/api/windows.graphics.directx)

#### <a name="directxprimitivetopologyhttpsdocsmicrosoftcomuwpapiwindowsgraphicsdirectxdirectxprimitivetopology"></a>[DirectXPrimitiveTopology](https://docs.microsoft.com/uwp/api/windows.graphics.directx.directxprimitivetopology)

DirectXPrimitiveTopology

### <a name="windowsgraphicsholographichttpsdocsmicrosoftcomuwpapiwindowsgraphicsholographic"></a>[Windows.Graphics.Holographic](https://docs.microsoft.com/uwp/api/windows.graphics.holographic)

#### <a name="holographiccamerahttpsdocsmicrosoftcomuwpapiwindowsgraphicsholographicholographiccamera"></a>[HolographicCamera](https://docs.microsoft.com/uwp/api/windows.graphics.holographic.holographiccamera)

HolographicCamera.ViewConfiguration

#### <a name="holographicdisplayhttpsdocsmicrosoftcomuwpapiwindowsgraphicsholographicholographicdisplay"></a>[HolographicDisplay](https://docs.microsoft.com/uwp/api/windows.graphics.holographic.holographicdisplay)

HolographicDisplay.TryGetViewConfiguration

#### <a name="holographicviewconfigurationhttpsdocsmicrosoftcomuwpapiwindowsgraphicsholographicholographicviewconfiguration"></a>[HolographicViewConfiguration](https://docs.microsoft.com/uwp/api/windows.graphics.holographic.holographicviewconfiguration)

HolographicViewConfiguration

#### <a name="holographicviewconfigurationkindhttpsdocsmicrosoftcomuwpapiwindowsgraphicsholographicholographicviewconfigurationkind"></a>[HolographicViewConfigurationKind](https://docs.microsoft.com/uwp/api/windows.graphics.holographic.holographicviewconfigurationkind)

HolographicViewConfigurationKind

#### <a name="holographicviewconfigurationhttpsdocsmicrosoftcomuwpapiwindowsgraphicsholographicholographicviewconfiguration"></a>[HolographicViewConfiguration](https://docs.microsoft.com/uwp/api/windows.graphics.holographic.holographicviewconfiguration)

HolographicViewConfiguration.Display <br> HolographicViewConfiguration.IsEnabled <br> HolographicViewConfiguration.IsStereo <br> HolographicViewConfiguration.Kind <br> HolographicViewConfiguration.NativeRenderTargetSize <br> HolographicViewConfiguration.PixelFormat <br> HolographicViewConfiguration.RefreshRate <br> HolographicViewConfiguration.RenderTargetSize <br> HolographicViewConfiguration.RequestRenderTargetSize <br> HolographicViewConfiguration.SupportedPixelFormats

## <a name="windowsmedia"></a>Windows.Media

### <a name="windowsmediadeviceshttpsdocsmicrosoftcomuwpapiwindowsmediadevices"></a>[Windows.Media.Devices](https://docs.microsoft.com/uwp/api/windows.media.devices)

#### <a name="infraredtorchcontrolhttpsdocsmicrosoftcomuwpapiwindowsmediadevicesinfraredtorchcontrol"></a>[InfraredTorchControl](https://docs.microsoft.com/uwp/api/windows.media.devices.infraredtorchcontrol)

InfraredTorchControl <br> InfraredTorchControl.CurrentMode <br> InfraredTorchControl.IsSupported <br> InfraredTorchControl.MaxPower <br> InfraredTorchControl.MinPower <br> InfraredTorchControl.Power <br> InfraredTorchControl.PowerStep <br> InfraredTorchControl.SupportedModes

#### <a name="infraredtorchmodehttpsdocsmicrosoftcomuwpapiwindowsmediadevicesinfraredtorchmode"></a>[InfraredTorchMode](https://docs.microsoft.com/uwp/api/windows.media.devices.infraredtorchmode)

InfraredTorchMode

#### <a name="videodevicecontrollerhttpsdocsmicrosoftcomuwpapiwindowsmediadevicesvideodevicecontroller"></a>[VideoDeviceController](https://docs.microsoft.com/uwp/api/windows.media.devices.videodevicecontroller)

VideoDeviceController.InfraredTorchControl

### <a name="windowsmediamiracasthttpsdocsmicrosoftcomuwpapiwindowsmediamiracast"></a>[Windows.Media.Miracast](https://docs.microsoft.com/uwp/api/windows.media.miracast)

#### <a name="miracastreceiverhttpsdocsmicrosoftcomuwpapiwindowsmediamiracastmiracastreceiver"></a>[MiracastReceiver](https://docs.microsoft.com/uwp/api/windows.media.miracast.miracastreceiver)

MiracastReceiver

#### <a name="miracastreceiverapplysettingsresulthttpsdocsmicrosoftcomuwpapiwindowsmediamiracastmiracastreceiverapplysettingsresult"></a>[MiracastReceiverApplySettingsResult](https://docs.microsoft.com/uwp/api/windows.media.miracast.miracastreceiverapplysettingsresult)

MiracastReceiverApplySettingsResult <br> MiracastReceiverApplySettingsResult.ExtendedError <br> MiracastReceiverApplySettingsResult.Status

#### <a name="miracastreceiverapplysettingsstatushttpsdocsmicrosoftcomuwpapiwindowsmediamiracastmiracastreceiverapplysettingsstatus"></a>[MiracastReceiverApplySettingsStatus](https://docs.microsoft.com/uwp/api/windows.media.miracast.miracastreceiverapplysettingsstatus)

MiracastReceiverApplySettingsStatus

#### <a name="miracastreceiverauthorizationmethodhttpsdocsmicrosoftcomuwpapiwindowsmediamiracastmiracastreceiverauthorizationmethod"></a>[MiracastReceiverAuthorizationMethod](https://docs.microsoft.com/uwp/api/windows.media.miracast.miracastreceiverauthorizationmethod)

MiracastReceiverAuthorizationMethod

#### <a name="miracastreceiverconnectionhttpsdocsmicrosoftcomuwpapiwindowsmediamiracastmiracastreceiverconnection"></a>[MiracastReceiverConnection](https://docs.microsoft.com/uwp/api/windows.media.miracast.miracastreceiverconnection)

MiracastReceiverConnection

#### <a name="miracastreceiverconnectioncreatedeventargshttpsdocsmicrosoftcomuwpapiwindowsmediamiracastmiracastreceiverconnectioncreatedeventargs"></a>[MiracastReceiverConnectionCreatedEventArgs](https://docs.microsoft.com/uwp/api/windows.media.miracast.miracastreceiverconnectioncreatedeventargs)

MiracastReceiverConnectionCreatedEventArgs <br> MiracastReceiverConnectionCreatedEventArgs.Connection <br> MiracastReceiverConnectionCreatedEventArgs.GetDeferral <br> MiracastReceiverConnectionCreatedEventArgs.Pin

#### <a name="miracastreceiverconnectionhttpsdocsmicrosoftcomuwpapiwindowsmediamiracastmiracastreceiverconnection"></a>[MiracastReceiverConnection](https://docs.microsoft.com/uwp/api/windows.media.miracast.miracastreceiverconnection)

MiracastReceiverConnection.Close <br> MiracastReceiverConnection.CursorImageChannel <br> MiracastReceiverConnection.Disconnect <br> MiracastReceiverConnection.Disconnect <br> MiracastReceiverConnection.InputDevices <br> MiracastReceiverConnection.PauseAsync <br> MiracastReceiverConnection.Pause <br> MiracastReceiverConnection.ResumeAsync <br> MiracastReceiverConnection.Resume <br> MiracastReceiverConnection.StreamControl <br> MiracastReceiverConnection.Transmitter

#### <a name="miracastreceivercursorimagechannelhttpsdocsmicrosoftcomuwpapiwindowsmediamiracastmiracastreceivercursorimagechannel"></a>[MiracastReceiverCursorImageChannel](https://docs.microsoft.com/uwp/api/windows.media.miracast.miracastreceivercursorimagechannel)

MiracastReceiverCursorImageChannel

#### <a name="miracastreceivercursorimagechannelsettingshttpsdocsmicrosoftcomuwpapiwindowsmediamiracastmiracastreceivercursorimagechannelsettings"></a>[MiracastReceiverCursorImageChannelSettings](https://docs.microsoft.com/uwp/api/windows.media.miracast.miracastreceivercursorimagechannelsettings)

MiracastReceiverCursorImageChannelSettings <br> MiracastReceiverCursorImageChannelSettings.IsEnabled <br> MiracastReceiverCursorImageChannelSettings.MaxImageSize

#### <a name="miracastreceivercursorimagechannelhttpsdocsmicrosoftcomuwpapiwindowsmediamiracastmiracastreceivercursorimagechannel"></a>[MiracastReceiverCursorImageChannel](https://docs.microsoft.com/uwp/api/windows.media.miracast.miracastreceivercursorimagechannel)

MiracastReceiverCursorImageChannel.ImageStream <br> MiracastReceiverCursorImageChannel.ImageStreamChanged <br> MiracastReceiverCursorImageChannel.IsEnabled <br> MiracastReceiverCursorImageChannel.MaxImageSize <br> MiracastReceiverCursorImageChannel.Position <br> MiracastReceiverCursorImageChannel.PositionChanged

#### <a name="miracastreceiverdisconnectedeventargshttpsdocsmicrosoftcomuwpapiwindowsmediamiracastmiracastreceiverdisconnectedeventargs"></a>[MiracastReceiverDisconnectedEventArgs](https://docs.microsoft.com/uwp/api/windows.media.miracast.miracastreceiverdisconnectedeventargs)

MiracastReceiverDisconnectedEventArgs <br> MiracastReceiverDisconnectedEventArgs.Connection

#### <a name="miracastreceiverdisconnectreasonhttpsdocsmicrosoftcomuwpapiwindowsmediamiracastmiracastreceiverdisconnectreason"></a>[MiracastReceiverDisconnectReason](https://docs.microsoft.com/uwp/api/windows.media.miracast.miracastreceiverdisconnectreason)

MiracastReceiverDisconnectReason

#### <a name="miracastreceivergamecontrollerdevicehttpsdocsmicrosoftcomuwpapiwindowsmediamiracastmiracastreceivergamecontrollerdevice"></a>[MiracastReceiverGameControllerDevice](https://docs.microsoft.com/uwp/api/windows.media.miracast.miracastreceivergamecontrollerdevice)

MiracastReceiverGameControllerDevice

#### <a name="miracastreceivergamecontrollerdeviceusagemodehttpsdocsmicrosoftcomuwpapiwindowsmediamiracastmiracastreceivergamecontrollerdeviceusagemode"></a>[MiracastReceiverGameControllerDeviceUsageMode](https://docs.microsoft.com/uwp/api/windows.media.miracast.miracastreceivergamecontrollerdeviceusagemode)

MiracastReceiverGameControllerDeviceUsageMode

#### <a name="miracastreceivergamecontrollerdevicehttpsdocsmicrosoftcomuwpapiwindowsmediamiracastmiracastreceivergamecontrollerdevice"></a>[MiracastReceiverGameControllerDevice](https://docs.microsoft.com/uwp/api/windows.media.miracast.miracastreceivergamecontrollerdevice)

MiracastReceiverGameControllerDevice.Changed <br> MiracastReceiverGameControllerDevice.IsRequestedByTransmitter <br> MiracastReceiverGameControllerDevice.IsTransmittingInput <br> MiracastReceiverGameControllerDevice.Mode <br> MiracastReceiverGameControllerDevice.TransmitInput

#### <a name="miracastreceiverinputdeviceshttpsdocsmicrosoftcomuwpapiwindowsmediamiracastmiracastreceiverinputdevices"></a>[MiracastReceiverInputDevices](https://docs.microsoft.com/uwp/api/windows.media.miracast.miracastreceiverinputdevices)

MiracastReceiverInputDevices <br> MiracastReceiverInputDevices.GameController <br> MiracastReceiverInputDevices.Keyboard

#### <a name="miracastreceiverkeyboarddevicehttpsdocsmicrosoftcomuwpapiwindowsmediamiracastmiracastreceiverkeyboarddevice"></a>[MiracastReceiverKeyboardDevice](https://docs.microsoft.com/uwp/api/windows.media.miracast.miracastreceiverkeyboarddevice)

MiracastReceiverKeyboardDevice <br> MiracastReceiverKeyboardDevice.Changed <br> MiracastReceiverKeyboardDevice.IsRequestedByTransmitter <br> MiracastReceiverKeyboardDevice.IsTransmittingInput <br> MiracastReceiverKeyboardDevice.TransmitInput

#### <a name="miracastreceiverlisteningstatushttpsdocsmicrosoftcomuwpapiwindowsmediamiracastmiracastreceiverlisteningstatus"></a>[MiracastReceiverListeningStatus](https://docs.microsoft.com/uwp/api/windows.media.miracast.miracastreceiverlisteningstatus)

MiracastReceiverListeningStatus

#### <a name="miracastreceivermediasourcecreatedeventargshttpsdocsmicrosoftcomuwpapiwindowsmediamiracastmiracastreceivermediasourcecreatedeventargs"></a>[MiracastReceiverMediaSourceCreatedEventArgs](https://docs.microsoft.com/uwp/api/windows.media.miracast.miracastreceivermediasourcecreatedeventargs)

MiracastReceiverMediaSourceCreatedEventArgs <br> MiracastReceiverMediaSourceCreatedEventArgs.Connection <br> MiracastReceiverMediaSourceCreatedEventArgs.CursorImageChannelSettings <br> MiracastReceiverMediaSourceCreatedEventArgs.GetDeferral <br> MiracastReceiverMediaSourceCreatedEventArgs.MediaSource

#### <a name="miracastreceiversessionhttpsdocsmicrosoftcomuwpapiwindowsmediamiracastmiracastreceiversession"></a>[MiracastReceiverSession](https://docs.microsoft.com/uwp/api/windows.media.miracast.miracastreceiversession)

MiracastReceiverSession

#### <a name="miracastreceiversessionstartresulthttpsdocsmicrosoftcomuwpapiwindowsmediamiracastmiracastreceiversessionstartresult"></a>[MiracastReceiverSessionStartResult](https://docs.microsoft.com/uwp/api/windows.media.miracast.miracastreceiversessionstartresult)

MiracastReceiverSessionStartResult <br> MiracastReceiverSessionStartResult.ExtendedError <br> MiracastReceiverSessionStartResult.Status

#### <a name="miracastreceiversessionstartstatushttpsdocsmicrosoftcomuwpapiwindowsmediamiracastmiracastreceiversessionstartstatus"></a>[MiracastReceiverSessionStartStatus](https://docs.microsoft.com/uwp/api/windows.media.miracast.miracastreceiversessionstartstatus)

MiracastReceiverSessionStartStatus

#### <a name="miracastreceiversessionhttpsdocsmicrosoftcomuwpapiwindowsmediamiracastmiracastreceiversession"></a>[MiracastReceiverSession](https://docs.microsoft.com/uwp/api/windows.media.miracast.miracastreceiversession)

MiracastReceiverSession.AllowConnectionTakeover <br> MiracastReceiverSession.Close <br> MiracastReceiverSession.ConnectionCreated <br> MiracastReceiverSession.Disconnected <br> MiracastReceiverSession.MaxSimultaneousConnections <br> MiracastReceiverSession.MediaSourceCreated <br> MiracastReceiverSession.StartAsync <br> MiracastReceiverSession.Start

#### <a name="miracastreceiversettingshttpsdocsmicrosoftcomuwpapiwindowsmediamiracastmiracastreceiversettings"></a>[MiracastReceiverSettings](https://docs.microsoft.com/uwp/api/windows.media.miracast.miracastreceiversettings)

MiracastReceiverSettings <br> MiracastReceiverSettings.AuthorizationMethod <br> MiracastReceiverSettings.FriendlyName <br> MiracastReceiverSettings.ModelName <br> MiracastReceiverSettings.ModelNumber <br> MiracastReceiverSettings.RequireAuthorizationFromKnownTransmitters

#### <a name="miracastreceiverstatushttpsdocsmicrosoftcomuwpapiwindowsmediamiracastmiracastreceiverstatus"></a>[MiracastReceiverStatus](https://docs.microsoft.com/uwp/api/windows.media.miracast.miracastreceiverstatus)

MiracastReceiverStatus <br> MiracastReceiverStatus.IsConnectionTakeoverSupported <br> MiracastReceiverStatus.KnownTransmitters <br> MiracastReceiverStatus.ListeningStatus <br> MiracastReceiverStatus.MaxSimultaneousConnections <br> MiracastReceiverStatus.WiFiStatus

#### <a name="miracastreceiverstreamcontrolhttpsdocsmicrosoftcomuwpapiwindowsmediamiracastmiracastreceiverstreamcontrol"></a>[MiracastReceiverStreamControl](https://docs.microsoft.com/uwp/api/windows.media.miracast.miracastreceiverstreamcontrol)

MiracastReceiverStreamControl <br> MiracastReceiverStreamControl.GetVideoStreamSettingsAsync <br> MiracastReceiverStreamControl.GetVideoStreamSettings <br> MiracastReceiverStreamControl.MuteAudio <br> MiracastReceiverStreamControl.SuggestVideoStreamSettingsAsync <br> MiracastReceiverStreamControl.SuggestVideoStreamSettings

#### <a name="miracastreceivervideostreamsettingshttpsdocsmicrosoftcomuwpapiwindowsmediamiracastmiracastreceivervideostreamsettings"></a>[MiracastReceiverVideoStreamSettings](https://docs.microsoft.com/uwp/api/windows.media.miracast.miracastreceivervideostreamsettings)

MiracastReceiverVideoStreamSettings <br> MiracastReceiverVideoStreamSettings.Bitrate <br> MiracastReceiverVideoStreamSettings.Size

#### <a name="miracastreceiverwifistatushttpsdocsmicrosoftcomuwpapiwindowsmediamiracastmiracastreceiverwifistatus"></a>[MiracastReceiverWiFiStatus](https://docs.microsoft.com/uwp/api/windows.media.miracast.miracastreceiverwifistatus)

MiracastReceiverWiFiStatus

#### <a name="miracastreceiverhttpsdocsmicrosoftcomuwpapiwindowsmediamiracastmiracastreceiver"></a>[MiracastReceiver](https://docs.microsoft.com/uwp/api/windows.media.miracast.miracastreceiver)

MiracastReceiver.ClearKnownTransmitters <br> MiracastReceiver.CreateSessionAsync <br> MiracastReceiver.CreateSession <br> MiracastReceiver.DisconnectAllAndApplySettingsAsync <br> MiracastReceiver.DisconnectAllAndApplySettings <br> MiracastReceiver.GetCurrentSettingsAsync <br> MiracastReceiver.GetCurrentSettings <br> MiracastReceiver.GetDefaultSettings <br> MiracastReceiver.GetStatusAsync <br> MiracastReceiver.GetStatus <br> MiracastReceiver.#ctor <br> MiracastReceiver.RemoveKnownTransmitter <br> MiracastReceiver.StatusChanged

#### <a name="miracasttransmitterhttpsdocsmicrosoftcomuwpapiwindowsmediamiracastmiracasttransmitter"></a>[MiracastTransmitter](https://docs.microsoft.com/uwp/api/windows.media.miracast.miracasttransmitter)

MiracastTransmitter

#### <a name="miracasttransmitterauthorizationstatushttpsdocsmicrosoftcomuwpapiwindowsmediamiracastmiracasttransmitterauthorizationstatus"></a>[MiracastTransmitterAuthorizationStatus](https://docs.microsoft.com/uwp/api/windows.media.miracast.miracasttransmitterauthorizationstatus)

MiracastTransmitterAuthorizationStatus

#### <a name="miracasttransmitterhttpsdocsmicrosoftcomuwpapiwindowsmediamiracastmiracasttransmitter"></a>[MiracastTransmitter](https://docs.microsoft.com/uwp/api/windows.media.miracast.miracasttransmitter)

MiracastTransmitter.AuthorizationStatus <br> MiracastTransmitter.GetConnections <br> MiracastTransmitter.LastConnectionTime <br> MiracastTransmitter.MacAddress <br> MiracastTransmitter.Name

## <a name="windowsnetworking"></a>Windows.Networking

### <a name="windowsnetworkingnetworkoperatorshttpsdocsmicrosoftcomuwpapiwindowsnetworkingnetworkoperators"></a>[Windows.Networking.NetworkOperators](https://docs.microsoft.com/uwp/api/windows.networking.networkoperators)

#### <a name="esimdiscovereventhttpsdocsmicrosoftcomuwpapiwindowsnetworkingnetworkoperatorsesimdiscoverevent"></a>[ESimDiscoverEvent](https://docs.microsoft.com/uwp/api/windows.networking.networkoperators.esimdiscoverevent)

ESimDiscoverEvent <br> ESimDiscoverEvent.MatchingId <br> ESimDiscoverEvent.RspServerAddress

#### <a name="esimdiscoverresulthttpsdocsmicrosoftcomuwpapiwindowsnetworkingnetworkoperatorsesimdiscoverresult"></a>[ESimDiscoverResult](https://docs.microsoft.com/uwp/api/windows.networking.networkoperators.esimdiscoverresult)

ESimDiscoverResult

#### <a name="esimdiscoverresultkindhttpsdocsmicrosoftcomuwpapiwindowsnetworkingnetworkoperatorsesimdiscoverresultkind"></a>[ESimDiscoverResultKind](https://docs.microsoft.com/uwp/api/windows.networking.networkoperators.esimdiscoverresultkind)

ESimDiscoverResultKind

#### <a name="esimdiscoverresulthttpsdocsmicrosoftcomuwpapiwindowsnetworkingnetworkoperatorsesimdiscoverresult"></a>[ESimDiscoverResult](https://docs.microsoft.com/uwp/api/windows.networking.networkoperators.esimdiscoverresult)

ESimDiscoverResult.Events <br> ESimDiscoverResult.Kind <br> ESimDiscoverResult.ProfileMetadata <br> ESimDiscoverResult.Result

#### <a name="esimhttpsdocsmicrosoftcomuwpapiwindowsnetworkingnetworkoperatorsesim"></a>[ESim](https://docs.microsoft.com/uwp/api/windows.networking.networkoperators.esim)

ESim.DiscoverAsync <br> ESim.DiscoverAsync <br> ESim.Discover <br> ESim.Discover

### <a name="windowsnetworkingpushnotificationshttpsdocsmicrosoftcomuwpapiwindowsnetworkingpushnotifications"></a>[Windows.Networking.PushNotifications](https://docs.microsoft.com/uwp/api/windows.networking.pushnotifications)

#### <a name="pushnotificationchannelmanagerhttpsdocsmicrosoftcomuwpapiwindowsnetworkingpushnotificationspushnotificationchannelmanager"></a>[PushNotificationChannelManager](https://docs.microsoft.com/uwp/api/windows.networking.pushnotifications.pushnotificationchannelmanager)

PushNotificationChannelManager.ChannelsRevoked

#### <a name="pushnotificationchannelsrevokedeventargshttpsdocsmicrosoftcomuwpapiwindowsnetworkingpushnotificationspushnotificationchannelsrevokedeventargs"></a>[PushNotificationChannelsRevokedEventArgs](https://docs.microsoft.com/uwp/api/windows.networking.pushnotifications.pushnotificationchannelsrevokedeventargs)

PushNotificationChannelsRevokedEventArgs

## <a name="windowsperception"></a>Windows.Perception

### <a name="windowsperceptionpeoplehttpsdocsmicrosoftcomuwpapiwindowsperceptionpeople"></a>[Windows.Perception.People](https://docs.microsoft.com/uwp/api/windows.perception.people)

#### <a name="eyesposehttpsdocsmicrosoftcomuwpapiwindowsperceptionpeopleeyespose"></a>[EyesPose](https://docs.microsoft.com/uwp/api/windows.perception.people.eyespose)

EyesPose <br> EyesPose.Gaze <br> EyesPose.IsCalibrationValid <br> EyesPose.IsSupported <br> EyesPose.RequestAccessAsync <br> EyesPose.UpdateTimestamp

#### <a name="handjointkindhttpsdocsmicrosoftcomuwpapiwindowsperceptionpeoplehandjointkind"></a>[HandJointKind](https://docs.microsoft.com/uwp/api/windows.perception.people.handjointkind)

HandJointKind

#### <a name="handmeshobserverhttpsdocsmicrosoftcomuwpapiwindowsperceptionpeoplehandmeshobserver"></a>[HandMeshObserver](https://docs.microsoft.com/uwp/api/windows.perception.people.handmeshobserver)

HandMeshObserver <br> HandMeshObserver.GetTriangleIndices <br> HandMeshObserver.GetVertexStateForPose <br> HandMeshObserver.ModelId <br> HandMeshObserver.NeutralPose <br> HandMeshObserver.NeutralPoseVersion <br> HandMeshObserver.Source <br> HandMeshObserver.TriangleIndexCount <br> HandMeshObserver.VertexCount

#### <a name="handmeshvertexhttpsdocsmicrosoftcomuwpapiwindowsperceptionpeoplehandmeshvertex"></a>[HandMeshVertex](https://docs.microsoft.com/uwp/api/windows.perception.people.handmeshvertex)

HandMeshVertex

#### <a name="handmeshvertexstatehttpsdocsmicrosoftcomuwpapiwindowsperceptionpeoplehandmeshvertexstate"></a>[HandMeshVertexState](https://docs.microsoft.com/uwp/api/windows.perception.people.handmeshvertexstate)

HandMeshVertexState <br> HandMeshVertexState.CoordinateSystem <br> HandMeshVertexState.GetVertices <br> HandMeshVertexState.UpdateTimestamp

#### <a name="handposehttpsdocsmicrosoftcomuwpapiwindowsperceptionpeoplehandpose"></a>[HandPose](https://docs.microsoft.com/uwp/api/windows.perception.people.handpose)

HandPose <br> HandPose.GetRelativeJoints <br> HandPose.GetRelativeJoint <br> HandPose.TryGetJoints <br> HandPose.TryGetJoint

#### <a name="jointposehttpsdocsmicrosoftcomuwpapiwindowsperceptionpeoplejointpose"></a>[JointPose](https://docs.microsoft.com/uwp/api/windows.perception.people.jointpose)

JointPose

#### <a name="jointposeaccuracyhttpsdocsmicrosoftcomuwpapiwindowsperceptionpeoplejointposeaccuracy"></a>[JointPoseAccuracy](https://docs.microsoft.com/uwp/api/windows.perception.people.jointposeaccuracy)

JointPoseAccuracy

### <a name="windowsperceptionspatialhttpsdocsmicrosoftcomuwpapiwindowsperceptionspatial"></a>[Windows.Perception.Spatial](https://docs.microsoft.com/uwp/api/windows.perception.spatial)

#### <a name="spatialrayhttpsdocsmicrosoftcomuwpapiwindowsperceptionspatialspatialray"></a>[SpatialRay](https://docs.microsoft.com/uwp/api/windows.perception.spatial.spatialray)

SpatialRay

### <a name="windowsperceptionspatialpreviewhttpsdocsmicrosoftcomuwpapiwindowsperceptionspatialpreview"></a>[Windows.Perception.Spatial.Preview](https://docs.microsoft.com/uwp/api/windows.perception.spatial.preview)

#### <a name="spatialgraphinteropframeofreferencepreviewhttpsdocsmicrosoftcomuwpapiwindowsperceptionspatialpreviewspatialgraphinteropframeofreferencepreview"></a>[SpatialGraphInteropFrameOfReferencePreview](https://docs.microsoft.com/uwp/api/windows.perception.spatial.preview.spatialgraphinteropframeofreferencepreview)

SpatialGraphInteropFrameOfReferencePreview <br> SpatialGraphInteropFrameOfReferencePreview.CoordinateSystem <br> SpatialGraphInteropFrameOfReferencePreview.CoordinateSystemToNodeTransform <br> SpatialGraphInteropFrameOfReferencePreview.NodeId

#### <a name="spatialgraphinteroppreviewhttpsdocsmicrosoftcomuwpapiwindowsperceptionspatialpreviewspatialgraphinteroppreview"></a>[SpatialGraphInteropPreview](https://docs.microsoft.com/uwp/api/windows.perception.spatial.preview.spatialgraphinteroppreview)

SpatialGraphInteropPreview.TryCreateFrameOfReference <br> SpatialGraphInteropPreview.TryCreateFrameOfReference <br> SpatialGraphInteropPreview.TryCreateFrameOfReference

## <a name="windowsstorage"></a>Windows.Storage

### <a name="windowsstorageaccesscachehttpsdocsmicrosoftcomuwpapiwindowsstorageaccesscache"></a>[Windows.Storage.AccessCache](https://docs.microsoft.com/uwp/api/windows.storage.accesscache)

#### <a name="storageapplicationpermissionshttpsdocsmicrosoftcomuwpapiwindowsstorageaccesscachestorageapplicationpermissions"></a>[StorageApplicationPermissions](https://docs.microsoft.com/uwp/api/windows.storage.accesscache.storageapplicationpermissions)

StorageApplicationPermissions.GetFutureAccessListForUser <br> StorageApplicationPermissions.GetMostRecentlyUsedListForUser

### <a name="windowsstoragepickershttpsdocsmicrosoftcomuwpapiwindowsstoragepickers"></a>[Windows.Storage.Pickers](https://docs.microsoft.com/uwp/api/windows.storage.pickers)

#### <a name="fileopenpickerhttpsdocsmicrosoftcomuwpapiwindowsstoragepickersfileopenpicker"></a>[FileOpenPicker](https://docs.microsoft.com/uwp/api/windows.storage.pickers.fileopenpicker)

FileOpenPicker.CreateForUser <br> FileOpenPicker.User

#### <a name="filesavepickerhttpsdocsmicrosoftcomuwpapiwindowsstoragepickersfilesavepicker"></a>[FileSavePicker](https://docs.microsoft.com/uwp/api/windows.storage.pickers.filesavepicker)

FileSavePicker.CreateForUser <br> FileSavePicker.User

#### <a name="folderpickerhttpsdocsmicrosoftcomuwpapiwindowsstoragepickersfolderpicker"></a>[FolderPicker](https://docs.microsoft.com/uwp/api/windows.storage.pickers.folderpicker)

FolderPicker.CreateForUser <br> FolderPicker.User

## <a name="windowssystem"></a>Windows.System

### <a name="windowssystemhttpsdocsmicrosoftcomuwpapiwindowssystem"></a>[Windows.System](https://docs.microsoft.com/uwp/api/windows.system)

#### <a name="dispatcherqueuehttpsdocsmicrosoftcomuwpapiwindowssystemdispatcherqueue"></a>[DispatcherQueue](https://docs.microsoft.com/uwp/api/windows.system.dispatcherqueue)

DispatcherQueue.HasThreadAccess

### <a name="windowssystemimplementationholographichttpsdocsmicrosoftcomuwpapiwindowssystemimplementationholographic"></a>[Windows.System.Implementation.Holographic](https://docs.microsoft.com/uwp/api/windows.system.implementation.holographic)

#### <a name="sysholographicruntimeregistrationhttpsdocsmicrosoftcomuwpapiwindowssystemimplementationholographicsysholographicruntimeregistration"></a>[SysHolographicRuntimeRegistration](https://docs.microsoft.com/uwp/api/windows.system.implementation.holographic.sysholographicruntimeregistration)

SysHolographicRuntimeRegistration <br> SysHolographicRuntimeRegistration.ActiveRuntime <br> SysHolographicRuntimeRegistration.IsActive <br> SysHolographicRuntimeRegistration.MakeActiveAsync <br> SysHolographicRuntimeRegistration.#ctor

#### <a name="sysholographicwindowingenvironmenthttpsdocsmicrosoftcomuwpapiwindowssystemimplementationholographicsysholographicwindowingenvironment"></a>[SysHolographicWindowingEnvironment](https://docs.microsoft.com/uwp/api/windows.system.implementation.holographic.sysholographicwindowingenvironment)

SysHolographicWindowingEnvironment.HomeGestureDetected <br> SysHolographicWindowingEnvironment.IsHomeGestureReady <br> SysHolographicWindowingEnvironment.IsHomeGestureReadyChanged <br> SysHolographicWindowingEnvironment.IsSystemHomeGestureHandlerSuppressed

### <a name="windowssystemprofilehttpsdocsmicrosoftcomuwpapiwindowssystemprofile"></a>[Windows.System.Profile](https://docs.microsoft.com/uwp/api/windows.system.profile)

#### <a name="appapplicabilityhttpsdocsmicrosoftcomuwpapiwindowssystemprofileappapplicability"></a>[AppApplicability](https://docs.microsoft.com/uwp/api/windows.system.profile.appapplicability)

AppApplicability <br> AppApplicability.GetUnsupportedAppRequirements

#### <a name="unsupportedapprequirementhttpsdocsmicrosoftcomuwpapiwindowssystemprofileunsupportedapprequirement"></a>[UnsupportedAppRequirement](https://docs.microsoft.com/uwp/api/windows.system.profile.unsupportedapprequirement)

UnsupportedAppRequirement

#### <a name="unsupportedapprequirementreasonshttpsdocsmicrosoftcomuwpapiwindowssystemprofileunsupportedapprequirementreasons"></a>[UnsupportedAppRequirementReasons](https://docs.microsoft.com/uwp/api/windows.system.profile.unsupportedapprequirementreasons)

UnsupportedAppRequirementReasons

#### <a name="unsupportedapprequirementhttpsdocsmicrosoftcomuwpapiwindowssystemprofileunsupportedapprequirement"></a>[UnsupportedAppRequirement](https://docs.microsoft.com/uwp/api/windows.system.profile.unsupportedapprequirement)

UnsupportedAppRequirement.Reasons <br> UnsupportedAppRequirement.Requirement

## <a name="windowsui"></a>Windows.UI

### <a name="windowsuihttpsdocsmicrosoftcomuwpapiwindowsui"></a>[Windows.UI](https://docs.microsoft.com/uwp/api/windows.ui)

#### <a name="uicontentroothttpsdocsmicrosoftcomuwpapiwindowsuiuicontentroot"></a>[UIContentRoot](https://docs.microsoft.com/uwp/api/windows.ui.uicontentroot)

UIContentRoot <br> UIContentRoot.UIContext

#### <a name="uicontexthttpsdocsmicrosoftcomuwpapiwindowsuiuicontext"></a>[UIContext](https://docs.microsoft.com/uwp/api/windows.ui.uicontext)

UIContext

### <a name="windowsuicompositionhttpsdocsmicrosoftcomuwpapiwindowsuicomposition"></a>[Windows.UI.Composition](https://docs.microsoft.com/uwp/api/windows.ui.composition)

#### <a name="compositiongraphicsdevicehttpsdocsmicrosoftcomuwpapiwindowsuicompositioncompositiongraphicsdevice"></a>[CompositionGraphicsDevice](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositiongraphicsdevice)

CompositionGraphicsDevice.CreateMipmapSurface <br> CompositionGraphicsDevice.Trim

#### <a name="compositionmipmapsurfacehttpsdocsmicrosoftcomuwpapiwindowsuicompositioncompositionmipmapsurface"></a>[CompositionMipmapSurface](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionmipmapsurface)

CompositionMipmapSurface <br> CompositionMipmapSurface.AlphaMode <br> CompositionMipmapSurface.GetDrawingSurfaceForLevel <br> CompositionMipmapSurface.LevelCount <br> CompositionMipmapSurface.PixelFormat <br> CompositionMipmapSurface.SizeInt32

#### <a name="compositionprojectedshadowhttpsdocsmicrosoftcomuwpapiwindowsuicompositioncompositionprojectedshadow"></a>[CompositionProjectedShadow](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionprojectedshadow)

CompositionProjectedShadow

#### <a name="compositionprojectedshadowcasterhttpsdocsmicrosoftcomuwpapiwindowsuicompositioncompositionprojectedshadowcaster"></a>[CompositionProjectedShadowCaster](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionprojectedshadowcaster)

CompositionProjectedShadowCaster

#### <a name="compositionprojectedshadowcastercollectionhttpsdocsmicrosoftcomuwpapiwindowsuicompositioncompositionprojectedshadowcastercollection"></a>[CompositionProjectedShadowCasterCollection](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionprojectedshadowcastercollection)

CompositionProjectedShadowCasterCollection <br> CompositionProjectedShadowCasterCollection.Count <br> CompositionProjectedShadowCasterCollection.First <br> CompositionProjectedShadowCasterCollection.InsertAbove <br> CompositionProjectedShadowCasterCollection.InsertAtBottom <br> CompositionProjectedShadowCasterCollection.InsertAtTop <br> CompositionProjectedShadowCasterCollection.InsertBelow <br> CompositionProjectedShadowCasterCollection.MaxRespectedCasters <br> CompositionProjectedShadowCasterCollection.RemoveAll <br> CompositionProjectedShadowCasterCollection.Remove

#### <a name="compositionprojectedshadowcasterhttpsdocsmicrosoftcomuwpapiwindowsuicompositioncompositionprojectedshadowcaster"></a>[CompositionProjectedShadowCaster](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionprojectedshadowcaster)

CompositionProjectedShadowCaster.Brush <br> CompositionProjectedShadowCaster.CastingVisual

#### <a name="compositionprojectedshadowreceiverhttpsdocsmicrosoftcomuwpapiwindowsuicompositioncompositionprojectedshadowreceiver"></a>[CompositionProjectedShadowReceiver](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionprojectedshadowreceiver)

CompositionProjectedShadowReceiver

#### <a name="compositionprojectedshadowreceiverunorderedcollectionhttpsdocsmicrosoftcomuwpapiwindowsuicompositioncompositionprojectedshadowreceiverunorderedcollection"></a>[CompositionProjectedShadowReceiverUnorderedCollection](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionprojectedshadowreceiverunorderedcollection)

CompositionProjectedShadowReceiverUnorderedCollection <br> CompositionProjectedShadowReceiverUnorderedCollection.Add <br> CompositionProjectedShadowReceiverUnorderedCollection.Count <br> CompositionProjectedShadowReceiverUnorderedCollection.First <br> CompositionProjectedShadowReceiverUnorderedCollection.RemoveAll <br> CompositionProjectedShadowReceiverUnorderedCollection.Remove

#### <a name="compositionprojectedshadowreceiverhttpsdocsmicrosoftcomuwpapiwindowsuicompositioncompositionprojectedshadowreceiver"></a>[CompositionProjectedShadowReceiver](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionprojectedshadowreceiver)

CompositionProjectedShadowReceiver.ReceivingVisual

#### <a name="compositionprojectedshadowhttpsdocsmicrosoftcomuwpapiwindowsuicompositioncompositionprojectedshadow"></a>[CompositionProjectedShadow](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionprojectedshadow)

CompositionProjectedShadow.BlurRadiusMultiplier <br> CompositionProjectedShadow.Casters <br> CompositionProjectedShadow.LightSource <br> CompositionProjectedShadow.MaxBlurRadius <br> CompositionProjectedShadow.MinBlurRadius <br> CompositionProjectedShadow.Receivers

#### <a name="compositionradialgradientbrushhttpsdocsmicrosoftcomuwpapiwindowsuicompositioncompositionradialgradientbrush"></a>[CompositionRadialGradientBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionradialgradientbrush)

CompositionRadialGradientBrush <br> CompositionRadialGradientBrush.EllipseCenter <br> CompositionRadialGradientBrush.EllipseRadius <br> CompositionRadialGradientBrush.GradientOriginOffset

#### <a name="compositionsurfacebrushhttpsdocsmicrosoftcomuwpapiwindowsuicompositioncompositionsurfacebrush"></a>[CompositionSurfaceBrush](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionsurfacebrush)

CompositionSurfaceBrush.SnapToPixels

#### <a name="compositiontransformhttpsdocsmicrosoftcomuwpapiwindowsuicompositioncompositiontransform"></a>[CompositionTransform](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositiontransform)

CompositionTransform

#### <a name="compositionvisualsurfacehttpsdocsmicrosoftcomuwpapiwindowsuicompositioncompositionvisualsurface"></a>[CompositionVisualSurface](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositionvisualsurface)

CompositionVisualSurface <br> CompositionVisualSurface.SourceOffset <br> CompositionVisualSurface.SourceSize <br> CompositionVisualSurface.SourceVisual

#### <a name="compositorhttpsdocsmicrosoftcomuwpapiwindowsuicompositioncompositor"></a>[Compositor](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositor)

Compositor.CreateProjectedShadowCaster <br> Compositor.CreateProjectedShadowReceiver <br> Compositor.CreateProjectedShadow <br> Compositor.CreateRadialGradientBrush <br> Compositor.CreateVisualSurface

#### <a name="ivisualelementhttpsdocsmicrosoftcomuwpapiwindowsuicompositionivisualelement"></a>[IVisualElement](https://docs.microsoft.com/uwp/api/windows.ui.composition.ivisualelement)

IVisualElement

### <a name="windowsuicompositioninteractionshttpsdocsmicrosoftcomuwpapiwindowsuicompositioninteractions"></a>[Windows.UI.Composition.Interactions](https://docs.microsoft.com/uwp/api/windows.ui.composition.interactions)

#### <a name="interactionbindingaxismodeshttpsdocsmicrosoftcomuwpapiwindowsuicompositioninteractionsinteractionbindingaxismodes"></a>[InteractionBindingAxisModes](https://docs.microsoft.com/uwp/api/windows.ui.composition.interactions.interactionbindingaxismodes)

InteractionBindingAxisModes

#### <a name="interactiontrackercustomanimationstateenteredargshttpsdocsmicrosoftcomuwpapiwindowsuicompositioninteractionsinteractiontrackercustomanimationstateenteredargs"></a>[InteractionTrackerCustomAnimationStateEnteredArgs](https://docs.microsoft.com/uwp/api/windows.ui.composition.interactions.interactiontrackercustomanimationstateenteredargs)

InteractionTrackerCustomAnimationStateEnteredArgs.IsFromBinding

#### <a name="interactiontrackeridlestateenteredargshttpsdocsmicrosoftcomuwpapiwindowsuicompositioninteractionsinteractiontrackeridlestateenteredargs"></a>[InteractionTrackerIdleStateEnteredArgs](https://docs.microsoft.com/uwp/api/windows.ui.composition.interactions.interactiontrackeridlestateenteredargs)

InteractionTrackerIdleStateEnteredArgs.IsFromBinding

#### <a name="interactiontrackerinertiastateenteredargshttpsdocsmicrosoftcomuwpapiwindowsuicompositioninteractionsinteractiontrackerinertiastateenteredargs"></a>[InteractionTrackerInertiaStateEnteredArgs](https://docs.microsoft.com/uwp/api/windows.ui.composition.interactions.interactiontrackerinertiastateenteredargs)

InteractionTrackerInertiaStateEnteredArgs.IsFromBinding

#### <a name="interactiontrackerinteractingstateenteredargshttpsdocsmicrosoftcomuwpapiwindowsuicompositioninteractionsinteractiontrackerinteractingstateenteredargs"></a>[InteractionTrackerInteractingStateEnteredArgs](https://docs.microsoft.com/uwp/api/windows.ui.composition.interactions.interactiontrackerinteractingstateenteredargs)

InteractionTrackerInteractingStateEnteredArgs.IsFromBinding

#### <a name="interactiontrackerhttpsdocsmicrosoftcomuwpapiwindowsuicompositioninteractionsinteractiontracker"></a>[InteractionTracker](https://docs.microsoft.com/uwp/api/windows.ui.composition.interactions.interactiontracker)

InteractionTracker.GetBindingMode <br> InteractionTracker.SetBindingMode

#### <a name="visualinteractionsourcehttpsdocsmicrosoftcomuwpapiwindowsuicompositioninteractionsvisualinteractionsource"></a>[VisualInteractionSource](https://docs.microsoft.com/uwp/api/windows.ui.composition.interactions.visualinteractionsource)

VisualInteractionSource.CreateFromIVisualElement

### <a name="windowsuicompositionsceneshttpsdocsmicrosoftcomuwpapiwindowsuicompositionscenes"></a>[Windows.UI.Composition.Scenes](https://docs.microsoft.com/uwp/api/windows.ui.composition.scenes)

#### <a name="scenealphamodehttpsdocsmicrosoftcomuwpapiwindowsuicompositionscenesscenealphamode"></a>[SceneAlphaMode](https://docs.microsoft.com/uwp/api/windows.ui.composition.scenes.scenealphamode)

SceneAlphaMode

#### <a name="sceneattributesemantichttpsdocsmicrosoftcomuwpapiwindowsuicompositionscenessceneattributesemantic"></a>[SceneAttributeSemantic](https://docs.microsoft.com/uwp/api/windows.ui.composition.scenes.sceneattributesemantic)

SceneAttributeSemantic

#### <a name="sceneboundingboxhttpsdocsmicrosoftcomuwpapiwindowsuicompositionscenessceneboundingbox"></a>[SceneBoundingBox](https://docs.microsoft.com/uwp/api/windows.ui.composition.scenes.sceneboundingbox)

SceneBoundingBox <br> SceneBoundingBox.Center <br> SceneBoundingBox.Extents <br> SceneBoundingBox.Max <br> SceneBoundingBox.Min <br> SceneBoundingBox.Size

#### <a name="scenecomponenthttpsdocsmicrosoftcomuwpapiwindowsuicompositionscenesscenecomponent"></a>[SceneComponent](https://docs.microsoft.com/uwp/api/windows.ui.composition.scenes.scenecomponent)

SceneComponent

#### <a name="scenecomponentcollectionhttpsdocsmicrosoftcomuwpapiwindowsuicompositionscenesscenecomponentcollection"></a>[SceneComponentCollection](https://docs.microsoft.com/uwp/api/windows.ui.composition.scenes.scenecomponentcollection)

SceneComponentCollection <br> SceneComponentCollection.Append <br> SceneComponentCollection.Clear <br> SceneComponentCollection.First <br> SceneComponentCollection.GetAt <br> SceneComponentCollection.GetMany <br> SceneComponentCollection.GetView <br> SceneComponentCollection.IndexOf <br> SceneComponentCollection.InsertAt <br> SceneComponentCollection.RemoveAtEnd <br> SceneComponentCollection.RemoveAt <br> SceneComponentCollection.ReplaceAll <br> SceneComponentCollection.SetAt <br> SceneComponentCollection.Size

#### <a name="scenecomponenttypehttpsdocsmicrosoftcomuwpapiwindowsuicompositionscenesscenecomponenttype"></a>[SceneComponentType](https://docs.microsoft.com/uwp/api/windows.ui.composition.scenes.scenecomponenttype)

SceneComponentType

#### <a name="scenecomponenthttpsdocsmicrosoftcomuwpapiwindowsuicompositionscenesscenecomponent"></a>[SceneComponent](https://docs.microsoft.com/uwp/api/windows.ui.composition.scenes.scenecomponent)

SceneComponent.ComponentType

#### <a name="scenematerialhttpsdocsmicrosoftcomuwpapiwindowsuicompositionscenesscenematerial"></a>[SceneMaterial](https://docs.microsoft.com/uwp/api/windows.ui.composition.scenes.scenematerial)

SceneMaterial

#### <a name="scenematerialinputhttpsdocsmicrosoftcomuwpapiwindowsuicompositionscenesscenematerialinput"></a>[SceneMaterialInput](https://docs.microsoft.com/uwp/api/windows.ui.composition.scenes.scenematerialinput)

SceneMaterialInput

#### <a name="scenemeshhttpsdocsmicrosoftcomuwpapiwindowsuicompositionscenesscenemesh"></a>[SceneMesh](https://docs.microsoft.com/uwp/api/windows.ui.composition.scenes.scenemesh)

SceneMesh

#### <a name="scenemeshmaterialattributemaphttpsdocsmicrosoftcomuwpapiwindowsuicompositionscenesscenemeshmaterialattributemap"></a>[SceneMeshMaterialAttributeMap](https://docs.microsoft.com/uwp/api/windows.ui.composition.scenes.scenemeshmaterialattributemap)

SceneMeshMaterialAttributeMap <br> SceneMeshMaterialAttributeMap.Clear <br> SceneMeshMaterialAttributeMap.First <br> SceneMeshMaterialAttributeMap.GetView <br> SceneMeshMaterialAttributeMap.HasKey <br> SceneMeshMaterialAttributeMap.Insert <br> SceneMeshMaterialAttributeMap.Lookup <br> SceneMeshMaterialAttributeMap.Remove <br> SceneMeshMaterialAttributeMap.Size

#### <a name="scenemeshrenderercomponenthttpsdocsmicrosoftcomuwpapiwindowsuicompositionscenesscenemeshrenderercomponent"></a>[SceneMeshRendererComponent](https://docs.microsoft.com/uwp/api/windows.ui.composition.scenes.scenemeshrenderercomponent)

SceneMeshRendererComponent <br> SceneMeshRendererComponent.Create <br> SceneMeshRendererComponent.Material <br> SceneMeshRendererComponent.Mesh <br> SceneMeshRendererComponent.UVMappings

#### <a name="scenemeshhttpsdocsmicrosoftcomuwpapiwindowsuicompositionscenesscenemesh"></a>[SceneMesh](https://docs.microsoft.com/uwp/api/windows.ui.composition.scenes.scenemesh)

SceneMesh.Bounds <br> SceneMesh.Create <br> SceneMesh.FillMeshAttribute <br> SceneMesh.PrimitiveTopology

#### <a name="scenemetallicroughnessmaterialhttpsdocsmicrosoftcomuwpapiwindowsuicompositionscenesscenemetallicroughnessmaterial"></a>[SceneMetallicRoughnessMaterial](https://docs.microsoft.com/uwp/api/windows.ui.composition.scenes.scenemetallicroughnessmaterial)

SceneMetallicRoughnessMaterial <br> SceneMetallicRoughnessMaterial.BaseColorFactor <br> SceneMetallicRoughnessMaterial.BaseColorInput <br> SceneMetallicRoughnessMaterial.Create <br> SceneMetallicRoughnessMaterial.MetallicFactor <br> SceneMetallicRoughnessMaterial.MetallicRoughnessInput <br> SceneMetallicRoughnessMaterial.RoughnessFactor

#### <a name="scenemodeltransformhttpsdocsmicrosoftcomuwpapiwindowsuicompositionscenesscenemodeltransform"></a>[SceneModelTransform](https://docs.microsoft.com/uwp/api/windows.ui.composition.scenes.scenemodeltransform)

SceneModelTransform <br> SceneModelTransform.Orientation <br> SceneModelTransform.RotationAngle <br> SceneModelTransform.RotationAngleInDegrees <br> SceneModelTransform.RotationAxis <br> SceneModelTransform.Scale <br> SceneModelTransform.Translation

#### <a name="scenenodehttpsdocsmicrosoftcomuwpapiwindowsuicompositionscenesscenenode"></a>[SceneNode](https://docs.microsoft.com/uwp/api/windows.ui.composition.scenes.scenenode)

SceneNode

#### <a name="scenenodecollectionhttpsdocsmicrosoftcomuwpapiwindowsuicompositionscenesscenenodecollection"></a>[SceneNodeCollection](https://docs.microsoft.com/uwp/api/windows.ui.composition.scenes.scenenodecollection)

SceneNodeCollection <br> SceneNodeCollection.Append <br> SceneNodeCollection.Clear <br> SceneNodeCollection.First <br> SceneNodeCollection.GetAt <br> SceneNodeCollection.GetMany <br> SceneNodeCollection.GetView <br> SceneNodeCollection.IndexOf <br> SceneNodeCollection.InsertAt <br> SceneNodeCollection.RemoveAtEnd <br> SceneNodeCollection.RemoveAt <br> SceneNodeCollection.ReplaceAll <br> SceneNodeCollection.SetAt <br> SceneNodeCollection.Size

#### <a name="scenenodehttpsdocsmicrosoftcomuwpapiwindowsuicompositionscenesscenenode"></a>[SceneNode](https://docs.microsoft.com/uwp/api/windows.ui.composition.scenes.scenenode)

SceneNode.Children <br> SceneNode.Components <br> SceneNode.Create <br> SceneNode.FindFirstComponentOfType <br> SceneNode.Parent <br> SceneNode.Transform

#### <a name="sceneobjecthttpsdocsmicrosoftcomuwpapiwindowsuicompositionscenessceneobject"></a>[SceneObject](https://docs.microsoft.com/uwp/api/windows.ui.composition.scenes.sceneobject)

SceneObject

#### <a name="scenepbrmaterialhttpsdocsmicrosoftcomuwpapiwindowsuicompositionscenesscenepbrmaterial"></a>[ScenePbrMaterial](https://docs.microsoft.com/uwp/api/windows.ui.composition.scenes.scenepbrmaterial)

ScenePbrMaterial <br> ScenePbrMaterial.AlphaCutoff <br> ScenePbrMaterial.AlphaMode <br> ScenePbrMaterial.EmissiveFactor <br> ScenePbrMaterial.EmissiveInput <br> ScenePbrMaterial.IsDoubleSided <br> ScenePbrMaterial.NormalInput <br> ScenePbrMaterial.NormalScale <br> ScenePbrMaterial.OcclusionInput <br> ScenePbrMaterial.OcclusionStrength

#### <a name="scenerenderercomponenthttpsdocsmicrosoftcomuwpapiwindowsuicompositionscenesscenerenderercomponent"></a>[SceneRendererComponent](https://docs.microsoft.com/uwp/api/windows.ui.composition.scenes.scenerenderercomponent)

SceneRendererComponent

#### <a name="scenesurfacematerialinputhttpsdocsmicrosoftcomuwpapiwindowsuicompositionscenesscenesurfacematerialinput"></a>[SceneSurfaceMaterialInput](https://docs.microsoft.com/uwp/api/windows.ui.composition.scenes.scenesurfacematerialinput)

SceneSurfaceMaterialInput <br> SceneSurfaceMaterialInput.BitmapInterpolationMode <br> SceneSurfaceMaterialInput.Create <br> SceneSurfaceMaterialInput.Surface <br> SceneSurfaceMaterialInput.WrappingUMode <br> SceneSurfaceMaterialInput.WrappingVMode

#### <a name="scenevisualhttpsdocsmicrosoftcomuwpapiwindowsuicompositionscenesscenevisual"></a>[SceneVisual](https://docs.microsoft.com/uwp/api/windows.ui.composition.scenes.scenevisual)

SceneVisual <br> SceneVisual.Create <br> SceneVisual.Root

#### <a name="scenewrappingmodehttpsdocsmicrosoftcomuwpapiwindowsuicompositionscenesscenewrappingmode"></a>[SceneWrappingMode](https://docs.microsoft.com/uwp/api/windows.ui.composition.scenes.scenewrappingmode)

SceneWrappingMode

### <a name="windowsuicorehttpsdocsmicrosoftcomuwpapiwindowsuicore"></a>[Windows.UI.Core](https://docs.microsoft.com/uwp/api/windows.ui.core)

#### <a name="corewindowhttpsdocsmicrosoftcomuwpapiwindowsuicorecorewindow"></a>[CoreWindow](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow)

CoreWindow.UIContext

### <a name="windowsuicorepreviewhttpsdocsmicrosoftcomuwpapiwindowsuicorepreview"></a>[Windows.UI.Core.Preview](https://docs.microsoft.com/uwp/api/windows.ui.core.preview)

#### <a name="coreappwindowpreviewhttpsdocsmicrosoftcomuwpapiwindowsuicorepreviewcoreappwindowpreview"></a>[CoreAppWindowPreview](https://docs.microsoft.com/uwp/api/windows.ui.core.preview.coreappwindowpreview)

CoreAppWindowPreview <br> CoreAppWindowPreview.GetIdFromWindow

### <a name="windowsuiinputhttpsdocsmicrosoftcomuwpapiwindowsuiinput"></a>[Windows.UI.Input](https://docs.microsoft.com/uwp/api/windows.ui.input)

#### <a name="attachableinputobjecthttpsdocsmicrosoftcomuwpapiwindowsuiinputattachableinputobject"></a>[AttachableInputObject](https://docs.microsoft.com/uwp/api/windows.ui.input.attachableinputobject)

AttachableInputObject <br> AttachableInputObject.Close

#### <a name="gazeinputaccessstatushttpsdocsmicrosoftcomuwpapiwindowsuiinputgazeinputaccessstatus"></a>[GazeInputAccessStatus](https://docs.microsoft.com/uwp/api/windows.ui.input.gazeinputaccessstatus)

GazeInputAccessStatus

#### <a name="inputactivationlistenerhttpsdocsmicrosoftcomuwpapiwindowsuiinputinputactivationlistener"></a>[InputActivationListener](https://docs.microsoft.com/uwp/api/windows.ui.input.inputactivationlistener)

InputActivationListener

#### <a name="inputactivationlisteneractivationchangedeventargshttpsdocsmicrosoftcomuwpapiwindowsuiinputinputactivationlisteneractivationchangedeventargs"></a>[InputActivationListenerActivationChangedEventArgs](https://docs.microsoft.com/uwp/api/windows.ui.input.inputactivationlisteneractivationchangedeventargs)

InputActivationListenerActivationChangedEventArgs <br> InputActivationListenerActivationChangedEventArgs.State

#### <a name="inputactivationlistenerhttpsdocsmicrosoftcomuwpapiwindowsuiinputinputactivationlistener"></a>[InputActivationListener](https://docs.microsoft.com/uwp/api/windows.ui.input.inputactivationlistener)

InputActivationListener.InputActivationChanged <br> InputActivationListener.State

#### <a name="inputactivationstatehttpsdocsmicrosoftcomuwpapiwindowsuiinputinputactivationstate"></a>[InputActivationState](https://docs.microsoft.com/uwp/api/windows.ui.input.inputactivationstate)

InputActivationState

### <a name="windowsuiinputpreviewhttpsdocsmicrosoftcomuwpapiwindowsuiinputpreview"></a>[Windows.UI.Input.Preview](https://docs.microsoft.com/uwp/api/windows.ui.input.preview)

#### <a name="inputactivationlistenerpreviewhttpsdocsmicrosoftcomuwpapiwindowsuiinputpreviewinputactivationlistenerpreview"></a>[InputActivationListenerPreview](https://docs.microsoft.com/uwp/api/windows.ui.input.preview.inputactivationlistenerpreview)

InputActivationListenerPreview <br> InputActivationListenerPreview.CreateForApplicationWindow

### <a name="windowsuiinputspatialhttpsdocsmicrosoftcomuwpapiwindowsuiinputspatial"></a>[Windows.UI.Input.Spatial](https://docs.microsoft.com/uwp/api/windows.ui.input.spatial)

#### <a name="spatialinteractionmanagerhttpsdocsmicrosoftcomuwpapiwindowsuiinputspatialspatialinteractionmanager"></a>[SpatialInteractionManager](https://docs.microsoft.com/uwp/api/windows.ui.input.spatial.spatialinteractionmanager)

SpatialInteractionManager.IsSourceKindSupported

#### <a name="spatialinteractionsourcestatehttpsdocsmicrosoftcomuwpapiwindowsuiinputspatialspatialinteractionsourcestate"></a>[SpatialInteractionSourceState](https://docs.microsoft.com/uwp/api/windows.ui.input.spatial.spatialinteractionsourcestate)

SpatialInteractionSourceState.TryGetHandPose

#### <a name="spatialinteractionsourcehttpsdocsmicrosoftcomuwpapiwindowsuiinputspatialspatialinteractionsource"></a>[SpatialInteractionSource](https://docs.microsoft.com/uwp/api/windows.ui.input.spatial.spatialinteractionsource)

SpatialInteractionSource.TryCreateHandMeshObserverAsync <br> SpatialInteractionSource.TryCreateHandMeshObserver

#### <a name="spatialpointerposehttpsdocsmicrosoftcomuwpapiwindowsuiinputspatialspatialpointerpose"></a>[SpatialPointerPose](https://docs.microsoft.com/uwp/api/windows.ui.input.spatial.spatialpointerpose)

SpatialPointerPose.Eyes <br> SpatialPointerPose.IsHeadCapturedBySystem

### <a name="windowsuinotificationshttpsdocsmicrosoftcomuwpapiwindowsuinotifications"></a>[Windows.UI.Notifications](https://docs.microsoft.com/uwp/api/windows.ui.notifications)

#### <a name="toastactivatedeventargshttpsdocsmicrosoftcomuwpapiwindowsuinotificationstoastactivatedeventargs"></a>[ToastActivatedEventArgs](https://docs.microsoft.com/uwp/api/windows.ui.notifications.toastactivatedeventargs)

ToastActivatedEventArgs.UserInput

#### <a name="toastnotificationhttpsdocsmicrosoftcomuwpapiwindowsuinotificationstoastnotification"></a>[ToastNotification](https://docs.microsoft.com/uwp/api/windows.ui.notifications.toastnotification)

ToastNotification.ExpiresOnReboot

### <a name="windowsuiviewmanagementhttpsdocsmicrosoftcomuwpapiwindowsuiviewmanagement"></a>[Windows.UI.ViewManagement](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement)

#### <a name="applicationviewhttpsdocsmicrosoftcomuwpapiwindowsuiviewmanagementapplicationview"></a>[ApplicationView](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview)

ApplicationView.ClearAllPersistedState <br> ApplicationView.ClearPersistedState <br> ApplicationView.GetDisplayRegions <br> ApplicationView.PersistedStateId <br> ApplicationView.UIContext <br> ApplicationView.WindowingEnvironment

#### <a name="inputpanehttpsdocsmicrosoftcomuwpapiwindowsuiviewmanagementinputpane"></a>[InputPane](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.inputpane)

InputPane.GetForUIContext

#### <a name="uisettingsautohidescrollbarschangedeventargshttpsdocsmicrosoftcomuwpapiwindowsuiviewmanagementuisettingsautohidescrollbarschangedeventargs"></a>[UISettingsAutoHideScrollBarsChangedEventArgs](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettingsautohidescrollbarschangedeventargs)

UISettingsAutoHideScrollBarsChangedEventArgs

#### <a name="uisettingshttpsdocsmicrosoftcomuwpapiwindowsuiviewmanagementuisettings"></a>[UISettings](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings)

UISettings.AutoHideScrollBars <br> UISettings.AutoHideScrollBarsChanged

### <a name="windowsuiviewmanagementcorehttpsdocsmicrosoftcomuwpapiwindowsuiviewmanagementcore"></a>[Windows.UI.ViewManagement.Core](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.core)

#### <a name="coreinputviewhttpsdocsmicrosoftcomuwpapiwindowsuiviewmanagementcorecoreinputview"></a>[CoreInputView](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.core.coreinputview)

CoreInputView.GetForUIContext

### <a name="windowsuiwindowmanagementhttpsdocsmicrosoftcomuwpapiwindowsuiwindowmanagement"></a>[Windows.UI.WindowManagement](https://docs.microsoft.com/uwp/api/windows.ui.windowmanagement)

#### <a name="appwindowhttpsdocsmicrosoftcomuwpapiwindowsuiwindowmanagementappwindow"></a>[AppWindow](https://docs.microsoft.com/uwp/api/windows.ui.windowmanagement.appwindow)

AppWindow

#### <a name="appwindowchangedeventargshttpsdocsmicrosoftcomuwpapiwindowsuiwindowmanagementappwindowchangedeventargs"></a>[AppWindowChangedEventArgs](https://docs.microsoft.com/uwp/api/windows.ui.windowmanagement.appwindowchangedeventargs)

AppWindowChangedEventArgs <br> AppWindowChangedEventArgs.DidAvailableWindowPresentationsChange <br> AppWindowChangedEventArgs.DidDisplayRegionsChange <br> AppWindowChangedEventArgs.DidFrameChange <br> AppWindowChangedEventArgs.DidSizeChange <br> AppWindowChangedEventArgs.DidTitleBarChange <br> AppWindowChangedEventArgs.DidVisibilityChange <br> AppWindowChangedEventArgs.DidWindowingEnvironmentChange <br> AppWindowChangedEventArgs.DidWindowPresentationChange

#### <a name="appwindowclosedeventargshttpsdocsmicrosoftcomuwpapiwindowsuiwindowmanagementappwindowclosedeventargs"></a>[AppWindowClosedEventArgs](https://docs.microsoft.com/uwp/api/windows.ui.windowmanagement.appwindowclosedeventargs)

AppWindowClosedEventArgs <br> AppWindowClosedEventArgs.Reason

#### <a name="appwindowclosedreasonhttpsdocsmicrosoftcomuwpapiwindowsuiwindowmanagementappwindowclosedreason"></a>[AppWindowClosedReason](https://docs.microsoft.com/uwp/api/windows.ui.windowmanagement.appwindowclosedreason)

AppWindowClosedReason

#### <a name="appwindowcloserequestedeventargshttpsdocsmicrosoftcomuwpapiwindowsuiwindowmanagementappwindowcloserequestedeventargs"></a>[AppWindowCloseRequestedEventArgs](https://docs.microsoft.com/uwp/api/windows.ui.windowmanagement.appwindowcloserequestedeventargs)

AppWindowCloseRequestedEventArgs <br> AppWindowCloseRequestedEventArgs.Cancel <br> AppWindowCloseRequestedEventArgs.GetDeferral

#### <a name="appwindowframehttpsdocsmicrosoftcomuwpapiwindowsuiwindowmanagementappwindowframe"></a>[AppWindowFrame](https://docs.microsoft.com/uwp/api/windows.ui.windowmanagement.appwindowframe)

AppWindowFrame

#### <a name="appwindowframestylehttpsdocsmicrosoftcomuwpapiwindowsuiwindowmanagementappwindowframestyle"></a>[AppWindowFrameStyle](https://docs.microsoft.com/uwp/api/windows.ui.windowmanagement.appwindowframestyle)

AppWindowFrameStyle

#### <a name="appwindowframehttpsdocsmicrosoftcomuwpapiwindowsuiwindowmanagementappwindowframe"></a>[AppWindowFrame](https://docs.microsoft.com/uwp/api/windows.ui.windowmanagement.appwindowframe)

AppWindowFrame.DragRegionVisuals <br> AppWindowFrame.GetFrameStyle <br> AppWindowFrame.SetFrameStyle

#### <a name="appwindowplacementhttpsdocsmicrosoftcomuwpapiwindowsuiwindowmanagementappwindowplacement"></a>[AppWindowPlacement](https://docs.microsoft.com/uwp/api/windows.ui.windowmanagement.appwindowplacement)

AppWindowPlacement <br> AppWindowPlacement.DisplayRegion <br> AppWindowPlacement.Offset <br> AppWindowPlacement.Size

#### <a name="appwindowpresentationconfigurationhttpsdocsmicrosoftcomuwpapiwindowsuiwindowmanagementappwindowpresentationconfiguration"></a>[AppWindowPresentationConfiguration](https://docs.microsoft.com/uwp/api/windows.ui.windowmanagement.appwindowpresentationconfiguration)

AppWindowPresentationConfiguration <br> AppWindowPresentationConfiguration.Kind

#### <a name="appwindowpresentationkindhttpsdocsmicrosoftcomuwpapiwindowsuiwindowmanagementappwindowpresentationkind"></a>[AppWindowPresentationKind](https://docs.microsoft.com/uwp/api/windows.ui.windowmanagement.appwindowpresentationkind)

AppWindowPresentationKind

#### <a name="appwindowpresenterhttpsdocsmicrosoftcomuwpapiwindowsuiwindowmanagementappwindowpresenter"></a>[AppWindowPresenter](https://docs.microsoft.com/uwp/api/windows.ui.windowmanagement.appwindowpresenter)

AppWindowPresenter <br> AppWindowPresenter.GetConfiguration <br> AppWindowPresenter.IsPresentationSupported <br> AppWindowPresenter.RequestPresentation <br> AppWindowPresenter.RequestPresentation

#### <a name="appwindowtitlebarhttpsdocsmicrosoftcomuwpapiwindowsuiwindowmanagementappwindowtitlebar"></a>[AppWindowTitleBar](https://docs.microsoft.com/uwp/api/windows.ui.windowmanagement.appwindowtitlebar)

AppWindowTitleBar

#### <a name="appwindowtitlebarocclusionhttpsdocsmicrosoftcomuwpapiwindowsuiwindowmanagementappwindowtitlebarocclusion"></a>[AppWindowTitleBarOcclusion](https://docs.microsoft.com/uwp/api/windows.ui.windowmanagement.appwindowtitlebarocclusion)

AppWindowTitleBarOcclusion <br> AppWindowTitleBarOcclusion.OccludingRect

#### <a name="appwindowtitlebarvisibilityhttpsdocsmicrosoftcomuwpapiwindowsuiwindowmanagementappwindowtitlebarvisibility"></a>[AppWindowTitleBarVisibility](https://docs.microsoft.com/uwp/api/windows.ui.windowmanagement.appwindowtitlebarvisibility)

AppWindowTitleBarVisibility

#### <a name="appwindowtitlebarhttpsdocsmicrosoftcomuwpapiwindowsuiwindowmanagementappwindowtitlebar"></a>[AppWindowTitleBar](https://docs.microsoft.com/uwp/api/windows.ui.windowmanagement.appwindowtitlebar)

AppWindowTitleBar.BackgroundColor <br> AppWindowTitleBar.ButtonBackgroundColor <br> AppWindowTitleBar.ButtonForegroundColor <br> AppWindowTitleBar.ButtonHoverBackgroundColor <br> AppWindowTitleBar.ButtonHoverForegroundColor <br> AppWindowTitleBar.ButtonInactiveBackgroundColor <br> AppWindowTitleBar.ButtonInactiveForegroundColor <br> AppWindowTitleBar.ButtonPressedBackgroundColor <br> AppWindowTitleBar.ButtonPressedForegroundColor <br> AppWindowTitleBar.ExtendsContentIntoTitleBar <br> AppWindowTitleBar.ForegroundColor <br> AppWindowTitleBar.GetPreferredVisibility <br> AppWindowTitleBar.GetTitleBarOcclusions <br> AppWindowTitleBar.InactiveBackgroundColor <br> AppWindowTitleBar.InactiveForegroundColor <br> AppWindowTitleBar.IsVisible <br> AppWindowTitleBar.SetPreferredVisibility

#### <a name="appwindowhttpsdocsmicrosoftcomuwpapiwindowsuiwindowmanagementappwindow"></a>[AppWindow](https://docs.microsoft.com/uwp/api/windows.ui.windowmanagement.appwindow)

AppWindow.Changed <br> AppWindow.ClearAllPersistedState <br> AppWindow.ClearPersistedState <br> AppWindow.CloseAsync <br> AppWindow.Closed <br> AppWindow.CloseRequested <br> AppWindow.Content <br> AppWindow.DispatcherQueue <br> AppWindow.Frame <br> AppWindow.GetDisplayRegions <br> AppWindow.GetPlacement <br> AppWindow.IsVisible <br> AppWindow.PersistedStateId <br> AppWindow.Presenter <br> AppWindow.RequestMoveAdjacentToCurrentView <br> AppWindow.RequestMoveAdjacentToWindow <br> AppWindow.RequestMoveRelativeToDisplayRegion <br> AppWindow.RequestMoveToDisplayRegion <br> AppWindow.RequestSize <br> AppWindow.Title <br> AppWindow.TitleBar <br> AppWindow.TryCreateAsync <br> AppWindow.TryShowAsync <br> AppWindow.UIContext <br> AppWindow.WindowingEnvironment

#### <a name="compactoverlaypresentationconfigurationhttpsdocsmicrosoftcomuwpapiwindowsuiwindowmanagementcompactoverlaypresentationconfiguration"></a>[CompactOverlayPresentationConfiguration](https://docs.microsoft.com/uwp/api/windows.ui.windowmanagement.compactoverlaypresentationconfiguration)

CompactOverlayPresentationConfiguration <br> CompactOverlayPresentationConfiguration.#ctor

#### <a name="defaultpresentationconfigurationhttpsdocsmicrosoftcomuwpapiwindowsuiwindowmanagementdefaultpresentationconfiguration"></a>[DefaultPresentationConfiguration](https://docs.microsoft.com/uwp/api/windows.ui.windowmanagement.defaultpresentationconfiguration)

DefaultPresentationConfiguration <br> DefaultPresentationConfiguration.#ctor

#### <a name="displayregionhttpsdocsmicrosoftcomuwpapiwindowsuiwindowmanagementdisplayregion"></a>[DisplayRegion](https://docs.microsoft.com/uwp/api/windows.ui.windowmanagement.displayregion)

DisplayRegion <br> DisplayRegion.Changed <br> DisplayRegion.DisplayMonitorDeviceId <br> DisplayRegion.IsVisible <br> DisplayRegion.WindowingEnvironment <br> DisplayRegion.WorkAreaOffset <br> DisplayRegion.WorkAreaSize

#### <a name="fullscreenpresentationconfigurationhttpsdocsmicrosoftcomuwpapiwindowsuiwindowmanagementfullscreenpresentationconfiguration"></a>[FullScreenPresentationConfiguration](https://docs.microsoft.com/uwp/api/windows.ui.windowmanagement.fullscreenpresentationconfiguration)

FullScreenPresentationConfiguration <br> FullScreenPresentationConfiguration.#ctor <br> FullScreenPresentationConfiguration.IsExclusive

#### <a name="windowingenvironmenthttpsdocsmicrosoftcomuwpapiwindowsuiwindowmanagementwindowingenvironment"></a>[WindowingEnvironment](https://docs.microsoft.com/uwp/api/windows.ui.windowmanagement.windowingenvironment)

WindowingEnvironment

#### <a name="windowingenvironmentaddedeventargshttpsdocsmicrosoftcomuwpapiwindowsuiwindowmanagementwindowingenvironmentaddedeventargs"></a>[WindowingEnvironmentAddedEventArgs](https://docs.microsoft.com/uwp/api/windows.ui.windowmanagement.windowingenvironmentaddedeventargs)

WindowingEnvironmentAddedEventArgs <br> WindowingEnvironmentAddedEventArgs.WindowingEnvironment

#### <a name="windowingenvironmentchangedeventargshttpsdocsmicrosoftcomuwpapiwindowsuiwindowmanagementwindowingenvironmentchangedeventargs"></a>[WindowingEnvironmentChangedEventArgs](https://docs.microsoft.com/uwp/api/windows.ui.windowmanagement.windowingenvironmentchangedeventargs)

WindowingEnvironmentChangedEventArgs

#### <a name="windowingenvironmentkindhttpsdocsmicrosoftcomuwpapiwindowsuiwindowmanagementwindowingenvironmentkind"></a>[WindowingEnvironmentKind](https://docs.microsoft.com/uwp/api/windows.ui.windowmanagement.windowingenvironmentkind)

WindowingEnvironmentKind

#### <a name="windowingenvironmentremovedeventargshttpsdocsmicrosoftcomuwpapiwindowsuiwindowmanagementwindowingenvironmentremovedeventargs"></a>[WindowingEnvironmentRemovedEventArgs](https://docs.microsoft.com/uwp/api/windows.ui.windowmanagement.windowingenvironmentremovedeventargs)

WindowingEnvironmentRemovedEventArgs <br> WindowingEnvironmentRemovedEventArgs.WindowingEnvironment

#### <a name="windowingenvironmenthttpsdocsmicrosoftcomuwpapiwindowsuiwindowmanagementwindowingenvironment"></a>[WindowingEnvironment](https://docs.microsoft.com/uwp/api/windows.ui.windowmanagement.windowingenvironment)

WindowingEnvironment.Changed <br> WindowingEnvironment.FindAll <br> WindowingEnvironment.FindAll <br> WindowingEnvironment.GetDisplayRegions <br> WindowingEnvironment.IsEnabled <br> WindowingEnvironment.Kind

### <a name="windowsuiwindowmanagementpreviewhttpsdocsmicrosoftcomuwpapiwindowsuiwindowmanagementpreview"></a>[Windows.UI.WindowManagement.Preview](https://docs.microsoft.com/uwp/api/windows.ui.windowmanagement.preview)

#### <a name="windowmanagementpreviewhttpsdocsmicrosoftcomuwpapiwindowsuiwindowmanagementpreviewwindowmanagementpreview"></a>[WindowManagementPreview](https://docs.microsoft.com/uwp/api/windows.ui.windowmanagement.preview.windowmanagementpreview)

WindowManagementPreview <br> WindowManagementPreview.SetPreferredMinSize

### <a name="windowsuixamlhttpsdocsmicrosoftcomuwpapiwindowsuixaml"></a>[Windows.UI.Xaml](https://docs.microsoft.com/uwp/api/windows.ui.xaml)

#### <a name="uielementweakcollectionhttpsdocsmicrosoftcomuwpapiwindowsuixamluielementweakcollection"></a>[UIElementWeakCollection](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielementweakcollection)

UIElementWeakCollection <br> UIElementWeakCollection.Append <br> UIElementWeakCollection.Clear <br> UIElementWeakCollection.First <br> UIElementWeakCollection.GetAt <br> UIElementWeakCollection.GetMany <br> UIElementWeakCollection.GetView <br> UIElementWeakCollection.IndexOf <br> UIElementWeakCollection.InsertAt <br> UIElementWeakCollection.RemoveAtEnd <br> UIElementWeakCollection.RemoveAt <br> UIElementWeakCollection.ReplaceAll <br> UIElementWeakCollection.SetAt <br> UIElementWeakCollection.Size <br> UIElementWeakCollection.#ctor

#### <a name="uielementhttpsdocsmicrosoftcomuwpapiwindowsuixamluielement"></a>[UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)

UIElement.ActualOffset <br> UIElement.ActualSize <br> UIElement.Shadow <br> UIElement.ShadowProperty <br> UIElement.UIContext <br> UIElement.XamlRoot

#### <a name="windowhttpsdocsmicrosoftcomuwpapiwindowsuixamlwindow"></a>[Window](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window)

Window.UIContext

#### <a name="xamlroothttpsdocsmicrosoftcomuwpapiwindowsuixamlxamlroot"></a>[XamlRoot](https://docs.microsoft.com/uwp/api/windows.ui.xaml.xamlroot)

XamlRoot

#### <a name="xamlrootchangedeventargshttpsdocsmicrosoftcomuwpapiwindowsuixamlxamlrootchangedeventargs"></a>[XamlRootChangedEventArgs](https://docs.microsoft.com/uwp/api/windows.ui.xaml.xamlrootchangedeventargs)

XamlRootChangedEventArgs

#### <a name="xamlroothttpsdocsmicrosoftcomuwpapiwindowsuixamlxamlroot"></a>[XamlRoot](https://docs.microsoft.com/uwp/api/windows.ui.xaml.xamlroot)

XamlRoot.Changed <br> XamlRoot.Content <br> XamlRoot.IsHostVisible <br> XamlRoot.RasterizationScale <br> XamlRoot.Size <br> XamlRoot.UIContext

### <a name="windowsuixamlcontrolshttpsdocsmicrosoftcomuwpapiwindowsuixamlcontrols"></a>[Windows.UI.Xaml.Controls](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls)

#### <a name="datepickerflyoutpresenterhttpsdocsmicrosoftcomuwpapiwindowsuixamlcontrolsdatepickerflyoutpresenter"></a>[DatePickerFlyoutPresenter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.datepickerflyoutpresenter)

DatePickerFlyoutPresenter.IsDefaultShadowEnabled <br> DatePickerFlyoutPresenter.IsDefaultShadowEnabledProperty

#### <a name="flyoutpresenterhttpsdocsmicrosoftcomuwpapiwindowsuixamlcontrolsflyoutpresenter"></a>[FlyoutPresenter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.flyoutpresenter)

FlyoutPresenter.IsDefaultShadowEnabled <br> FlyoutPresenter.IsDefaultShadowEnabledProperty

#### <a name="inktoolbarhttpsdocsmicrosoftcomuwpapiwindowsuixamlcontrolsinktoolbar"></a>[InkToolbar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar)

InkToolbar.TargetInkPresenter <br> InkToolbar.TargetInkPresenterProperty

#### <a name="menuflyoutpresenterhttpsdocsmicrosoftcomuwpapiwindowsuixamlcontrolsmenuflyoutpresenter"></a>[MenuFlyoutPresenter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.menuflyoutpresenter)

MenuFlyoutPresenter.IsDefaultShadowEnabled <br> MenuFlyoutPresenter.IsDefaultShadowEnabledProperty

#### <a name="timepickerflyoutpresenterhttpsdocsmicrosoftcomuwpapiwindowsuixamlcontrolstimepickerflyoutpresenter"></a>[TimePickerFlyoutPresenter](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.timepickerflyoutpresenter)

TimePickerFlyoutPresenter.IsDefaultShadowEnabled <br> TimePickerFlyoutPresenter.IsDefaultShadowEnabledProperty

#### <a name="twopaneviewhttpsdocsmicrosoftcomuwpapiwindowsuixamlcontrolstwopaneview"></a>[TwoPaneView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.twopaneview)

TwoPaneView

#### <a name="twopaneviewmodehttpsdocsmicrosoftcomuwpapiwindowsuixamlcontrolstwopaneviewmode"></a>[TwoPaneViewMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.twopaneviewmode)

TwoPaneViewMode

#### <a name="twopaneviewpriorityhttpsdocsmicrosoftcomuwpapiwindowsuixamlcontrolstwopaneviewpriority"></a>[TwoPaneViewPriority](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.twopaneviewpriority)

TwoPaneViewPriority

#### <a name="twopaneviewtallmodeconfigurationhttpsdocsmicrosoftcomuwpapiwindowsuixamlcontrolstwopaneviewtallmodeconfiguration"></a>[TwoPaneViewTallModeConfiguration](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.twopaneviewtallmodeconfiguration)

TwoPaneViewTallModeConfiguration

#### <a name="twopaneviewhttpsdocsmicrosoftcomuwpapiwindowsuixamlcontrolstwopaneview"></a>[TwoPaneView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.twopaneview)

TwoPaneView.MinTallModeHeight <br> TwoPaneView.MinTallModeHeightProperty <br> TwoPaneView.MinWideModeWidth <br> TwoPaneView.MinWideModeWidthProperty <br> TwoPaneView.Mode <br> TwoPaneView.ModeChanged <br> TwoPaneView.ModeProperty <br> TwoPaneView.Pane1 <br> TwoPaneView.Pane1Length <br> TwoPaneView.Pane1LengthProperty <br> TwoPaneView.Pane1Property <br> TwoPaneView.Pane2 <br> TwoPaneView.Pane2Length <br> TwoPaneView.Pane2LengthProperty <br> TwoPaneView.Pane2Property <br> TwoPaneView.PanePriority <br> TwoPaneView.PanePriorityProperty <br> TwoPaneView.TallModeConfiguration <br> TwoPaneView.TallModeConfigurationProperty <br> TwoPaneView.#ctor <br> TwoPaneView.WideModeConfiguration <br> TwoPaneView.WideModeConfigurationProperty

### <a name="windowsuixamlcontrolsmapshttpsdocsmicrosoftcomuwpapiwindowsuixamlcontrolsmaps"></a>[Windows.UI.Xaml.Controls.Maps](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps)

#### <a name="mapcontrolhttpsdocsmicrosoftcomuwpapiwindowsuixamlcontrolsmapsmapcontrol"></a>[MapControl](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol)

MapControl.CanTiltDown <br> MapControl.CanTiltDownProperty <br> MapControl.CanTiltUp <br> MapControl.CanTiltUpProperty <br> MapControl.CanZoomIn <br> MapControl.CanZoomInProperty <br> MapControl.CanZoomOut <br> MapControl.CanZoomOutProperty

### <a name="windowsuixamlcontrolsprimitiveshttpsdocsmicrosoftcomuwpapiwindowsuixamlcontrolsprimitives"></a>[Windows.UI.Xaml.Controls.Primitives](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives)

#### <a name="appbartemplatesettingshttpsdocsmicrosoftcomuwpapiwindowsuixamlcontrolsprimitivesappbartemplatesettings"></a>[AppBarTemplateSettings](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.appbartemplatesettings)

AppBarTemplateSettings.NegativeCompactVerticalDelta <br> AppBarTemplateSettings.NegativeHiddenVerticalDelta <br> AppBarTemplateSettings.NegativeMinimalVerticalDelta

#### <a name="commandbartemplatesettingshttpsdocsmicrosoftcomuwpapiwindowsuixamlcontrolsprimitivescommandbartemplatesettings"></a>[CommandBarTemplateSettings](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.commandbartemplatesettings)

CommandBarTemplateSettings.OverflowContentCompactYTranslation <br> CommandBarTemplateSettings.OverflowContentHiddenYTranslation <br> CommandBarTemplateSettings.OverflowContentMinimalYTranslation

#### <a name="flyoutbasehttpsdocsmicrosoftcomuwpapiwindowsuixamlcontrolsprimitivesflyoutbase"></a>[FlyoutBase](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase)

FlyoutBase.IsConstrainedToRootBounds <br> FlyoutBase.ShouldConstrainToRootBounds <br> FlyoutBase.ShouldConstrainToRootBoundsProperty <br> FlyoutBase.XamlRoot

#### <a name="popuphttpsdocsmicrosoftcomuwpapiwindowsuixamlcontrolsprimitivespopup"></a>[Popup](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.popup)

Popup.IsConstrainedToRootBounds <br> Popup.ShouldConstrainToRootBounds <br> Popup.ShouldConstrainToRootBoundsProperty

### <a name="windowsuixamldocumentshttpsdocsmicrosoftcomuwpapiwindowsuixamldocuments"></a>[Windows.UI.Xaml.Documents](https://docs.microsoft.com/uwp/api/windows.ui.xaml.documents)

#### <a name="textelementhttpsdocsmicrosoftcomuwpapiwindowsuixamldocumentstextelement"></a>[TextElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.documents.textelement)

TextElement.XamlRoot

### <a name="windowsuixamlhostinghttpsdocsmicrosoftcomuwpapiwindowsuixamlhosting"></a>[Windows.UI.Xaml.Hosting](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting)

#### <a name="elementcompositionpreviewhttpsdocsmicrosoftcomuwpapiwindowsuixamlhostingelementcompositionpreview"></a>[ElementCompositionPreview](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.elementcompositionpreview)

ElementCompositionPreview.GetAppWindowContent <br> ElementCompositionPreview.SetAppWindowContent

### <a name="windowsuixamlinputhttpsdocsmicrosoftcomuwpapiwindowsuixamlinput"></a>[Windows.UI.Xaml.Input](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input)

#### <a name="focusmanagerhttpsdocsmicrosoftcomuwpapiwindowsuixamlinputfocusmanager"></a>[FocusManager](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.focusmanager)

FocusManager.GetFocusedElement

### <a name="windowsuixamlmediahttpsdocsmicrosoftcomuwpapiwindowsuixamlmedia"></a>[Windows.UI.Xaml.Media](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media)

#### <a name="acrylicbrushhttpsdocsmicrosoftcomuwpapiwindowsuixamlmediaacrylicbrush"></a>[AcrylicBrush](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.acrylicbrush)

AcrylicBrush.TintLuminosityOpacity <br> AcrylicBrush.TintLuminosityOpacityProperty

#### <a name="shadowhttpsdocsmicrosoftcomuwpapiwindowsuixamlmediashadow"></a>[Shadow](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.shadow)

그림자

#### <a name="themeshadowhttpsdocsmicrosoftcomuwpapiwindowsuixamlmediathemeshadow"></a>[ThemeShadow](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.themeshadow)

ThemeShadow <br> ThemeShadow.Receivers <br> ThemeShadow.#ctor

#### <a name="visualtreehelperhttpsdocsmicrosoftcomuwpapiwindowsuixamlmediavisualtreehelper"></a>[VisualTreeHelper](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.visualtreehelper)

VisualTreeHelper.GetOpenPopupsForXamlRoot

### <a name="windowsuixamlmediaanimationhttpsdocsmicrosoftcomuwpapiwindowsuixamlmediaanimation"></a>[Windows.UI.Xaml.Media.Animation](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation)

#### <a name="gravityconnectedanimationconfigurationhttpsdocsmicrosoftcomuwpapiwindowsuixamlmediaanimationgravityconnectedanimationconfiguration"></a>[GravityConnectedAnimationConfiguration](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.gravityconnectedanimationconfiguration)

GravityConnectedAnimationConfiguration.IsShadowEnabled

## <a name="windowsweb"></a>Windows.Web

### <a name="windowswebhttphttpsdocsmicrosoftcomuwpapiwindowswebhttp"></a>[Windows.Web.Http](https://docs.microsoft.com/uwp/api/windows.web.http)

#### <a name="httpclienthttpsdocsmicrosoftcomuwpapiwindowswebhttphttpclient"></a>[HttpClient](https://docs.microsoft.com/uwp/api/windows.web.http.httpclient)

HttpClient.TryDeleteAsync <br> HttpClient.TryGetAsync <br> HttpClient.TryGetAsync <br> HttpClient.TryGetBufferAsync <br> HttpClient.TryGetInputStreamAsync <br> HttpClient.TryGetStringAsync <br> HttpClient.TryPostAsync <br> HttpClient.TryPutAsync <br> HttpClient.TrySendRequestAsync <br> HttpClient.TrySendRequestAsync

#### <a name="httpgetbufferresulthttpsdocsmicrosoftcomuwpapiwindowswebhttphttpgetbufferresult"></a>[HttpGetBufferResult](https://docs.microsoft.com/uwp/api/windows.web.http.httpgetbufferresult)

HttpGetBufferResult <br> HttpGetBufferResult.Close <br> HttpGetBufferResult.ExtendedError <br> HttpGetBufferResult.RequestMessage <br> HttpGetBufferResult.ResponseMessage <br> HttpGetBufferResult.Succeeded <br> HttpGetBufferResult.ToString <br> HttpGetBufferResult.Value

#### <a name="httpgetinputstreamresulthttpsdocsmicrosoftcomuwpapiwindowswebhttphttpgetinputstreamresult"></a>[HttpGetInputStreamResult](https://docs.microsoft.com/uwp/api/windows.web.http.httpgetinputstreamresult)

HttpGetInputStreamResult <br> HttpGetInputStreamResult.Close <br> HttpGetInputStreamResult.ExtendedError <br> HttpGetInputStreamResult.RequestMessage <br> HttpGetInputStreamResult.ResponseMessage <br> HttpGetInputStreamResult.Succeeded <br> HttpGetInputStreamResult.ToString <br> HttpGetInputStreamResult.Value

#### <a name="httpgetstringresulthttpsdocsmicrosoftcomuwpapiwindowswebhttphttpgetstringresult"></a>[HttpGetStringResult](https://docs.microsoft.com/uwp/api/windows.web.http.httpgetstringresult)

HttpGetStringResult <br> HttpGetStringResult.Close <br> HttpGetStringResult.ExtendedError <br> HttpGetStringResult.RequestMessage <br> HttpGetStringResult.ResponseMessage <br> HttpGetStringResult.Succeeded <br> HttpGetStringResult.ToString <br> HttpGetStringResult.Value

#### <a name="httprequestresulthttpsdocsmicrosoftcomuwpapiwindowswebhttphttprequestresult"></a>[HttpRequestResult](https://docs.microsoft.com/uwp/api/windows.web.http.httprequestresult)

HttpRequestResult <br> HttpRequestResult.Close <br> HttpRequestResult.ExtendedError <br> HttpRequestResult.RequestMessage <br> HttpRequestResult.ResponseMessage <br> HttpRequestResult.Succeeded <br> HttpRequestResult.ToString

### <a name="windowswebhttpfiltershttpsdocsmicrosoftcomuwpapiwindowswebhttpfilters"></a>[Windows.Web.Http.Filters](https://docs.microsoft.com/uwp/api/windows.web.http.filters)

#### <a name="httpbaseprotocolfilterhttpsdocsmicrosoftcomuwpapiwindowswebhttpfiltershttpbaseprotocolfilter"></a>[HttpBaseProtocolFilter](https://docs.microsoft.com/uwp/api/windows.web.http.filters.httpbaseprotocolfilter)

HttpBaseProtocolFilter.CreateForUser <br> HttpBaseProtocolFilter.User

IWebViewControl2 <br> IWebViewControl2.AddInitializeScript
