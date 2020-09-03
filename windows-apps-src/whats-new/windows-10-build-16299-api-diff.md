---
title: Windows 10 Fall Creators Update API 변경 사항
description: 개발자는 다음 목록을 사용하여 Windows 10 빌드 16299에서 새롭게 추가되었거나 변경된 네임스페이스를 식별할 수 있습니다.
keywords: Windows 10, 1709, API, 16299
ms.date: 11/02/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: f40f4014c38e235610903da287968043daf52cfe
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167077"
---
# <a name="new-apis-in-windows-10-build-16299"></a>Windows 10 빌드 16299의 새로운 API

신규 및 업데이트 API 네임스페이스가 Windows 10 빌드 16299(Fall Creators Update 또는 버전 1709라고도 함)의 개발자들에게 제공되었습니다. 아래는 이 릴리스에서 추가 또는 수정된 네임스페이스에 게시된 설명서의 전체 목록입니다.

이전의 공개 릴리스에 추가된 API에 대한 정보는 [Windows 10 크리에이터스 업데이트의 신규 API](windows-10-build-15063-api-diff.md)를 참조하세요.

## <a name="windowsapplicationmodel"></a>windows.applicationmodel

### <a name="windowsapplicationmodelactivation"></a>[windows.applicationmodel.activation](/uwp/api/windows.applicationmodel.activation)

#### <a name="commandlineactivatedeventargs"></a>[commandlineactivatedeventargs](/uwp/api/windows.applicationmodel.activation.commandlineactivatedeventargs)

commandlineactivatedeventargs <br> commandlineactivatedeventargs.kind <br> commandlineactivatedeventargs.operation <br> commandlineactivatedeventargs.previousexecutionstate <br> commandlineactivatedeventargs.splashscreen <br> commandlineactivatedeventargs.user

#### <a name="commandlineactivationoperation"></a>[commandlineactivationoperation](/uwp/api/windows.applicationmodel.activation.commandlineactivationoperation)

commandlineactivationoperation <br> commandlineactivationoperation.arguments <br> commandlineactivationoperation.currentdirectorypath <br> commandlineactivationoperation.exitcode <br> commandlineactivationoperation.getdeferral

#### <a name="icommandlineactivatedeventargs"></a>[icommandlineactivatedeventargs](/uwp/api/windows.applicationmodel.activation.icommandlineactivatedeventargs)

icommandlineactivatedeventargs <br> icommandlineactivatedeventargs.operation

#### <a name="istartuptaskactivatedeventargs"></a>[istartuptaskactivatedeventargs](/uwp/api/windows.applicationmodel.activation.istartuptaskactivatedeventargs)

istartuptaskactivatedeventargs <br> istartuptaskactivatedeventargs.taskid

#### <a name="startuptaskactivatedeventargs"></a>[startuptaskactivatedeventargs](/uwp/api/windows.applicationmodel.activation.startuptaskactivatedeventargs)

startuptaskactivatedeventargs <br> startuptaskactivatedeventargs.kind <br> startuptaskactivatedeventargs.previousexecutionstate <br> startuptaskactivatedeventargs.splashscreen <br> startuptaskactivatedeventargs.taskid <br> startuptaskactivatedeventargs.user

### <a name="windowsapplicationmodelappointments"></a>[windows.applicationmodel.appointments](/uwp/api/windows.applicationmodel.appointments)

#### <a name="appointmentstore"></a>[appointmentstore](/uwp/api/windows.applicationmodel.appointments.appointmentstore)

appointmentstore.getchangetracker

#### <a name="appointmentstorechangetracker"></a>[appointmentstorechangetracker](/uwp/api/windows.applicationmodel.appointments.appointmentstorechangetracker)

appointmentstorechangetracker.istracking

### <a name="windowsapplicationmodelappservice"></a>[windows.applicationmodel.appservice](/uwp/api/windows.applicationmodel.appservice)

#### <a name="appservicetriggerdetails"></a>[appservicetriggerdetails](/uwp/api/windows.applicationmodel.appservice.appservicetriggerdetails)

appservicetriggerdetails.checkcallerforcapabilityasync

### <a name="windowsapplicationmodelbackground"></a>[windows.applicationmodel.background](/uwp/api/windows.applicationmodel.background)

#### <a name="geovisittrigger"></a>[geovisittrigger](/uwp/api/windows.applicationmodel.background.geovisittrigger)

geovisittrigger <br> geovisittrigger.geovisittrigger <br> geovisittrigger.monitoringscope

#### <a name="paymentappcanmakepaymenttrigger"></a>[paymentappcanmakepaymenttrigger](/uwp/api/windows.applicationmodel.background.paymentappcanmakepaymenttrigger)

paymentappcanmakepaymenttrigger <br> paymentappcanmakepaymenttrigger.paymentappcanmakepaymenttrigger

### <a name="windowsapplicationmodelcalls"></a>[windows.applicationmodel.calls](/uwp/api/windows.applicationmodel.calls)

#### <a name="voipcallcoordinator"></a>[voipcallcoordinator](/uwp/api/windows.applicationmodel.calls.voipcallcoordinator)

voipcallcoordinator.setupnewacceptedcall

#### <a name="voipphonecall"></a>[voipphonecall](/uwp/api/windows.applicationmodel.calls.voipphonecall)

voipphonecall.tryshowappui

### <a name="windowsapplicationmodelcontactsdataprovider"></a>[windows.applicationmodel.contacts.dataprovider](/uwp/api/windows.applicationmodel.contacts.dataprovider)

#### <a name="contactdataproviderconnection"></a>[contactdataproviderconnection](/uwp/api/windows.applicationmodel.contacts.dataprovider.contactdataproviderconnection)

contactdataproviderconnection.createorupdatecontactrequested <br> contactdataproviderconnection.deletecontactrequested

#### <a name="contactlistcreateorupdatecontactrequest"></a>[contactlistcreateorupdatecontactrequest](/uwp/api/windows.applicationmodel.contacts.dataprovider.contactlistcreateorupdatecontactrequest)

contactlistcreateorupdatecontactrequest <br> contactlistcreateorupdatecontactrequest.contact <br> contactlistcreateorupdatecontactrequest.contactlistid <br> contactlistcreateorupdatecontactrequest.reportcompletedasync <br> contactlistcreateorupdatecontactrequest.reportfailedasync

#### <a name="contactlistcreateorupdatecontactrequesteventargs"></a>[contactlistcreateorupdatecontactrequesteventargs](/uwp/api/windows.applicationmodel.contacts.dataprovider.contactlistcreateorupdatecontactrequesteventargs)

contactlistcreateorupdatecontactrequesteventargs <br> contactlistcreateorupdatecontactrequesteventargs.getdeferral <br> contactlistcreateorupdatecontactrequesteventargs.request

#### <a name="contactlistdeletecontactrequest"></a>[contactlistdeletecontactrequest](/uwp/api/windows.applicationmodel.contacts.dataprovider.contactlistdeletecontactrequest)

contactlistdeletecontactrequest <br> contactlistdeletecontactrequest.contactid <br> contactlistdeletecontactrequest.contactlistid <br> contactlistdeletecontactrequest.reportcompletedasync <br> contactlistdeletecontactrequest.reportfailedasync

#### <a name="contactlistdeletecontactrequesteventargs"></a>[contactlistdeletecontactrequesteventargs](/uwp/api/windows.applicationmodel.contacts.dataprovider.contactlistdeletecontactrequesteventargs)

contactlistdeletecontactrequesteventargs <br> contactlistdeletecontactrequesteventargs.getdeferral <br> contactlistdeletecontactrequesteventargs.request

### <a name="windowsapplicationmodelcontacts"></a>[windows.applicationmodel.contacts](/uwp/api/windows.applicationmodel.contacts)

#### <a name="contactchangetracker"></a>[contactchangetracker](/uwp/api/windows.applicationmodel.contacts.contactchangetracker)

contactchangetracker.istracking

#### <a name="contactlist"></a>[contactlist](/uwp/api/windows.applicationmodel.contacts.contactlist)

contactlist.getchangetracker <br> contactlist.limitedwriteoperations

#### <a name="contactlistlimitedwriteoperations"></a>[contactlistlimitedwriteoperations](/uwp/api/windows.applicationmodel.contacts.contactlistlimitedwriteoperations)

contactlistlimitedwriteoperations <br> contactlistlimitedwriteoperations.trycreateorupdatecontactasync <br> contactlistlimitedwriteoperations.trydeletecontactasync

#### <a name="contactstore"></a>[contactstore](/uwp/api/windows.applicationmodel.contacts.contactstore)

contactstore.getchangetracker

### <a name="windowsapplicationmodelcore"></a>[windows.applicationmodel.core](/uwp/api/windows.applicationmodel.core)

#### <a name="applistentry"></a>[applistentry](/uwp/api/windows.applicationmodel.core.applistentry)

applistentry.appusermodelid

#### <a name="apprestartfailurereason"></a>[apprestartfailurereason](/uwp/api/windows.applicationmodel.core.apprestartfailurereason)

apprestartfailurereason

#### <a name="coreapplication"></a>[coreapplication](/uwp/api/windows.applicationmodel.core.coreapplication)

coreapplication.requestrestartasync <br> coreapplication.requestrestartforuserasync

#### <a name="coreapplicationview"></a>[coreapplicationview](/uwp/api/windows.applicationmodel.core.coreapplicationview)

coreapplicationview.dispatcherqueue

### <a name="windowsapplicationmodeldatatransfersharetarget"></a>[windows.applicationmodel.datatransfer.sharetarget](/uwp/api/windows.applicationmodel.datatransfer.sharetarget)

#### <a name="shareoperation"></a>[shareoperation](/uwp/api/windows.applicationmodel.datatransfer.sharetarget.shareoperation)

shareoperation.contacts

### <a name="windowsapplicationmodeldatatransfer"></a>[windows.applicationmodel.datatransfer](/uwp/api/windows.applicationmodel.datatransfer)

#### <a name="datatransfermanager"></a>[datatransfermanager](/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager)

datatransfermanager.showshareui

#### <a name="shareuioptions"></a>[shareuioptions](/uwp/api/windows.applicationmodel.datatransfer.shareuioptions)

shareuioptions <br> shareuioptions.selectionrect <br> shareuioptions.shareuioptions <br> shareuioptions.theme

#### <a name="shareuitheme"></a>[shareuitheme](/uwp/api/windows.applicationmodel.datatransfer.shareuitheme)

shareuitheme

### <a name="windowsapplicationmodelemail"></a>[windows.applicationmodel.email](/uwp/api/windows.applicationmodel.email)

#### <a name="emailmailbox"></a>[emailmailbox](/uwp/api/windows.applicationmodel.email.emailmailbox)

emailmailbox.getchangetracker

### <a name="windowsapplicationmodelpaymentsprovider"></a>[windows.applicationmodel.payments.provider](/uwp/api/windows.applicationmodel.payments.provider)

#### <a name="paymentappcanmakepaymenttriggerdetails"></a>[paymentappcanmakepaymenttriggerdetails](/uwp/api/windows.applicationmodel.payments.provider.paymentappcanmakepaymenttriggerdetails)

paymentappcanmakepaymenttriggerdetails <br> paymentappcanmakepaymenttriggerdetails.reportcanmakepaymentresult <br> paymentappcanmakepaymenttriggerdetails.request

### <a name="windowsapplicationmodelpayments"></a>[windows.applicationmodel.payments](/uwp/api/windows.applicationmodel.payments)

#### <a name="paymentcanmakepaymentresult"></a>[paymentcanmakepaymentresult](/uwp/api/windows.applicationmodel.payments.paymentcanmakepaymentresult)

paymentcanmakepaymentresult <br> paymentcanmakepaymentresult.paymentcanmakepaymentresult <br> paymentcanmakepaymentresult.status

#### <a name="paymentcanmakepaymentresultstatus"></a>[paymentcanmakepaymentresultstatus](/uwp/api/windows.applicationmodel.payments.paymentcanmakepaymentresultstatus)

paymentcanmakepaymentresultstatus

#### <a name="paymentmediator"></a>[paymentmediator](/uwp/api/windows.applicationmodel.payments.paymentmediator)

paymentmediator.canmakepaymentasync

#### <a name="paymentrequest"></a>[paymentrequest](/uwp/api/windows.applicationmodel.payments.paymentrequest)

paymentrequest.id <br> paymentrequest.paymentrequest

### <a name="windowsapplicationmodeluseractivitiescore"></a>[windows.applicationmodel.useractivities.core](/uwp/api/windows.applicationmodel.useractivities.core)

#### <a name="coreuseractivitymanager"></a>[coreuseractivitymanager](/uwp/api/windows.applicationmodel.useractivities.core.coreuseractivitymanager)

coreuseractivitymanager <br> coreuseractivitymanager.createuseractivitysessioninbackground <br> coreuseractivitymanager.deleteuseractivitysessionsintimerangeasync

### <a name="windowsapplicationmodeluseractivities"></a>[windows.applicationmodel.useractivities](/uwp/api/windows.applicationmodel.useractivities)

#### <a name="iuseractivitycontentinfo"></a>[iuseractivitycontentinfo](/uwp/api/windows.applicationmodel.useractivities.iuseractivitycontentinfo)

iuseractivitycontentinfo <br> iuseractivitycontentinfo.tojson

#### <a name="useractivity"></a>[useractivity](/uwp/api/windows.applicationmodel.useractivities.useractivity)

useractivity <br> useractivity.activationuri <br> useractivity.activityid <br> useractivity.contentinfo <br> useractivity.contenttype <br> useractivity.contenturi <br> useractivity.createsession <br> useractivity.fallbackuri <br> useractivity.saveasync <br> useractivity.state <br> useractivity.visualelements

#### <a name="useractivityattribution"></a>[useractivityattribution](/uwp/api/windows.applicationmodel.useractivities.useractivityattribution)

useractivityattribution <br> useractivityattribution.addimagequery <br> useractivityattribution.alternatetext <br> useractivityattribution.iconuri <br> useractivityattribution.useractivityattribution <br> useractivityattribution.useractivityattribution

#### <a name="useractivitychannel"></a>[useractivitychannel](/uwp/api/windows.applicationmodel.useractivities.useractivitychannel)

useractivitychannel <br> useractivitychannel.deleteactivityasync <br> useractivitychannel.deleteallactivitiesasync <br> useractivitychannel.getdefault <br> useractivitychannel.getorcreateuseractivityasync

#### <a name="useractivitycontentinfo"></a>[useractivitycontentinfo](/uwp/api/windows.applicationmodel.useractivities.useractivitycontentinfo)

useractivitycontentinfo <br> useractivitycontentinfo.fromjson <br> useractivitycontentinfo.tojson

#### <a name="useractivitysession"></a>[useractivitysession](/uwp/api/windows.applicationmodel.useractivities.useractivitysession)

useractivitysession <br> useractivitysession.activityid <br> useractivitysession.close

#### <a name="useractivitystate"></a>[useractivitystate](/uwp/api/windows.applicationmodel.useractivities.useractivitystate)

useractivitystate

#### <a name="useractivityvisualelements"></a>[useractivityvisualelements](/uwp/api/windows.applicationmodel.useractivities.useractivityvisualelements)

useractivityvisualelements <br> useractivityvisualelements.attribution <br> useractivityvisualelements.backgroundcolor <br> useractivityvisualelements.content <br> useractivityvisualelements.description <br> useractivityvisualelements.displaytext

### <a name="windowsapplicationmodel"></a>[windows.applicationmodel](/uwp/api/windows.applicationmodel)

#### <a name="designmode"></a>[designmode](/uwp/api/windows.applicationmodel.designmode)

designmode.designmode2enabled

#### <a name="packagecatalog"></a>[packagecatalog](/uwp/api/windows.applicationmodel.packagecatalog)

packagecatalog.removeoptionalpackagesasync

#### <a name="packagecatalogremoveoptionalpackagesresult"></a>[packagecatalogremoveoptionalpackagesresult](/uwp/api/windows.applicationmodel.packagecatalogremoveoptionalpackagesresult)

packagecatalogremoveoptionalpackagesresult <br> packagecatalogremoveoptionalpackagesresult.extendederror <br> packagecatalogremoveoptionalpackagesresult.packagesremoved

## <a name="windowsdevices"></a>windows.devices

### <a name="windowsdevicesbluetoothgenericattributeprofile"></a>[windows.devices.bluetooth.genericattributeprofile](/uwp/api/windows.devices.bluetooth.genericattributeprofile)

#### <a name="gattclientnotificationresult"></a>[gattclientnotificationresult](/uwp/api/windows.devices.bluetooth.genericattributeprofile.gattclientnotificationresult)

gattclientnotificationresult.bytessent

### <a name="windowsdevicesbluetooth"></a>[windows.devices.bluetooth](/uwp/api/windows.devices.bluetooth)

#### <a name="bluetoothdevice"></a>[bluetoothdevice](/uwp/api/windows.devices.bluetooth.bluetoothdevice)

bluetoothdevice.bluetoothdeviceid

#### <a name="bluetoothdeviceid"></a>[bluetoothdeviceid](/uwp/api/windows.devices.bluetooth.bluetoothdeviceid)

bluetoothdeviceid.fromid

#### <a name="bluetoothledevice"></a>[bluetoothledevice](/uwp/api/windows.devices.bluetooth.bluetoothledevice)

bluetoothledevice.bluetoothdeviceid

### <a name="windowsdevicesgeolocation"></a>[windows.devices.geolocation](/uwp/api/windows.devices.geolocation)

#### <a name="geovisit"></a>[geovisit](/uwp/api/windows.devices.geolocation.geovisit)

geovisit <br> geovisit.position <br> geovisit.statechange <br> geovisit.timestamp

#### <a name="geovisitmonitor"></a>[geovisitmonitor](/uwp/api/windows.devices.geolocation.geovisitmonitor)

geovisitmonitor <br> geovisitmonitor.geovisitmonitor <br> geovisitmonitor.getlastreportasync <br> geovisitmonitor.monitoringscope <br> geovisitmonitor.start <br> geovisitmonitor.stop <br> geovisitmonitor.visitstatechanged

#### <a name="geovisitstatechangedeventargs"></a>[geovisitstatechangedeventargs](/uwp/api/windows.devices.geolocation.geovisitstatechangedeventargs)

geovisitstatechangedeventargs <br> geovisitstatechangedeventargs.visit

#### <a name="geovisittriggerdetails"></a>[geovisittriggerdetails](/uwp/api/windows.devices.geolocation.geovisittriggerdetails)

geovisittriggerdetails <br> geovisittriggerdetails.readreports

#### <a name="visitmonitoringscope"></a>[visitmonitoringscope](/uwp/api/windows.devices.geolocation.visitmonitoringscope)

visitmonitoringscope

#### <a name="visitstatechange"></a>[visitstatechange](/uwp/api/windows.devices.geolocation.visitstatechange)

visitstatechange

### <a name="windowsdevicespointofservice"></a>[windows.devices.pointofservice](/uwp/api/windows.devices.pointofservice)

#### <a name="claimedlinedisplay"></a>[claimedlinedisplay](/uwp/api/windows.devices.pointofservice.claimedlinedisplay)

claimedlinedisplay.checkhealthasync <br> claimedlinedisplay.checkpowerstatusasync <br> claimedlinedisplay.customglyphs <br> claimedlinedisplay.getattributes <br> claimedlinedisplay.getstatisticsasync <br> claimedlinedisplay.maxbitmapsizeinpixels <br> claimedlinedisplay.statusupdated <br> claimedlinedisplay.supportedcharactersets <br> claimedlinedisplay.supportedscreensizesincharacters <br> claimedlinedisplay.trycleardescriptorsasync <br> claimedlinedisplay.trycreatewindowasync <br> claimedlinedisplay.trysetdescriptorasync <br> claimedlinedisplay.trystorestoragefilebitmapasync <br> claimedlinedisplay.trystorestoragefilebitmapasync <br> claimedlinedisplay.trystorestoragefilebitmapasync <br> claimedlinedisplay.tryupdateattributesasync

