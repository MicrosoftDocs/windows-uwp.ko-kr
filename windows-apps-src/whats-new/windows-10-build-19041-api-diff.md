---
title: Windows 10 빌드 19041 API 변경 사항
description: 개발자는 다음 목록을 사용하여 Windows 10 빌드 19041에서 새롭게 추가되었거나 변경된 네임스페이스를 식별할 수 있습니다.
keywords: Windows 10 API, 19041, 2004
ms.date: 05/12/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ed79a18b451c3237463fbb2910e5e69e46b6a6ef
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89166867"
---
# <a name="new-apis-in-windows-10-build-19041"></a>Windows 10 빌드 19041의 새로운 API

신규 및 업데이트 API 네임스페이스가 Windows 10 빌드 19041(SDK 버전 2004라고도 함)의 개발자들에게 제공되었습니다. 아래는 이 릴리스에서 추가 또는 수정된 네임스페이스에 게시된 설명서의 전체 목록입니다.

이전의 공개 릴리스에 추가된 API에 대한 정보는 [Windows 10 10월 업데이트의 신규 API](windows-10-build-18362-api-diff.md)를 참조하세요.

## <a name="windowsai"></a>Windows.AI

### <a name="windowsaimachinelearning"></a>[Windows.AI.MachineLearning](/uwp/api/windows.ai.machinelearning)

#### <a name="learningmodelsessionoptions"></a>[LearningModelSessionOptions](/uwp/api/windows.ai.machinelearning.learningmodelsessionoptions)

LearningModelSessionOptions.CloseModelOnSessionCreation

## <a name="windowsapplicationmodel"></a>Windows.ApplicationModel

### <a name="windowsapplicationmodelbackground"></a>[Windows.ApplicationModel.Background](/uwp/api/windows.applicationmodel.background)

#### <a name="backgroundtaskbuilder"></a>[BackgroundTaskBuilder](/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder)

BackgroundTaskBuilder.SetTaskEntryPointClsid

#### <a name="bluetoothleadvertisementpublishertrigger"></a>[BluetoothLEAdvertisementPublisherTrigger](/uwp/api/windows.applicationmodel.background.bluetoothleadvertisementpublishertrigger)

BluetoothLEAdvertisementPublisherTrigger.IncludeTransmitPowerLevel <br> BluetoothLEAdvertisementPublisherTrigger.IsAnonymous <br> BluetoothLEAdvertisementPublisherTrigger.PreferredTransmitPowerLevelInDBm <br> BluetoothLEAdvertisementPublisherTrigger.UseExtendedFormat

#### <a name="bluetoothleadvertisementwatchertrigger"></a>[BluetoothLEAdvertisementWatcherTrigger](/uwp/api/windows.applicationmodel.background.bluetoothleadvertisementwatchertrigger)

BluetoothLEAdvertisementWatcherTrigger.AllowExtendedAdvertisements

### <a name="windowsapplicationmodelconversationalagent"></a>[Windows.ApplicationModel.ConversationalAgent](/uwp/api/windows.applicationmodel.conversationalagent)

#### <a name="activationsignaldetectionconfiguration"></a>[ActivationSignalDetectionConfiguration](/uwp/api/windows.applicationmodel.conversationalagent.activationsignaldetectionconfiguration)

ActivationSignalDetectionConfiguration <br> ActivationSignalDetectionConfiguration.ApplyTrainingData <br> ActivationSignalDetectionConfiguration.ApplyTrainingDataAsync <br> ActivationSignalDetectionConfiguration.AvailabilityChanged <br> ActivationSignalDetectionConfiguration.AvailabilityInfo <br> ActivationSignalDetectionConfiguration.ClearModelData <br> ActivationSignalDetectionConfiguration.ClearModelDataAsync <br> ActivationSignalDetectionConfiguration.ClearTrainingData <br> ActivationSignalDetectionConfiguration.ClearTrainingDataAsync <br> ActivationSignalDetectionConfiguration.DisplayName <br> ActivationSignalDetectionConfiguration.GetModelData <br> ActivationSignalDetectionConfiguration.GetModelDataAsync <br> ActivationSignalDetectionConfiguration.GetModelDataType <br> ActivationSignalDetectionConfiguration.GetModelDataTypeAsync <br> ActivationSignalDetectionConfiguration.IsActive <br> ActivationSignalDetectionConfiguration.ModelId <br> ActivationSignalDetectionConfiguration.SetEnabled <br> ActivationSignalDetectionConfiguration.SetEnabledAsync <br> ActivationSignalDetectionConfiguration.SetModelData <br> ActivationSignalDetectionConfiguration.SetModelDataAsync <br> ActivationSignalDetectionConfiguration.SignalId <br> ActivationSignalDetectionConfiguration.TrainingDataFormat <br> ActivationSignalDetectionConfiguration.TrainingStepsCompleted <br> ActivationSignalDetectionConfiguration.TrainingStepsRemaining

#### <a name="activationsignaldetectiontrainingdataformat"></a>[ActivationSignalDetectionTrainingDataFormat](/uwp/api/windows.applicationmodel.conversationalagent.activationsignaldetectiontrainingdataformat)

ActivationSignalDetectionTrainingDataFormat

#### <a name="activationsignaldetector"></a>[ActivationSignalDetector](/uwp/api/windows.applicationmodel.conversationalagent.activationsignaldetector)

ActivationSignalDetector <br> ActivationSignalDetector.CanCreateConfigurations <br> ActivationSignalDetector.CreateConfiguration <br> ActivationSignalDetector.CreateConfigurationAsync <br> ActivationSignalDetector.GetConfiguration <br> ActivationSignalDetector.GetConfigurationAsync <br> ActivationSignalDetector.GetConfigurations <br> ActivationSignalDetector.GetConfigurationsAsync <br> ActivationSignalDetector.GetSupportedModelIdsForSignalId <br> ActivationSignalDetector.GetSupportedModelIdsForSignalIdAsync <br> ActivationSignalDetector.Kind <br> ActivationSignalDetector.ProviderId <br> ActivationSignalDetector.RemoveConfiguration <br> ActivationSignalDetector.RemoveConfigurationAsync <br> ActivationSignalDetector.SupportedModelDataTypes <br> ActivationSignalDetector.SupportedPowerStates <br> ActivationSignalDetector.SupportedTrainingDataFormats

#### <a name="activationsignaldetectorkind"></a>[ActivationSignalDetectorKind](/uwp/api/windows.applicationmodel.conversationalagent.activationsignaldetectorkind)

ActivationSignalDetectorKind

#### <a name="activationsignaldetectorpowerstate"></a>[ActivationSignalDetectorPowerState](/uwp/api/windows.applicationmodel.conversationalagent.activationsignaldetectorpowerstate)

ActivationSignalDetectorPowerState

#### <a name="conversationalagentdetectormanager"></a>[ConversationalAgentDetectorManager](/uwp/api/windows.applicationmodel.conversationalagent.conversationalagentdetectormanager)

