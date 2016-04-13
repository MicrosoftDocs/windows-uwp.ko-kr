---
ms.assetid: 25B18BA5-E584-4537-9F19-BB2C8C52DFE1
title: 앱 접근 권한 값 선언
description: 사진, 음악 또는 디바이스(예: 카메라 또는 마이크)와 같은 특정 리소스 및 API에 액세스하려면 UWP(유니버설 Windows 플랫폼) 앱의 패키지 매니페스트에서 접근 권한 값을 선언해야 합니다.
---
# 앱 접근 권한 값 선언

\[ Windows 10의 UWP 앱에 맞게 업데이트되었습니다. Windows 8.x 문서는 [보관](http://go.microsoft.com/fwlink/p/?linkid=619132)을 참조하세요. \]

사진, 음악 또는 장치(예: 카메라 또는 마이크)와 같은 특정 리소스 및 API에 액세스하려면 UWP(유니버설 Windows 플랫폼) 앱의 [패키지 매니페스트](https://msdn.microsoft.com/library/windows/apps/BR211474)에서 접근 권한 값을 선언해야 합니다.

앱의 [패키지 매니페스트](https://msdn.microsoft.com/library/windows/apps/BR211474)에서 접근 권한 값을 선언하여 특정 리소스 또는 API에 대한 액세스를 요청합니다. Microsoft Visual Studio에서 [매니페스트 디자이너](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/br230259.aspx)를 사용하여 일반 접근 권한 값을 선언하거나 수동으로 추가할 수 있습니다. 자세한 내용은 [패키지 매니페스트에 접근 권한 값을 지정하는 방법](https://msdn.microsoft.com/library/windows/apps/BR211477)을 참조하세요. 고객은 스토어에서 앱 구입 시 앱에서 선언한 모든 접근 권한 값에 관한 알림을 받아야 합니다. 앱에 필요하지 않은 접근 권한 값은 선언하지 마세요.

일부 접근 권한 값은 앱에 *중요한 리소스*에 대한 액세스 권한을 제공합니다. 이러한 리소스는 사용자의 개인 데이터에 액세스하거나 사용자의 비용이 들 수 있으므로 중요한 것으로 간주됩니다. 설정 앱에서 관리되는 개인 정보 설정을 통해 사용자는 중요한 리소스에 대한 액세스를 동적으로 제어할 수 있습니다. 따라서 앱에서는 중요한 리소스를 항상 사용 가능한 것으로 가정하지 않는 것이 중요합니다. 중요한 리소스에 액세스하는 방법에 대한 자세한 내용은 [개인정보 인식 앱에 대한 지침](https://msdn.microsoft.com/library/windows/apps/Hh768223)을 참조하세요. 앱에 *중요한 리소스*에 대한 액세스 권한을 제공하는 기능은 기능 시나리오 옆에 별표(\*)로 주석 처리됩니다.

이 문서에서는 아래에 설명된 4가지 접근 권한 값 범주를 검토합니다.

-   대부분의 공통적인 앱 시나리오에 적용되는 범용 접근 권한 값입니다.

-   앱이 주변 장치 및 내부 장치에 액세스할 수 있게 하는 장치 기능입니다.

-   스토어에 제출하기 위한 특별 회사 계정이 있어야 사용할 수 있는 특수 용도 접근 권한 값입니다. 회사 계정에 대한 자세한 내용은 [계정 유형, 위치 및 수수료](https://msdn.microsoft.com/library/windows/apps/JJ863494)를 참조하세요.

-   Microsoft 및 해당 파트너만 사용할 수 있는 제한된 접근 권한 값입니다.

## 범용 접근 권한 값

범용 접근 권한 값은 대부분의 앱 시나리오에 적용됩니다.

<table>
        <thead>
            <tr>
                <th>접근 권한 값 시나리오</th>
                <th>접근 권한 값 사용</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>**음악***</td>
                <td>
                    The **musicLibrary** capability provides programmatic access to the user's Music, allowing the app to enumerate and access all files in the library without user interaction. This capability is typically used in jukebox apps that make use of the entire Music library.

                    The [**file picker**](https://msdn.microsoft.com/library/windows/apps/BR207847) provides a robust UI mechanism that lets users open files for use with an app. Declare the **musicLibrary** capability only when the scenarios for your app require programmatic access and can't be realized by using the **file picker**.

                    The **musicLibrary** capability must include the **uap** namespace when you declare it in your app's package manifest as shown below.
                    <table>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
                                    <pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="musicLibrary"/&gt;
&lt;/기능&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**사진***</td>
                <td>
                    **picturesLibrary** 접근 권한 값을 사용하면 사용자의 사진에 프로그래밍 방식으로 액세스할 수 있으므로 앱이 사용자 조작 없이 라이브러리의 모든 파일을 열거하고 액세스할 수 있습니다. 이 접근 권한 값은 주로 전체 사진 라이브러리를 사용해야 하는 사진 앱에서 사용됩니다.

                    The [**file picker**](https://msdn.microsoft.com/library/windows/apps/BR207847) provides a robust UI mechanism that lets users open files for use with an app. Declare the **picturesLibrary** capability only when the scenarios for your app require programmatic access and can't be realized them by using the **file picker**.

                    The **picturesLibrary** capability must include the **uap** namespace when you declare it in your app's package manifest as shown below.
                    <table>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
<pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="picturesLibrary"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**동영상***</td>
                <td>
                    The **videosLibrary** capability provides programmatic access to the user's Videos, allowing the app to enumerate and access all files in the library without user interaction. This capability is typically used in movie-playback apps that make use of the entire Videos library.

                    The [**file picker**](https://msdn.microsoft.com/library/windows/apps/BR207847) provides a robust UI mechanism that lets users open files for use with an app. Declare the **videosLibrary** capability only when the scenarios for your app require programmatic access and can't be realized by using the **file picker**.

                    The **videosLibrary** capability must include the **uap** namespace when you declare it in your app's package manifest as shown below.
                    <table>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
<pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="videosLibrary"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**이동식 저장소**</td>
                <td>
                    The **removableStorage** capability provides programmatic access to files on removable storage, like USB keys and external hard drives, filtered to the file-type associations declared in the package manifest. For example, if a document-reader app declares a .doc file-type association, it can open .doc files on the removable storage device, but not other types of files. Be careful when you declare this capability, because users may include a variety of info in their removable storage devices, and will expect your app to provide a valid justification for programmatic access to the removable storage for all files of the declared type.

                    Users will expect your app to handle any file associations that you declare. So don't declare file associations that your app cannot handle responsibly. The [**file picker**](https://msdn.microsoft.com/library/windows/apps/BR207847) provides a robust UI mechanism that lets users open files for use with an app.

                    Declare the **removableStorage** capability only when the scenarios for your app require programmatic access and can't be realized by using the [**file picker**](https://msdn.microsoft.com/library/windows/apps/BR207847).

                    The **removableStorage** capability must include the **uap** namespace when you declare it in your app's package manifest as shown below.
                    <table>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
<pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="removableStorage"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**인터넷 및 공용 네트워크***</td>
                <td>
                    There are two capabilities that provide different levels of access to the Internet and public networks.

                    The **internetClient** capability indicates that apps can receive incoming data from the Internet. Cannot act as a server. No local network access.

                    The **internetClientServer** capability indicates that apps can receive incoming data from the Internet. Can act as a server. No local network access.

                    Most apps that have a web service component will use **internetClient**. Apps that enable peer-to-peer (P2P) scenarios where the app needs to listen for incoming network connections should use **internetClientServer**. The **internetClientServer** capability includes the access that the **internetClient** capability provides, so you don't need to specify **internetClient** when you specify **internetClientServer**.
                </td>
            </tr>
            <tr>
                <td>**Homes and work networks***</td>
                <td>
                    The **privateNetworkClientServer** capability provides inbound and outbound access to home and work networks through the firewall. This capability is typically used for games that communicate across the local area network (LAN), and for apps that share data across a variety of local devices. If your app specifies **musicLibrary**, **picturesLibrary**, or **videosLibrary**, you don't need to use this capability to access the corresponding library in a Home Group. On Windows, this capability does not provide access to the Internet.
                </td>
            </tr>
            <tr>
                <td>**Appointments**</td>
                <td>
                    The **appointments** capability provides access to the user’s appointment store. This capability allows read access to appointments obtained from the synced network accounts and to other apps that write to the appointment store. With this capability, your app can create new calendars and write appointments to calendars that it creates.

                    The **appointments** capability must include the **uap** namespace when you declare it in your app's package manifest as shown below.
                    <table>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
<pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="appointments"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**연락처***</td>
                <td>
                    The **contacts** capability provides access to the aggregated view of the contacts from various contacts stores. This capability gives the app limited access (network permitting rules apply) to contacts that were synced from various networks and the local contact store.

                    The **contacts** capability must include the **uap** namespace when you declare it in your app's package manifest as shown below.
                    <table>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
<pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="contacts"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**코드 생성**</td>
                <td>
                    The **codeGeneration** capability allows apps to access the following functions which provide JIT capabilities to apps.

                    - [**VirtualProtectFromApp**](https://msdn.microsoft.com/library/windows/desktop/Mt169846)
                    - [**CreateFileMappingFromApp**](https://msdn.microsoft.com/library/windows/desktop/Hh994453)
                    - [**OpenFileMappingFromApp**](https://msdn.microsoft.com/library/windows/desktop/Mt169844)
                    - [**MapViewOfFileFromApp**](https://msdn.microsoft.com/library/windows/desktop/Hh994454)
                </td>
            </tr>
            <tr>
                <td>**AllJoyn**</td>
                <td>
                    The **allJoyn** capability allows AllJoyn-enabled apps and devices on a network to discover and interact with each other.

                    All apps that access APIs in the [**Windows.Devices.AllJoyn**](https://msdn.microsoft.com/library/windows/apps/Dn894971) namespace must use this capability.
                </td>
            </tr>
            <tr>
                <td>**Phone calls**</td>
                <td>
                    The **phoneCall** capability allows apps to access all of the phone lines on the device and perform the following functions.

                    - Place a call on the phone line and show the system dialer without prompting the user.
                    - Access line-related metadata.
                    - Access line-related triggers.
                    - Allows the user-selected spam filter app to set and check block list and call origin information.

                    The **phoneCall** capability must include the **uap** namespace when you declare it in your app's package manifest as shown below.
                    <table>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
<pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="phoneCall"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                    The **phoneCallHistoryPublic** capability allows apps to read cellular and some VOIP call history information on the device. This capability also allows the app to write VOIP call history entries. This capability is required to access all members of the [**PhoneCallHistoryStore**](https://msdn.microsoft.com/library/windows/apps/Dn705931) class.
                </td>
            </tr>
            <tr>
                <td>**녹음된 통화 폴더***</td>
                <td>
                    <p>**recordedCallsFolder** 장치 접근 권한 값을 통해 앱은 녹음된 통화 폴더에 액세스할 수 있습니다.</p>
                    <p>**recordedCallsFolder** 접근 권한 값은 아래 표시된 대로 앱의 패키지 매니페스트에서 선언할 때 **mobile** 네임스페이스를 포함해야 합니다.</p>
                    <table>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
                                    <pre><code>&lt;Capabilities&gt;
    &lt;mobile:Capability Name="recordedCallsFolder"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**사용자 계정 정보***</td>
                <td>
                    <p>**userAccountInformation** 접근 권한 값을 통해 앱은 사용자의 이름 및 사진에 액세스할 수 있습니다.</p>
                    <p>이 접근 권한 값은 Windows.System.User 네임스페이스의 일부 API에 액세스하는 데 필요합니다.</p>
                    <p>**userAccountInformation** 접근 권한 값은 아래 표시된 대로 앱의 패키지 매니페스트에서 선언할 때 **uap** 네임스페이스를 포함해야 합니다.</p>
                    <table>
                        <colgroup>
                            <col width="100%" />
                        </colgroup>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
                                    <pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="userAccountInformation"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**VOIP 호출**</td>
                <td>
                    <p>**voipCall** 접근 권한 값을 통해 앱은 [**Windows.ApplicationModel.Calls**](https://msdn.microsoft.com/library/windows/apps/Dn297266) 네임스페이스의 VOIP 호출 API에 액세스할 수 있습니다.</p>
                    <p>**voipCall** 접근 권한 값은 아래 표시된 대로 앱의 패키지 매니페스트에서 선언할 때 **uap** 네임스페이스를 포함해야 합니다.</p>
                    <table>
                        <colgroup>
                            <col width="100%" />
                        </colgroup>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
                                    <pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="voipCall"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**3D 개체**</td>
                <td>
                    <p>앱에서는 **objects3D** 접근 권한 값을 통해 3D 개체 파일에 프로그래밍 방식으로 액세스할 수 있습니다. 이 접근 권한 값은 주로 전체 3D 개체 라이브러리에 액세스해야 하는 3D 앱과 게임에서 사용됩니다.</p>
                    <p>이 접근 권한 값은 [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/BR227346) 네임스페이스에서 API를 사용하여 3D 개체를 포함하는 폴더에 액세스하는 데 필요합니다.</p>
                    <p>**objects3D** 접근 권한 값은 아래 표시된 대로 앱의 패키지 매니페스트에서 선언할 때 **uap** 네임스페이스를 포함해야 합니다.</p>
                    <table>
                        <colgroup>
                            <col width="100%" />
                        </colgroup>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
                                    <pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="objects3d"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**읽기 차단된 메시지***</td>
                <td>
                    <p>**blockedChatMessages**접근 권한 값을 통해 앱은 스팸 필터 앱에 의해 차단된 SMS 및 MMS 메시지를 읽을 수 있습니다.</p>
                    <p>이 접근 권한 값은 [**Windows.ApplicationModel.Chat**](https://msdn.microsoft.com/library/windows/apps/Dn642321) 네임스페이스에서 API를 사용하여 차단된 메시지에 액세스하는 데 필요합니다.</p>
                    <p>**blockedChatMessages** 접근 권한 값은 아래 표시된 대로 앱의 패키지 매니페스트에서 선언할 때 **uap** 네임스페이스를 포함해야 합니다.</p>
                    <table>
                        <colgroup>
                            <col width="100%" />
                        </colgroup>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
                                    <pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="blockedChatMessages"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**채팅 메시지 액세스**</td>
                <td>
                    <p>**chat** 접근 권한 값을 통해 앱은 문자 메시지를 읽고 삭제할 수 있습니다. 이 접근 권한 값을 통해 앱은 시스템 데이터 저장소의 채팅 메시지를 저장할 수 있습니다.</p>
                    <p>이 접근 권한 값은 [**Windows.ApplicationModel.Chat**](https://msdn.microsoft.com/library/windows/apps/Dn642321) 네임스페이스의 일부 API를 사용하는 데 필요합니다.</p>
                    <p>**chat** 접근 권한 값은 아래 표시된 대로 앱의 패키지 매니페스트에서 선언할 때 **uap** 네임스페이스를 포함해야 합니다.</p>
                    <table>
                        <colgroup>
                            <col width="100%" />
                        </colgroup>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
                                    <pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="chat"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**IoT 하위 수준의 버스 하드웨어**</td>
                <td>
                    <p>**lowLevelDevices** 접근 권한 값을 통해 IoT 디바이스에서 실행되는 앱은 GPIO, I2C, SPI, ADC 및 PWM 같은 낮은 수준의 버스 하드웨어에 액세스할 수 있습니다.</p>
                    <p>이 기능은 [**Windows.Devices.Spi**](https://msdn.microsoft.com/library/windows/apps/Dn708178) 네임스페이스의 일부 API에 액세스하는 데 필요합니다.</p>
                    <p>**lowLevelDevices** 접근 권한 값은 아래 표시된 대로 앱의 패키지 매니페스트에서 선언할 때 **iot** 네임스페이스를 포함해야 합니다.</p>
                    <table>
                        <colgroup>
                            <col width="100%" />
                        </colgroup>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
                                    <pre><code>&lt;Capabilities&gt;
    &lt;iot:Capability Name="lowLevelDevices"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**IoT 시스템 관리**</td>
                <td>
                    <p>**systemManagement** 접근 권한 값을 통해 앱은 종료 또는 다시 부팅, 로캘 및 표준 시간대와 같은 기본 시스템 관리 권한을 가질 수 있습니다.</p>
                    <p>[
            **Windows.System**](https://msdn.microsoft.com/library/windows/apps/BR241814) 네임스페이스에서 일부 API에 액세스하려면 이 접근 권한 값이 필요합니다.</p>
                    <p>**systemManagement** 접근 권한 값은 아래 표시된 대로 앱의 패키지 매니페스트에서 선언할 때 **iot** 네임스페이스를 포함해야 합니다.</p>
                    <table>
                        <colgroup>
                            <col width="100%" />
                        </colgroup>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
                                    <pre><code>&lt;Capabilities&gt;
    &lt;iot:Capability Name="systemManagement"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
        </tbody>
</table>

 
## 장치 접근 권한 값

장치 접근 권한 값은 앱이 주변 장치 및 내부 장치에 액세스할 수 있게 합니다. 장치 접근 권한 값은 앱 패키지 매니페스트의 **DeviceCapability** 요소를 사용하여 지정합니다. 이 요소에는 추가 자식 요소가 필요할 수 있으며 일부 장치 접근 권한 값을 패키지 매니페스트에 수동으로 추가해야 합니다. 자세한 내용은 [패키지 매니페스트에서 장치 접근 권한 값을 지정하는 방법](https://msdn.microsoft.com/library/windows/apps/Dn263092) 및 [**DeviceCapability Schema reference**](https://msdn.microsoft.com/library/windows/apps/BR211430)를 참조하세요.

| 접근 권한 값 시나리오 | 접근 권한 값 사용 |
|---------------------|------------------|
| **위치**\* | **location** 접근 권한 값을 사용하면 PC에 있는 GPS 센서와 같은 전용 하드웨어에서 검색되거나 사용 가능한 네트워크 정보에서 파생된 위치 기능에 액세스할 수 있습니다. 앱은 사용자가 **설정** 참 메뉴에서 위치 서비스를 사용하지 않도록 설정한 경우를 해결해야 합니다. |
| **마이크** | **microphone** 접근 권한 값을 사용하면 마이크의 오디오 피드에 액세스할 수 있으므로 앱이 연결된 마이크에서 녹음할 수 있습니다. 앱은 사용자가 **설정** 참 메뉴에서 마이크를 사용하지 않도록 설정한 경우를 해결해야 합니다. |
| **근접** | **proximity** 접근 권한 값을 사용하면 근접한 여러 장치가 서로 통신할 수 있습니다. 이 접근 권한 값은 주로 캐주얼 멀티 플레이어 게임 및 정보를 교환하는 앱에서 사용됩니다. 장치에서는 Bluetooth, Wi-Fi, 인터넷을 비롯하여 최적의 가용 연결을 제공하는 통신 기술을 사용하려고 합니다. 이 접근 권한 값은 장치 간에 통신을 시작하는 데만 사용됩니다. |
| **Webcam** | **webcam** 접근 권한 값은 기본 제공 카메라나 외부 webcam의 화상 대화에 대한 액세스 권한을 제공하여 앱에서 사진 및 동영상을 캡처할 수 있도록 합니다. Windows에서 앱은 사용자가 **설정** 참 메뉴에서 카메라를 사용하지 않도록 설정한 경우를 처리해야 합니다.<br/>**webcam** 접근 권한 값은 비디오 스트림에 대한 액세스 권한만 부여합니다. 오디오 스트림에 대한 액세스 권한도 부여하려면 **microphone** 접근 권한 값을 추가해야 합니다. |
| **USB** | **USB** 디바이스 기능은 [USB 디바이스용 앱 매니페스트 패키지 업데이트](http://go.microsoft.com/fwlink/p/?LinkId=302259)에서 API 액세스를 가능하게 합니다. |
| **HID(휴먼 인터페이스 장치)** | **humaninterfacedevice** 디바이스 기능은 [HID용 디바이스 기능을 지정하는 방법](https://msdn.microsoft.com/library/windows/apps/Dn263091)에서 API 액세스가 가능하게 합니다. |
| **POS(서비스 지점)** | **pointOfService** 장치 접근 권한 값은 [**Windows.Devices.PointOfService**](https://msdn.microsoft.com/library/windows/apps/Dn298071) 네임스페이스에서 API 액세스를 가능하게 합니다. 이 네임스페이스는 앱이 POS(서비스 시점) 바코드 스캐너 및 자기 띠 판독기에 액세스할 수 있게 합니다. 이 네임스페이스는 Windows 스토어 앱에서 다양한 제조업체의 POS 장치에 액세스할 수 있도록 하는 공급업체 중립 인터페이스를 제공합니다. |
| **Bluetooth** | **bluetooth** 장치 접근 권한 값은 앱이 GATT(일반 특성) 또는 클래식 기본 속도(RFCOMM) 프로토콜을 통해 이미 연결된 Bluetooth 장치와 통신할 수 있도록 합니다.<br/>이 접근 권한 값은 [**Windows.Devices.Bluetooth**](https://msdn.microsoft.com/library/windows/apps/Dn263413) 네임스페이스의 일부 API를 사용하는 데 필요합니다. |
| **Wi-Fi 네트워킹** | **wiFiControl** 장치 접근 권한 값을 통해 앱은 Wi-Fi 네트워크를 검색하고 Wi-Fi 네트워크에 연결할 수 있습니다.<br/>이 접근 권한 값은 [**Windows.Devices.WiFi**](https://msdn.microsoft.com/library/windows/apps/Dn975224) 네임스페이스의 일부 API를 사용하는 데 필요합니다. |
| **송수신 장치 상태** | **radios** 장치 접근 권한 값을 통해 앱은 Wi-Fi 송수신 장치와 Bluetooth 송수신 장치를 전환할 수 있습니다.<br/>이 접근 권한 값은 [**Windows.Devices.Radios**](https://msdn.microsoft.com/library/windows/apps/Dn996447) 네임스페이스의 API를 사용하는 데 필요합니다.  |
| **광 디스크** | **optical** 장치 기능을 통해 앱은 CD, DVD 및 블루레이와 같은 광 디스크 드라이브의 기능에 액세스할 수 있습니다.<br/>이 접근 권한 값은 [**Windows.Devices.Custom**](https://msdn.microsoft.com/library/windows/apps/Dn263667) 네임스페이스의 일부 API를 사용하는 데 필요합니다. |
| **동작 활동** | **activity** 장치 기능을 통해 앱은 장치의 현재 동작을 감지할 수 있습니다.<br/>이 접근 권한 값은 [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/BR206408) 네임스페이스의 일부 API를 사용하는 데 필요합니다. |

## 특수 및 제한된 접근 권한 값

**중요**  
특수 및 제한된 접근 권한 값은 매우 구체적인 시나리오를 위한 것입니다. 이 접근 권한 값은 사용이 엄격히 제한되며 추가 스토어 등록 정책 및 검토가 적용됩니다.

예를 들어 사용자가 자신의 ID를 확인하는 디지털 인증서와 함께 스마트 카드를 제공하는 2단계 인증을 사용하는 뱅킹과 같이 이러한 접근 권한 값이 필요하고 적합한 경우가 있습니다. 다른 예로는 기본적으로 기업 고객용으로 디자인되고 사용자의 도메인 자격 증명 없이는 액세스할 수 없는 회사 리소스에 액세스해야 하는 앱을 들 수 있습니다.

특수 용도 접근 권한 값을 적용하는 앱을 스토어에 제출하려면 회사 계정이 필요합니다. 그에 반해서, 제한된 접근 권한 값은 스토어에 대한 특별 회사 계정이 필요하지 않으며 개발자가 사용할 수 없습니다. 제한된 접근 권한 값은 Microsoft 및 해당 파트너가 개발한 앱에만 사용할 수 있습니다. 회사 계정에 대한 자세한 내용은 [계정 유형, 위치 및 요금](https://msdn.microsoft.com/library/windows/apps/JJ863494)을 참조하세요.

제한된 모든 접근 권한 값은 앱의 패키지 매니페스트에서 다른 접근 권한 값과 다르게 선언할 경우 **rescap** 네임스페이스를 포함해야 합니다. 다음 예제에서는 **appCaptureSettings** 접근 권한 값을 선언하는 방법을 보여 줍니다.

```xml
<Capabilities>
    <rescap:Capability Name="appCaptureSettings"/>
</Capabilities>
```

아래 나와 있는 것처럼 Package.appxmanifest 파일의 맨 위에 **xmlns:rescap** 네임스페이스 선언도 추가해야 합니다.

```xml
<Package
    xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
    xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
    xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
    xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
    IgnorableNamespaces="uap mp wincap rescap">
```

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">접근 권한 값 시나리오</th>
<th align="left">접근 권한 값 사용</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">**Enterprise**</td>
<td align="left"><p>Windows 도메인 자격 증명은 사용자가 자신의 자격 증명을 사용하여 원격 리소스에 로그인할 수 있도록 하며, 마치 사용자가 사용자 이름과 암호를 제공한 것처럼 작동합니다. **enterpriseAuthentication** 특수 접근 권한 값은 주로 대기업에서 서버에 연결하는 LOB(기간 업무) 앱에 사용됩니다.</p>
<p>인터넷을 통한 일반적인 통신에는 이 기능이 필요하지 않습니다.</p>

<p>**enterpriseAuthentication** 특수 접근 권한 값은 일반 LOB(기간 업무) 앱을 지원하기 위한 것입니다. 회사 리소스에 액세스할 필요가 없는 앱에서는 이 접근 권한 값을 선언하지 마세요. [
            **파일 선택기**](https://msdn.microsoft.com/library/windows/apps/BR207847)는 사용자가 앱에서 사용할 네트워크 공유의 파일을 열 수 있는 강력한 UI 메커니즘을 제공합니다. 앱의 시나리오에서 프로그래밍 방식 액세스가 필요하고 **파일 선택기**를 사용하여 이 시나리오를 실현할 수 없는 경우에만 **enterpriseAuthentication** 특수 접근 권한 값을 선언하세요.</p>
<p>**enterpriseAuthentication** 접근 권한 값은 아래 표시된 대로 앱의 패키지 매니페스트에서 선언할 때 **uap** 네임스페이스를 포함해야 합니다.</p>
<div class="code">
<span codelanguage="XML"></span>
<table>
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">XML</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="enterpriseAuthentication"/&gt;
&lt;/Capabilities&gt;</code></pre></td>
</tr>
</tbody>
</table>
</div>
<p>**enterpriseDataPolicy** 접근 권한 값을 통해 앱은 장치에 대한 엔터프라이즈별 정책을 정의하고 사용할 수 있습니다. 다음 클래스의 모든 구성원을 사용하려면 이 접근 권한 값이 필요합니다.</p>
<ul>
<li>[**FileProtectionManager**](https://msdn.microsoft.com/library/windows/apps/Dn705151)</li>
<li>[**DataProtectionManager**](https://msdn.microsoft.com/library/windows/apps/Dn706017)</li>
<li>[**ProtectionPolicyManager**](https://msdn.microsoft.com/library/windows/apps/Dn705170)</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">**공유 사용자 인증서**</td>
<td align="left"><p>**sharedUserCertificates** 특수 접근 권한 값을 사용하면 앱에서 스마트 카드에 저장된 인증서와 같이 공유 사용자 저장소의 소프트웨어 및 하드웨어 기반 인증서를 추가하고 액세스할 수 있습니다. 이 접근 권한 값은 주로 스마트 카드로 인증하는 금융 또는 엔터프라이즈 앱에 사용됩니다.</p>
<p>**sharedUserCertificates** 접근 권한 값은 아래 표시된 대로 앱의 패키지 매니페스트에서 선언할 때 **uap** 네임스페이스를 포함해야 합니다.</p>
<div class="code">
<span codelanguage="XML"></span>
<table>
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">XML</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="sharedUserCertificates"/&gt;
&lt;/Capabilities&gt;</code></pre></td>
</tr>
</tbody>
</table>
</div></td>
</tr>
<tr class="odd">
<td align="left">**문서***</td>
<td align="left"><p>**documentsLibrary** 특수 접근 권한 값에서는 OneDrive에 대한 오프라인 액세스를 지원하기 위해 패키지 매니페스트에 선언된 파일 형식 연결로 필터링하여 사용자의 문서에 프로그래밍 방식으로 액세스할 수 있게 합니다. 예를 들어 DOC 뷰어 앱이 .doc 파일 형식 연결을 선언한 경우 문서에서 .doc 파일을 열 수 있지만 다른 형식의 파일은 열 수 없습니다.</p>
<p>**documentsLibrary** 특수 접근 권한 값을 선언하는 앱은 홈 그룹 컴퓨터의 문서에 액세스할 수 없습니다. [
            file picker](https://msdn.microsoft.com/library/windows/apps/Hh465174)는 사용자가 앱에서 사용할 파일을 열 수 있는 강력한 UI 메커니즘을 제공합니다. 파일 선택기를 사용할 수 없는 경우에만 **documentsLibrary** 특수 기능을 선언합니다.</p>
<p>**documentsLibrary** 특수 접근 권한 값을 사용하려면 앱에서 다음 작업을 해야 합니다.</p>
<ul>
<li>유효한 OneDrive URL 또는 리소스 ID를 사용하여 플랫폼 간에서 오프라인으로 특정 OneDrive 콘텐츠에 액세스할 수 있도록 합니다.</li>
<li>오프라인에 있는 동안 열려 있는 파일을 사용자의 OneDrive에 자동으로 저장합니다.</li>
</ul>
<p>이러한 두 가지 용도로 **documentsLibrary** 특수 접근 권한 값을 사용하는 앱은 선택적으로 이 접근 권한 값을 사용하여 다른 문서에 포함된 콘텐츠를 열 수도 있습니다. **documentsLibrary** 특수 접근 권한 값은 위와 같은 용도로만 사용할 수 있습니다.</p>
<ul>
<li><p>앱은 휴대폰 내부 저장소의 문서 라이브러리에 액세스할 수 없습니다. 하지만 다른 앱에서 SD 카드(옵션)에 문서 폴더를 만드는 경우 앱은 해당 폴더를 볼 수 있습니다.</p></li>
</ul>
<p>**documentsLibrary** 접근 권한 값은 아래 표시된 대로 앱의 패키지 매니페스트에서 선언할 때 **uap** 네임스페이스를 포함해야 합니다.</p>
<div class="code">
<span codelanguage="XML"></span>
<table>
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">XML</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="documentsLibrary"/&gt;
&lt;/Capabilities&gt;</code></pre></td>
</tr>
</tbody>
</table>
</div></td>
</tr>
<tr class="even">
<td align="left">**게임 DVR 설정**</td>
<td align="left"><p>**appCaptureSettings** 제한된 접근 권한 값을 통해 앱은 게임 DVR에 대한 사용자 설정을 제어할 수 있습니다.</p>
<p>이 접근 권한 값은 [**Windows.Media.Capture**](https://msdn.microsoft.com/library/windows/apps/BR226738) 네임스페이스의 일부 API를 사용하는 데 필요합니다.</p></td>
</tr>
<tr class="odd">
<td align="left">**셀룰러**</td>
<td align="left"><p>**cellularDeviceControl** 제한된 접근 권한 값을 통해 앱은 셀룰러 장치를 제어할 수 있습니다.</p>
<p>**cellularDeviceIdentity** 접근 권한 값을 통해 앱은 셀룰러 ID 데이터에 액세스할 수 있습니다.</p>
<p>**cellularMessaging** 접근 권한 값을 통해 앱은 SMS 및 RCS를 사용할 수 있습니다.</p>
<p>이러한 기능은 [**Windows.Devices.Sms**](https://msdn.microsoft.com/library/windows/apps/BR206567) 네임스페이스의 일부 API를 사용하는 데 필요합니다.</p>
<p>Windows 10에서 시작, [**AppIDList**](https://msdn.microsoft.com/library/windows/apps/Dn393996)를 호출하는 앱).</p></td>
</tr>
<tr class="even">
<td align="left">**장치 잠금 해제**</td>
<td align="left"><p>**deviceUnlock** 제한된 접근 권한 값을 통해 앱은 개발자 및 엔터프라이즈 테스트용 로드 시나리오에 대해 장치의 잠금을 해제할 수 있습니다.</p></td>
</tr>
<tr class="odd">
<td align="left">**듀얼 SIM 타일**</td>
<td align="left"><p>**dualSimTiles** 제한된 접근 권한 값을 통해 앱은 SIM이 여러 개인 장치에서 추가 앱 목록 항목을 만들 수 있습니다.</p>
<p>이 접근 권한 값은 [**Windows.UI.StartScreen**](https://msdn.microsoft.com/library/windows/apps/BR242235) 네임스페이스의 일부 API를 사용하는 데 필요합니다.</p></td>
</tr>
<tr class="even">
<td align="left">**엔터프라이즈 공유 저장소**</td>
<td align="left"><p>**enterpriseDeviceLockdown** 제한된 접근 권한 값을 통해 앱은 장치 잠금 API를 사용하고 엔터프라이즈 공유 저장소 폴더에 액세스할 수 있습니다.</p></td>
</tr>
<tr class="odd">
<td align="left">**시스템 입력 주입**</td>
<td align="left"><p>**inputInjection** 제한된 접근 권한 값을 통해 앱은 HID, 터치, 펜, 키보드 또는 마우스와 같은 다양한 형식의 입력을 프로그래밍 방식으로 시스템에 주입할 수 있습니다. 이 접근 권한 값은 일반적으로 시스템을 제어할 수 있는 협업 앱에 사용됩니다.</p>
<div class="alert">
**참고** PC의 경우 이 기능이 있는 앱의 입력 주입은 동일한 앱 컨테이너의 프로세스에서만 수신됩니다.
</div>
</td>
</tr>
<tr class="even">
<td align="left">**입력 관찰***</td>
<td align="left"><p>**inputObservation** 제한된 접근 권한 값을 통해 앱은 최종 대상과 관계없이 시스템에서 수신되는 HID, 터치, 펜, 키보드 또는 마우스와 같은 다양한 형식의 원시 입력을 관찰할 수 있습니다.</p></td>
</tr>
<tr class="odd">
<td align="left">**입력 표시 안 함**</td>
<td align="left"><p>**inputSuppression** 제한된 접근 권한 값을 통해 앱은 HID, 터치, 펜, 키보드 또는 마우스와 같은 다양한 형식의 원시 입력을 시스템에서 받지 않도록 할 수 있습니다.</p></td>
</tr>
<tr class="even">
<td align="left">**VPN 앱**</td>
<td align="left"><p>**networkingVpnProvider** 제한된 접근 권한 값을 통해 앱은 연결을 관리하고 VPN 플러그 인 기능을 제공하는 기능을 비롯한 VPN 기능에 대한 모든 권한을 가질 수 있습니다.</p>
<p>이 접근 권한 값은 [**Windows.Networking.Vpn**](https://msdn.microsoft.com/library/windows/apps/Dn434040) 네임스페이스의 일부 API를 사용하는 데 필요합니다.</p></td>
</tr>
<tr class="odd">
<td align="left">**다른 앱 관리**</td>
<td align="left"><p>**packageManagement** 제한된 접근 권한 값을 통해 앱은 다른 앱을 직접 관리할 수 있습니다.</p>
<p>**packageQuery** 장치 기능을 통해 앱은 다른 앱에 대한 정보를 수집할 수 있습니다.</p>
<p>이러한 접근 권한 값은 [**PackageManager**](https://msdn.microsoft.com/library/windows/apps/BR240960) 클래스의 일부 메서드 및 속성에 액세스하는 데 필요합니다.</p></td>
</tr>
<tr class="even">
<td align="left">**화면 프로젝션**</td>
<td align="left"><p>**screenDuplication** 제한된 접근 권한 값을 통해 앱은 다른 장치의 화면에 표시할 수 있습니다.</p>
<p>이 접근 권한 값은 DirectX 네임스페이스의 API를 사용하는 데 필요합니다.</p></td>
</tr>
<tr class="odd">
<td align="left">**사용자 계정 이름**</td>
<td align="left"><p>**userPrincipalName** 제한된 접근 권한 값을 통해 앱은 사진의 미리 보기 캐시를 수정하고 이 캐시에 액세스할 수 있습니다.</p>
<p>이 접근 권한 값은 [**GetUserNameEx**](https://msdn.microsoft.com/library/windows/desktop/ms724435) 함수를 호출하는 데 필요합니다.</p></td>
</tr>
<tr class="even">
<td align="left">**전자지갑**</td>
<td align="left"><p>**walletSystem** 제한된 접근 권한 값을 통해 앱은 저장된 전자지갑 카드에 대한 모든 권한을 가질 수 있습니다.</p>
<p>이 접근 권한 값은 [**Windows.ApplicationModel.Wallet.System**](https://msdn.microsoft.com/library/windows/apps/Mt171610) 네임스페이스의 API를 사용하는 데 필요합니다.</p></td>
</tr>
<tr class="odd">
<td align="left">**위치 기록**</td>
<td align="left"><p>**locationHistory** 제한된 접근 권한 값을 통해 앱은 장치의 위치 기록에 액세스할 수 있습니다.</p>
<p>이 접근 권한 값은 [**Windows.Devices.Geolocation**](https://msdn.microsoft.com/library/windows/apps/BR225603) 네임스페이스의 API를 사용하는 데 필요합니다.</p></td>
</tr>
<tr class="even">
<td align="left">**앱 닫기 확인**</td>
<td align="left"><p>**confirmAppClose** 제한된 접근 권한 값을 통해 앱은 앱 자체 및 해당 창을 닫고 앱의 닫기를 지연할 수 있습니다.</p>
<p>이 접근 권한 값은 [**Windows.UI.ViewManagement**](https://msdn.microsoft.com/library/windows/apps/BR242295) 네임스페이스의 API를 사용하는 데 필요합니다.</p></td>
</tr>
<tr class="odd">
<td align="left">**통화 기록***</td>
<td align="left"><p>**phoneCallHistory** 제한된 접근 권한 값을 통해 앱은 통화 기록을 읽고 기록에서 항목을 삭제할 수 있습니다.</p>
<p>이 접근 권한 값은 [**Windows.ApplicationModel.Chat**](https://msdn.microsoft.com/library/windows/apps/Dn642321) 네임스페이스의 API를 사용하는 데 필요합니다.</p></td>
</tr>
<tr class="even">
<td align="left">**시스템 수준 약속 액세스**</td>
<td align="left"><p>**appointmentsSystem** 제한된 접근 권한 값을 통해 앱은 사용자 일정의 모든 약속을 읽고 수정할 수 있습니다.</p>
<p>이 접근 권한 값은 [**Windows.ApplicationModel.Appointment**](https://msdn.microsoft.com/library/windows/apps/Dn263359) 네임스페이스의 API를 사용하는 데 필요합니다.</p></td>
</tr>
<tr class="odd">
<td align="left">**시스템 수준 채팅 메시지 액세스***</td>
<td align="left"><p>**chatSystem** 제한된 접근 권한 값을 통해 앱은 모든 SMS 및 MMS 메시지를 읽고 쓸 수 있습니다.</p>
<p>이 접근 권한 값은 [**Windows.ApplicationModel.Chat**](https://msdn.microsoft.com/library/windows/apps/Dn642321) 네임스페이스의 API를 사용하는 데 필요합니다.</p></td>
</tr>
<tr class="even">
<td align="left">**시스템 수준 연락처 액세스**</td>
<td align="left"><p>**contactsSystem** 제한된 접근 권한 값을 통해 앱은 제한되거나 중요하다고 지정된 연락처 정보를 읽고 기존 연락처 정보를 수정할 수 있습니다.</p>
<p>이 접근 권한 값은 [**Windows.ApplicationModel.Chat**](https://msdn.microsoft.com/library/windows/apps/Dn642321) 네임스페이스의 API를 사용하는 데 필요합니다.</p></td>
</tr>
<tr class="odd">
<td align="left">**메일 액세스***</td>
<td align="left"><p>**email** 제한된 접근 권한 값을 통해 앱은 사용자 메일을 읽고, 심사하고, 보낼 수 있습니다.</p>
<p>이 접근 권한 값은 [**Windows.ApplicationModel.Email**](https://msdn.microsoft.com/library/windows/apps/Dn631285) 네임스페이스의 API를 사용하는 데 필요합니다.</p></td>
</tr>
<tr class="even">
<td align="left">**시스템 수준 메일 액세스**</td>
<td align="left"><p>**emailSystem** 제한된 접근 권한 값을 통해 앱은 사용자가 제한하거나 중요한 메일을 읽고, 심사하고, 보낼 수 있습니다.</p>
<p>이 접근 권한 값은 [**Windows.ApplicationModel.Email**](https://msdn.microsoft.com/library/windows/apps/Dn631285) 네임스페이스의 API를 사용하는 데 필요합니다.</p></td>
</tr>
<tr class="odd">
<td align="left">**시스템 수준 통화 기록 액세스**</td>
<td align="left"><p>**phoneCallHistorySystem** 제한된 접근 권한 값을 통해 앱은 기존 항목을 변경하고 새 항목을 작성하여 통화 기록을 완전히 수정할 수 있습니다.</p>
<p>이 접근 권한 값은 [**Windows.ApplicationModel.Calls**](https://msdn.microsoft.com/library/windows/apps/Dn297266) 네임스페이스의 API를 사용하는 데 필요합니다.</p></td>
</tr>
<tr class="even">
<td align="left">**문자 메시지 보내기***</td>
<td align="left"><p>**smsSend** 제한된 접근 권한 값을 통해 앱은 SMS 및 MMS 메시지를 보낼 수 있습니다.</p>
<p>이 접근 권한 값은 [**Windows.ApplicationModel.Chat**](https://msdn.microsoft.com/library/windows/apps/Dn642321) 네임스페이스의 API를 사용하는 데 필요합니다.</p></td>
</tr>
<tr class="odd">
<td align="left">**모든 사용자 데이터에 대한 시스템 수준 액세스**</td>
<td align="left"><p>**userDataSystem** 제한된 접근 권한 값을 통해 앱은 사용자 데이터 시스템 데이터 저장소에 액세스할 수 있습니다.</p></td>
</tr>
<tr class="even">
<td align="left">**스토어 미리 보기 기능**</td>
<td align="left"><p>**previewStore** 제한된 접근 권한 값을 통해 앱은 앱에서 바로 구매 제품의 SKU를 검색하고 구매할 수 있습니다.</p>
<p>이 접근 권한 값은 [**Windows.ApplicationModel.Store.Preview**](https://msdn.microsoft.com/library/windows/apps/Mt185546) 네임스페이스의 특정 API를 사용하는 데 필요합니다.</p></td>
</tr>
<tr class="odd">
<td align="left">**최초 로그인 설정**</td>
<td align="left"><p>**firstSignInSettings** 제한된 접근 권한 값을 통해 앱은 사용자가 장치에 처음 로그인할 때 설정된 사용자 설정에 액세스할 수 있습니다.</p></td>
</tr>
<tr class="even">
<td align="left">**Windows 팀 환경**</td>
<td align="left"><p>**teamEditionExperience** 제한된 접근 권한 값을 통해 앱은 Windows 팀 세션의 많은 환경적 측면을 제어하는 내부 API에 액세스할 수 있습니다. Windows 팀 세션은 Microsoft Surface Hub와 같은 팀 장치에서 실행될 수 있습니다.</p></td>
</tr>
<tr class="odd">
<td align="left">**원격 잠금 해제**</td>
<td align="left"><p>**remotePassportAuthentication** 제한된 접근 권한 값을 통해 앱은 원격 PC의 잠금을 해제하는 데 사용될 수 있는 자격 증명에 액세스할 수 있습니다.</p></td>
</tr>
<tr class="even">
<td align="left">**미리 보기 컴퍼지션**</td>
<td align="left"><p>**previewUiComposition** 제한된 접근 권한 값을 통해 앱은 완료되기 전에 API에 대한 피드백을 제공할 수 있도록 사용자 인터페이스에 대한 [**Windows.UI.Composition**](https://msdn.microsoft.com/library/windows/apps/Dn706878) 네임스페이스를 미리 볼 수 있습니다. 자세한 내용은 wincomposition@microsoft.com에 문의하세요.</p></td>
</tr>
<tr class="odd">
<td align="left">**보안 평가 잠금**</td>
<td align="left"><p>**secureAssessment** 제한된 접근 권한 값을 통해 앱은 보안 평가를 위해 Windows를 단일 앱 모드로 잠글 수 있습니다.</p></td>
</tr>
<tr class="even">
<td align="left">**연결 관리자 프로비저닝**</td>
<td align="left"><p>**networkConnectionManagerProvisioning** 제한된 접근 권한 값을 통해 앱은 장치를 WWAN 및 WLAN 인터페이스에 연결하는 정책을 정의할 수 있습니다. 이 접근 권한 값을 사용하는 앱은 통신사가 해당 모바일 네트워크에 연결하는 장치를 제어하기 위해 만듭니다.</p></td>
</tr>
<tr class="odd">
<td align="left">**데이터 요금제 프로비저닝**</td>
<td align="left"><p>**networkDataPlanProvisioning** 제한된 접근 권한 값을 통해 앱은 장치에서 데이터 요금제에 대한 정보를 수집하고 네트워크 사용량을 읽을 수 있습니다. 이 접근 권한 값을 사용하는 앱은 통신사가 고객의 실제 데이터 사용량을 OS 데이터 사용량 설정에 통합하기 위해 만듭니다.</p></td>
</tr>
<tr class="even">
<td align="left">**소프트웨어 라이선스**</td>
<td align="left"><p>**slapiQueryLicenseValue** 제한된 접근 권한 값을 통해 앱은 소프트웨어 라이선싱 정책을 쿼리할 수 있습니다.</p></td>
</tr>
<tr class="odd">
<td align="left">**확장 실행**</td>
<td align="left"><p>**extendedExecutionBackgroundAudio** 제한된 접근 권한 값을 통해 앱은 프로그라운드에 없을 때 오디오를 재생할 수 있습니다.</p>
<p>**extendedExecutionCritical** 제한된 접근 권한 값을 통해 앱은 중요한 확장 실행 세션을 시작할 수 있습니다.</p>
<p>**extendedExecutionUnconstrained** 제한된 접근 권한 값을 통해 앱은 제약이 없는 확장 실행 세션을 시작할 수 있습니다.</p></td>
</tr>
<tr class="even">
<td align="left">**모바일 장치 관리**</td>
<td align="left"><p>**deviceManagementDmAccount** 제한된 접근 권한 값을 통해 앱은 MO OMA-DM(Mobile Operator Open Mobile Alliance - Device Management) 계정을 프로비전 및 구성할 수 있습니다.</p>
<p>**deviceManagementFoundation** 제한된 접근 권한 값을 통해 앱은 장치에서 MDM(모바일 장치 관리) CSP(구성 서비스 공급자) 인프라에 대한 기본 액세스 권한을 가질 수 있습니다. 특정 CSP에 액세스하려면 다른 접근 권한 값이 필요합니다.</p>
<p>**deviceManagementWapSecurityPolicies** 제한된 접근 권한 값을 통해 앱은 MM, SI/SL(서비스 표시/서비스 로드), OMA-CP(Open Mobile Alliance - Client Provisioning) 등 WAP(Wireless Application Protocol) 기반 서비스를 구성할 수 있습니다.</p>
<p>**deviceManagementEmailAccount** 제한된 접근 권한 값을 통해 통신사는 사용자에게 프로비전하는 장치에서 메일 계정을 추가하고 관리하기 위한 앱을 만들 수 있습니다.</p></td>
</tr>
<tr class="odd">
<td align="left">**패키지 정책 제어**</td>
<td align="left"><p>**packagePolicySystem** 제한된 접근 권한 값을 통해 앱은 장치에 설치된 앱과 관련된 시스템 정책을 제어할 수 있습니다.</p></td>
</tr>
<tr class="even">
<td align="left">**게임 목록**</td>
<td align="left"><p>**gameList** 제한된 접근 권한 값을 통해 앱은 시스템에 설치된 알려진 게임 목록을 가져올 수 있습니다.</p></td>
</tr>
<tr class="odd">
<td align="left">**Xbox 액세서리**</td>
<td align="left"><p>**xboxAccessoryManagement** 제한된 접근 권한 값을 통해 앱은 Xbox 하드웨어 사양을 준수하는 Xbox 장치를 직접 관리할 수 있습니다.</p></td>
</tr>
<tr class="even">
<td align="left">**액세서리에 대한 음성 인식**</td>
<td align="left"><p>**cortanaSpeechAccessory** 제한된 접근 권한 값을 통해 앱은 명령을 호출하고 Cortana에 전달할 수 있습니다.</p></td>
</tr>
<tr class="odd">
<td align="left">**액세서리 관리**</td>
<td align="left"><p>**accessoryManager** 제한된 접근 권한 값을 통해 앱은 액세서리로 전달되고 사용자에게 표시될 수 있도록 액세서리 앱으로 등록하고 특정 앱 알림에 옵트인(opt-in)할 수 있습니다.</p></td>
</tr>
<tr class="even">
<td align="left">**드라이버 액세스**</td>
<td align="left"><p>**interopServices** 제한된 접근 권한 값을 통해 앱은 드라이버를 직접 조작할 수 있습니다.</p></td>
</tr>
<tr class="odd">
<td align="left">**전경 관찰**</td>
<td align="left"><p>**inputForegroundObservation** 제한된 접근 권한 값을 통해 전경의 앱은 키보드 입력을 가로채고 앱이 아닌 모든 키보드 입력 처리를 바이패스할 수 있습니다. SAS 조합은 이 접근 권한 값으로 가로챌 수 없습니다. 이 접근 권한 값은 [**KeyboardDeliveryInterceptor**](https://msdn.microsoft.com/library/windows/apps/Mt608395) 클래스의 구성원에 액세스하는 데 필요합니다.</p></td>
</tr>
<tr class="even">
<td align="left">**OEM 및 MO 파트너 앱**</td>
<td align="left"><p>**oemDeployment** 제한된 접근 권한 값을 통해 Microsoft 파트너가 만든 앱은 장치에 새 앱을 설치하고 현재 설치된 앱을 쿼리할 수 있습니다.</p>
<p>**oemPublicDirectory** 제한된 접근 권한 값을 통해 Microsoft 파트너가 만든 앱은 공유 앱 폴더에 액세스할 수 있습니다.</p></td>
</tr>
<tr class="odd">
<td align="left">**앱 라이선싱**</td>
<td align="left"><p>**appLicensing** 제한된 접근 권한 값을 통해 라이선스 없이 앱을 실행할 수 있습니다. 매니페스트에서 이 접근 권한 값을 선언하는 경우 스토어에 앱을 제출할 수 없습니다. 스토어 제출에 대해 이 접근 권한 값에 대한 모든 액세스 요청이 항상 거부됩니다.</p></td>
</tr>
<tr class="even">
<td align="left">**위치 시스템**</td>
<td align="left"><p>**locationSystem** 제한된 접근 권한 값을 통해 앱은 디바이스의 기본 위치 설정과 같은 특정 권한 위치 구성을 수행할 수 있습니다. 매니페스트에서 이 접근 권한 값을 선언하는 경우 스토어에 앱을 제출할 수 없습니다. 스토어 제출에 대해 이 접근 권한 값에 대한 모든 액세스 요청이 항상 거부됩니다.</p></td>
</tr>
</tbody>
</table>

**참고**  
이 문서는 UWP 앱을 작성하는 Windows 10 개발자용입니다. Windows 8.x 또는 Windows Phone 8.x를 개발하는 경우 [보관된 문서](http://go.microsoft.com/fwlink/p/?linkid=619132)를 참조하세요.

## 관련 항목

* [매니페스트 디자이너](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/br230259.aspx)
* [개인 정보 인식 앱에 대한 지침](https://msdn.microsoft.com/library/windows/apps/Hh768223)
* [패키지 매니페스트에 접근 권한 값을 지정하는 방법](https://msdn.microsoft.com/library/windows/apps/BR211477)
* [패키지 매니페스트에 장치 접근 권한 값을 지정하는 방법](https://msdn.microsoft.com/library/windows/apps/Dn263092)
 


<!--HONumber=Mar16_HO5-->