#### <a name="linedisplay"></a>[linedisplay](/uwp/api/windows.devices.pointofservice.linedisplay)

linedisplay.checkpowerstatusasync <br> linedisplay.statisticscategoryselector

#### <a name="linedisplayattributes"></a>[linedisplayattributes](/uwp/api/windows.devices.pointofservice.linedisplayattributes)

linedisplayattributes <br> linedisplayattributes.blinkrate <br> linedisplayattributes.brightness <br> linedisplayattributes.characterset <br> linedisplayattributes.currentwindow <br> linedisplayattributes.ischaractersetmappingenabled <br> linedisplayattributes.ispowernotifyenabled <br> linedisplayattributes.screensizeincharacters

#### <a name="linedisplaycursor"></a>[linedisplaycursor](/uwp/api/windows.devices.pointofservice.linedisplaycursor)

linedisplaycursor <br> linedisplaycursor.cancustomize <br> linedisplaycursor.getattributes <br> linedisplaycursor.isblinksupported <br> linedisplaycursor.isblocksupported <br> linedisplaycursor.ishalfblocksupported <br> linedisplaycursor.isothersupported <br> linedisplaycursor.isreversesupported <br> linedisplaycursor.isunderlinesupported <br> linedisplaycursor.tryupdateattributesasync

#### <a name="linedisplaycursorattributes"></a>[linedisplaycursorattributes](/uwp/api/windows.devices.pointofservice.linedisplaycursorattributes)

linedisplaycursorattributes <br> linedisplaycursorattributes.cursortype <br> linedisplaycursorattributes.isautoadvanceenabled <br> linedisplaycursorattributes.isblinkenabled <br> linedisplaycursorattributes.position

#### <a name="linedisplaycursortype"></a>[linedisplaycursortype](/uwp/api/windows.devices.pointofservice.linedisplaycursortype)

linedisplaycursortype

#### <a name="linedisplaycustomglyphs"></a>[linedisplaycustomglyphs](/uwp/api/windows.devices.pointofservice.linedisplaycustomglyphs)

linedisplaycustomglyphs <br> linedisplaycustomglyphs.sizeinpixels <br> linedisplaycustomglyphs.supportedglyphcodes <br> linedisplaycustomglyphs.tryredefineasync

#### <a name="linedisplaydescriptorstate"></a>[linedisplaydescriptorstate](/uwp/api/windows.devices.pointofservice.linedisplaydescriptorstate)

linedisplaydescriptorstate

#### <a name="linedisplayhorizontalalignment"></a>[linedisplayhorizontalalignment](/uwp/api/windows.devices.pointofservice.linedisplayhorizontalalignment)

linedisplayhorizontalalignment

#### <a name="linedisplaymarquee"></a>[linedisplaymarquee](/uwp/api/windows.devices.pointofservice.linedisplaymarquee)

linedisplaymarquee <br> linedisplaymarquee.format <br> linedisplaymarquee.repeatwaitinterval <br> linedisplaymarquee.scrollwaitinterval <br> linedisplaymarquee.trystartscrollingasync <br> linedisplaymarquee.trystopscrollingasync

#### <a name="linedisplaymarqueeformat"></a>[linedisplaymarqueeformat](/uwp/api/windows.devices.pointofservice.linedisplaymarqueeformat)

linedisplaymarqueeformat

#### <a name="linedisplaypowerstatus"></a>[linedisplaypowerstatus](/uwp/api/windows.devices.pointofservice.linedisplaypowerstatus)

linedisplaypowerstatus

#### <a name="linedisplaystatisticscategoryselector"></a>[linedisplaystatisticscategoryselector](/uwp/api/windows.devices.pointofservice.linedisplaystatisticscategoryselector)

linedisplaystatisticscategoryselector <br> linedisplaystatisticscategoryselector.allstatistics <br> linedisplaystatisticscategoryselector.manufacturerstatistics <br> linedisplaystatisticscategoryselector.unifiedposstatistics

#### <a name="linedisplaystatusupdatedeventargs"></a>[linedisplaystatusupdatedeventargs](/uwp/api/windows.devices.pointofservice.linedisplaystatusupdatedeventargs)

linedisplaystatusupdatedeventargs <br> linedisplaystatusupdatedeventargs.status

#### <a name="linedisplaystoredbitmap"></a>[linedisplaystoredbitmap](/uwp/api/windows.devices.pointofservice.linedisplaystoredbitmap)

linedisplaystoredbitmap <br> linedisplaystoredbitmap.escapesequence <br> linedisplaystoredbitmap.trydeleteasync

#### <a name="linedisplayverticalalignment"></a>[linedisplayverticalalignment](/uwp/api/windows.devices.pointofservice.linedisplayverticalalignment)

linedisplayverticalalignment

#### <a name="linedisplaywindow"></a>[linedisplaywindow](/uwp/api/windows.devices.pointofservice.linedisplaywindow)

linedisplaywindow.cursor <br> linedisplaywindow.marquee <br> linedisplaywindow.readcharacteratcursorasync <br> linedisplaywindow.trydisplaystoragefilebitmapatcursorasync <br> linedisplaywindow.trydisplaystoragefilebitmapatcursorasync <br> linedisplaywindow.trydisplaystoragefilebitmapatcursorasync <br> linedisplaywindow.trydisplaystoragefilebitmapatpointasync <br> linedisplaywindow.trydisplaystoragefilebitmapatpointasync <br> linedisplaywindow.trydisplaystoredbitmapatcursorasync

### <a name="windowsdevicessensorscustom"></a>[windows.devices.sensors.custom](/uwp/api/windows.devices.sensors.custom)

#### <a name="customsensor"></a>[customsensor](/uwp/api/windows.devices.sensors.custom.customsensor)

customsensor.maxbatchsize <br> customsensor.reportlatency

#### <a name="customsensorreading"></a>[customsensorreading](/uwp/api/windows.devices.sensors.custom.customsensorreading)

customsensorreading.performancecount

### <a name="windowsdevicessensors"></a>[windows.devices.sensors](/uwp/api/windows.devices.sensors)

#### <a name="accelerometer"></a>[accelerometer](/uwp/api/windows.devices.sensors.accelerometer)

accelerometer.fromidasync <br> accelerometer.getdeviceselector

#### <a name="accelerometerreading"></a>[accelerometerreading](/uwp/api/windows.devices.sensors.accelerometerreading)

accelerometerreading.performancecount <br> accelerometerreading.properties

#### <a name="altimeter"></a>[altimeter](/uwp/api/windows.devices.sensors.altimeter)

altimeter.maxbatchsize <br> altimeter.reportlatency

#### <a name="altimeterreading"></a>[altimeterreading](/uwp/api/windows.devices.sensors.altimeterreading)

altimeterreading.performancecount <br> altimeterreading.properties

#### <a name="barometer"></a>[barometer](/uwp/api/windows.devices.sensors.barometer)

barometer.fromidasync <br> barometer.getdeviceselector <br> barometer.maxbatchsize <br> barometer.reportlatency

#### <a name="barometerreading"></a>[barometerreading](/uwp/api/windows.devices.sensors.barometerreading)

barometerreading.performancecount <br> barometerreading.properties

#### <a name="compass"></a>[compass](/uwp/api/windows.devices.sensors.compass)

compass.fromidasync <br> compass.getdeviceselector <br> compass.maxbatchsize <br> compass.reportlatency

#### <a name="compassreading"></a>[compassreading](/uwp/api/windows.devices.sensors.compassreading)

compassreading.performancecount <br> compassreading.properties

#### <a name="gyrometer"></a>[gyrometer](/uwp/api/windows.devices.sensors.gyrometer)

gyrometer.fromidasync <br> gyrometer.getdeviceselector <br> gyrometer.maxbatchsize <br> gyrometer.reportlatency

#### <a name="gyrometerreading"></a>[gyrometerreading](/uwp/api/windows.devices.sensors.gyrometerreading)

gyrometerreading.performancecount <br> gyrometerreading.properties

#### <a name="inclinometer"></a>[inclinometer](/uwp/api/windows.devices.sensors.inclinometer)

inclinometer.fromidasync <br> inclinometer.getdeviceselector <br> inclinometer.maxbatchsize <br> inclinometer.reportlatency

#### <a name="inclinometerreading"></a>[inclinometerreading](/uwp/api/windows.devices.sensors.inclinometerreading)

inclinometerreading.performancecount <br> inclinometerreading.properties

#### <a name="lightsensor"></a>[lightsensor](/uwp/api/windows.devices.sensors.lightsensor)

lightsensor.fromidasync <br> lightsensor.getdeviceselector <br> lightsensor.maxbatchsize <br> lightsensor.reportlatency

#### <a name="lightsensorreading"></a>[lightsensorreading](/uwp/api/windows.devices.sensors.lightsensorreading)

lightsensorreading.performancecount <br> lightsensorreading.properties

#### <a name="magnetometer"></a>[magnetometer](/uwp/api/windows.devices.sensors.magnetometer)

magnetometer.fromidasync <br> magnetometer.getdeviceselector <br> magnetometer.maxbatchsize <br> magnetometer.reportlatency

#### <a name="magnetometerreading"></a>[magnetometerreading](/uwp/api/windows.devices.sensors.magnetometerreading)

magnetometerreading.performancecount <br> magnetometerreading.properties

#### <a name="orientationsensor"></a>[orientationsensor](/uwp/api/windows.devices.sensors.orientationsensor)

orientationsensor.fromidasync <br> orientationsensor.getdeviceselector <br> orientationsensor.getdeviceselector <br> orientationsensor.maxbatchsize <br> orientationsensor.reportlatency

#### <a name="orientationsensorreading"></a>[orientationsensorreading](/uwp/api/windows.devices.sensors.orientationsensorreading)

orientationsensorreading.performancecount <br> orientationsensorreading.properties

### <a name="windowsdevicessmartcards"></a>[windows.devices.smartcards](/uwp/api/windows.devices.smartcards)

#### <a name="smartcardcryptogramgenerator"></a>[smartcardcryptogramgenerator](/uwp/api/windows.devices.smartcards.smartcardcryptogramgenerator)

smartcardcryptogramgenerator.issupported

#### <a name="smartcardemulator"></a>[smartcardemulator](/uwp/api/windows.devices.smartcards.smartcardemulator)

smartcardemulator.issupported

### <a name="windowsdeviceswifi"></a>[windows.devices.wifi](/uwp/api/windows.devices.wifi)

#### <a name="wifiadapter"></a>[wifiadapter](/uwp/api/windows.devices.wifi.wifiadapter)

wifiadapter.connectasync <br> wifiadapter.getwpsconfigurationasync

#### <a name="wificonnectionmethod"></a>[wificonnectionmethod](/uwp/api/windows.devices.wifi.wificonnectionmethod)

wificonnectionmethod

#### <a name="wifiwpsconfigurationresult"></a>[wifiwpsconfigurationresult](/uwp/api/windows.devices.wifi.wifiwpsconfigurationresult)

wifiwpsconfigurationresult <br> wifiwpsconfigurationresult.status <br> wifiwpsconfigurationresult.supportedwpskinds

#### <a name="wifiwpsconfigurationstatus"></a>[wifiwpsconfigurationstatus](/uwp/api/windows.devices.wifi.wifiwpsconfigurationstatus)

wifiwpsconfigurationstatus

#### <a name="wifiwpskind"></a>[wifiwpskind](/uwp/api/windows.devices.wifi.wifiwpskind)

wifiwpskind

## <a name="windowsgaming"></a>windows.gaming

### <a name="windowsgaminginput"></a>[windows.gaming.input](/uwp/api/windows.gaming.input)

#### <a name="rawgamecontroller"></a>[rawgamecontroller](/uwp/api/windows.gaming.input.rawgamecontroller)

rawgamecontroller.displayname <br> rawgamecontroller.nonroamableid <br> rawgamecontroller.simplehapticscontrollers

### <a name="windowsgamingpreviewgamesenumeration"></a>[windows.gaming.preview.gamesenumeration](/uwp/api/windows.gaming.preview.gamesenumeration)

#### <a name="gamelist"></a>[gamelist](/uwp/api/windows.gaming.preview.gamesenumeration.gamelist)

gamelist.mergeentriesasync <br> gamelist.unmergeentryasync

#### <a name="gamelistentry"></a>[gamelistentry](/uwp/api/windows.gaming.preview.gamesenumeration.gamelistentry)

gamelistentry.gamemodeconfiguration <br> gamelistentry.launchablestate <br> gamelistentry.launcherexecutable <br> gamelistentry.launchparameters <br> gamelistentry.setlauncherexecutablefileasync <br> gamelistentry.setlauncherexecutablefileasync <br> gamelistentry.settitleidasync <br> gamelistentry.titleid

#### <a name="gamelistentrylaunchablestate"></a>[gamelistentrylaunchablestate](/uwp/api/windows.gaming.preview.gamesenumeration.gamelistentrylaunchablestate)

gamelistentrylaunchablestate

#### <a name="gamemodeconfiguration"></a>[gamemodeconfiguration](/uwp/api/windows.gaming.preview.gamesenumeration.gamemodeconfiguration)

gamemodeconfiguration <br> gamemodeconfiguration.affinitizetoexclusivecpus <br> gamemodeconfiguration.cpuexclusivitymaskhigh <br> gamemodeconfiguration.cpuexclusivitymasklow <br> gamemodeconfiguration.isenabled <br> gamemodeconfiguration.maxcpucount <br> gamemodeconfiguration.percentgpumemoryallocatedtogame <br> gamemodeconfiguration.percentgpumemoryallocatedtosystemcompositor <br> gamemodeconfiguration.percentgputimeallocatedtogame <br> gamemodeconfiguration.relatedprocessnames <br> gamemodeconfiguration.saveasync

#### <a name="gamemodeuserconfiguration"></a>[gamemodeuserconfiguration](/uwp/api/windows.gaming.preview.gamesenumeration.gamemodeuserconfiguration)

gamemodeuserconfiguration <br> gamemodeuserconfiguration.gamingrelatedprocessnames <br> gamemodeuserconfiguration.getdefault <br> gamemodeuserconfiguration.saveasync

### <a name="windowsgamingui"></a>[windows.gaming.ui](/uwp/api/windows.gaming.ui)

#### <a name="gamechatmessageorigin"></a>[gamechatmessageorigin](/uwp/api/windows.gaming.ui.gamechatmessageorigin)

gamechatmessageorigin

#### <a name="gamechatmessagereceivedeventargs"></a>[gamechatmessagereceivedeventargs](/uwp/api/windows.gaming.ui.gamechatmessagereceivedeventargs)

gamechatmessagereceivedeventargs <br> gamechatmessagereceivedeventargs.appdisplayname <br> gamechatmessagereceivedeventargs.appid <br> gamechatmessagereceivedeventargs.message <br> gamechatmessagereceivedeventargs.origin <br> gamechatmessagereceivedeventargs.sendername

#### <a name="gamechatoverlay"></a>[gamechatoverlay](/uwp/api/windows.gaming.ui.gamechatoverlay)

gamechatoverlay <br> gamechatoverlay.addmessage <br> gamechatoverlay.desiredposition <br> gamechatoverlay.getdefault

#### <a name="gamechatoverlaymessagesource"></a>[gamechatoverlaymessagesource](/uwp/api/windows.gaming.ui.gamechatoverlaymessagesource)

gamechatoverlaymessagesource <br> gamechatoverlaymessagesource.gamechatoverlaymessagesource <br> gamechatoverlaymessagesource.messagereceived <br> gamechatoverlaymessagesource.setdelaybeforeclosingaftermessagereceived

#### <a name="gamechatoverlayposition"></a>[gamechatoverlayposition](/uwp/api/windows.gaming.ui.gamechatoverlayposition)

gamechatoverlayposition

#### <a name="gamemonitor"></a>[gamemonitor](/uwp/api/windows.gaming.ui.gamemonitor)

gamemonitor <br> gamemonitor.getdefault <br> gamemonitor.requestpermissionasync

#### <a name="gamemonitoringpermission"></a>[gamemonitoringpermission](/uwp/api/windows.gaming.ui.gamemonitoringpermission)

gamemonitoringpermission

#### <a name="gameuiprovideractivatedeventargs"></a>[gameuiprovideractivatedeventargs](/uwp/api/windows.gaming.ui.gameuiprovideractivatedeventargs)

gameuiprovideractivatedeventargs <br> gameuiprovideractivatedeventargs.gameuiargs <br> gameuiprovideractivatedeventargs.kind <br> gameuiprovideractivatedeventargs.previousexecutionstate <br> gameuiprovideractivatedeventargs.reportcompleted <br> gameuiprovideractivatedeventargs.splashscreen

## <a name="windowsgraphics"></a>windows.graphics

### <a name="windowsgraphicsholographic"></a>[windows.graphics.holographic](/uwp/api/windows.graphics.holographic)

#### <a name="holographiccamera"></a>[holographiccamera](/uwp/api/windows.graphics.holographic.holographiccamera)

holographiccamera.isprimarylayerenabled <br> holographiccamera.maxquadlayercount <br> holographiccamera.quadlayers

#### <a name="holographiccamerarenderingparameters"></a>[holographiccamerarenderingparameters](/uwp/api/windows.graphics.holographic.holographiccamerarenderingparameters)

holographiccamerarenderingparameters.iscontentprotectionenabled

#### <a name="holographicdisplay"></a>[holographicdisplay](/uwp/api/windows.graphics.holographic.holographicdisplay)

holographicdisplay.refreshrate

#### <a name="holographicframe"></a>[holographicframe](/uwp/api/windows.graphics.holographic.holographicframe)

holographicframe.getquadlayerupdateparameters

#### <a name="holographicquadlayer"></a>[holographicquadlayer](/uwp/api/windows.graphics.holographic.holographicquadlayer)

holographicquadlayer <br> holographicquadlayer.close <br> holographicquadlayer.holographicquadlayer <br> holographicquadlayer.holographicquadlayer <br> holographicquadlayer.pixelformat <br> holographicquadlayer.size

#### <a name="holographicquadlayerupdateparameters"></a>[holographicquadlayerupdateparameters](/uwp/api/windows.graphics.holographic.holographicquadlayerupdateparameters)

holographicquadlayerupdateparameters <br> holographicquadlayerupdateparameters.acquirebuffertoupdatecontent <br> holographicquadlayerupdateparameters.updatecontentprotectionenabled <br> holographicquadlayerupdateparameters.updateextents <br> holographicquadlayerupdateparameters.updatelocationwithdisplayrelativemode <br> holographicquadlayerupdateparameters.updatelocationwithstationarymode <br> holographicquadlayerupdateparameters.updateviewport

#### <a name="holographicspace"></a>[holographicspace](/uwp/api/windows.graphics.holographic.holographicspace)

holographicspace.isconfigured

### <a name="windowsgraphicsprintingprintticket"></a>[windows.graphics.printing.printticket](/uwp/api/windows.graphics.printing.printticket)

#### <a name="printticketcapabilities"></a>[printticketcapabilities](/uwp/api/windows.graphics.printing.printticket.printticketcapabilities)

printticketcapabilities <br> printticketcapabilities.documentbindingfeature <br> printticketcapabilities.documentcollatefeature <br> printticketcapabilities.documentduplexfeature <br> printticketcapabilities.documentholepunchfeature <br> printticketcapabilities.documentinputbinfeature <br> printticketcapabilities.documentnupfeature <br> printticketcapabilities.documentstaplefeature <br> printticketcapabilities.getfeature <br> printticketcapabilities.getparameterdefinition <br> printticketcapabilities.jobpasscodefeature <br> printticketcapabilities.name <br> printticketcapabilities.pageborderlessfeature <br> printticketcapabilities.pagemediasizefeature <br> printticketcapabilities.pagemediatypefeature <br> printticketcapabilities.pageorientationfeature <br> printticketcapabilities.pageoutputcolorfeature <br> printticketcapabilities.pageoutputqualityfeature <br> printticketcapabilities.pageresolutionfeature <br> printticketcapabilities.xmlnamespace <br> printticketcapabilities.xmlnode