ConversationalAgentDetectorManager <br> ConversationalAgentDetectorManager.Default <br> ConversationalAgentDetectorManager.GetActivationSignalDetectors <br> ConversationalAgentDetectorManager.GetActivationSignalDetectorsAsync <br> ConversationalAgentDetectorManager.GetAllActivationSignalDetectors <br> ConversationalAgentDetectorManager.GetAllActivationSignalDetectorsAsync

#### <a name="detectionconfigurationavailabilitychangekind"></a>[DetectionConfigurationAvailabilityChangeKind](/uwp/api/windows.applicationmodel.conversationalagent.detectionconfigurationavailabilitychangedeventargs)

DetectionConfigurationAvailabilityChangeKind <br> DetectionConfigurationAvailabilityChangedEventArgs

#### <a name="detectionconfigurationavailabilitychangedeventargs"></a>[DetectionConfigurationAvailabilityChangedEventArgs](/uwp/api/windows.applicationmodel.conversationalagent.detectionconfigurationavailabilitychangekind)

DetectionConfigurationAvailabilityChangedEventArgs.Kind

#### <a name="detectionconfigurationavailabilityinfo"></a>[DetectionConfigurationAvailabilityInfo](/uwp/api/windows.applicationmodel.conversationalagent.detectionconfigurationavailabilityinfo)

DetectionConfigurationAvailabilityInfo <br> DetectionConfigurationAvailabilityInfo.HasLockScreenPermission <br> DetectionConfigurationAvailabilityInfo.HasPermission <br> DetectionConfigurationAvailabilityInfo.HasSystemResourceAccess <br> DetectionConfigurationAvailabilityInfo.IsEnabled

#### <a name="detectionconfigurationtrainingstatus"></a>[DetectionConfigurationTrainingStatus](/uwp/api/windows.applicationmodel.conversationalagent.detectionconfigurationtrainingstatus)

DetectionConfigurationTrainingStatus

### <a name="windowsapplicationmodel"></a>[Windows.ApplicationModel](/uwp/api/windows.applicationmodel)

#### <a name="appinfo"></a>[AppInfo](/uwp/api/windows.applicationmodel.appinfo)

AppInfo.Current <br> AppInfo.GetFromAppUserModelId <br> AppInfo.GetFromAppUserModelIdForUser <br> AppInfo.Package

#### <a name="iappinfostatics"></a>[IAppInfoStatics](/uwp/api/windows.applicationmodel.iappinfostatics)

IAppInfoStatics <br> IAppInfoStatics.Current <br> IAppInfoStatics.GetFromAppUserModelId <br> IAppInfoStatics.GetFromAppUserModelIdForUser

#### <a name="package"></a>[Package](/uwp/api/windows.applicationmodel.package)

Package.EffectiveExternalLocation <br> Package.EffectiveExternalPath <br> Package.EffectivePath <br> Package.GetAppListEntries <br> Package.GetLogoAsRandomAccessStreamReference <br> Package.InstalledPath <br> Package.IsStub <br> Package.MachineExternalLocation <br> Package.MachineExternalPath <br> Package.MutablePath <br> Package.UserExternalLocation <br> Package.UserExternalPath

## <a name="windowsdevices"></a>Windows.Devices

### <a name="windowsdevicesbluetoothadvertisement"></a>[Windows.Devices.Bluetooth.Advertisement](/uwp/api/windows.devices.bluetooth.advertisement)

#### <a name="bluetoothleadvertisementpublisher"></a>[BluetoothLEAdvertisementPublisher](/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementpublisher)

BluetoothLEAdvertisementPublisher.IncludeTransmitPowerLevel <br> BluetoothLEAdvertisementPublisher.IsAnonymous <br> BluetoothLEAdvertisementPublisher.PreferredTransmitPowerLevelInDBm <br> BluetoothLEAdvertisementPublisher.UseExtendedAdvertisement

#### <a name="bluetoothleadvertisementpublisherstatuschangedeventargs"></a>[BluetoothLEAdvertisementPublisherStatusChangedEventArgs](/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementpublisherstatuschangedeventargs)

BluetoothLEAdvertisementPublisherStatusChangedEventArgs.SelectedTransmitPowerLevelInDBm

#### <a name="bluetoothleadvertisementreceivedeventargs"></a>[BluetoothLEAdvertisementReceivedEventArgs](/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementreceivedeventargs)

BluetoothLEAdvertisementReceivedEventArgs.BluetoothAddressType <br> BluetoothLEAdvertisementReceivedEventArgs.IsAnonymous <br> BluetoothLEAdvertisementReceivedEventArgs.IsConnectable <br> BluetoothLEAdvertisementReceivedEventArgs.IsDirected <br> BluetoothLEAdvertisementReceivedEventArgs.IsScanResponse <br> BluetoothLEAdvertisementReceivedEventArgs.IsScannable <br> BluetoothLEAdvertisementReceivedEventArgs.TransmitPowerLevelInDBm

#### <a name="bluetoothleadvertisementwatcher"></a>[BluetoothLEAdvertisementWatcher](/uwp/api/windows.devices.bluetooth.advertisement.bluetoothleadvertisementwatcher)

BluetoothLEAdvertisementWatcher.AllowExtendedAdvertisements

### <a name="windowsdevicesbluetoothbackground"></a>[Windows.Devices.Bluetooth.Background](/uwp/api/windows.devices.bluetooth.background)

#### <a name="bluetoothleadvertisementpublishertriggerdetails"></a>[BluetoothLEAdvertisementPublisherTriggerDetails](/uwp/api/windows.devices.bluetooth.background.bluetoothleadvertisementpublishertriggerdetails)

BluetoothLEAdvertisementPublisherTriggerDetails.SelectedTransmitPowerLevelInDBm

### <a name="windowsdevicesbluetooth"></a>[Windows.Devices.Bluetooth](/uwp/api/windows.devices.bluetooth)

#### <a name="bluetoothadapter"></a>[BluetoothAdapter](/uwp/api/windows.devices.bluetooth.bluetoothadapter)

BluetoothAdapter.IsExtendedAdvertisingSupported <br> BluetoothAdapter.MaxAdvertisementDataLength

### <a name="windowsdevicesdisplay"></a>[Windows.Devices.Display](/uwp/api/windows.devices.display)

#### <a name="displaymonitor"></a>[DisplayMonitor](/uwp/api/windows.devices.display.displaymonitor)

DisplayMonitor.IsDolbyVisionSupportedInHdrMode

### <a name="windowsdevicesinput"></a>[Windows.Devices.Input](/uwp/api/windows.devices.input)

#### <a name="penbuttonlistener"></a>[PenButtonListener](/uwp/api/windows.devices.input.penbuttonlistener)

PenButtonListener <br> PenButtonListener.GetDefault <br> PenButtonListener.IsSupported <br> PenButtonListener.IsSupportedChanged <br> PenButtonListener.TailButtonClicked <br> PenButtonListener.TailButtonDoubleClicked <br> PenButtonListener.TailButtonLongPressed

#### <a name="pendocklistener"></a>[PenDockListener](/uwp/api/windows.devices.input.pendockedeventargs)

PenDockListener

#### <a name="pendocklistener"></a>[PenDockListener](/uwp/api/windows.devices.input.pendocklistener)

PenDockListener.Docked <br> PenDockListener.GetDefault <br> PenDockListener.IsSupported <br> PenDockListener.IsSupportedChanged <br> PenDockListener.Undocked <br> PenDockedEventArgs

#### <a name="pentailbuttonclickedeventargs"></a>[PenTailButtonClickedEventArgs](/uwp/api/windows.devices.input.pentailbuttonclickedeventargs)

PenTailButtonClickedEventArgs

#### <a name="pentailbuttondoubleclickedeventargs"></a>[PenTailButtonDoubleClickedEventArgs](/uwp/api/windows.devices.input.pentailbuttondoubleclickedeventargs)

PenTailButtonDoubleClickedEventArgs

#### <a name="pentailbuttonlongpressedeventargs"></a>[PenTailButtonLongPressedEventArgs](/uwp/api/windows.devices.input.pentailbuttonlongpressedeventargs)

PenTailButtonLongPressedEventArgs

#### <a name="penundockedeventargs"></a>[PenUndockedEventArgs](/uwp/api/windows.devices.input.penundockedeventargs)

PenUndockedEventArgs

### <a name="windowsdevicessensors"></a>[Windows.Devices.Sensors](/uwp/api/windows.devices.sensors)

#### <a name="accelerometer"></a>[Accelerometer](/uwp/api/windows.devices.sensors.accelerometer)

Accelerometer.ReportThreshold

#### <a name="accelerometerdatathreshold"></a>[AccelerometerDataThreshold](/uwp/api/windows.devices.sensors.accelerometerdatathreshold)

AccelerometerDataThreshold <br> AccelerometerDataThreshold.XAxisInGForce <br> AccelerometerDataThreshold.YAxisInGForce <br> AccelerometerDataThreshold.ZAxisInGForce

#### <a name="barometer"></a>[Barometer](/uwp/api/windows.devices.sensors.barometer)

Barometer.ReportThreshold

#### <a name="barometerdatathreshold"></a>[BarometerDataThreshold](/uwp/api/windows.devices.sensors.barometerdatathreshold)

BarometerDataThreshold <br> BarometerDataThreshold.Hectopascals

#### <a name="compass"></a>[Compass](/uwp/api/windows.devices.sensors.compass)

Compass.ReportThreshold

#### <a name="compassdatathreshold"></a>[CompassDataThreshold](/uwp/api/windows.devices.sensors.compassdatathreshold)

CompassDataThreshold <br> CompassDataThreshold.Degrees

#### <a name="gyrometer"></a>[Gyrometer](/uwp/api/windows.devices.sensors.gyrometer)

Gyrometer.ReportThreshold

#### <a name="gyrometerdatathreshold"></a>[GyrometerDataThreshold](/uwp/api/windows.devices.sensors.gyrometerdatathreshold)

GyrometerDataThreshold <br> GyrometerDataThreshold.XAxisInDegreesPerSecond <br> GyrometerDataThreshold.YAxisInDegreesPerSecond <br> GyrometerDataThreshold.ZAxisInDegreesPerSecond

#### <a name="inclinometer"></a>[Inclinometer](/uwp/api/windows.devices.sensors.inclinometer)

Inclinometer.ReportThreshold

#### <a name="inclinometerdatathreshold"></a>[InclinometerDataThreshold](/uwp/api/windows.devices.sensors.inclinometerdatathreshold)

InclinometerDataThreshold <br> InclinometerDataThreshold.PitchInDegrees <br> InclinometerDataThreshold.RollInDegrees <br> InclinometerDataThreshold.YawInDegrees

#### <a name="lightsensor"></a>[LightSensor](/uwp/api/windows.devices.sensors.lightsensor)

LightSensor.ReportThreshold

#### <a name="lightsensordatathreshold"></a>[LightSensorDataThreshold](/uwp/api/windows.devices.sensors.lightsensordatathreshold)

LightSensorDataThreshold <br> LightSensorDataThreshold.AbsoluteLux <br> LightSensorDataThreshold.LuxPercentage

#### <a name="magnetometer"></a>[Magnetometer](/uwp/api/windows.devices.sensors.magnetometer)

Magnetometer.ReportThreshold

#### <a name="magnetometerdatathreshold"></a>[MagnetometerDataThreshold](/uwp/api/windows.devices.sensors.magnetometerdatathreshold)

MagnetometerDataThreshold <br> MagnetometerDataThreshold.XAxisMicroteslas <br> MagnetometerDataThreshold.YAxisMicroteslas <br> MagnetometerDataThreshold.ZAxisMicroteslas

## <a name="windowsfoundation"></a>Windows.Foundation

### <a name="windowsfoundationmetadata"></a>[Windows.Foundation.Metadata](/uwp/api/windows.foundation.metadata)

#### <a name="attributenameattribute"></a>[AttributeNameAttribute](/uwp/api/windows.foundation.metadata.attributenameattribute)

AttributeNameAttribute <br> AttributeNameAttribute.#ctor

#### <a name="fastabiattribute"></a>[FastAbiAttribute](/uwp/api/windows.foundation.metadata.fastabiattribute)

FastAbiAttribute <br> FastAbiAttribute.#ctor <br> FastAbiAttribute.#ctor <br> FastAbiAttribute.#ctor

#### <a name="noexceptionattribute"></a>[NoExceptionAttribute](/uwp/api/windows.foundation.metadata.noexceptionattribute)

NoExceptionAttribute <br> NoExceptionAttribute.#ctor

## <a name="windowsglobalization"></a>Windows.Globalization

### <a name="windowsglobalization"></a>[Windows.Globalization](/uwp/api/windows.globalization)

#### <a name="language"></a>[언어](/uwp/api/windows.globalization.language)

Language.AbbreviatedName <br> Language.GetMuiCompatibleLanguageListFromLanguageTags

## <a name="windowsgraphics"></a>Windows.Graphics

### <a name="windowsgraphicscapture"></a>[Windows.Graphics.Capture](/uwp/api/windows.graphics.capture)

#### <a name="graphicscapturesession"></a>[GraphicsCaptureSession](/uwp/api/windows.graphics.capture.graphicscapturesession)

GraphicsCaptureSession.IsCursorCaptureEnabled

### <a name="windowsgraphicsholographic"></a>[Windows.Graphics.Holographic](/uwp/api/windows.graphics.holographic)

#### <a name="holographicframe"></a>[HolographicFrame](/uwp/api/windows.graphics.holographic.holographicframe)

HolographicFrame.Id

#### <a name="holographicframeid"></a>[HolographicFrameId](/uwp/api/windows.graphics.holographic.holographicframeid)

HolographicFrameId

#### <a name="holographicframerenderingreport"></a>[HolographicFrameRenderingReport](/uwp/api/windows.graphics.holographic.holographicframerenderingreport)

HolographicFrameRenderingReport <br> HolographicFrameRenderingReport.FrameId <br> HolographicFrameRenderingReport.MissedLatchCount <br> HolographicFrameRenderingReport.SystemRelativeActualGpuFinishTime <br> HolographicFrameRenderingReport.SystemRelativeFrameReadyTime <br> HolographicFrameRenderingReport.SystemRelativeTargetLatchTime