#### <a name="printticketfeature"></a>[printticketfeature](/uwp/api/windows.graphics.printing.printticket.printticketfeature)

printticketfeature <br> printticketfeature.displayname <br> printticketfeature.getoption <br> printticketfeature.getselectedoption <br> printticketfeature.name <br> printticketfeature.options <br> printticketfeature.selectiontype <br> printticketfeature.setselectedoption <br> printticketfeature.xmlnamespace <br> printticketfeature.xmlnode

#### <a name="printticketfeatureselectiontype"></a>[printticketfeatureselectiontype](/uwp/api/windows.graphics.printing.printticket.printticketfeatureselectiontype)

printticketfeatureselectiontype

#### <a name="printticketoption"></a>[printticketoption](/uwp/api/windows.graphics.printing.printticket.printticketoption)

printticketoption <br> printticketoption.displayname <br> printticketoption.getpropertynode <br> printticketoption.getpropertyvalue <br> printticketoption.getscoredpropertynode <br> printticketoption.getscoredpropertyvalue <br> printticketoption.name <br> printticketoption.xmlnamespace <br> printticketoption.xmlnode

#### <a name="printticketparameterdatatype"></a>[printticketparameterdatatype](/uwp/api/windows.graphics.printing.printticket.printticketparameterdatatype)

printticketparameterdatatype

#### <a name="printticketparameterdefinition"></a>[printticketparameterdefinition](/uwp/api/windows.graphics.printing.printticket.printticketparameterdefinition)

printticketparameterdefinition <br> printticketparameterdefinition.datatype <br> printticketparameterdefinition.name <br> printticketparameterdefinition.rangemax <br> printticketparameterdefinition.rangemin <br> printticketparameterdefinition.unittype <br> printticketparameterdefinition.xmlnamespace <br> printticketparameterdefinition.xmlnode

#### <a name="printticketparameterinitializer"></a>[printticketparameterinitializer](/uwp/api/windows.graphics.printing.printticket.printticketparameterinitializer)

printticketparameterinitializer <br> printticketparameterinitializer.name <br> printticketparameterinitializer.value <br> printticketparameterinitializer.xmlnamespace <br> printticketparameterinitializer.xmlnode

#### <a name="printticketvalue"></a>[printticketvalue](/uwp/api/windows.graphics.printing.printticket.printticketvalue)

printticketvalue <br> printticketvalue.getvalueasinteger <br> printticketvalue.getvalueasstring <br> printticketvalue.type

#### <a name="printticketvaluetype"></a>[printticketvaluetype](/uwp/api/windows.graphics.printing.printticket.printticketvaluetype)

printticketvaluetype

#### <a name="workflowprintticket"></a>[workflowprintticket](/uwp/api/windows.graphics.printing.printticket.workflowprintticket)

workflowprintticket <br> workflowprintticket.documentbindingfeature <br> workflowprintticket.documentcollatefeature <br> workflowprintticket.documentduplexfeature <br> workflowprintticket.documentholepunchfeature <br> workflowprintticket.documentinputbinfeature <br> workflowprintticket.documentnupfeature <br> workflowprintticket.documentstaplefeature <br> workflowprintticket.getcapabilities <br> workflowprintticket.getfeature <br> workflowprintticket.getparameterinitializer <br> workflowprintticket.jobpasscodefeature <br> workflowprintticket.mergeandvalidateticket <br> workflowprintticket.name <br> workflowprintticket.notifyxmlchangedasync <br> workflowprintticket.pageborderlessfeature <br> workflowprintticket.pagemediasizefeature <br> workflowprintticket.pagemediatypefeature <br> workflowprintticket.pageorientationfeature <br> workflowprintticket.pageoutputcolorfeature <br> workflowprintticket.pageoutputqualityfeature <br> workflowprintticket.pageresolutionfeature <br> workflowprintticket.setparameterinitializerasinteger <br> workflowprintticket.setparameterinitializerasstring <br> workflowprintticket.validateasync <br> workflowprintticket.xmlnamespace <br> workflowprintticket.xmlnode

#### <a name="workflowprintticketvalidationresult"></a>[workflowprintticketvalidationresult](/uwp/api/windows.graphics.printing.printticket.workflowprintticketvalidationresult)

workflowprintticketvalidationresult <br> workflowprintticketvalidationresult.extendederror <br> workflowprintticketvalidationresult.validated

### <a name="windowsgraphicsprintingworkflow"></a>[windows.graphics.printing.workflow](/uwp/api/windows.graphics.printing.workflow)

#### <a name="printworkflowbackgroundsession"></a>[printworkflowbackgroundsession](/uwp/api/windows.graphics.printing.workflow.printworkflowbackgroundsession)

printworkflowbackgroundsession <br> printworkflowbackgroundsession.setuprequested <br> printworkflowbackgroundsession.start <br> printworkflowbackgroundsession.status <br> printworkflowbackgroundsession.submitted

#### <a name="printworkflowbackgroundsetuprequestedeventargs"></a>[printworkflowbackgroundsetuprequestedeventargs](/uwp/api/windows.graphics.printing.workflow.printworkflowbackgroundsetuprequestedeventargs)

printworkflowbackgroundsetuprequestedeventargs <br> printworkflowbackgroundsetuprequestedeventargs.configuration <br> printworkflowbackgroundsetuprequestedeventargs.getdeferral <br> printworkflowbackgroundsetuprequestedeventargs.getuserprintticketasync <br> printworkflowbackgroundsetuprequestedeventargs.setrequiresui

#### <a name="printworkflowconfiguration"></a>[printworkflowconfiguration](/uwp/api/windows.graphics.printing.workflow.printworkflowconfiguration)

printworkflowconfiguration <br> printworkflowconfiguration.jobtitle <br> printworkflowconfiguration.sessionid <br> printworkflowconfiguration.sourceappdisplayname

#### <a name="printworkflowforegroundsession"></a>[printworkflowforegroundsession](/uwp/api/windows.graphics.printing.workflow.printworkflowforegroundsession)

printworkflowforegroundsession <br> printworkflowforegroundsession.setuprequested <br> printworkflowforegroundsession.start <br> printworkflowforegroundsession.status <br> printworkflowforegroundsession.xpsdataavailable

#### <a name="printworkflowforegroundsetuprequestedeventargs"></a>[printworkflowforegroundsetuprequestedeventargs](/uwp/api/windows.graphics.printing.workflow.printworkflowforegroundsetuprequestedeventargs)

printworkflowforegroundsetuprequestedeventargs <br> printworkflowforegroundsetuprequestedeventargs.configuration <br> printworkflowforegroundsetuprequestedeventargs.getdeferral <br> printworkflowforegroundsetuprequestedeventargs.getuserprintticketasync

#### <a name="printworkflowobjectmodelsourcefilecontent"></a>[printworkflowobjectmodelsourcefilecontent](/uwp/api/windows.graphics.printing.workflow.printworkflowobjectmodelsourcefilecontent)

printworkflowobjectmodelsourcefilecontent

#### <a name="printworkflowobjectmodeltargetpackage"></a>[printworkflowobjectmodeltargetpackage](/uwp/api/windows.graphics.printing.workflow.printworkflowobjectmodeltargetpackage)

printworkflowobjectmodeltargetpackage

#### <a name="printworkflowsessionstatus"></a>[printworkflowsessionstatus](/uwp/api/windows.graphics.printing.workflow.printworkflowsessionstatus)

printworkflowsessionstatus

#### <a name="printworkflowsourcecontent"></a>[printworkflowsourcecontent](/uwp/api/windows.graphics.printing.workflow.printworkflowsourcecontent)

printworkflowsourcecontent <br> printworkflowsourcecontent.getjobprintticketasync <br> printworkflowsourcecontent.getsourcespooldataasstreamcontent <br> printworkflowsourcecontent.getsourcespooldataasxpsobjectmodel

#### <a name="printworkflowspoolstreamcontent"></a>[printworkflowspoolstreamcontent](/uwp/api/windows.graphics.printing.workflow.printworkflowspoolstreamcontent)

printworkflowspoolstreamcontent <br> printworkflowspoolstreamcontent.getinputstream

#### <a name="printworkflowstreamtarget"></a>[printworkflowstreamtarget](/uwp/api/windows.graphics.printing.workflow.printworkflowstreamtarget)

printworkflowstreamtarget <br> printworkflowstreamtarget.getoutputstream

#### <a name="printworkflowsubmittedeventargs"></a>[printworkflowsubmittedeventargs](/uwp/api/windows.graphics.printing.workflow.printworkflowsubmittedeventargs)

printworkflowsubmittedeventargs <br> printworkflowsubmittedeventargs.getdeferral <br> printworkflowsubmittedeventargs.gettarget <br> printworkflowsubmittedeventargs.operation

#### <a name="printworkflowsubmittedoperation"></a>[printworkflowsubmittedoperation](/uwp/api/windows.graphics.printing.workflow.printworkflowsubmittedoperation)

printworkflowsubmittedoperation <br> printworkflowsubmittedoperation.complete <br> printworkflowsubmittedoperation.configuration <br> printworkflowsubmittedoperation.xpscontent

#### <a name="printworkflowsubmittedstatus"></a>[printworkflowsubmittedstatus](/uwp/api/windows.graphics.printing.workflow.printworkflowsubmittedstatus)

printworkflowsubmittedstatus

#### <a name="printworkflowtarget"></a>[printworkflowtarget](/uwp/api/windows.graphics.printing.workflow.printworkflowtarget)

printworkflowtarget <br> printworkflowtarget.targetasstream <br> printworkflowtarget.targetasxpsobjectmodelpackage

#### <a name="printworkflowtriggerdetails"></a>[printworkflowtriggerdetails](/uwp/api/windows.graphics.printing.workflow.printworkflowtriggerdetails)

printworkflowtriggerdetails <br> printworkflowtriggerdetails.printworkflowsession

#### <a name="printworkflowuiactivatedeventargs"></a>[printworkflowuiactivatedeventargs](/uwp/api/windows.graphics.printing.workflow.printworkflowuiactivatedeventargs)

printworkflowuiactivatedeventargs <br> printworkflowuiactivatedeventargs.kind <br> printworkflowuiactivatedeventargs.previousexecutionstate <br> printworkflowuiactivatedeventargs.printworkflowsession <br> printworkflowuiactivatedeventargs.splashscreen <br> printworkflowuiactivatedeventargs.user

#### <a name="printworkflowxpsdataavailableeventargs"></a>[printworkflowxpsdataavailableeventargs](/uwp/api/windows.graphics.printing.workflow.printworkflowxpsdataavailableeventargs)

printworkflowxpsdataavailableeventargs <br> printworkflowxpsdataavailableeventargs.getdeferral <br> printworkflowxpsdataavailableeventargs.operation

### <a name="windowsgraphicsprinting3d"></a>[windows.graphics.printing3d](/uwp/api/windows.graphics.printing3d)

#### <a name="printing3d3mfpackage"></a>[printing3d3mfpackage](/uwp/api/windows.graphics.printing3d.printing3d3mfpackage)

printing3d3mfpackage.compression

#### <a name="printing3dpackagecompression"></a>[printing3dpackagecompression](/uwp/api/windows.graphics.printing3d.printing3dpackagecompression)

printing3dpackagecompression

## <a name="windowsmanagement"></a>windows.management

### <a name="windowsmanagementdeployment"></a>[windows.management.deployment](/uwp/api/windows.management.deployment)

#### <a name="addpackagebyappinstalleroptions"></a>[addpackagebyappinstalleroptions](/uwp/api/windows.management.deployment.addpackagebyappinstalleroptions)

addpackagebyappinstalleroptions

#### <a name="packagemanager"></a>[packagemanager](/uwp/api/windows.management.deployment.packagemanager)

packagemanager.addpackageasync <br> packagemanager.addpackagebyappinstallerfileasync <br> packagemanager.provisionpackageforallusersasync <br> packagemanager.requestaddpackageasync <br> packagemanager.requestaddpackagebyappinstallerfileasync <br> packagemanager.stagepackageasync

## <a name="windowsmedia"></a>windows.media

### <a name="windowsmediaappbroadcasting"></a>[windows.media.appbroadcasting](/uwp/api/windows.media.appbroadcasting)

#### <a name="appbroadcastingmonitor"></a>[appbroadcastingmonitor](/uwp/api/windows.media.appbroadcasting.appbroadcastingmonitor)

appbroadcastingmonitor <br> appbroadcastingmonitor.appbroadcastingmonitor <br> appbroadcastingmonitor.iscurrentappbroadcasting <br> appbroadcastingmonitor.iscurrentappbroadcastingchanged

#### <a name="appbroadcastingstatus"></a>[appbroadcastingstatus](/uwp/api/windows.media.appbroadcasting.appbroadcastingstatus)

appbroadcastingstatus <br> appbroadcastingstatus.canstartbroadcast <br> appbroadcastingstatus.details

#### <a name="appbroadcastingstatusdetails"></a>[appbroadcastingstatusdetails](/uwp/api/windows.media.appbroadcasting.appbroadcastingstatusdetails)

appbroadcastingstatusdetails <br> appbroadcastingstatusdetails.isanyappbroadcasting <br> appbroadcastingstatusdetails.isappinactive <br> appbroadcastingstatusdetails.isblockedforapp <br> appbroadcastingstatusdetails.iscaptureresourceunavailable <br> appbroadcastingstatusdetails.isdisabledbysystem <br> appbroadcastingstatusdetails.isdisabledbyuser <br> appbroadcastingstatusdetails.isgamestreaminprogress <br> appbroadcastingstatusdetails.isgpuconstrained

#### <a name="appbroadcastingui"></a>[appbroadcastingui](/uwp/api/windows.media.appbroadcasting.appbroadcastingui)

appbroadcastingui <br> appbroadcastingui.getdefault <br> appbroadcastingui.getforuser <br> appbroadcastingui.getstatus <br> appbroadcastingui.showbroadcastui

### <a name="windowsmediaapprecording"></a>[windows.media.apprecording](/uwp/api/windows.media.apprecording)

#### <a name="apprecordingmanager"></a>[apprecordingmanager](/uwp/api/windows.media.apprecording.apprecordingmanager)

apprecordingmanager <br> apprecordingmanager.getdefault <br> apprecordingmanager.getstatus <br> apprecordingmanager.recordtimespantofileasync <br> apprecordingmanager.savescreenshottofilesasync <br> apprecordingmanager.startrecordingtofileasync <br> apprecordingmanager.supportedscreenshotmediaencodingsubtypes

#### <a name="apprecordingresult"></a>[apprecordingresult](/uwp/api/windows.media.apprecording.apprecordingresult)

apprecordingresult <br> apprecordingresult.duration <br> apprecordingresult.extendederror <br> apprecordingresult.isfiletruncated <br> apprecordingresult.succeeded

#### <a name="apprecordingsavedscreenshotinfo"></a>[apprecordingsavedscreenshotinfo](/uwp/api/windows.media.apprecording.apprecordingsavedscreenshotinfo)

apprecordingsavedscreenshotinfo <br> apprecordingsavedscreenshotinfo.file <br> apprecordingsavedscreenshotinfo.mediaencodingsubtype

#### <a name="apprecordingsavescreenshotoption"></a>[apprecordingsavescreenshotoption](/uwp/api/windows.media.apprecording.apprecordingsavescreenshotoption)

apprecordingsavescreenshotoption

#### <a name="apprecordingsavescreenshotresult"></a>[apprecordingsavescreenshotresult](/uwp/api/windows.media.apprecording.apprecordingsavescreenshotresult)

apprecordingsavescreenshotresult <br> apprecordingsavescreenshotresult.extendederror <br> apprecordingsavescreenshotresult.savedscreenshotinfos <br> apprecordingsavescreenshotresult.succeeded

#### <a name="apprecordingstatus"></a>[apprecordingstatus](/uwp/api/windows.media.apprecording.apprecordingstatus)

apprecordingstatus <br> apprecordingstatus.canrecord <br> apprecordingstatus.canrecordtimespan <br> apprecordingstatus.details <br> apprecordingstatus.historicalbufferduration

#### <a name="apprecordingstatusdetails"></a>[apprecordingstatusdetails](/uwp/api/windows.media.apprecording.apprecordingstatusdetails)

apprecordingstatusdetails <br> apprecordingstatusdetails.isanyappbroadcasting <br> apprecordingstatusdetails.isappinactive <br> apprecordingstatusdetails.isblockedforapp <br> apprecordingstatusdetails.iscaptureresourceunavailable <br> apprecordingstatusdetails.isdisabledbysystem <br> apprecordingstatusdetails.isdisabledbyuser <br> apprecordingstatusdetails.isgamestreaminprogress <br> apprecordingstatusdetails.isgpuconstrained <br> apprecordingstatusdetails.istimespanrecordingdisabled

### <a name="windowsmediacaptureframes"></a>[windows.media.capture.frames](/uwp/api/windows.media.capture.frames)

#### <a name="mediaframereader"></a>[mediaframereader](/uwp/api/windows.media.capture.frames.mediaframereader)

mediaframereader.acquisitionmode

#### <a name="mediaframereaderacquisitionmode"></a>[mediaframereaderacquisitionmode](/uwp/api/windows.media.capture.frames.mediaframereaderacquisitionmode)

mediaframereaderacquisitionmode

#### <a name="multisourcemediaframereader"></a>[multisourcemediaframereader](/uwp/api/windows.media.capture.frames.multisourcemediaframereader)

multisourcemediaframereader.acquisitionmode

### <a name="windowsmediacapture"></a>[windows.media.capture](/uwp/api/windows.media.capture)

#### <a name="appbroadcastbackgroundservice"></a>[appbroadcastbackgroundservice](/uwp/api/windows.media.capture.appbroadcastbackgroundservice)

appbroadcastbackgroundservice.broadcastchannel <br> appbroadcastbackgroundservice.broadcastchannelchanged <br> appbroadcastbackgroundservice.broadcastlanguage <br> appbroadcastbackgroundservice.broadcastlanguagechanged <br> appbroadcastbackgroundservice.broadcasttitlechanged

#### <a name="appbroadcastbackgroundservicesignininfo"></a>[appbroadcastbackgroundservicesignininfo](/uwp/api/windows.media.capture.appbroadcastbackgroundservicesignininfo)

appbroadcastbackgroundservicesignininfo.usernamechanged

#### <a name="appbroadcastbackgroundservicestreaminfo"></a>[appbroadcastbackgroundservicestreaminfo](/uwp/api/windows.media.capture.appbroadcastbackgroundservicestreaminfo)

appbroadcastbackgroundservicestreaminfo.reportproblemwithstream

#### <a name="appcapture"></a>[appcapture](/uwp/api/windows.media.capture.appcapture)

appcapture.setallowedasync

#### <a name="appcapturemetadatapriority"></a>[appcapturemetadatapriority](/uwp/api/windows.media.capture.appcapturemetadatapriority)

appcapturemetadatapriority

#### <a name="appcapturemetadatawriter"></a>[appcapturemetadatawriter](/uwp/api/windows.media.capture.appcapturemetadatawriter)

appcapturemetadatawriter <br> appcapturemetadatawriter.adddoubleevent <br> appcapturemetadatawriter.addint32event <br> appcapturemetadatawriter.addstringevent <br> appcapturemetadatawriter.appcapturemetadatawriter <br> appcapturemetadatawriter.close <br> appcapturemetadatawriter.metadatapurged <br> appcapturemetadatawriter.remainingstoragebytesavailable <br> appcapturemetadatawriter.startdoublestate <br> appcapturemetadatawriter.startint32state <br> appcapturemetadatawriter.startstringstate <br> appcapturemetadatawriter.stopallstates <br> appcapturemetadatawriter.stopstate