#### <a name="holographicframescanoutmonitor"></a>[HolographicFrameScanoutMonitor](/uwp/api/windows.graphics.holographic.holographicframescanoutmonitor)

HolographicFrameScanoutMonitor <br> HolographicFrameScanoutMonitor.Close <br> HolographicFrameScanoutMonitor.ReadReports

#### <a name="holographicframescanoutreport"></a>[HolographicFrameScanoutReport](/uwp/api/windows.graphics.holographic.holographicframescanoutreport)

HolographicFrameScanoutReport <br> HolographicFrameScanoutReport.MissedScanoutCount <br> HolographicFrameScanoutReport.RenderingReport <br> HolographicFrameScanoutReport.SystemRelativeLatchTime <br> HolographicFrameScanoutReport.SystemRelativePhotonTime <br> HolographicFrameScanoutReport.SystemRelativeScanoutStartTime

#### <a name="holographicspace"></a>[HolographicSpace](/uwp/api/windows.graphics.holographic.holographicspace)

HolographicSpace.CreateFrameScanoutMonitor

## <a name="windowsmanagement"></a>Windows.Management

### <a name="windowsmanagementdeployment"></a>[Windows.Management.Deployment](/uwp/api/windows.management.deployment)

#### <a name="addpackageoptions"></a>[AddPackageOptions](/uwp/api/windows.management.deployment.addpackageoptions)

AddPackageOptions <br> AddPackageOptions.#ctor <br> AddPackageOptions.AllowUnsigned <br> AddPackageOptions.DeferRegistrationWhenPackagesAreInUse <br> AddPackageOptions.DependencyPackageUris <br> AddPackageOptions.DeveloperMode <br> AddPackageOptions.ExternalLocationUri <br> AddPackageOptions.ForceAppShutdown <br> AddPackageOptions.ForceTargetAppShutdown <br> AddPackageOptions.ForceUpdateFromAnyVersion <br> AddPackageOptions.InstallAllResources <br> AddPackageOptions.OptionalPackageFamilyNames <br> AddPackageOptions.OptionalPackageUris <br> AddPackageOptions.RelatedPackageUris <br> AddPackageOptions.RequiredContentGroupOnly <br> AddPackageOptions.RetainFilesOnFailure <br> AddPackageOptions.StageInPlace <br> AddPackageOptions.StubPackageOption <br> AddPackageOptions.TargetVolume

#### <a name="packagemanager"></a>[PackageManager](/uwp/api/windows.management.deployment.packagemanager)

PackageManager.AddPackageByUriAsync <br> PackageManager.FindProvisionedPackages <br> PackageManager.GetPackageStubPreference <br> PackageManager.RegisterPackageByUriAsync <br> PackageManager.RegisterPackagesByFullNameAsync <br> PackageManager.SetPackageStubPreference <br> PackageManager.StagePackageByUriAsync

#### <a name="packagestubpreference"></a>[PackageStubPreference](/uwp/api/windows.management.deployment.packagestubpreference)

PackageStubPreference

#### <a name="registerpackageoptions"></a>[RegisterPackageOptions](/uwp/api/windows.management.deployment.registerpackageoptions)

RegisterPackageOptions <br> RegisterPackageOptions.#ctor <br> RegisterPackageOptions.AllowUnsigned <br> RegisterPackageOptions.AppDataVolume <br> RegisterPackageOptions.DeferRegistrationWhenPackagesAreInUse <br> RegisterPackageOptions.DependencyPackageUris <br> RegisterPackageOptions.DeveloperMode <br> RegisterPackageOptions.ExternalLocationUri <br> RegisterPackageOptions.ForceAppShutdown <br> RegisterPackageOptions.ForceTargetAppShutdown <br> RegisterPackageOptions.ForceUpdateFromAnyVersion <br> RegisterPackageOptions.InstallAllResources <br> RegisterPackageOptions.OptionalPackageFamilyNames <br> RegisterPackageOptions.StageInPlace

#### <a name="stagepackageoptions"></a>[StagePackageOptions](/uwp/api/windows.management.deployment.stagepackageoptions)

StagePackageOptions <br> StagePackageOptions.#ctor <br> StagePackageOptions.AllowUnsigned <br> StagePackageOptions.DependencyPackageUris <br> StagePackageOptions.DeveloperMode <br> StagePackageOptions.ExternalLocationUri <br> StagePackageOptions.ForceUpdateFromAnyVersion <br> StagePackageOptions.InstallAllResources <br> StagePackageOptions.OptionalPackageFamilyNames <br> StagePackageOptions.OptionalPackageUris <br> StagePackageOptions.RelatedPackageUris <br> StagePackageOptions.RequiredContentGroupOnly <br> StagePackageOptions.StageInPlace <br> StagePackageOptions.StubPackageOption <br> StagePackageOptions.TargetVolume

#### <a name="stubpackageoption"></a>[StubPackageOption](/uwp/api/windows.management.deployment.stubpackageoption)

StubPackageOption

## <a name="windowsmedia"></a>Windows.Media

### <a name="windowsmediaaudio"></a>[Windows.Media.Audio](/uwp/api/windows.media.audio)

#### <a name="audioplaybackconnection"></a>[AudioPlaybackConnection](/uwp/api/windows.media.audio.audioplaybackconnection)

AudioPlaybackConnection <br> AudioPlaybackConnection.Close <br> AudioPlaybackConnection.DeviceId <br> AudioPlaybackConnection.GetDeviceSelector <br> AudioPlaybackConnection.Open <br> AudioPlaybackConnection.OpenAsync <br> AudioPlaybackConnection.Start <br> AudioPlaybackConnection.StartAsync <br> AudioPlaybackConnection.State <br> AudioPlaybackConnection.StateChanged <br> AudioPlaybackConnection.TryCreateFromId

#### <a name="audioplaybackconnectionopenresult"></a>[AudioPlaybackConnectionOpenResult](/uwp/api/windows.media.audio.audioplaybackconnectionopenresult)

AudioPlaybackConnectionOpenResult <br> AudioPlaybackConnectionOpenResult.ExtendedError <br> AudioPlaybackConnectionOpenResult.Status

#### <a name="audioplaybackconnectionopenresultstatus"></a>[AudioPlaybackConnectionOpenResultStatus](/uwp/api/windows.media.audio.audioplaybackconnectionopenresultstatus)

AudioPlaybackConnectionOpenResultStatus

#### <a name="audioplaybackconnectionstate"></a>[AudioPlaybackConnectionState](/uwp/api/windows.media.audio.audioplaybackconnectionstate)

AudioPlaybackConnectionState

### <a name="windowsmediacaptureframes"></a>[Windows.Media.Capture.Frames](/uwp/api/windows.media.capture.frames)

#### <a name="mediaframesourceinfo"></a>[MediaFrameSourceInfo](/uwp/api/windows.media.capture.frames.mediaframesourceinfo)

MediaFrameSourceInfo.GetRelativePanel

### <a name="windowsmediacapture"></a>[Windows.Media.Capture](/uwp/api/windows.media.capture)

#### <a name="mediacapture"></a>[MediaCapture](/uwp/api/windows.media.capture.mediacapture)

MediaCapture.CreateRelativePanelWatcher