### <a name="windowsmediacore"></a>[windows.media.core](/uwp/api/windows.media.core)

#### <a name="audiostreamdescriptor"></a>[audiostreamdescriptor](/uwp/api/windows.media.core.audiostreamdescriptor)

audiostreamdescriptor.label

#### <a name="imediastreamdescriptor2"></a>[imediastreamdescriptor2](/uwp/api/windows.media.core.imediastreamdescriptor2)

imediastreamdescriptor2 <br> imediastreamdescriptor2.label

#### <a name="initializemediastreamsourcerequestedeventargs"></a>[initializemediastreamsourcerequestedeventargs](/uwp/api/windows.media.core.initializemediastreamsourcerequestedeventargs)

initializemediastreamsourcerequestedeventargs <br> initializemediastreamsourcerequestedeventargs.getdeferral <br> initializemediastreamsourcerequestedeventargs.randomaccessstream <br> initializemediastreamsourcerequestedeventargs.source

#### <a name="lowlightfusion"></a>[lowlightfusion](/uwp/api/windows.media.core.lowlightfusion)

lowlightfusion <br> lowlightfusion.fuseasync <br> lowlightfusion.maxsupportedframecount <br> lowlightfusion.supportedbitmappixelformats

#### <a name="lowlightfusionresult"></a>[lowlightfusionresult](/uwp/api/windows.media.core.lowlightfusionresult)

lowlightfusionresult <br> lowlightfusionresult.close <br> lowlightfusionresult.frame

#### <a name="mediasource"></a>[mediasource](/uwp/api/windows.media.core.mediasource)

mediasource.createfrommediaframesource

#### <a name="mediasourceappserviceconnection"></a>[mediasourceappserviceconnection](/uwp/api/windows.media.core.mediasourceappserviceconnection)

mediasourceappserviceconnection <br> mediasourceappserviceconnection.initializemediastreamsourcerequested <br> mediasourceappserviceconnection.mediasourceappserviceconnection <br> mediasourceappserviceconnection.start

#### <a name="mediastreamsource"></a>[mediastreamsource](/uwp/api/windows.media.core.mediastreamsource)

mediastreamsource.islive

#### <a name="msestreamsource"></a>[msestreamsource](/uwp/api/windows.media.core.msestreamsource)

msestreamsource.liveseekablerange

#### <a name="sceneanalysiseffectframe"></a>[sceneanalysiseffectframe](/uwp/api/windows.media.core.sceneanalysiseffectframe)

sceneanalysiseffectframe.analysisrecommendation

#### <a name="sceneanalysisrecommendation"></a>[sceneanalysisrecommendation](/uwp/api/windows.media.core.sceneanalysisrecommendation)

sceneanalysisrecommendation

#### <a name="videostreamdescriptor"></a>[videostreamdescriptor](/uwp/api/windows.media.core.videostreamdescriptor)

videostreamdescriptor.label

### <a name="windowsmediadialprotocol"></a>[windows.media.dialprotocol](/uwp/api/windows.media.dialprotocol)

#### <a name="dialreceiverapp"></a>[dialreceiverapp](/uwp/api/windows.media.dialprotocol.dialreceiverapp)

dialreceiverapp <br> dialreceiverapp.current <br> dialreceiverapp.getadditionaldataasync <br> dialreceiverapp.setadditionaldataasync

### <a name="windowsmediamediaproperties"></a>[windows.media.mediaproperties](/uwp/api/windows.media.mediaproperties)

#### <a name="mediaencodingprofile"></a>[mediaencodingprofile](/uwp/api/windows.media.mediaproperties.mediaencodingprofile)

mediaencodingprofile.getaudiotracks <br> mediaencodingprofile.getvideotracks <br> mediaencodingprofile.setaudiotracks <br> mediaencodingprofile.setvideotracks

### <a name="windowsmediaplayback"></a>[windows.media.playback](/uwp/api/windows.media.playback)

#### <a name="mediaplaybacksessionbufferingstartedeventargs"></a>[mediaplaybacksessionbufferingstartedeventargs](/uwp/api/windows.media.playback.mediaplaybacksessionbufferingstartedeventargs)

mediaplaybacksessionbufferingstartedeventargs <br> mediaplaybacksessionbufferingstartedeventargs.isplaybackinterruption

#### <a name="mediaplayer"></a>[mediaplayer](/uwp/api/windows.media.playback.mediaplayer)

mediaplayer.rendersubtitlestosurface <br> mediaplayer.rendersubtitlestosurface <br> mediaplayer.subtitleframechanged

### <a name="windowsmediaspeechrecognition"></a>[windows.media.speechrecognition](/uwp/api/windows.media.speechrecognition)

#### <a name="speechrecognizer"></a>[speechrecognizer](/uwp/api/windows.media.speechrecognition.speechrecognizer)

speechrecognizer.trysetsystemspeechlanguageasync

### <a name="windowsmediaspeechsynthesis"></a>[windows.media.speechsynthesis](/uwp/api/windows.media.speechsynthesis)

#### <a name="speechsynthesizer"></a>[speechsynthesizer](/uwp/api/windows.media.speechsynthesis.speechsynthesizer)

speechsynthesizer.trysetdefaultvoiceasync

#### <a name="speechsynthesizeroptions"></a>[speechsynthesizeroptions](/uwp/api/windows.media.speechsynthesis.speechsynthesizeroptions)

speechsynthesizeroptions.audiopitch <br> speechsynthesizeroptions.audiovolume <br> speechsynthesizeroptions.speakingrate

### <a name="windowsmediastreamingadaptive"></a>[windows.media.streaming.adaptive](/uwp/api/windows.media.streaming.adaptive)

#### <a name="adaptivemediasourcediagnosticavailableeventargs"></a>[adaptivemediasourcediagnosticavailableeventargs](/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcediagnosticavailableeventargs)

adaptivemediasourcediagnosticavailableeventargs.extendederror

## <a name="windowsnetworking"></a>windows.networking

### <a name="windowsnetworkingbackgroundtransfer"></a>[windows.networking.backgroundtransfer](/uwp/api/windows.networking.backgroundtransfer)

#### <a name="backgroundtransferfilerange"></a>[backgroundtransferfilerange](/uwp/api/windows.networking.backgroundtransfer.backgroundtransferfilerange)

backgroundtransferfilerange

#### <a name="backgroundtransferrangesdownloadedeventargs"></a>[backgroundtransferrangesdownloadedeventargs](/uwp/api/windows.networking.backgroundtransfer.backgroundtransferrangesdownloadedeventargs)

backgroundtransferrangesdownloadedeventargs <br> backgroundtransferrangesdownloadedeventargs.addedranges <br> backgroundtransferrangesdownloadedeventargs.getdeferral <br> backgroundtransferrangesdownloadedeventargs.wasdownloadrestarted

#### <a name="downloadoperation"></a>[downloadoperation](/uwp/api/windows.networking.backgroundtransfer.downloadoperation)

downloadoperation.currentweberrorstatus <br> downloadoperation.getdownloadedranges <br> downloadoperation.getresultrandomaccessstreamreference <br> downloadoperation.israndomaccessrequired <br> downloadoperation.rangesdownloaded <br> downloadoperation.recoverableweberrorstatuses

### <a name="windowsnetworkingconnectivity"></a>[windows.networking.connectivity](/uwp/api/windows.networking.connectivity)

#### <a name="connectionprofile"></a>[connectionprofile](/uwp/api/windows.networking.connectivity.connectionprofile)

connectionprofile.getprovidernetworkusageasync

#### <a name="providernetworkusage"></a>[providernetworkusage](/uwp/api/windows.networking.connectivity.providernetworkusage)

providernetworkusage <br> providernetworkusage.bytesreceived <br> providernetworkusage.bytessent <br> providernetworkusage.providerid

### <a name="windowsnetworkingnetworkoperators"></a>[windows.networking.networkoperators](/uwp/api/windows.networking.networkoperators)

#### <a name="mobilebroadbandantennasar"></a>[mobilebroadbandantennasar](/uwp/api/windows.networking.networkoperators.mobilebroadbandantennasar)

mobilebroadbandantennasar <br> mobilebroadbandantennasar.antennaindex <br> mobilebroadbandantennasar.sarbackoffindex

#### <a name="mobilebroadbandcellcdma"></a>[mobilebroadbandcellcdma](/uwp/api/windows.networking.networkoperators.mobilebroadbandcellcdma)

mobilebroadbandcellcdma <br> mobilebroadbandcellcdma.basestationid <br> mobilebroadbandcellcdma.basestationlastbroadcastgpstime <br> mobilebroadbandcellcdma.basestationlatitude <br> mobilebroadbandcellcdma.basestationlongitude <br> mobilebroadbandcellcdma.basestationpncode <br> mobilebroadbandcellcdma.networkid <br> mobilebroadbandcellcdma.pilotsignalstrengthindb <br> mobilebroadbandcellcdma.systemid

#### <a name="mobilebroadbandcellgsm"></a>[mobilebroadbandcellgsm](/uwp/api/windows.networking.networkoperators.mobilebroadbandcellgsm)

mobilebroadbandcellgsm <br> mobilebroadbandcellgsm.basestationid <br> mobilebroadbandcellgsm.cellid <br> mobilebroadbandcellgsm.channelnumber <br> mobilebroadbandcellgsm.locationareacode <br> mobilebroadbandcellgsm.providerid <br> mobilebroadbandcellgsm.receivedsignalstrengthindbm <br> mobilebroadbandcellgsm.timingadvanceinbitperiods

#### <a name="mobilebroadbandcelllte"></a>[mobilebroadbandcelllte](/uwp/api/windows.networking.networkoperators.mobilebroadbandcelllte)

mobilebroadbandcelllte <br> mobilebroadbandcelllte.cellid <br> mobilebroadbandcelllte.channelnumber <br> mobilebroadbandcelllte.physicalcellid <br> mobilebroadbandcelllte.providerid <br> mobilebroadbandcelllte.referencesignalreceivedpowerindbm <br> mobilebroadbandcelllte.referencesignalreceivedqualityindbm <br> mobilebroadbandcelllte.timingadvanceinbitperiods <br> mobilebroadbandcelllte.trackingareacode

#### <a name="mobilebroadbandcellsinfo"></a>[mobilebroadbandcellsinfo](/uwp/api/windows.networking.networkoperators.mobilebroadbandcellsinfo)

mobilebroadbandcellsinfo <br> mobilebroadbandcellsinfo.neighboringcellscdma <br> mobilebroadbandcellsinfo.neighboringcellsgsm <br> mobilebroadbandcellsinfo.neighboringcellslte <br> mobilebroadbandcellsinfo.neighboringcellstdscdma <br> mobilebroadbandcellsinfo.neighboringcellsumts <br> mobilebroadbandcellsinfo.servingcellscdma <br> mobilebroadbandcellsinfo.servingcellsgsm <br> mobilebroadbandcellsinfo.servingcellslte <br> mobilebroadbandcellsinfo.servingcellstdscdma <br> mobilebroadbandcellsinfo.servingcellsumts

#### <a name="mobilebroadbandcelltdscdma"></a>[mobilebroadbandcelltdscdma](/uwp/api/windows.networking.networkoperators.mobilebroadbandcelltdscdma)

mobilebroadbandcelltdscdma <br> mobilebroadbandcelltdscdma.cellid <br> mobilebroadbandcelltdscdma.cellparameterid <br> mobilebroadbandcelltdscdma.channelnumber <br> mobilebroadbandcelltdscdma.locationareacode <br> mobilebroadbandcelltdscdma.pathlossindb <br> mobilebroadbandcelltdscdma.providerid <br> mobilebroadbandcelltdscdma.receivedsignalcodepowerindbm <br> mobilebroadbandcelltdscdma.timingadvanceinbitperiods

#### <a name="mobilebroadbandcellumts"></a>[mobilebroadbandcellumts](/uwp/api/windows.networking.networkoperators.mobilebroadbandcellumts)

mobilebroadbandcellumts <br> mobilebroadbandcellumts.cellid <br> mobilebroadbandcellumts.channelnumber <br> mobilebroadbandcellumts.locationareacode <br> mobilebroadbandcellumts.pathlossindb <br> mobilebroadbandcellumts.primaryscramblingcode <br> mobilebroadbandcellumts.providerid <br> mobilebroadbandcellumts.receivedsignalcodepowerindbm <br> mobilebroadbandcellumts.signaltonoiseratioindb

#### <a name="mobilebroadbandmodem"></a>[mobilebroadbandmodem](/uwp/api/windows.networking.networkoperators.mobilebroadbandmodem)

mobilebroadbandmodem.getispassthroughenabledasync <br> mobilebroadbandmodem.setispassthroughenabledasync

#### <a name="mobilebroadbandmodemconfiguration"></a>[mobilebroadbandmodemconfiguration](/uwp/api/windows.networking.networkoperators.mobilebroadbandmodemconfiguration)

mobilebroadbandmodemconfiguration.sarmanager

#### <a name="mobilebroadbandmodemstatus"></a>[mobilebroadbandmodemstatus](/uwp/api/windows.networking.networkoperators.mobilebroadbandmodemstatus)

mobilebroadbandmodemstatus

#### <a name="mobilebroadbandnetwork"></a>[mobilebroadbandnetwork](/uwp/api/windows.networking.networkoperators.mobilebroadbandnetwork)

mobilebroadbandnetwork.getcellsinfoasync

#### <a name="mobilebroadbandsarmanager"></a>[mobilebroadbandsarmanager](/uwp/api/windows.networking.networkoperators.mobilebroadbandsarmanager)

mobilebroadbandsarmanager <br> mobilebroadbandsarmanager.antennas <br> mobilebroadbandsarmanager.disablebackoffasync <br> mobilebroadbandsarmanager.enablebackoffasync <br> mobilebroadbandsarmanager.getistransmittingasync <br> mobilebroadbandsarmanager.hysteresistimerperiod <br> mobilebroadbandsarmanager.isbackoffenabled <br> mobilebroadbandsarmanager.issarcontrolledbyhardware <br> mobilebroadbandsarmanager.iswifihardwareintegrated <br> mobilebroadbandsarmanager.revertsartohardwarecontrolasync <br> mobilebroadbandsarmanager.setconfigurationasync <br> mobilebroadbandsarmanager.settransmissionstatechangedhysteresisasync <br> mobilebroadbandsarmanager.starttransmissionstatemonitoring <br> mobilebroadbandsarmanager.stoptransmissionstatemonitoring <br> mobilebroadbandsarmanager.transmissionstatechanged

#### <a name="mobilebroadbandtransmissionstatechangedeventargs"></a>[mobilebroadbandtransmissionstatechangedeventargs](/uwp/api/windows.networking.networkoperators.mobilebroadbandtransmissionstatechangedeventargs)

mobilebroadbandtransmissionstatechangedeventargs <br> mobilebroadbandtransmissionstatechangedeventargs.istransmitting

### <a name="windowsnetworkingsockets"></a>[windows.networking.sockets](/uwp/api/windows.networking.sockets)

#### <a name="messagewebsocketcontrol"></a>[messagewebsocketcontrol](/uwp/api/windows.networking.sockets.messagewebsocketcontrol)

messagewebsocketcontrol.actualunsolicitedponginterval <br> messagewebsocketcontrol.clientcertificate <br> messagewebsocketcontrol.desiredunsolicitedponginterval <br> messagewebsocketcontrol.receivemode

#### <a name="messagewebsocketmessagereceivedeventargs"></a>[messagewebsocketmessagereceivedeventargs](/uwp/api/windows.networking.sockets.messagewebsocketmessagereceivedeventargs)

messagewebsocketmessagereceivedeventargs.ismessagecomplete

#### <a name="messagewebsocketreceivemode"></a>[messagewebsocketreceivemode](/uwp/api/windows.networking.sockets.messagewebsocketreceivemode)

messagewebsocketreceivemode

#### <a name="streamsocketcontrol"></a>[streamsocketcontrol](/uwp/api/windows.networking.sockets.streamsocketcontrol)

streamsocketcontrol.minprotectionlevel

#### <a name="streamwebsocketcontrol"></a>[streamwebsocketcontrol](/uwp/api/windows.networking.sockets.streamwebsocketcontrol)

streamwebsocketcontrol.actualunsolicitedponginterval <br> streamwebsocketcontrol.clientcertificate <br> streamwebsocketcontrol.desiredunsolicitedponginterval

## <a name="windowsphone"></a>windows.phone

### <a name="windowsphonenetworkingvoip"></a>[windows.phone.networking.voip](/uwp/api/windows.phone.networking.voip)

#### <a name="voipcallcoordinator"></a>[voipcallcoordinator](/uwp/api/windows.phone.networking.voip.voipcallcoordinator)

voipcallcoordinator.setupnewacceptedcall

#### <a name="voipphonecall"></a>[voipphonecall](/uwp/api/windows.phone.networking.voip.voipphonecall)

voipphonecall.tryshowappui

## <a name="windowssecurity"></a>windows.security

### <a name="windowssecurityauthenticationwebprovider"></a>[windows.security.authentication.web.provider](/uwp/api/windows.security.authentication.web.provider)

#### <a name="webaccountmanager"></a>[webaccountmanager](/uwp/api/windows.security.authentication.web.provider.webaccountmanager)

webaccountmanager.invalidateappcacheforaccountasync <br> webaccountmanager.invalidateappcacheforallaccountsasync

### <a name="windowssecurityenterprisedata"></a>[windows.security.enterprisedata](/uwp/api/windows.security.enterprisedata)

#### <a name="fileprotectioninfo"></a>[fileprotectioninfo](/uwp/api/windows.security.enterprisedata.fileprotectioninfo)

fileprotectioninfo.isprotectwhileopensupported

## <a name="windowsservices"></a>windows.services

### <a name="windowsservicesmapsguidance"></a>[windows.services.maps.guidance](/uwp/api/windows.services.maps.guidance)

#### <a name="guidanceroadsegment"></a>[guidanceroadsegment](/uwp/api/windows.services.maps.guidance.guidanceroadsegment)

guidanceroadsegment.isscenic

### <a name="windowsservicesmapslocalsearch"></a>[windows.services.maps.localsearch](/uwp/api/windows.services.maps.localsearch)

#### <a name="placeinfohelper"></a>[placeinfohelper](/uwp/api/windows.services.maps.localsearch.placeinfohelper)

placeinfohelper <br> placeinfohelper.createfromlocallocation

### <a name="windowsservicesmaps"></a>[windows.services.maps](/uwp/api/windows.services.maps)

#### <a name="maproute"></a>[maproute](/uwp/api/windows.services.maps.maproute)

maproute.isscenic

#### <a name="maprouteoptimization"></a>[maprouteoptimization](/uwp/api/windows.services.maps.maprouteoptimization)

maprouteoptimization.scenic

#### <a name="placeinfo"></a>[placeinfo](/uwp/api/windows.services.maps.placeinfo)

placeinfo <br> placeinfo.create <br> placeinfo.create <br> placeinfo.createfromidentifier <br> placeinfo.createfromidentifier <br> placeinfo.createfrommaplocation <br> placeinfo.displayaddress <br> placeinfo.displayname <br> placeinfo.geoshape <br> placeinfo.identifier <br> placeinfo.isshowsupported <br> placeinfo.show <br> placeinfo.show

#### <a name="placeinfocreateoptions"></a>[placeinfocreateoptions](/uwp/api/windows.services.maps.placeinfocreateoptions)

placeinfocreateoptions <br> placeinfocreateoptions.displayaddress <br> placeinfocreateoptions.displayname <br> placeinfocreateoptions.placeinfocreateoptions

## <a name="windowsstorage"></a>windows.storage

### <a name="windowsstorageprovider"></a>[windows.storage.provider](/uwp/api/windows.storage.provider)

#### <a name="storageproviderhydrationpolicy"></a>[storageproviderhydrationpolicy](/uwp/api/windows.storage.provider.storageproviderhydrationpolicy)