#### <a name="mediacaptureinitializationsettings"></a>[MediaCaptureInitializationSettings](/uwp/api/windows.media.capture.mediacaptureinitializationsettings)

MediaCaptureInitializationSettings.DeviceUri <br> MediaCaptureInitializationSettings.DeviceUriPasswordCredential

#### <a name="mediacapturerelativepanelwatcher"></a>[MediaCaptureRelativePanelWatcher](/uwp/api/windows.media.capture.mediacapturerelativepanelwatcher)

MediaCaptureRelativePanelWatcher <br> MediaCaptureRelativePanelWatcher.Changed <br> MediaCaptureRelativePanelWatcher.Close <br> MediaCaptureRelativePanelWatcher.RelativePanel <br> MediaCaptureRelativePanelWatcher.Start <br> MediaCaptureRelativePanelWatcher.Stop

### <a name="windowsmediadevices"></a>[Windows.Media.Devices](/uwp/api/windows.media.devices)

#### <a name="panelbasedoptimizationcontrol"></a>[PanelBasedOptimizationControl](/uwp/api/windows.media.devices.panelbasedoptimizationcontrol)

PanelBasedOptimizationControl <br> PanelBasedOptimizationControl.IsSupported <br> PanelBasedOptimizationControl.Panel

#### <a name="videodevicecontroller"></a>[VideoDeviceController](/uwp/api/windows.media.devices.videodevicecontroller)

VideoDeviceController.PanelBasedOptimizationControl

### <a name="windowsmediamediaproperties"></a>[Windows.Media.MediaProperties](/uwp/api/windows.media.mediaproperties)

#### <a name="mediaencodingsubtypes"></a>[MediaEncodingSubtypes](/uwp/api/windows.media.mediaproperties.mediaencodingsubtypes)

MediaEncodingSubtypes.Pgs <br> MediaEncodingSubtypes.Srt <br> MediaEncodingSubtypes.Ssa <br> MediaEncodingSubtypes.VobSub

#### <a name="timedmetadataencodingproperties"></a>[TimedMetadataEncodingProperties](/uwp/api/windows.media.mediaproperties.timedmetadataencodingproperties)

TimedMetadataEncodingProperties.CreatePgs <br> TimedMetadataEncodingProperties.CreateSrt <br> TimedMetadataEncodingProperties.CreateSsa <br> TimedMetadataEncodingProperties.CreateVobSub

## <a name="windowsnetworking"></a>Windows.Networking

### <a name="windowsnetworkingbackgroundtransfer"></a>[Windows.Networking.BackgroundTransfer](/uwp/api/windows.networking.backgroundtransfer)

#### <a name="downloadoperation"></a>[DownloadOperation](/uwp/api/windows.networking.backgroundtransfer.downloadoperation)

DownloadOperation.RemoveRequestHeader <br> DownloadOperation.SetRequestHeader

#### <a name="uploadoperation"></a>[UploadOperation](/uwp/api/windows.networking.backgroundtransfer.uploadoperation)

UploadOperation.RemoveRequestHeader <br> UploadOperation.SetRequestHeader

### <a name="windowsnetworkingnetworkoperators"></a>[Windows.Networking.NetworkOperators](/uwp/api/windows.networking.networkoperators)

#### <a name="networkoperatortetheringaccesspointconfiguration"></a>[NetworkOperatorTetheringAccessPointConfiguration](/uwp/api/windows.networking.networkoperators.networkoperatortetheringaccesspointconfiguration)

NetworkOperatorTetheringAccessPointConfiguration.Band <br> NetworkOperatorTetheringAccessPointConfiguration.IsBandSupported <br> NetworkOperatorTetheringAccessPointConfiguration.IsBandSupportedAsync

#### <a name="networkoperatortetheringmanager"></a>[NetworkOperatorTetheringManager](/uwp/api/windows.networking.networkoperators.networkoperatortetheringmanager)

NetworkOperatorTetheringManager.DisableNoConnectionsTimeout <br> NetworkOperatorTetheringManager.DisableNoConnectionsTimeoutAsync <br> NetworkOperatorTetheringManager.EnableNoConnectionsTimeout <br> NetworkOperatorTetheringManager.EnableNoConnectionsTimeoutAsync <br> NetworkOperatorTetheringManager.IsNoConnectionsTimeoutEnabled

#### <a name="tetheringwifiband"></a>[TetheringWiFiBand](/uwp/api/windows.networking.networkoperators.tetheringwifiband)

TetheringWiFiBand

### <a name="windowsnetworkingpushnotifications"></a>[Windows.Networking.PushNotifications](/uwp/api/windows.networking.pushnotifications)

#### <a name="rawnotification"></a>[RawNotification](/uwp/api/windows.networking.pushnotifications.rawnotification)

RawNotification.ContentBytes

## <a name="windowssecurity"></a>Windows.Security

### <a name="windowssecurityauthenticationwebcore"></a>[Windows.Security.Authentication.Web.Core](/uwp/api/windows.security.authentication.web.core)

#### <a name="webaccountmonitor"></a>[WebAccountMonitor](/uwp/api/windows.security.authentication.web.core.webaccountmonitor)

WebAccountMonitor.AccountPictureUpdated

### <a name="windowssecurityisolation"></a>[Windows.Security.Isolation](/uwp/api/windows.security.isolation)

#### <a name="isolatedwindowsenvironment"></a>[IsolatedWindowsEnvironment](/uwp/api/windows.security.isolation.isolatedwindowsenvironment)

IsolatedWindowsEnvironment <br> IsolatedWindowsEnvironment.CreateAsync <br> IsolatedWindowsEnvironment.CreateAsync <br> IsolatedWindowsEnvironment.FindByOwnerId <br> IsolatedWindowsEnvironment.GetById <br> IsolatedWindowsEnvironment.Id <br> IsolatedWindowsEnvironment.LaunchFileWithUIAsync <br> IsolatedWindowsEnvironment.LaunchFileWithUIAsync <br> IsolatedWindowsEnvironment.RegisterMessageReceiver <br> IsolatedWindowsEnvironment.ShareFolderAsync <br> IsolatedWindowsEnvironment.ShareFolderAsync <br> IsolatedWindowsEnvironment.StartProcessSilentlyAsync <br> IsolatedWindowsEnvironment.StartProcessSilentlyAsync <br> IsolatedWindowsEnvironment.TerminateAsync <br> IsolatedWindowsEnvironment.TerminateAsync

#### <a name="isolatedwindowsenvironment"></a>[IsolatedWindowsEnvironment](/uwp/api/windows.security.isolation.isolatedwindowsenvironmentactivator)

IsolatedWindowsEnvironment.UnregisterMessageReceiver

#### <a name="isolatedwindowsenvironmentactivator"></a>[IsolatedWindowsEnvironmentActivator](/uwp/api/windows.security.isolation.isolatedwindowsenvironmentallowedclipboardformats)

IsolatedWindowsEnvironmentActivator

#### <a name="isolatedwindowsenvironmentallowedclipboardformats"></a>[IsolatedWindowsEnvironmentAllowedClipboardFormats](/uwp/api/windows.security.isolation.isolatedwindowsenvironmentavailableprinters)