storageproviderhydrationpolicy

#### <a name="storageproviderhydrationpolicymodifier"></a>[storageproviderhydrationpolicymodifier](/uwp/api/windows.storage.provider.storageproviderhydrationpolicymodifier)

storageproviderhydrationpolicymodifier

#### <a name="insyncpolicy"></a>[insyncpolicy](/uwp/api/windows.storage.provider.storageproviderinsyncpolicy)

storageproviderinsyncpolicy

#### <a name="istorageprovideritempropertysource"></a>[istorageprovideritempropertysource](/uwp/api/windows.storage.provider.istorageprovideritempropertysource)

istorageprovideritempropertysource <br> istorageprovideritempropertysource.getitemproperties

#### <a name="istorageproviderpropertycapabilities"></a>[istorageproviderpropertycapabilities](/uwp/api/windows.storage.provider.istorageproviderpropertycapabilities)

istorageproviderpropertycapabilities <br> istorageproviderpropertycapabilities.ispropertysupported

#### <a name="populationpolicy"></a>[populationpolicy](/uwp/api/windows.storage.provider.storageproviderpopulationpolicy)

storageproviderpopulationpolicy

#### <a name="protectionmode"></a>[protectionmode](/uwp/api/windows.storage.provider.storageproviderprotectionmode)

storageproviderprotectionmode

#### <a name="storageprovideritemproperties"></a>[storageprovideritemproperties](/uwp/api/windows.storage.provider.storageprovideritemproperties)

storageprovideritemproperties <br> storageprovideritemproperties.setasync

#### <a name="storageprovideritemproperty"></a>[storageprovideritemproperty](/uwp/api/windows.storage.provider.storageprovideritemproperty)

storageprovideritemproperty <br> storageprovideritemproperty.iconresource <br> storageprovideritemproperty.id <br> storageprovideritemproperty.storageprovideritemproperty <br> storageprovideritemproperty.value

#### <a name="storageprovideritempropertydefinition"></a>[storageprovideritempropertydefinition](/uwp/api/windows.storage.provider.storageprovideritempropertydefinition)

storageprovideritempropertydefinition <br> storageprovideritempropertydefinition.displaynameresource <br> storageprovideritempropertydefinition.id <br> storageprovideritempropertydefinition.storageprovideritempropertydefinition

#### <a name="storageprovidersyncrootinfo"></a>[storageprovidersyncrootinfo](/uwp/api/windows.storage.provider.storageprovidersyncrootinfo)

storageprovidersyncrootinfo <br> storageprovidersyncrootinfo.allowpinning <br> storageprovidersyncrootinfo.context <br> storageprovidersyncrootinfo.displaynameresource <br> storageprovidersyncrootinfo.hydrationpolicy <br> storageprovidersyncrootinfo.hydrationpolicymodifier <br> storageprovidersyncrootinfo.iconresource <br> storageprovidersyncrootinfo.id <br> storageprovidersyncrootinfo.insyncpolicy <br> storageprovidersyncrootinfo.path <br> storageprovidersyncrootinfo.populationpolicy <br> storageprovidersyncrootinfo.protectionmode <br> storageprovidersyncrootinfo.recyclebinuri <br> storageprovidersyncrootinfo.showsiblingsasgroup <br> storageprovidersyncrootinfo.storageprovideritempropertydefinitions <br> storageprovidersyncrootinfo.storageprovidersyncrootinfo <br> storageprovidersyncrootinfo.version

#### <a name="storageprovidersyncrootmanager"></a>[storageprovidersyncrootmanager](/uwp/api/windows.storage.provider.storageprovidersyncrootmanager)

storageprovidersyncrootmanager <br> storageprovidersyncrootmanager.getcurrentsyncroots <br> storageprovidersyncrootmanager.getsyncrootinformationforfolder <br> storageprovidersyncrootmanager.getsyncrootinformationforid <br> storageprovidersyncrootmanager.register <br> storageprovidersyncrootmanager.unregister

### <a name="windowsstoragestreams"></a>[windows.storage.streams](/uwp/api/windows.storage.streams)

#### <a name="fileopendisposition"></a>[fileopendisposition](/uwp/api/windows.storage.streams.fileopendisposition)

fileopendisposition

#### <a name="filerandomaccessstream"></a>[filerandomaccessstream](/uwp/api/windows.storage.streams.filerandomaccessstream)

filerandomaccessstream.openasync <br> filerandomaccessstream.openasync <br> filerandomaccessstream.openforuserasync <br> filerandomaccessstream.openforuserasync <br> filerandomaccessstream.opentransactedwriteasync <br> filerandomaccessstream.opentransactedwriteasync <br> filerandomaccessstream.opentransactedwriteforuserasync <br> filerandomaccessstream.opentransactedwriteforuserasync

### <a name="windowsstorage"></a>[windows.storage](/uwp/api/windows.storage)

#### <a name="appdatapaths"></a>[appdatapaths](/uwp/api/windows.storage.appdatapaths)

appdatapaths <br> appdatapaths.cookies <br> appdatapaths.desktop <br> appdatapaths.documents <br> appdatapaths.favorites <br> appdatapaths.getdefault <br> appdatapaths.getforuser <br> appdatapaths.history <br> appdatapaths.internetcache <br> appdatapaths.localappdata <br> appdatapaths.programdata <br> appdatapaths.roamingappdata

#### <a name="storagelibrary"></a>[storagelibrary](/uwp/api/windows.storage.storagelibrary)

storagelibrary.arefoldersuggestionsavailableasync

#### <a name="storageprovider"></a>[storageprovider](/uwp/api/windows.storage.storageprovider)

storageprovider.ispropertysupportedforpartialfileasync

#### <a name="systemdatapaths"></a>[systemdatapaths](/uwp/api/windows.storage.systemdatapaths)

systemdatapaths <br> systemdatapaths.fonts <br> systemdatapaths.getdefault <br> systemdatapaths.programdata <br> systemdatapaths.public <br> systemdatapaths.publicdesktop <br> systemdatapaths.publicdocuments <br> systemdatapaths.publicdownloads <br> systemdatapaths.publicmusic <br> systemdatapaths.publicpictures <br> systemdatapaths.publicvideos <br> systemdatapaths.system <br> systemdatapaths.systemarm <br> systemdatapaths.systemhost <br> systemdatapaths.systemx64 <br> systemdatapaths.systemx86 <br> systemdatapaths.userprofiles <br> systemdatapaths.windows

#### <a name="userdatapaths"></a>[userdatapaths](/uwp/api/windows.storage.userdatapaths)

userdatapaths <br> userdatapaths.cameraroll <br> userdatapaths.cookies <br> userdatapaths.desktop <br> userdatapaths.documents <br> userdatapaths.downloads <br> userdatapaths.favorites <br> userdatapaths.getdefault <br> userdatapaths.getforuser <br> userdatapaths.history <br> userdatapaths.internetcache <br> userdatapaths.localappdata <br> userdatapaths.localappdatalow <br> userdatapaths.music <br> userdatapaths.pictures <br> userdatapaths.profile <br> userdatapaths.recent <br> userdatapaths.roamingappdata <br> userdatapaths.savedpictures <br> userdatapaths.screenshots <br> userdatapaths.templates <br> userdatapaths.videos

## <a name="windowssystem"></a>windows.system

### <a name="windowssystemdiagnostics"></a>[windows.system.diagnostics](/uwp/api/windows.system.diagnostics)

#### <a name="diagnosticactionresult"></a>[diagnosticactionresult](/uwp/api/windows.system.diagnostics.diagnosticactionresult)

diagnosticactionresult <br> diagnosticactionresult.extendederror <br> diagnosticactionresult.results

#### <a name="diagnosticactionstate"></a>[diagnosticactionstate](/uwp/api/windows.system.diagnostics.diagnosticactionstate)

diagnosticactionstate

#### <a name="diagnosticinvoker"></a>[diagnosticinvoker](/uwp/api/windows.system.diagnostics.diagnosticinvoker)

diagnosticinvoker <br> diagnosticinvoker.getdefault <br> diagnosticinvoker.getforuser <br> diagnosticinvoker.issupported <br> diagnosticinvoker.rundiagnosticactionasync

#### <a name="processdiagnosticinfo"></a>[processdiagnosticinfo](/uwp/api/windows.system.diagnostics.processdiagnosticinfo)

processdiagnosticinfo.getappdiagnosticinfos <br> processdiagnosticinfo.ispackaged <br> processdiagnosticinfo.trygetforprocessid

### <a name="windowssystemprofilesystemmanufacturers"></a>[windows.system.profile.systemmanufacturers](/uwp/api/windows.system.profile.systemmanufacturers)

#### <a name="oemsupportinfo"></a>[oemsupportinfo](/uwp/api/windows.system.profile.systemmanufacturers.oemsupportinfo)

oemsupportinfo <br> oemsupportinfo.supportapplink <br> oemsupportinfo.supportlink <br> oemsupportinfo.supportprovider

#### <a name="systemsupportinfo"></a>[systemsupportinfo](/uwp/api/windows.system.profile.systemmanufacturers.systemsupportinfo)

systemsupportinfo <br> systemsupportinfo.localsystemedition <br> systemsupportinfo.oemsupportinfo

### <a name="windowssystemremotesystems"></a>[windows.system.remotesystems](/uwp/api/windows.system.remotesystems)

#### <a name="remotesystem"></a>[remotesystem](/uwp/api/windows.system.remotesystems.remotesystem)

remotesystem.manufacturerdisplayname <br> remotesystem.modeldisplayname

#### <a name="remotesystemkinds"></a>[remotesystemkinds](/uwp/api/windows.system.remotesystems.remotesystemkinds)

remotesystemkinds.iot <br> remotesystemkinds.laptop <br> remotesystemkinds.tablet

### <a name="windowssystemuserprofile"></a>[windows.system.userprofile](/uwp/api/windows.system.userprofile)

#### <a name="globalizationpreferences"></a>[globalizationpreferences](/uwp/api/windows.system.userprofile.globalizationpreferences)

globalizationpreferences.trysethomegeographicregion <br> globalizationpreferences.trysetlanguages

### <a name="windowssystem"></a>[windows.system](/uwp/api/windows.system)

#### <a name="appdiagnosticinfo"></a>[appdiagnosticinfo](/uwp/api/windows.system.appdiagnosticinfo)

appdiagnosticinfo.createresourcegroupwatcher <br> appdiagnosticinfo.createwatcher <br> appdiagnosticinfo.getresourcegroups <br> appdiagnosticinfo.requestaccessasync <br> appdiagnosticinfo.requestinfoforappasync <br> appdiagnosticinfo.requestinfoforappasync <br> appdiagnosticinfo.requestinfoforpackageasync

#### <a name="appdiagnosticinfowatcher"></a>[appdiagnosticinfowatcher](/uwp/api/windows.system.appdiagnosticinfowatcher)

appdiagnosticinfowatcher <br> appdiagnosticinfowatcher.added <br> appdiagnosticinfowatcher.enumerationcompleted <br> appdiagnosticinfowatcher.removed <br> appdiagnosticinfowatcher.start <br> appdiagnosticinfowatcher.status <br> appdiagnosticinfowatcher.stop <br> appdiagnosticinfowatcher.stopped

#### <a name="appdiagnosticinfowatchereventargs"></a>[appdiagnosticinfowatchereventargs](/uwp/api/windows.system.appdiagnosticinfowatchereventargs)

appdiagnosticinfowatchereventargs <br> appdiagnosticinfowatchereventargs.appdiagnosticinfo

#### <a name="appdiagnosticinfowatcherstatus"></a>[appdiagnosticinfowatcherstatus](/uwp/api/windows.system.appdiagnosticinfowatcherstatus)

appdiagnosticinfowatcherstatus

#### <a name="appmemoryreport"></a>[appmemoryreport](/uwp/api/windows.system.appmemoryreport)

appmemoryreport.expectedtotalcommitlimit

#### <a name="appresourcegroupbackgroundtaskreport"></a>[appresourcegroupbackgroundtaskreport](/uwp/api/windows.system.appresourcegroupbackgroundtaskreport)

appresourcegroupbackgroundtaskreport <br> appresourcegroupbackgroundtaskreport.entrypoint <br> appresourcegroupbackgroundtaskreport.name <br> appresourcegroupbackgroundtaskreport.taskid <br> appresourcegroupbackgroundtaskreport.trigger

#### <a name="appresourcegroupenergyquotastate"></a>[appresourcegroupenergyquotastate](/uwp/api/windows.system.appresourcegroupenergyquotastate)

appresourcegroupenergyquotastate

#### <a name="appresourcegroupexecutionstate"></a>[appresourcegroupexecutionstate](/uwp/api/windows.system.appresourcegroupexecutionstate)

appresourcegroupexecutionstate

#### <a name="appresourcegroupinfo"></a>[appresourcegroupinfo](/uwp/api/windows.system.appresourcegroupinfo)

appresourcegroupinfo <br> appresourcegroupinfo.getbackgroundtaskreports <br> appresourcegroupinfo.getmemoryreport <br> appresourcegroupinfo.getprocessdiagnosticinfos <br> appresourcegroupinfo.getstatereport <br> appresourcegroupinfo.instanceid <br> appresourcegroupinfo.isshared

#### <a name="appresourcegroupinfowatcher"></a>[appresourcegroupinfowatcher](/uwp/api/windows.system.appresourcegroupinfowatcher)

appresourcegroupinfowatcher <br> appresourcegroupinfowatcher.added <br> appresourcegroupinfowatcher.enumerationcompleted <br> appresourcegroupinfowatcher.executionstatechanged <br> appresourcegroupinfowatcher.removed <br> appresourcegroupinfowatcher.start <br> appresourcegroupinfowatcher.status <br> appresourcegroupinfowatcher.stop <br> appresourcegroupinfowatcher.stopped

#### <a name="appresourcegroupinfowatchereventargs"></a>[appresourcegroupinfowatchereventargs](/uwp/api/windows.system.appresourcegroupinfowatchereventargs)

appresourcegroupinfowatchereventargs <br> appresourcegroupinfowatchereventargs.appdiagnosticinfos <br> appresourcegroupinfowatchereventargs.appresourcegroupinfo

#### <a name="appresourcegroupinfowatcherexecutionstatechangedeventargs"></a>[appresourcegroupinfowatcherexecutionstatechangedeventargs](/uwp/api/windows.system.appresourcegroupinfowatcherexecutionstatechangedeventargs)

appresourcegroupinfowatcherexecutionstatechangedeventargs <br> appresourcegroupinfowatcherexecutionstatechangedeventargs.appdiagnosticinfos <br> appresourcegroupinfowatcherexecutionstatechangedeventargs.appresourcegroupinfo

#### <a name="appresourcegroupinfowatcherstatus"></a>[appresourcegroupinfowatcherstatus](/uwp/api/windows.system.appresourcegroupinfowatcherstatus)

appresourcegroupinfowatcherstatus

#### <a name="appresourcegroupmemoryreport"></a>[appresourcegroupmemoryreport](/uwp/api/windows.system.appresourcegroupmemoryreport)

appresourcegroupmemoryreport <br> appresourcegroupmemoryreport.commitusagelevel <br> appresourcegroupmemoryreport.commitusagelimit <br> appresourcegroupmemoryreport.privatecommitusage <br> appresourcegroupmemoryreport.totalcommitusage

#### <a name="appresourcegroupstatereport"></a>[appresourcegroupstatereport](/uwp/api/windows.system.appresourcegroupstatereport)

appresourcegroupstatereport <br> appresourcegroupstatereport.energyquotastate <br> appresourcegroupstatereport.executionstate

#### <a name="datetimesettings"></a>[datetimesettings](/uwp/api/windows.system.datetimesettings)

datetimesettings <br> datetimesettings.setsystemdatetime

#### <a name="diagnosticaccessstatus"></a>[diagnosticaccessstatus](/uwp/api/windows.system.diagnosticaccessstatus)

diagnosticaccessstatus

#### <a name="dispatcherqueue"></a>[dispatcherqueue](/uwp/api/windows.system.dispatcherqueue)

dispatcherqueue <br> dispatcherqueue.createtimer <br> dispatcherqueue.getforcurrentthread <br> dispatcherqueue.shutdowncompleted <br> dispatcherqueue.shutdownstarting <br> dispatcherqueue.tryenqueue <br> dispatcherqueue.tryenqueue

#### <a name="dispatcherqueuecontroller"></a>[dispatcherqueuecontroller](/uwp/api/windows.system.dispatcherqueuecontroller)

dispatcherqueuecontroller <br> dispatcherqueuecontroller.createondedicatedthread <br> dispatcherqueuecontroller.dispatcherqueue <br> dispatcherqueuecontroller.shutdownqueueasync

#### <a name="dispatcherqueuehandler"></a>[dispatcherqueuehandler](/uwp/api/windows.system.dispatcherqueuehandler)

dispatcherqueuehandler

#### <a name="dispatcherqueuepriority"></a>[dispatcherqueuepriority](/uwp/api/windows.system.dispatcherqueuepriority)

dispatcherqueuepriority

#### <a name="dispatcherqueueshutdownstartingeventargs"></a>[dispatcherqueueshutdownstartingeventargs](/uwp/api/windows.system.dispatcherqueueshutdownstartingeventargs)

dispatcherqueueshutdownstartingeventargs <br> dispatcherqueueshutdownstartingeventargs.getdeferral

#### <a name="dispatcherqueuetimer"></a>[dispatcherqueuetimer](/uwp/api/windows.system.dispatcherqueuetimer)

dispatcherqueuetimer <br> dispatcherqueuetimer.interval <br> dispatcherqueuetimer.isrepeating <br> dispatcherqueuetimer.isrunning <br> dispatcherqueuetimer.start <br> dispatcherqueuetimer.stop <br> dispatcherqueuetimer.tick

#### <a name="memorymanager"></a>[memorymanager](/uwp/api/windows.system.memorymanager)

memorymanager.expectedappmemoryusagelimit

## <a name="windowsui"></a>windows.ui

### <a name="windowsuicompositioneffects"></a>[windows.ui.composition.effects](/uwp/api/windows.ui.composition.effects)

#### <a name="scenelightingeffect"></a>[scenelightingeffect](/uwp/api/windows.ui.composition.effects.scenelightingeffect)

scenelightingeffect.reflectancemodel

#### <a name="scenelightingeffectreflectancemodel"></a>[scenelightingeffectreflectancemodel](/uwp/api/windows.ui.composition.effects.scenelightingeffectreflectancemodel)

scenelightingeffectreflectancemodel

### <a name="windowsuicompositioninteractions"></a>[windows.ui.composition.interactions](/uwp/api/windows.ui.composition.interactions)

#### <a name="interactiontracker"></a>[interactiontracker](/uwp/api/windows.ui.composition.interactions.interactiontracker)

interactiontracker.configurevector2positioninertiamodifiers

#### <a name="interactiontrackerinertianaturalmotion"></a>[interactiontrackerinertianaturalmotion](/uwp/api/windows.ui.composition.interactions.interactiontrackerinertianaturalmotion)

interactiontrackerinertianaturalmotion <br> interactiontrackerinertianaturalmotion.condition <br> interactiontrackerinertianaturalmotion.create <br> interactiontrackerinertianaturalmotion.naturalmotion

#### <a name="interactiontrackervector2inertiamodifier"></a>[interactiontrackervector2inertiamodifier](/uwp/api/windows.ui.composition.interactions.interactiontrackervector2inertiamodifier)

interactiontrackervector2inertiamodifier

#### <a name="interactiontrackervector2inertianaturalmotion"></a>[interactiontrackervector2inertianaturalmotion](/uwp/api/windows.ui.composition.interactions.interactiontrackervector2inertianaturalmotion)

interactiontrackervector2inertianaturalmotion <br> interactiontrackervector2inertianaturalmotion.condition <br> interactiontrackervector2inertianaturalmotion.create <br> interactiontrackervector2inertianaturalmotion.naturalmotion

### <a name="windowsuicomposition"></a>[windows.ui.composition](/uwp/api/windows.ui.composition)

#### <a name="ambientlight"></a>[ambientlight](/uwp/api/windows.ui.composition.ambientlight)

ambientlight.intensity

#### <a name="compositionanimation"></a>[compositionanimation](/uwp/api/windows.ui.composition.compositionanimation)

compositionanimation.initialvalueexpressions

#### <a name="compositioncolorgradientstop"></a>[compositioncolorgradientstop](/uwp/api/windows.ui.composition.compositioncolorgradientstop)

compositioncolorgradientstop <br> compositioncolorgradientstop.color <br> compositioncolorgradientstop.offset

#### <a name="compositioncolorgradientstopcollection"></a>[compositioncolorgradientstopcollection](/uwp/api/windows.ui.composition.compositioncolorgradientstopcollection)

compositioncolorgradientstopcollection <br> compositioncolorgradientstopcollection.append <br> compositioncolorgradientstopcollection.clear <br> compositioncolorgradientstopcollection.first <br> compositioncolorgradientstopcollection.getat <br> compositioncolorgradientstopcollection.getmany <br> compositioncolorgradientstopcollection.getview <br> compositioncolorgradientstopcollection.indexof <br> compositioncolorgradientstopcollection.insertat <br> compositioncolorgradientstopcollection.removeat <br> compositioncolorgradientstopcollection.removeatend <br> compositioncolorgradientstopcollection.replaceall <br> compositioncolorgradientstopcollection.setat <br> compositioncolorgradientstopcollection.size

#### <a name="compositiondropshadowsourcepolicy"></a>[compositiondropshadowsourcepolicy](/uwp/api/windows.ui.composition.compositiondropshadowsourcepolicy)

compositiondropshadowsourcepolicy

#### <a name="compositiongradientbrush"></a>[compositiongradientbrush](/uwp/api/windows.ui.composition.compositiongradientbrush)

compositiongradientbrush <br> compositiongradientbrush.anchorpoint <br> compositiongradientbrush.centerpoint <br> compositiongradientbrush.colorstops <br> compositiongradientbrush.extendmode <br> compositiongradientbrush.interpolationspace <br> compositiongradientbrush.offset <br> compositiongradientbrush.rotationangle <br> compositiongradientbrush.rotationangleindegrees <br> compositiongradientbrush.scale <br> compositiongradientbrush.transformmatrix

#### <a name="compositiongradientextendmode"></a>[compositiongradientextendmode](/uwp/api/windows.ui.composition.compositiongradientextendmode)

compositiongradientextendmode

#### <a name="compositionlight"></a>[compositionlight](/uwp/api/windows.ui.composition.compositionlight)

compositionlight.exclusionsfromtargets

#### <a name="compositionlineargradientbrush"></a>[compositionlineargradientbrush](/uwp/api/windows.ui.composition.compositionlineargradientbrush)

compositionlineargradientbrush <br> compositionlineargradientbrush.endpoint <br> compositionlineargradientbrush.startpoint

#### <a name="compositionobject"></a>[compositionobject](/uwp/api/windows.ui.composition.compositionobject)

compositionobject.dispatcherqueue

#### <a name="compositor"></a>[compositor](/uwp/api/windows.ui.composition.compositor)

compositor.createcolorgradientstop <br> compositor.createcolorgradientstop <br> compositor.createlineargradientbrush <br> compositor.createspringscalaranimation <br> compositor.createspringvector2animation <br> compositor.createspringvector3animation

#### <a name="distantlight"></a>[distantlight](/uwp/api/windows.ui.composition.distantlight)

distantlight.intensity

#### <a name="dropshadow"></a>[dropshadow](/uwp/api/windows.ui.composition.dropshadow)

dropshadow.sourcepolicy

#### <a name="initialvalueexpressioncollection"></a>[initialvalueexpressioncollection](/uwp/api/windows.ui.composition.initialvalueexpressioncollection)

initialvalueexpressioncollection <br> initialvalueexpressioncollection.clear <br> initialvalueexpressioncollection.first <br> initialvalueexpressioncollection.getview <br> initialvalueexpressioncollection.haskey <br> initialvalueexpressioncollection.insert <br> initialvalueexpressioncollection.lookup <br> initialvalueexpressioncollection.remove <br> initialvalueexpressioncollection.size

#### <a name="layervisual"></a>[layervisual](/uwp/api/windows.ui.composition.layervisual)

layervisual.shadow

#### <a name="naturalmotionanimation"></a>[naturalmotionanimation](/uwp/api/windows.ui.composition.naturalmotionanimation)

naturalmotionanimation <br> naturalmotionanimation.delaybehavior <br> naturalmotionanimation.delaytime <br> naturalmotionanimation.stopbehavior

#### <a name="pointlight"></a>[pointlight](/uwp/api/windows.ui.composition.pointlight)

pointlight.intensity

#### <a name="scalarnaturalmotionanimation"></a>[scalarnaturalmotionanimation](/uwp/api/windows.ui.composition.scalarnaturalmotionanimation)

scalarnaturalmotionanimation <br> scalarnaturalmotionanimation.finalvalue <br> scalarnaturalmotionanimation.initialvalue <br> scalarnaturalmotionanimation.initialvelocity

#### <a name="spotlight"></a>[spotlight](/uwp/api/windows.ui.composition.spotlight)

spotlight.innerconeintensity <br> spotlight.outerconeintensity

#### <a name="springscalarnaturalmotionanimation"></a>[springscalarnaturalmotionanimation](/uwp/api/windows.ui.composition.springscalarnaturalmotionanimation)

springscalarnaturalmotionanimation <br> springscalarnaturalmotionanimation.dampingratio <br> springscalarnaturalmotionanimation.period

#### <a name="springvector2naturalmotionanimation"></a>[springvector2naturalmotionanimation](/uwp/api/windows.ui.composition.springvector2naturalmotionanimation)

springvector2naturalmotionanimation <br> springvector2naturalmotionanimation.dampingratio <br> springvector2naturalmotionanimation.period

#### <a name="springvector3naturalmotionanimation"></a>[springvector3naturalmotionanimation](/uwp/api/windows.ui.composition.springvector3naturalmotionanimation)

springvector3naturalmotionanimation <br> springvector3naturalmotionanimation.dampingratio <br> springvector3naturalmotionanimation.period

#### <a name="vector2naturalmotionanimation"></a>[vector2naturalmotionanimation](/uwp/api/windows.ui.composition.vector2naturalmotionanimation)

vector2naturalmotionanimation <br> vector2naturalmotionanimation.finalvalue <br> vector2naturalmotionanimation.initialvalue <br> vector2naturalmotionanimation.initialvelocity

#### <a name="vector3naturalmotionanimation"></a>[vector3naturalmotionanimation](/uwp/api/windows.ui.composition.vector3naturalmotionanimation)

vector3naturalmotionanimation <br> vector3naturalmotionanimation.finalvalue <br> vector3naturalmotionanimation.initialvalue <br> vector3naturalmotionanimation.initialvelocity

### <a name="windowsuicore"></a>[windows.ui.core](/uwp/api/windows.ui.core)

#### <a name="corewindow"></a>[corewindow](/uwp/api/windows.ui.core.corewindow)

corewindow.activationmode <br> corewindow.dispatcherqueue

#### <a name="corewindowactivationmode"></a>[corewindowactivationmode](/uwp/api/windows.ui.core.corewindowactivationmode)

corewindowactivationmode

### <a name="windowsuiinputinkingcore"></a>[windows.ui.input.inking.core](/uwp/api/windows.ui.input.inking.core)

#### <a name="coreincrementalinkstroke"></a>[coreincrementalinkstroke](/uwp/api/windows.ui.input.inking.core.coreincrementalinkstroke)

coreincrementalinkstroke <br> coreincrementalinkstroke.appendinkpoints <br> coreincrementalinkstroke.boundingrect <br> coreincrementalinkstroke.coreincrementalinkstroke <br> coreincrementalinkstroke.createinkstroke <br> coreincrementalinkstroke.drawingattributes <br> coreincrementalinkstroke.pointtransform

#### <a name="coreinkpresenterhost"></a>[coreinkpresenterhost](/uwp/api/windows.ui.input.inking.core.coreinkpresenterhost)

coreinkpresenterhost <br> coreinkpresenterhost.coreinkpresenterhost <br> coreinkpresenterhost.inkpresenter <br> coreinkpresenterhost.rootvisual

### <a name="windowsuiinputpreviewinjection"></a>[windows.ui.input.preview.injection](/uwp/api/windows.ui.input.preview.injection)

#### <a name="injectedinputgamepadinfo"></a>[injectedinputgamepadinfo](/uwp/api/windows.ui.input.preview.injection.injectedinputgamepadinfo)

injectedinputgamepadinfo <br> injectedinputgamepadinfo.buttons <br> injectedinputgamepadinfo.injectedinputgamepadinfo <br> injectedinputgamepadinfo.injectedinputgamepadinfo <br> injectedinputgamepadinfo.leftthumbstickx <br> injectedinputgamepadinfo.leftthumbsticky <br> injectedinputgamepadinfo.lefttrigger <br> injectedinputgamepadinfo.rightthumbstickx <br> injectedinputgamepadinfo.rightthumbsticky <br> injectedinputgamepadinfo.righttrigger

#### <a name="inputinjector"></a>[inputinjector](/uwp/api/windows.ui.input.preview.injection.inputinjector)

inputinjector.initializegamepadinjection <br> inputinjector.injectgamepadinput <br> inputinjector.trycreateforappbroadcastonly <br> inputinjector.uninitializegamepadinjection

### <a name="windowsuiinputspatial"></a>[windows.ui.input.spatial](/uwp/api/windows.ui.input.spatial)

#### <a name="spatialinteractioncontroller"></a>[spatialinteractioncontroller](/uwp/api/windows.ui.input.spatial.spatialinteractioncontroller)

spatialinteractioncontroller.trygetrenderablemodelasync

#### <a name="spatialinteractionsource"></a>[spatialinteractionsource](/uwp/api/windows.ui.input.spatial.spatialinteractionsource)

spatialinteractionsource.handedness

#### <a name="spatialinteractionsourcehandedness"></a>[spatialinteractionsourcehandedness](/uwp/api/windows.ui.input.spatial.spatialinteractionsourcehandedness)

spatialinteractionsourcehandedness

#### <a name="spatialinteractionsourcelocation"></a>[spatialinteractionsourcelocation](/uwp/api/windows.ui.input.spatial.spatialinteractionsourcelocation)

spatialinteractionsourcelocation.angularvelocity <br> spatialinteractionsourcelocation.positionaccuracy <br> spatialinteractionsourcelocation.sourcepointerpose

#### <a name="spatialinteractionsourcepositionaccuracy"></a>[spatialinteractionsourcepositionaccuracy](/uwp/api/windows.ui.input.spatial.spatialinteractionsourcepositionaccuracy)

spatialinteractionsourcepositionaccuracy

#### <a name="spatialpointerinteractionsourcepose"></a>[spatialpointerinteractionsourcepose](/uwp/api/windows.ui.input.spatial.spatialpointerinteractionsourcepose)

spatialpointerinteractionsourcepose.orientation <br> spatialpointerinteractionsourcepose.positionaccuracy

### <a name="windowsuiinput"></a>[windows.ui.input](/uwp/api/windows.ui.input)

#### <a name="radialcontrollerconfiguration"></a>[radialcontrollerconfiguration](/uwp/api/windows.ui.input.radialcontrollerconfiguration)

radialcontrollerconfiguration.appcontroller <br> radialcontrollerconfiguration.isappcontrollerenabled

### <a name="windowsuishell"></a>[windows.ui.shell](/uwp/api/windows.ui.shell)

#### <a name="adaptivecardbuilder"></a>[adaptivecardbuilder](/uwp/api/windows.ui.shell.adaptivecardbuilder)

adaptivecardbuilder <br> adaptivecardbuilder.createadaptivecardfromjson

#### <a name="iadaptivecard"></a>[iadaptivecard](/uwp/api/windows.ui.shell.iadaptivecard)

iadaptivecard <br> iadaptivecard.tojson

#### <a name="iadaptivecardbuilderstatics"></a>[iadaptivecardbuilderstatics](/uwp/api/windows.ui.shell.iadaptivecardbuilderstatics)

iadaptivecardbuilderstatics <br> iadaptivecardbuilderstatics.createadaptivecardfromjson

#### <a name="taskbarmanager"></a>[taskbarmanager](/uwp/api/windows.ui.shell.taskbarmanager)

taskbarmanager <br> taskbarmanager.getdefault <br> taskbarmanager.isapplistentrypinnedasync <br> taskbarmanager.iscurrentapppinnedasync <br> taskbarmanager.ispinningallowed <br> taskbarmanager.issupported <br> taskbarmanager.requestpinapplistentryasync <br> taskbarmanager.requestpincurrentappasync

### <a name="windowsuistartscreen"></a>[windows.ui.startscreen](/uwp/api/windows.ui.startscreen)

#### <a name="secondarytilevisualelements"></a>[secondarytilevisualelements](/uwp/api/windows.ui.startscreen.secondarytilevisualelements)

secondarytilevisualelements.mixedrealitymodel

#### <a name="tilemixedrealitymodel"></a>[tilemixedrealitymodel](/uwp/api/windows.ui.startscreen.tilemixedrealitymodel)

tilemixedrealitymodel <br> tilemixedrealitymodel.boundingbox <br> tilemixedrealitymodel.uri

### <a name="windowsuiviewmanagementcore"></a>[windows.ui.viewmanagement.core](/uwp/api/windows.ui.viewmanagement.core)

#### <a name="coreinputview"></a>[coreinputview](/uwp/api/windows.ui.viewmanagement.core.coreinputview)

coreinputview <br> coreinputview.getcoreinputviewocclusions <br> coreinputview.getforcurrentview <br> coreinputview.occlusionschanged <br> coreinputview.tryhideprimaryview <br> coreinputview.tryshowprimaryview

#### <a name="coreinputviewocclusion"></a>[coreinputviewocclusion](/uwp/api/windows.ui.viewmanagement.core.coreinputviewocclusion)

coreinputviewocclusion <br> coreinputviewocclusion.occludingrect <br> coreinputviewocclusion.occlusionkind

#### <a name="coreinputviewocclusionkind"></a>[coreinputviewocclusionkind](/uwp/api/windows.ui.viewmanagement.core.coreinputviewocclusionkind)

coreinputviewocclusionkind

#### <a name="coreinputviewocclusionschangedeventargs"></a>[coreinputviewocclusionschangedeventargs](/uwp/api/windows.ui.viewmanagement.core.coreinputviewocclusionschangedeventargs)

coreinputviewocclusionschangedeventargs <br> coreinputviewocclusionschangedeventargs.handled <br> coreinputviewocclusionschangedeventargs.occlusions

### <a name="windowsuiwebui"></a>[windows.ui.webui](/uwp/api/windows.ui.webui)

#### <a name="webuiapplication"></a>[webuiapplication](/uwp/api/windows.ui.webui.webuiapplication)

webuiapplication.requestrestartasync <br> webuiapplication.requestrestartforuserasync

#### <a name="webuicommandlineactivatedeventargs"></a>[webuicommandlineactivatedeventargs](/uwp/api/windows.ui.webui.webuicommandlineactivatedeventargs)

webuicommandlineactivatedeventargs <br> webuicommandlineactivatedeventargs.activatedoperation <br> webuicommandlineactivatedeventargs.kind <br> webuicommandlineactivatedeventargs.operation <br> webuicommandlineactivatedeventargs.previousexecutionstate <br> webuicommandlineactivatedeventargs.splashscreen <br> webuicommandlineactivatedeventargs.user

#### <a name="webuistartuptaskactivatedeventargs"></a>[webuistartuptaskactivatedeventargs](/uwp/api/windows.ui.webui.webuistartuptaskactivatedeventargs)

webuistartuptaskactivatedeventargs <br> webuistartuptaskactivatedeventargs.kind <br> webuistartuptaskactivatedeventargs.previousexecutionstate <br> webuistartuptaskactivatedeventargs.splashscreen <br> webuistartuptaskactivatedeventargs.taskid <br> webuistartuptaskactivatedeventargs.user

### <a name="windowsuixamlautomationpeers"></a>[windows.ui.xaml.automation.peers](/uwp/api/windows.ui.xaml.automation.peers)

#### <a name="automationnotificationkind"></a>[automationnotificationkind](/uwp/api/windows.ui.xaml.automation.peers.automationnotificationkind)

automationnotificationkind

#### <a name="automationnotificationprocessing"></a>[automationnotificationprocessing](/uwp/api/windows.ui.xaml.automation.peers.automationnotificationprocessing)

automationnotificationprocessing

#### <a name="automationpeer"></a>[automationpeer](/uwp/api/windows.ui.xaml.automation.peers.automationpeer)

automationpeer.raisenotificationevent

#### <a name="colorpickersliderautomationpeer"></a>[colorpickersliderautomationpeer](/uwp/api/windows.ui.xaml.automation.peers.colorpickersliderautomationpeer)

colorpickersliderautomationpeer <br> colorpickersliderautomationpeer.colorpickersliderautomationpeer

#### <a name="colorspectrumautomationpeer"></a>[colorspectrumautomationpeer](/uwp/api/windows.ui.xaml.automation.peers.colorspectrumautomationpeer)

colorspectrumautomationpeer <br> colorspectrumautomationpeer.colorspectrumautomationpeer

#### <a name="navigationviewitemautomationpeer"></a>[navigationviewitemautomationpeer](/uwp/api/windows.ui.xaml.automation.peers.navigationviewitemautomationpeer)

navigationviewitemautomationpeer <br> navigationviewitemautomationpeer.navigationviewitemautomationpeer

#### <a name="personpictureautomationpeer"></a>[personpictureautomationpeer](/uwp/api/windows.ui.xaml.automation.peers.personpictureautomationpeer)

personpictureautomationpeer <br> personpictureautomationpeer.personpictureautomationpeer

#### <a name="ratingcontrolautomationpeer"></a>[ratingcontrolautomationpeer](/uwp/api/windows.ui.xaml.automation.peers.ratingcontrolautomationpeer)

ratingcontrolautomationpeer <br> ratingcontrolautomationpeer.ratingcontrolautomationpeer

### <a name="windowsuixamlcontrolsmaps"></a>[windows.ui.xaml.controls.maps](/uwp/api/windows.ui.xaml.controls.maps)

#### <a name="mapcontrol"></a>[mapcontrol](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol)

mapcontrol.layers <br> mapcontrol.layersproperty <br> mapcontrol.trygetlocationfromoffset <br> mapcontrol.trygetlocationfromoffset

#### <a name="mapcontroldatahelper"></a>[mapcontroldatahelper](/uwp/api/windows.ui.xaml.controls.maps.mapcontroldatahelper)

mapcontroldatahelper.createmapcontrol

#### <a name="mapelement3d"></a>[mapelement3d](/uwp/api/windows.ui.xaml.controls.maps.mapelement3d)

mapelement3d <br> mapelement3d.heading <br> mapelement3d.headingproperty <br> mapelement3d.location <br> mapelement3d.locationproperty <br> mapelement3d.mapelement3d <br> mapelement3d.model <br> mapelement3d.pitch <br> mapelement3d.pitchproperty <br> mapelement3d.roll <br> mapelement3d.rollproperty <br> mapelement3d.scale <br> mapelement3d.scaleproperty

#### <a name="mapelement"></a>[mapelement](/uwp/api/windows.ui.xaml.controls.maps.mapelement)

mapelement.mapstylesheetentry <br> mapelement.mapstylesheetentryproperty <br> mapelement.mapstylesheetentrystate <br> mapelement.mapstylesheetentrystateproperty <br> mapelement.tag <br> mapelement.tagproperty