IsolatedWindowsEnvironmentAllowedClipboardFormats

#### <a name="isolatedwindowsenvironmentavailableprinters"></a>[IsolatedWindowsEnvironmentAvailablePrinters](/uwp/api/windows.security.isolation.isolatedwindowsenvironmentclipboardcopypastedirections)

IsolatedWindowsEnvironmentAvailablePrinters

#### <a name="isolatedwindowsenvironmentclipboardcopypastedirections"></a>[IsolatedWindowsEnvironmentClipboardCopyPasteDirections](/uwp/api/windows.security.isolation.isolatedwindowsenvironmentcreateprogress)

IsolatedWindowsEnvironmentClipboardCopyPasteDirections

#### <a name="isolatedwindowsenvironmentcreateprogress"></a>[IsolatedWindowsEnvironmentCreateProgress](/uwp/api/windows.security.isolation.isolatedwindowsenvironmentcreateresult)

IsolatedWindowsEnvironmentCreateProgress <br> IsolatedWindowsEnvironmentCreateResult <br> IsolatedWindowsEnvironmentCreateResult.Environment <br> IsolatedWindowsEnvironmentCreateResult.ExtendedError

#### <a name="isolatedwindowsenvironmentcreateresult"></a>[IsolatedWindowsEnvironmentCreateResult](/uwp/api/windows.security.isolation.isolatedwindowsenvironmentcreatestatus)

IsolatedWindowsEnvironmentCreateResult.Status

#### <a name="isolatedwindowsenvironmentcreatestatus"></a>[IsolatedWindowsEnvironmentCreateStatus](/uwp/api/windows.security.isolation.isolatedwindowsenvironmentfile)

IsolatedWindowsEnvironmentCreateStatus <br> IsolatedWindowsEnvironmentFile <br> IsolatedWindowsEnvironmentFile.Close <br> IsolatedWindowsEnvironmentFile.HostPath

#### <a name="isolatedwindowsenvironmentfile"></a>[IsolatedWindowsEnvironmentFile](/uwp/api/windows.security.isolation.isolatedwindowsenvironmenthost)

IsolatedWindowsEnvironmentFile.Id <br> IsolatedWindowsEnvironmentHost <br> IsolatedWindowsEnvironmentHost.HostErrors

#### <a name="isolatedwindowsenvironmenthost"></a>[IsolatedWindowsEnvironmentHost](/uwp/api/windows.security.isolation.isolatedwindowsenvironmenthosterror)

IsolatedWindowsEnvironmentHost.IsReady

#### <a name="isolatedwindowsenvironmenthosterror"></a>[IsolatedWindowsEnvironmentHostError](/uwp/api/windows.security.isolation.isolatedwindowsenvironmentlaunchfileresult)

IsolatedWindowsEnvironmentHostError <br> IsolatedWindowsEnvironmentLaunchFileResult <br> IsolatedWindowsEnvironmentLaunchFileResult.ExtendedError <br> IsolatedWindowsEnvironmentLaunchFileResult.File

#### <a name="isolatedwindowsenvironmentlaunchfileresult"></a>[IsolatedWindowsEnvironmentLaunchFileResult](/uwp/api/windows.security.isolation.isolatedwindowsenvironmentlaunchfilestatus)

IsolatedWindowsEnvironmentLaunchFileResult.Status

#### <a name="isolatedwindowsenvironmentlaunchfilestatus"></a>[IsolatedWindowsEnvironmentLaunchFileStatus](/uwp/api/windows.security.isolation.isolatedwindowsenvironmentoptions)

IsolatedWindowsEnvironmentLaunchFileStatus <br> IsolatedWindowsEnvironmentOptions <br> IsolatedWindowsEnvironmentOptions.#ctor <br> IsolatedWindowsEnvironmentOptions.AllowCameraAndMicrophoneAccess <br> IsolatedWindowsEnvironmentOptions.AllowGraphicsHardwareAcceleration <br> IsolatedWindowsEnvironmentOptions.AllowedClipboardFormats <br> IsolatedWindowsEnvironmentOptions.AvailablePrinters <br> IsolatedWindowsEnvironmentOptions.ClipboardCopyPasteDirections <br> IsolatedWindowsEnvironmentOptions.EnvironmentOwnerId <br> IsolatedWindowsEnvironmentOptions.PersistUserProfile <br> IsolatedWindowsEnvironmentOptions.ShareHostFolderForUntrustedItems <br> IsolatedWindowsEnvironmentOptions.SharedFolderNameInEnvironment

#### <a name="isolatedwindowsenvironmentoptions"></a>[IsolatedWindowsEnvironmentOptions](/uwp/api/windows.security.isolation.isolatedwindowsenvironmentownerregistration)

IsolatedWindowsEnvironmentOptions.SharedHostFolderPath <br> IsolatedWindowsEnvironmentOwnerRegistration <br> IsolatedWindowsEnvironmentOwnerRegistration.Register

#### <a name="isolatedwindowsenvironmentownerregistration"></a>[IsolatedWindowsEnvironmentOwnerRegistration](/uwp/api/windows.security.isolation.isolatedwindowsenvironmentownerregistrationdata)

IsolatedWindowsEnvironmentOwnerRegistration.Unregister <br> IsolatedWindowsEnvironmentOwnerRegistrationData <br> IsolatedWindowsEnvironmentOwnerRegistrationData.#ctor <br> IsolatedWindowsEnvironmentOwnerRegistrationData.ActivationFileExtensions <br> IsolatedWindowsEnvironmentOwnerRegistrationData.ProcessesRunnableAsSystem <br> IsolatedWindowsEnvironmentOwnerRegistrationData.ProcessesRunnableAsUser

#### <a name="isolatedwindowsenvironmentownerregistrationdata"></a>[IsolatedWindowsEnvironmentOwnerRegistrationData](/uwp/api/windows.security.isolation.isolatedwindowsenvironmentownerregistrationresult)

IsolatedWindowsEnvironmentOwnerRegistrationData.ShareableFolders <br> IsolatedWindowsEnvironmentOwnerRegistrationResult <br> IsolatedWindowsEnvironmentOwnerRegistrationResult.ExtendedError

#### <a name="isolatedwindowsenvironmentownerregistrationresult"></a>[IsolatedWindowsEnvironmentOwnerRegistrationResult](/uwp/api/windows.security.isolation.isolatedwindowsenvironmentownerregistrationstatus)

IsolatedWindowsEnvironmentOwnerRegistrationResult.Status

#### <a name="isolatedwindowsenvironmentownerregistrationstatus"></a>[IsolatedWindowsEnvironmentOwnerRegistrationStatus](/uwp/api/windows.security.isolation.isolatedwindowsenvironmentprocess)

IsolatedWindowsEnvironmentOwnerRegistrationStatus <br> IsolatedWindowsEnvironmentProcess <br> IsolatedWindowsEnvironmentProcess.ExitCode <br> IsolatedWindowsEnvironmentProcess.State <br> IsolatedWindowsEnvironmentProcess.WaitForExit <br> IsolatedWindowsEnvironmentProcess.WaitForExitAsync

#### <a name="isolatedwindowsenvironmentprocess"></a>[IsolatedWindowsEnvironmentProcess](/uwp/api/windows.security.isolation.isolatedwindowsenvironmentprocessstate)

IsolatedWindowsEnvironmentProcess.WaitForExitWithTimeout

#### <a name="isolatedwindowsenvironmentprocessstate"></a>[IsolatedWindowsEnvironmentProcessState](/uwp/api/windows.security.isolation.isolatedwindowsenvironmentprogressstate)

IsolatedWindowsEnvironmentProcessState

#### <a name="isolatedwindowsenvironmentprogressstate"></a>[IsolatedWindowsEnvironmentProgressState](/uwp/api/windows.security.isolation.isolatedwindowsenvironmentsharefolderrequestoptions)

IsolatedWindowsEnvironmentProgressState <br> IsolatedWindowsEnvironmentShareFolderRequestOptions <br> IsolatedWindowsEnvironmentShareFolderRequestOptions.#ctor

#### <a name="isolatedwindowsenvironmentsharefolderrequestoptions"></a>[IsolatedWindowsEnvironmentShareFolderRequestOptions](/uwp/api/windows.security.isolation.isolatedwindowsenvironmentsharefolderresult)

IsolatedWindowsEnvironmentShareFolderRequestOptions.AllowWrite <br> IsolatedWindowsEnvironmentShareFolderResult <br> IsolatedWindowsEnvironmentShareFolderResult.ExtendedError

#### <a name="isolatedwindowsenvironmentsharefolderresult"></a>[IsolatedWindowsEnvironmentShareFolderResult](/uwp/api/windows.security.isolation.isolatedwindowsenvironmentsharefolderstatus)

IsolatedWindowsEnvironmentShareFolderResult.Status

#### <a name="isolatedwindowsenvironmentsharefolderstatus"></a>[IsolatedWindowsEnvironmentShareFolderStatus](/uwp/api/windows.security.isolation.isolatedwindowsenvironmentstartprocessresult)

IsolatedWindowsEnvironmentShareFolderStatus <br> IsolatedWindowsEnvironmentStartProcessResult <br> IsolatedWindowsEnvironmentStartProcessResult.ExtendedError <br> IsolatedWindowsEnvironmentStartProcessResult.Process

#### <a name="isolatedwindowsenvironmentstartprocessresult"></a>[IsolatedWindowsEnvironmentStartProcessResult](/uwp/api/windows.security.isolation.isolatedwindowsenvironmentstartprocessstatus)

IsolatedWindowsEnvironmentStartProcessResult.Status

#### <a name="isolatedwindowsenvironmentstartprocessstatus"></a>[IsolatedWindowsEnvironmentStartProcessStatus](/uwp/api/windows.security.isolation.isolatedwindowsenvironmenttelemetryparameters)

IsolatedWindowsEnvironmentStartProcessStatus <br> IsolatedWindowsEnvironmentTelemetryParameters <br> IsolatedWindowsEnvironmentTelemetryParameters.#ctor

#### <a name="isolatedwindowsenvironmenttelemetryparameters"></a>[IsolatedWindowsEnvironmentTelemetryParameters](/uwp/api/windows.security.isolation.isolatedwindowshostmessenger)

IsolatedWindowsEnvironmentTelemetryParameters.CorrelationId <br> IsolatedWindowsHostMessenger <br> IsolatedWindowsHostMessenger.GetFileId

#### <a name="isolatedwindowshostmessenger"></a>[IsolatedWindowsHostMessenger](/uwp/api/windows.security.isolation.messagereceivedcallback)

IsolatedWindowsHostMessenger.PostMessageToReceiver

#### <a name="messagereceivedcallback"></a>[MessageReceivedCallback](/uwp/api/windows.security.isolation.windows)

MessageReceivedCallback

## <a name="windowsstorage"></a>Windows.Storage

### <a name="windowsstorageprovider"></a>[Windows.Storage.Provider](/uwp/api/windows.storage.provider)

#### <a name="storageprovidersyncrootmanager"></a>[StorageProviderSyncRootManager](/uwp/api/windows.storage.provider.storageprovidersyncrootmanager)

StorageProviderSyncRootManager.IsSupported

### <a name="windowsstorage"></a>[Windows.Storage](/uwp/api/windows.storage)

#### <a name="knownfolders"></a>[KnownFolders](/uwp/api/windows.storage.knownfolders)

KnownFolders.GetFolderAsync <br> KnownFolders.RequestAccessAsync <br> KnownFolders.RequestAccessForUserAsync

#### <a name="knownfoldersaccessstatus"></a>[KnownFoldersAccessStatus](/uwp/api/windows.storage.knownfoldersaccessstatus)

KnownFoldersAccessStatus

#### <a name="storagefile"></a>[StorageFile](/uwp/api/windows.storage.storagefile)

StorageFile.GetFileFromPathForUserAsync

#### <a name="storagefolder"></a>[StorageFolder](/uwp/api/windows.storage.storagefolder)

StorageFolder.GetFolderFromPathForUserAsync

## <a name="windowssystem"></a>Windows.System

### <a name="windowssystem"></a>[Windows.System](/uwp/api/windows.system)

#### <a name="userchangedeventargs"></a>[UserChangedEventArgs](/uwp/api/windows.system.userchangedeventargs)

UserChangedEventArgs.ChangedPropertyKinds

#### <a name="userwatcherupdatekind"></a>[UserWatcherUpdateKind](/uwp/api/windows.system.userwatcherupdatekind)

UserWatcherUpdateKind

## <a name="windowsui"></a>Windows.UI

### <a name="windowsuicompositioninteractions"></a>[Windows.UI.Composition.Interactions](/uwp/api/windows.ui.composition.interactions)

#### <a name="interactiontracker"></a>[InteractionTracker](/uwp/api/windows.ui.composition.interactions.interactiontracker)

InteractionTracker.TryUpdatePosition

#### <a name="interactiontrackerpositionupdateoption"></a>[InteractionTrackerPositionUpdateOption](/uwp/api/windows.ui.composition.interactions.interactiontrackerpositionupdateoption)

InteractionTrackerPositionUpdateOption

### <a name="windowsuicorepreviewcommunications"></a>[Windows.UI.Core.Preview.Communications](/uwp/api/windows.ui.core.preview.communications)

#### <a name="previewmeetinginfodisplaykind"></a>[PreviewMeetingInfoDisplayKind](/uwp/api/windows.ui.core.preview.communications.previewmeetinginfodisplaykind)

PreviewMeetingInfoDisplayKind

#### <a name="previewsystemstate"></a>[PreviewSystemState](/uwp/api/windows.ui.core.preview.communications.previewsystemstate)

PreviewSystemState

#### <a name="previewteamcleanuprequestedeventargs"></a>[PreviewTeamCleanupRequestedEventArgs](/uwp/api/windows.ui.core.preview.communications.previewteamcleanuprequestedeventargs)

PreviewTeamCleanupRequestedEventArgs <br> PreviewTeamCleanupRequestedEventArgs.GetDeferral

#### <a name="previewteamcommandinvokedeventargs"></a>[PreviewTeamCommandInvokedEventArgs](/uwp/api/windows.ui.core.preview.communications.previewteamcommandinvokedeventargs)