#### <a name="mapelementslayer"></a>[mapelementslayer](/uwp/api/windows.ui.xaml.controls.maps.mapelementslayer)

mapelementslayer <br> mapelementslayer.mapcontextrequested <br> mapelementslayer.mapelementclick <br> mapelementslayer.mapelementpointerentered <br> mapelementslayer.mapelementpointerexited <br> mapelementslayer.mapelements <br> mapelementslayer.mapelementslayer <br> mapelementslayer.mapelementsproperty

#### <a name="mapelementslayerclickeventargs"></a>[mapelementslayerclickeventargs](/uwp/api/windows.ui.xaml.controls.maps.mapelementslayerclickeventargs)

mapelementslayerclickeventargs <br> mapelementslayerclickeventargs.location <br> mapelementslayerclickeventargs.mapelements <br> mapelementslayerclickeventargs.mapelementslayerclickeventargs <br> mapelementslayerclickeventargs.position

#### <a name="mapelementslayercontextrequestedeventargs"></a>[mapelementslayercontextrequestedeventargs](/uwp/api/windows.ui.xaml.controls.maps.mapelementslayercontextrequestedeventargs)

mapelementslayercontextrequestedeventargs <br> mapelementslayercontextrequestedeventargs.location <br> mapelementslayercontextrequestedeventargs.mapelements <br> mapelementslayercontextrequestedeventargs.mapelementslayercontextrequestedeventargs <br> mapelementslayercontextrequestedeventargs.position

#### <a name="mapelementslayerpointerenteredeventargs"></a>[mapelementslayerpointerenteredeventargs](/uwp/api/windows.ui.xaml.controls.maps.mapelementslayerpointerenteredeventargs)

mapelementslayerpointerenteredeventargs <br> mapelementslayerpointerenteredeventargs.location <br> mapelementslayerpointerenteredeventargs.mapelement <br> mapelementslayerpointerenteredeventargs.mapelementslayerpointerenteredeventargs <br> mapelementslayerpointerenteredeventargs.position

#### <a name="mapelementslayerpointerexitedeventargs"></a>[mapelementslayerpointerexitedeventargs](/uwp/api/windows.ui.xaml.controls.maps.mapelementslayerpointerexitedeventargs)

mapelementslayerpointerexitedeventargs <br> mapelementslayerpointerexitedeventargs.location <br> mapelementslayerpointerexitedeventargs.mapelement <br> mapelementslayerpointerexitedeventargs.mapelementslayerpointerexitedeventargs <br> mapelementslayerpointerexitedeventargs.position

#### <a name="maplayer"></a>[maplayer](/uwp/api/windows.ui.xaml.controls.maps.maplayer)

maplayer <br> maplayer.maplayer <br> maplayer.maptabindex <br> maplayer.maptabindexproperty <br> maplayer.visible <br> maplayer.visibleproperty <br> maplayer.zindex <br> maplayer.zindexproperty

#### <a name="mapmodel3d"></a>[mapmodel3d](/uwp/api/windows.ui.xaml.controls.maps.mapmodel3d)

mapmodel3d <br> mapmodel3d.createfrom3mfasync <br> mapmodel3d.createfrom3mfasync <br> mapmodel3d.mapmodel3d

#### <a name="mapmodel3dshadingoption"></a>[mapmodel3dshadingoption](/uwp/api/windows.ui.xaml.controls.maps.mapmodel3dshadingoption)

mapmodel3dshadingoption

#### <a name="mapstylesheetentries"></a>[mapstylesheetentries](/uwp/api/windows.ui.xaml.controls.maps.mapstylesheetentries)

mapstylesheetentries <br> mapstylesheetentries.admindistrict <br> mapstylesheetentries.admindistrictcapital <br> mapstylesheetentries.airport <br> mapstylesheetentries.area <br> mapstylesheetentries.arterialroad <br> mapstylesheetentries.building <br> mapstylesheetentries.business <br> mapstylesheetentries.capital <br> mapstylesheetentries.cemetery <br> mapstylesheetentries.continent <br> mapstylesheetentries.controlledaccesshighway <br> mapstylesheetentries.countryregion <br> mapstylesheetentries.countryregioncapital <br> mapstylesheetentries.district <br> mapstylesheetentries.drivingroute <br> mapstylesheetentries.education <br> mapstylesheetentries.educationbuilding <br> mapstylesheetentries.foodpoint <br> mapstylesheetentries.forest <br> mapstylesheetentries.golfcourse <br> mapstylesheetentries.highspeedramp <br> mapstylesheetentries.highway <br> mapstylesheetentries.indigenouspeoplesreserve <br> mapstylesheetentries.island <br> mapstylesheetentries.majorroad <br> mapstylesheetentries.medical <br> mapstylesheetentries.medicalbuilding <br> mapstylesheetentries.military <br> mapstylesheetentries.naturalpoint <br> mapstylesheetentries.nautical <br> mapstylesheetentries.neighborhood <br> mapstylesheetentries.park <br> mapstylesheetentries.peak <br> mapstylesheetentries.playingfield <br> mapstylesheetentries.point <br> mapstylesheetentries.pointofinterest <br> mapstylesheetentries.political <br> mapstylesheetentries.populatedplace <br> mapstylesheetentries.railway <br> mapstylesheetentries.ramp <br> mapstylesheetentries.reserve <br> mapstylesheetentries.river <br> mapstylesheetentries.road <br> mapstylesheetentries.roadexit <br> mapstylesheetentries.roadshield <br> mapstylesheetentries.routeline <br> mapstylesheetentries.runway <br> mapstylesheetentries.sand <br> mapstylesheetentries.shoppingcenter <br> mapstylesheetentries.stadium <br> mapstylesheetentries.street <br> mapstylesheetentries.structure <br> mapstylesheetentries.tollroad <br> mapstylesheetentries.trail <br> mapstylesheetentries.transit <br> mapstylesheetentries.transitbuilding <br> mapstylesheetentries.transportation <br> mapstylesheetentries.unpavedstreet <br> mapstylesheetentries.vegetation <br> mapstylesheetentries.volcanicpeak <br> mapstylesheetentries.walkingroute <br> mapstylesheetentries.water <br> mapstylesheetentries.waterpoint <br> mapstylesheetentries.waterroute

#### <a name="mapstylesheetentrystates"></a>[mapstylesheetentrystates](/uwp/api/windows.ui.xaml.controls.maps.mapstylesheetentrystates)

mapstylesheetentrystates <br> mapstylesheetentrystates.disabled <br> mapstylesheetentrystates.hover <br> mapstylesheetentrystates.selected

### <a name="windowsuixamlcontrolsprimitives"></a>[windows.ui.xaml.controls.primitives](/uwp/api/windows.ui.xaml.controls.primitives)

#### <a name="colorpickerslider"></a>[colorpickerslider](/uwp/api/windows.ui.xaml.controls.primitives.colorpickerslider)

colorpickerslider <br> colorpickerslider.colorchannel <br> colorpickerslider.colorchannelproperty <br> colorpickerslider.colorpickerslider

#### <a name="colorspectrum"></a>[colorspectrum](/uwp/api/windows.ui.xaml.controls.primitives.colorspectrum)

colorspectrum <br> colorspectrum.color <br> colorspectrum.colorchanged <br> colorspectrum.colorproperty <br> colorspectrum.colorspectrum <br> colorspectrum.components <br> colorspectrum.componentsproperty <br> colorspectrum.hsvcolor <br> colorspectrum.hsvcolorproperty <br> colorspectrum.maxhue <br> colorspectrum.maxhueproperty <br> colorspectrum.maxsaturation <br> colorspectrum.maxsaturationproperty <br> colorspectrum.maxvalue <br> colorspectrum.maxvalueproperty <br> colorspectrum.minhue <br> colorspectrum.minhueproperty <br> colorspectrum.minsaturation <br> colorspectrum.minsaturationproperty <br> colorspectrum.minvalue <br> colorspectrum.minvalueproperty <br> colorspectrum.shape <br> colorspectrum.shapeproperty

#### <a name="flyoutbase"></a>[flyoutbase](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase)

flyoutbase.onprocesskeyboardaccelerators <br> flyoutbase.tryinvokekeyboardaccelerator

#### <a name="layoutinformation"></a>[layoutinformation](/uwp/api/windows.ui.xaml.controls.primitives.layoutinformation)

layoutinformation.getavailablesize

#### <a name="listviewitempresenter"></a>[listviewitempresenter](/uwp/api/windows.ui.xaml.controls.primitives.listviewitempresenter)

listviewitempresenter.revealbackground <br> listviewitempresenter.revealbackgroundproperty <br> listviewitempresenter.revealbackgroundshowsabovecontent <br> listviewitempresenter.revealbackgroundshowsabovecontentproperty <br> listviewitempresenter.revealborderbrush <br> listviewitempresenter.revealborderbrushproperty <br> listviewitempresenter.revealborderthickness <br> listviewitempresenter.revealborderthicknessproperty

### <a name="windowsuixamlcontrols"></a>[windows.ui.xaml.controls](/uwp/api/windows.ui.xaml.controls)

#### <a name="bitmapiconsource"></a>[bitmapiconsource](/uwp/api/windows.ui.xaml.controls.bitmapiconsource)

bitmapiconsource <br> bitmapiconsource.bitmapiconsource <br> bitmapiconsource.showasmonochrome <br> bitmapiconsource.showasmonochromeproperty <br> bitmapiconsource.urisource <br> bitmapiconsource.urisourceproperty

#### <a name="charactercasing"></a>[charactercasing](/uwp/api/windows.ui.xaml.controls.charactercasing)

charactercasing

#### <a name="colorchangedeventargs"></a>[colorchangedeventargs](/uwp/api/windows.ui.xaml.controls.colorchangedeventargs)

colorchangedeventargs <br> colorchangedeventargs.newcolor <br> colorchangedeventargs.oldcolor

#### <a name="colorpicker"></a>[colorpicker](/uwp/api/windows.ui.xaml.controls.colorpicker)

colorpicker <br> colorpicker.color <br> colorpicker.colorchanged <br> colorpicker.colorpicker <br> colorpicker.colorproperty <br> colorpicker.colorspectrumcomponents <br> colorpicker.colorspectrumcomponentsproperty <br> colorpicker.colorspectrumshape <br> colorpicker.colorspectrumshapeproperty <br> colorpicker.isalphaenabled <br> colorpicker.isalphaenabledproperty <br> colorpicker.isalphaslidervisible <br> colorpicker.isalphaslidervisibleproperty <br> colorpicker.isalphatextinputvisible <br> colorpicker.isalphatextinputvisibleproperty <br> colorpicker.iscolorchanneltextinputvisible <br> colorpicker.iscolorchanneltextinputvisibleproperty <br> colorpicker.iscolorpreviewvisible <br> colorpicker.iscolorpreviewvisibleproperty <br> colorpicker.iscolorslidervisible <br> colorpicker.iscolorslidervisibleproperty <br> colorpicker.iscolorspectrumvisible <br> colorpicker.iscolorspectrumvisibleproperty <br> colorpicker.ishexinputvisible <br> colorpicker.ishexinputvisibleproperty <br> colorpicker.ismorebuttonvisible <br> colorpicker.ismorebuttonvisibleproperty <br> colorpicker.maxhue <br> colorpicker.maxhueproperty <br> colorpicker.maxsaturation <br> colorpicker.maxsaturationproperty <br> colorpicker.maxvalue <br> colorpicker.maxvalueproperty <br> colorpicker.minhue <br> colorpicker.minhueproperty <br> colorpicker.minsaturation <br> colorpicker.minsaturationproperty <br> colorpicker.minvalue <br> colorpicker.minvalueproperty <br> colorpicker.previouscolor <br> colorpicker.previouscolorproperty

#### <a name="colorpickerhsvchannel"></a>[colorpickerhsvchannel](/uwp/api/windows.ui.xaml.controls.colorpickerhsvchannel)

colorpickerhsvchannel

#### <a name="colorspectrumcomponents"></a>[colorspectrumcomponents](/uwp/api/windows.ui.xaml.controls.colorspectrumcomponents)

colorspectrumcomponents

#### <a name="colorspectrumshape"></a>[colorspectrumshape](/uwp/api/windows.ui.xaml.controls.colorspectrumshape)

colorspectrumshape

#### <a name="combobox"></a>[combobox](/uwp/api/windows.ui.xaml.controls.combobox)

combobox.placeholderforeground <br> combobox.placeholderforegroundproperty

#### <a name="contentdialog"></a>[contentdialog](/uwp/api/windows.ui.xaml.controls.contentdialog)

contentdialog.showasync

#### <a name="contentdialogplacement"></a>[contentdialogplacement](/uwp/api/windows.ui.xaml.controls.contentdialogplacement)

contentdialogplacement

#### <a name="control"></a>[control](/uwp/api/windows.ui.xaml.controls.control)

control.oncharacterreceived <br> control.onpreviewkeydown <br> control.onpreviewkeyup

#### <a name="disabledformattingaccelerators"></a>[disabledformattingaccelerators](/uwp/api/windows.ui.xaml.controls.disabledformattingaccelerators)

disabledformattingaccelerators

#### <a name="fonticonsource"></a>[fonticonsource](/uwp/api/windows.ui.xaml.controls.fonticonsource)

fonticonsource <br> fonticonsource.fontfamily <br> fonticonsource.fontfamilyproperty <br> fonticonsource.fonticonsource <br> fonticonsource.fontsize <br> fonticonsource.fontsizeproperty <br> fonticonsource.fontstyle <br> fonticonsource.fontstyleproperty <br> fonticonsource.fontweight <br> fonticonsource.fontweightproperty <br> fonticonsource.glyph <br> fonticonsource.glyphproperty <br> fonticonsource.istextscalefactorenabled <br> fonticonsource.istextscalefactorenabledproperty <br> fonticonsource.mirroredwhenrighttoleft <br> fonticonsource.mirroredwhenrighttoleftproperty

#### <a name="grid"></a>[grid](/uwp/api/windows.ui.xaml.controls.grid)

grid.columnspacing <br> grid.columnspacingproperty <br> grid.rowspacing <br> grid.rowspacingproperty

#### <a name="iconsource"></a>[iconsource](/uwp/api/windows.ui.xaml.controls.iconsource)

iconsource <br> iconsource.foreground <br> iconsource.foregroundproperty

#### <a name="istexttrimmedchangedeventargs"></a>[istexttrimmedchangedeventargs](/uwp/api/windows.ui.xaml.controls.istexttrimmedchangedeventargs)

istexttrimmedchangedeventargs

#### <a name="mediatransportcontrols"></a>[mediatransportcontrols](/uwp/api/windows.ui.xaml.controls.mediatransportcontrols)

mediatransportcontrols.hide <br> mediatransportcontrols.isrepeatbuttonvisible <br> mediatransportcontrols.isrepeatbuttonvisibleproperty <br> mediatransportcontrols.isrepeatenabled <br> mediatransportcontrols.isrepeatenabledproperty <br> mediatransportcontrols.show <br> mediatransportcontrols.showandhideautomatically <br> mediatransportcontrols.showandhideautomaticallyproperty

#### <a name="navigationview"></a>[navigationview](/uwp/api/windows.ui.xaml.controls.navigationview)

navigationview <br> navigationview.alwaysshowheader <br> navigationview.alwaysshowheaderproperty <br> navigationview.autosuggestbox <br> navigationview.autosuggestboxproperty <br> navigationview.compactmodethresholdwidth <br> navigationview.compactmodethresholdwidthproperty <br> navigationview.compactpanelength <br> navigationview.compactpanelengthproperty <br> navigationview.containerfrommenuitem <br> navigationview.displaymode <br> navigationview.displaymodechanged <br> navigationview.displaymodeproperty <br> navigationview.expandedmodethresholdwidth <br> navigationview.expandedmodethresholdwidthproperty <br> navigationview.header <br> navigationview.headerproperty <br> navigationview.headertemplate <br> navigationview.headertemplateproperty <br> navigationview.ispaneopen <br> navigationview.ispaneopenproperty <br> navigationview.ispanetogglebuttonvisible <br> navigationview.ispanetogglebuttonvisibleproperty <br> navigationview.issettingsvisible <br> navigationview.issettingsvisibleproperty <br> navigationview.iteminvoked <br> navigationview.menuitemcontainerstyle <br> navigationview.menuitemcontainerstyleproperty <br> navigationview.menuitemcontainerstyleselector <br> navigationview.menuitemcontainerstyleselectorproperty <br> navigationview.menuitemfromcontainer <br> navigationview.menuitems <br> navigationview.menuitemsproperty <br> navigationview.menuitemssource <br> navigationview.menuitemssourceproperty <br> navigationview.menuitemtemplate <br> navigationview.menuitemtemplateproperty <br> navigationview.menuitemtemplateselector <br> navigationview.menuitemtemplateselectorproperty <br> navigationview.navigationview <br> navigationview.openpanelength <br> navigationview.openpanelengthproperty <br> navigationview.panefooter <br> navigationview.panefooterproperty <br> navigationview.panetogglebuttonstyle <br> navigationview.panetogglebuttonstyleproperty <br> navigationview.selecteditem <br> navigationview.selecteditemproperty <br> navigationview.selectionchanged <br> navigationview.settingsitem <br> navigationview.settingsitemproperty

#### <a name="navigationviewdisplaymode"></a>[navigationviewdisplaymode](/uwp/api/windows.ui.xaml.controls.navigationviewdisplaymode)

navigationviewdisplaymode

#### <a name="navigationviewdisplaymodechangedeventargs"></a>[navigationviewdisplaymodechangedeventargs](/uwp/api/windows.ui.xaml.controls.navigationviewdisplaymodechangedeventargs)

navigationviewdisplaymodechangedeventargs <br> navigationviewdisplaymodechangedeventargs.displaymode

#### <a name="navigationviewitem"></a>[navigationviewitem](/uwp/api/windows.ui.xaml.controls.navigationviewitem)

navigationviewitem <br> navigationviewitem.compactpanelength <br> navigationviewitem.compactpanelengthproperty <br> navigationviewitem.icon <br> navigationviewitem.iconproperty <br> navigationviewitem.navigationviewitem

#### <a name="navigationviewitembase"></a>[navigationviewitembase](/uwp/api/windows.ui.xaml.controls.navigationviewitembase)

navigationviewitembase

#### <a name="navigationviewitemheader"></a>[navigationviewitemheader](/uwp/api/windows.ui.xaml.controls.navigationviewitemheader)

navigationviewitemheader <br> navigationviewitemheader.navigationviewitemheader

#### <a name="navigationviewiteminvokedeventargs"></a>[navigationviewiteminvokedeventargs](/uwp/api/windows.ui.xaml.controls.navigationviewiteminvokedeventargs)

navigationviewiteminvokedeventargs <br> navigationviewiteminvokedeventargs.invokeditem <br> navigationviewiteminvokedeventargs.issettingsinvoked <br> navigationviewiteminvokedeventargs.navigationviewiteminvokedeventargs

#### <a name="navigationviewitemseparator"></a>[navigationviewitemseparator](/uwp/api/windows.ui.xaml.controls.navigationviewitemseparator)

navigationviewitemseparator <br> navigationviewitemseparator.navigationviewitemseparator

#### <a name="navigationviewlist"></a>[navigationviewlist](/uwp/api/windows.ui.xaml.controls.navigationviewlist)

navigationviewlist <br> navigationviewlist.navigationviewlist

#### <a name="navigationviewselectionchangedeventargs"></a>[navigationviewselectionchangedeventargs](/uwp/api/windows.ui.xaml.controls.navigationviewselectionchangedeventargs)

navigationviewselectionchangedeventargs <br> navigationviewselectionchangedeventargs.issettingsselected <br> navigationviewselectionchangedeventargs.selecteditem

#### <a name="parallaxsourceoffsetkind"></a>[parallaxsourceoffsetkind](/uwp/api/windows.ui.xaml.controls.parallaxsourceoffsetkind)

parallaxsourceoffsetkind

#### <a name="parallaxview"></a>[parallaxview](/uwp/api/windows.ui.xaml.controls.parallaxview)

parallaxview <br> parallaxview.child <br> parallaxview.childproperty <br> parallaxview.horizontalshift <br> parallaxview.horizontalshiftproperty <br> parallaxview.horizontalsourceendoffset <br> parallaxview.horizontalsourceendoffsetproperty <br> parallaxview.horizontalsourceoffsetkind <br> parallaxview.horizontalsourceoffsetkindproperty <br> parallaxview.horizontalsourcestartoffset <br> parallaxview.horizontalsourcestartoffsetproperty <br> parallaxview.ishorizontalshiftclamped <br> parallaxview.ishorizontalshiftclampedproperty <br> parallaxview.isverticalshiftclamped <br> parallaxview.isverticalshiftclampedproperty <br> parallaxview.maxhorizontalshiftratio <br> parallaxview.maxhorizontalshiftratioproperty <br> parallaxview.maxverticalshiftratio <br> parallaxview.maxverticalshiftratioproperty <br> parallaxview.parallaxview <br> parallaxview.refreshautomatichorizontaloffsets <br> parallaxview.refreshautomaticverticaloffsets <br> parallaxview.source <br> parallaxview.sourceproperty <br> parallaxview.verticalshift <br> parallaxview.verticalshiftproperty <br> parallaxview.verticalsourceendoffset <br> parallaxview.verticalsourceendoffsetproperty <br> parallaxview.verticalsourceoffsetkind <br> parallaxview.verticalsourceoffsetkindproperty <br> parallaxview.verticalsourcestartoffset <br> parallaxview.verticalsourcestartoffsetproperty

#### <a name="passwordbox"></a>[passwordbox](/uwp/api/windows.ui.xaml.controls.passwordbox)

passwordbox.passwordchanging

#### <a name="passwordboxpasswordchangingeventargs"></a>[passwordboxpasswordchangingeventargs](/uwp/api/windows.ui.xaml.controls.passwordboxpasswordchangingeventargs)

passwordboxpasswordchangingeventargs <br> passwordboxpasswordchangingeventargs.iscontentchanging

#### <a name="pathiconsource"></a>[pathiconsource](/uwp/api/windows.ui.xaml.controls.pathiconsource)

pathiconsource <br> pathiconsource.data <br> pathiconsource.dataproperty <br> pathiconsource.pathiconsource

#### <a name="personpicture"></a>[personpicture](/uwp/api/windows.ui.xaml.controls.personpicture)

personpicture <br> personpicture.badgeglyph <br> personpicture.badgeglyphproperty <br> personpicture.badgeimagesource <br> personpicture.badgeimagesourceproperty <br> personpicture.badgenumber <br> personpicture.badgenumberproperty <br> personpicture.badgetext <br> personpicture.badgetextproperty <br> personpicture.contact <br> personpicture.contactproperty <br> personpicture.displayname <br> personpicture.displaynameproperty <br> personpicture.initials <br> personpicture.initialsproperty <br> personpicture.isgroup <br> personpicture.isgroupproperty <br> personpicture.personpicture <br> personpicture.prefersmallimage <br> personpicture.prefersmallimageproperty <br> personpicture.profilepicture <br> personpicture.profilepictureproperty

#### <a name="ratingcontrol"></a>[ratingcontrol](/uwp/api/windows.ui.xaml.controls.ratingcontrol)

ratingcontrol <br> ratingcontrol.caption <br> ratingcontrol.captionproperty <br> ratingcontrol.initialsetvalue <br> ratingcontrol.initialsetvalueproperty <br> ratingcontrol.isclearenabled <br> ratingcontrol.isclearenabledproperty <br> ratingcontrol.isreadonly <br> ratingcontrol.isreadonlyproperty <br> ratingcontrol.iteminfo <br> ratingcontrol.iteminfoproperty <br> ratingcontrol.maxrating <br> ratingcontrol.maxratingproperty <br> ratingcontrol.placeholdervalue <br> ratingcontrol.placeholdervalueproperty <br> ratingcontrol.ratingcontrol <br> ratingcontrol.value <br> ratingcontrol.valuechanged <br> ratingcontrol.valueproperty

#### <a name="ratingitemfontinfo"></a>[ratingitemfontinfo](/uwp/api/windows.ui.xaml.controls.ratingitemfontinfo)

ratingitemfontinfo <br> ratingitemfontinfo.disabledglyph <br> ratingitemfontinfo.disabledglyphproperty <br> ratingitemfontinfo.glyph <br> ratingitemfontinfo.glyphproperty <br> ratingitemfontinfo.placeholderglyph <br> ratingitemfontinfo.placeholderglyphproperty <br> ratingitemfontinfo.pointeroverglyph <br> ratingitemfontinfo.pointeroverglyphproperty <br> ratingitemfontinfo.pointeroverplaceholderglyph <br> ratingitemfontinfo.pointeroverplaceholderglyphproperty <br> ratingitemfontinfo.ratingitemfontinfo <br> ratingitemfontinfo.unsetglyph <br> ratingitemfontinfo.unsetglyphproperty

#### <a name="ratingitemimageinfo"></a>[ratingitemimageinfo](/uwp/api/windows.ui.xaml.controls.ratingitemimageinfo)

ratingitemimageinfo <br> ratingitemimageinfo.disabledimage <br> ratingitemimageinfo.disabledimageproperty <br> ratingitemimageinfo.image <br> ratingitemimageinfo.imageproperty <br> ratingitemimageinfo.placeholderimage <br> ratingitemimageinfo.placeholderimageproperty <br> ratingitemimageinfo.pointeroverimage <br> ratingitemimageinfo.pointeroverimageproperty <br> ratingitemimageinfo.pointeroverplaceholderimage <br> ratingitemimageinfo.pointeroverplaceholderimageproperty <br> ratingitemimageinfo.ratingitemimageinfo <br> ratingitemimageinfo.unsetimage <br> ratingitemimageinfo.unsetimageproperty

#### <a name="ratingiteminfo"></a>[ratingiteminfo](/uwp/api/windows.ui.xaml.controls.ratingiteminfo)

ratingiteminfo <br> ratingiteminfo.ratingiteminfo

#### <a name="richeditbox"></a>[richeditbox](/uwp/api/windows.ui.xaml.controls.richeditbox)

richeditbox.charactercasing <br> richeditbox.charactercasingproperty <br> richeditbox.copyingtoclipboard <br> richeditbox.cuttingtoclipboard <br> richeditbox.disabledformattingaccelerators <br> richeditbox.disabledformattingacceleratorsproperty <br> richeditbox.horizontaltextalignment <br> richeditbox.horizontaltextalignmentproperty

#### <a name="richtextblock"></a>[richtextblock](/uwp/api/windows.ui.xaml.controls.richtextblock)

richtextblock.horizontaltextalignment <br> richtextblock.horizontaltextalignmentproperty <br> richtextblock.istexttrimmed <br> richtextblock.istexttrimmedchanged <br> richtextblock.istexttrimmedproperty <br> richtextblock.texthighlighters

#### <a name="richtextblockoverflow"></a>[richtextblockoverflow](/uwp/api/windows.ui.xaml.controls.richtextblockoverflow)

richtextblockoverflow.istexttrimmed <br> richtextblockoverflow.istexttrimmedchanged <br> richtextblockoverflow.istexttrimmedproperty

#### <a name="splitview"></a>[splitview](/uwp/api/windows.ui.xaml.controls.splitview)

splitview.paneopened <br> splitview.paneopening

#### <a name="stackpanel"></a>[stackpanel](/uwp/api/windows.ui.xaml.controls.stackpanel)

stackpanel.spacing <br> stackpanel.spacingproperty

#### <a name="swipebehavioroninvoked"></a>[swipebehavioroninvoked](/uwp/api/windows.ui.xaml.controls.swipebehavioroninvoked)

swipebehavioroninvoked

#### <a name="swipecontrol"></a>[swipecontrol](/uwp/api/windows.ui.xaml.controls.swipecontrol)

swipecontrol <br> swipecontrol.bottomitems <br> swipecontrol.bottomitemsproperty <br> swipecontrol.close <br> swipecontrol.leftitems <br> swipecontrol.leftitemsproperty <br> swipecontrol.rightitems <br> swipecontrol.rightitemsproperty <br> swipecontrol.swipecontrol <br> swipecontrol.topitems <br> swipecontrol.topitemsproperty

#### <a name="swipeitem"></a>[swipeitem](/uwp/api/windows.ui.xaml.controls.swipeitem)

swipeitem <br> swipeitem.background <br> swipeitem.backgroundproperty <br> swipeitem.behavioroninvoked <br> swipeitem.behavioroninvokedproperty <br> swipeitem.command <br> swipeitem.commandparameter <br> swipeitem.commandparameterproperty <br> swipeitem.commandproperty <br> swipeitem.foreground <br> swipeitem.foregroundproperty <br> swipeitem.iconsource <br> swipeitem.iconsourceproperty <br> swipeitem.invoked <br> swipeitem.swipeitem <br> swipeitem.text <br> swipeitem.textproperty

#### <a name="swipeiteminvokedeventargs"></a>[swipeiteminvokedeventargs](/uwp/api/windows.ui.xaml.controls.swipeiteminvokedeventargs)

swipeiteminvokedeventargs <br> swipeiteminvokedeventargs.swipecontrol

#### <a name="swipeitems"></a>[swipeitems](/uwp/api/windows.ui.xaml.controls.swipeitems)

swipeitems <br> swipeitems.append <br> swipeitems.clear <br> swipeitems.first <br> swipeitems.getat <br> swipeitems.getmany <br> swipeitems.getview <br> swipeitems.indexof <br> swipeitems.insertat <br> swipeitems.mode <br> swipeitems.modeproperty <br> swipeitems.removeat <br> swipeitems.removeatend <br> swipeitems.replaceall <br> swipeitems.setat <br> swipeitems.size <br> swipeitems.swipeitems

#### <a name="swipemode"></a>[swipemode](/uwp/api/windows.ui.xaml.controls.swipemode)

swipemode

#### <a name="symboliconsource"></a>[symboliconsource](/uwp/api/windows.ui.xaml.controls.symboliconsource)

symboliconsource <br> symboliconsource.symbol <br> symboliconsource.symboliconsource <br> symboliconsource.symbolproperty

#### <a name="textblock"></a>[textblock](/uwp/api/windows.ui.xaml.controls.textblock)

textblock.horizontaltextalignment <br> textblock.horizontaltextalignmentproperty <br> textblock.istexttrimmed <br> textblock.istexttrimmedchanged <br> textblock.istexttrimmedproperty <br> textblock.texthighlighters

#### <a name="textbox"></a>[textbox](/uwp/api/windows.ui.xaml.controls.textbox)

textbox.beforetextchanging <br> textbox.charactercasing <br> textbox.charactercasingproperty <br> textbox.copyingtoclipboard <br> textbox.cuttingtoclipboard <br> textbox.horizontaltextalignment <br> textbox.horizontaltextalignmentproperty <br> textbox.placeholderforeground <br> textbox.placeholderforegroundproperty

#### <a name="textboxbeforetextchangingeventargs"></a>[textboxbeforetextchangingeventargs](/uwp/api/windows.ui.xaml.controls.textboxbeforetextchangingeventargs)

textboxbeforetextchangingeventargs <br> textboxbeforetextchangingeventargs.cancel <br> textboxbeforetextchangingeventargs.newtext

#### <a name="textcontrolcopyingtoclipboardeventargs"></a>[textcontrolcopyingtoclipboardeventargs](/uwp/api/windows.ui.xaml.controls.textcontrolcopyingtoclipboardeventargs)

textcontrolcopyingtoclipboardeventargs <br> textcontrolcopyingtoclipboardeventargs.handled

#### <a name="textcontrolcuttingtoclipboardeventargs"></a>[textcontrolcuttingtoclipboardeventargs](/uwp/api/windows.ui.xaml.controls.textcontrolcuttingtoclipboardeventargs)

textcontrolcuttingtoclipboardeventargs <br> textcontrolcuttingtoclipboardeventargs.handled

### <a name="windowsuixamldocuments"></a>[windows.ui.xaml.documents](/uwp/api/windows.ui.xaml.documents)

#### <a name="block"></a>[block](/uwp/api/windows.ui.xaml.documents.block)

block.horizontaltextalignment <br> block.horizontaltextalignmentproperty

#### <a name="hyperlink"></a>[hyperlink](/uwp/api/windows.ui.xaml.documents.hyperlink)

hyperlink.istabstop <br> hyperlink.istabstopproperty <br> hyperlink.tabindex <br> hyperlink.tabindexproperty

#### <a name="texthighlighter"></a>[texthighlighter](/uwp/api/windows.ui.xaml.documents.texthighlighter)

texthighlighter <br> texthighlighter.background <br> texthighlighter.backgroundproperty <br> texthighlighter.foreground <br> texthighlighter.foregroundproperty <br> texthighlighter.ranges <br> texthighlighter.texthighlighter

#### <a name="texthighlighterbase"></a>[texthighlighterbase](/uwp/api/windows.ui.xaml.documents.texthighlighterbase)

texthighlighterbase

#### <a name="textrange"></a>[textrange](/uwp/api/windows.ui.xaml.documents.textrange)

textrange

### <a name="windowsuixamlhosting"></a>[windows.ui.xaml.hosting](/uwp/api/windows.ui.xaml.hosting)

#### <a name="designerappmanager"></a>[designerappmanager](/uwp/api/windows.ui.xaml.hosting.designerappmanager)

designerappmanager <br> designerappmanager.appusermodelid <br> designerappmanager.close <br> designerappmanager.createnewviewasync <br> designerappmanager.designerappexited <br> designerappmanager.designerappmanager <br> designerappmanager.loadobjectintoappasync

#### <a name="designerappview"></a>[designerappview](/uwp/api/windows.ui.xaml.hosting.designerappview)

designerappview <br> designerappview.applicationviewid <br> designerappview.appusermodelid <br> designerappview.close <br> designerappview.updateviewasync <br> designerappview.viewsize <br> designerappview.viewstate

#### <a name="designerappviewstate"></a>[designerappviewstate](/uwp/api/windows.ui.xaml.hosting.designerappviewstate)

designerappviewstate

### <a name="windowsuixamlinput"></a>[windows.ui.xaml.input](/uwp/api/windows.ui.xaml.input)

#### <a name="characterreceivedroutedeventargs"></a>[characterreceivedroutedeventargs](/uwp/api/windows.ui.xaml.input.characterreceivedroutedeventargs)

characterreceivedroutedeventargs <br> characterreceivedroutedeventargs.character <br> characterreceivedroutedeventargs.handled <br> characterreceivedroutedeventargs.keystatus

#### <a name="keyboardaccelerator"></a>[keyboardaccelerator](/uwp/api/windows.ui.xaml.input.keyboardaccelerator)

keyboardaccelerator <br> keyboardaccelerator.invoked <br> keyboardaccelerator.isenabled <br> keyboardaccelerator.isenabledproperty <br> keyboardaccelerator.key <br> keyboardaccelerator.keyboardaccelerator <br> keyboardaccelerator.keyproperty <br> keyboardaccelerator.modifiers <br> keyboardaccelerator.modifiersproperty <br> keyboardaccelerator.scopeowner <br> keyboardaccelerator.scopeownerproperty

#### <a name="keyboardacceleratorinvokedeventargs"></a>[keyboardacceleratorinvokedeventargs](/uwp/api/windows.ui.xaml.input.keyboardacceleratorinvokedeventargs)

keyboardacceleratorinvokedeventargs <br> keyboardacceleratorinvokedeventargs.element <br> keyboardacceleratorinvokedeventargs.handled

#### <a name="pointerroutedeventargs"></a>[pointerroutedeventargs](/uwp/api/windows.ui.xaml.input.pointerroutedeventargs)

pointerroutedeventargs.isgenerated

#### <a name="processkeyboardacceleratoreventargs"></a>[processkeyboardacceleratoreventargs](/uwp/api/windows.ui.xaml.input.processkeyboardacceleratoreventargs)

processkeyboardacceleratoreventargs <br> processkeyboardacceleratoreventargs.handled <br> processkeyboardacceleratoreventargs.key <br> processkeyboardacceleratoreventargs.modifiers

### <a name="windowsuixamlmarkup"></a>[windows.ui.xaml.markup](/uwp/api/windows.ui.xaml.markup)

#### <a name="markupextension"></a>[markupextension](/uwp/api/windows.ui.xaml.markup.markupextension)

markupextension <br> markupextension.markupextension <br> markupextension.providevalue

#### <a name="markupextensionreturntypeattribute"></a>[markupextensionreturntypeattribute](/uwp/api/windows.ui.xaml.markup.markupextensionreturntypeattribute)

markupextensionreturntypeattribute <br> markupextensionreturntypeattribute.markupextensionreturntypeattribute

### <a name="windowsuixamlmedia"></a>[windows.ui.xaml.media](/uwp/api/windows.ui.xaml.media)

#### <a name="acrylicbackgroundsource"></a>[acrylicbackgroundsource](/uwp/api/windows.ui.xaml.media.acrylicbackgroundsource)

acrylicbackgroundsource

#### <a name="acrylicbrush"></a>[acrylicbrush](/uwp/api/windows.ui.xaml.media.acrylicbrush)

acrylicbrush <br> acrylicbrush.acrylicbrush <br> acrylicbrush.alwaysusefallback <br> acrylicbrush.alwaysusefallbackproperty <br> acrylicbrush.backgroundsource <br> acrylicbrush.backgroundsourceproperty <br> acrylicbrush.tintcolor <br> acrylicbrush.tintcolorproperty <br> acrylicbrush.tintopacity <br> acrylicbrush.tintopacityproperty <br> acrylicbrush.tinttransitionduration <br> acrylicbrush.tinttransitiondurationproperty

#### <a name="revealbackgroundbrush"></a>[revealbackgroundbrush](/uwp/api/windows.ui.xaml.media.revealbackgroundbrush)

revealbackgroundbrush <br> revealbackgroundbrush.revealbackgroundbrush

#### <a name="revealborderbrush"></a>[revealborderbrush](/uwp/api/windows.ui.xaml.media.revealborderbrush)

revealborderbrush <br> revealborderbrush.revealborderbrush

#### <a name="revealbrush"></a>[revealbrush](/uwp/api/windows.ui.xaml.media.revealbrush)

revealbrush <br> revealbrush.alwaysusefallback <br> revealbrush.alwaysusefallbackproperty <br> revealbrush.color <br> revealbrush.colorproperty <br> revealbrush.getstate <br> revealbrush.revealbrush <br> revealbrush.setstate <br> revealbrush.stateproperty <br> revealbrush.targettheme <br> revealbrush.targetthemeproperty

#### <a name="revealbrushstate"></a>[revealbrushstate](/uwp/api/windows.ui.xaml.media.revealbrushstate)

revealbrushstate

### <a name="windowsuixaml"></a>[windows.ui.xaml](/uwp/api/windows.ui.xaml)

#### <a name="frameworkelement"></a>[frameworkelement](/uwp/api/windows.ui.xaml.frameworkelement)

frameworkelement.actualtheme <br> frameworkelement.actualthemechanged <br> frameworkelement.actualthemeproperty

#### <a name="uielement"></a>[uielement](/uwp/api/windows.ui.xaml.uielement)

uielement.characterreceived <br> uielement.characterreceivedevent <br> uielement.getchildrenintabfocusorder <br> uielement.keyboardaccelerators <br> uielement.onprocesskeyboardaccelerators <br> uielement.previewkeydown <br> uielement.previewkeydownevent <br> uielement.previewkeyup <br> uielement.previewkeyupevent <br> uielement.processkeyboardaccelerators <br> uielement.tryinvokekeyboardaccelerator