PreviewTeamCommandInvokedEventArgs </br> PreviewTeamCommandInvokedEventArgs.Command

#### <a name="previewteamdevicecredentials"></a>[PreviewTeamDeviceCredentials](/uwp/api/windows.ui.core.preview.communications.previewteamdevicecredentials)

PreviewTeamDeviceCredentials <br> PreviewTeamDeviceCredentials.DomainUserName <br> PreviewTeamDeviceCredentials.Password <br> PreviewTeamDeviceCredentials.UserPrincipalName

#### <a name="previewteamendmeetingkind"></a>[PreviewTeamEndMeetingKind](/uwp/api/windows.ui.core.preview.communications.previewteamendmeetingkind)

PreviewTeamEndMeetingKind

#### <a name="previewteamendmeetingrequestedeventargs"></a>[PreviewTeamEndMeetingRequestedEventArgs](/uwp/api/windows.ui.core.preview.communications.previewteamendmeetingrequestedeventargs)

PreviewTeamEndMeetingRequestedEventArgs <br> PreviewTeamEndMeetingRequestedEventArgs.GetDeferral

#### <a name="previewteamjoinmeetingrequestedeventargs"></a>[PreviewTeamJoinMeetingRequestedEventArgs](/uwp/api/windows.ui.core.preview.communications.previewteamjoinmeetingrequestedeventargs)

PreviewTeamJoinMeetingRequestedEventArgs <br> PreviewTeamJoinMeetingRequestedEventArgs.GetDeferral <br> PreviewTeamJoinMeetingRequestedEventArgs.MeetingUri

#### <a name="previewteamview"></a>[PreviewTeamView](/uwp/api/windows.ui.core.preview.communications.previewteamview)

PreviewTeamView <br> PreviewTeamView.CleanupRequested <br> PreviewTeamView.CommandInvoked <br> PreviewTeamView.EndMeetingRequested <br> PreviewTeamView.EnterFullScreen <br> PreviewTeamView.GetForCurrentView <br> PreviewTeamView.IsFullScreen <br> PreviewTeamView.IsFullScreenChanged <br> PreviewTeamView.IsScreenSharing <br> PreviewTeamView.IsScreenSharingChanged <br> PreviewTeamView.JoinMeetingRequested <br> PreviewTeamView.JoinMeetingWithUri <br> PreviewTeamView.LeaveFullScreen <br> PreviewTeamView.MeetingInfoDisplayType <br> PreviewTeamView.MeetingUri <br> PreviewTeamView.NotifyMeetingEnded <br> PreviewTeamView.RequestForeground <br> PreviewTeamView.SetButtonLabel <br> PreviewTeamView.SetTitle <br> PreviewTeamView.SharingScreenBounds <br> PreviewTeamView.SharingScreenBoundsChanged <br> PreviewTeamView.StartSharingScreen <br> PreviewTeamView.StopSharingScreen <br> PreviewTeamView.SystemState

#### <a name="previewteamview"></a>[PreviewTeamView](/uwp/api/windows.ui.core.preview.communications.previewteamview)

PreviewTeamView.SystemStateChanged

#### <a name="previewteamviewcommand"></a>[PreviewTeamViewCommand](/uwp/api/windows.ui.core.preview.communications.previewteamviewcommand)

PreviewTeamViewCommand

### <a name="windowsuiinputinking"></a>[Windows.UI.Input.Inking](/uwp/api/windows.ui.input.inking)

#### <a name="inkmodelerattributes"></a>[InkModelerAttributes](/uwp/api/windows.ui.input.inking.inkmodelerattributes)

InkModelerAttributes.UseVelocityBasedPressure

### <a name="windowsuiinput"></a>[Windows.UI.Input](/uwp/api/windows.ui.input)

#### <a name="crossslidingeventargs"></a>[CrossSlidingEventArgs](/uwp/api/windows.ui.input.crossslidingeventargs)

CrossSlidingEventArgs.ContactCount

#### <a name="draggingeventargs"></a>[DraggingEventArgs](/uwp/api/windows.ui.input.draggingeventargs)

DraggingEventArgs.ContactCount

#### <a name="gesturerecognizer"></a>[GestureRecognizer](/uwp/api/windows.ui.input.gesturerecognizer)

GestureRecognizer.HoldMaxContactCount <br> GestureRecognizer.HoldMinContactCount <br> GestureRecognizer.HoldRadius <br> GestureRecognizer.HoldStartDelay <br> GestureRecognizer.TapMaxContactCount <br> GestureRecognizer.TapMinContactCount <br> GestureRecognizer.TranslationMaxContactCount <br> GestureRecognizer.TranslationMinContactCount

#### <a name="holdingeventargs"></a>[HoldingEventArgs](/uwp/api/windows.ui.input.holdingeventargs)

HoldingEventArgs.ContactCount <br> HoldingEventArgs.CurrentContactCount

#### <a name="manipulationcompletedeventargs"></a>[ManipulationCompletedEventArgs](/uwp/api/windows.ui.input.manipulationcompletedeventargs)

ManipulationCompletedEventArgs.ContactCount <br> ManipulationCompletedEventArgs.CurrentContactCount

#### <a name="manipulationinertiastartingeventargs"></a>[ManipulationInertiaStartingEventArgs](/uwp/api/windows.ui.input.manipulationinertiastartingeventargs)

ManipulationInertiaStartingEventArgs.ContactCount

#### <a name="manipulationstartedeventargs"></a>[ManipulationStartedEventArgs](/uwp/api/windows.ui.input.manipulationstartedeventargs)

ManipulationStartedEventArgs.ContactCount

#### <a name="manipulationupdatedeventargs"></a>[ManipulationUpdatedEventArgs](/uwp/api/windows.ui.input.manipulationupdatedeventargs)

ManipulationUpdatedEventArgs.ContactCount <br> ManipulationUpdatedEventArgs.CurrentContactCount

#### <a name="righttappedeventargs"></a>[RightTappedEventArgs](/uwp/api/windows.ui.input.righttappedeventargs)

RightTappedEventArgs.ContactCount

#### <a name="tappedeventargs"></a>[TappedEventArgs](/uwp/api/windows.ui.input.tappedeventargs)

TappedEventArgs.ContactCount 

### <a name="windowsuiviewmanagement"></a>[Windows.UI.ViewManagement](/uwp/api/windows.ui.viewmanagement)

#### <a name="uisettings"></a>[UISettings](/uwp/api/windows.ui.viewmanagement.uisettings)

UISettings.AnimationsEnabledChanged <br> UISettings.MessageDurationChanged

#### <a name="uisettingsanimationsenabledchangedeventargs"></a>[UISettingsAnimationsEnabledChangedEventArgs](/uwp/api/windows.ui.viewmanagement.uisettingsanimationsenabledchangedeventargs)

UISettingsAnimationsEnabledChangedEventArgs

#### <a name="uisettingsmessagedurationchangedeventargs"></a>[UISettingsMessageDurationChangedEventArgs](/uwp/api/windows.ui.viewmanagement.uisettingsmessagedurationchangedeventargs)

UISettingsMessageDurationChangedEventArgs