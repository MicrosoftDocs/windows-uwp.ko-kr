# 변환된 데스크톱 앱에 서명

이 문서에서는 UWP(유니버설 Windows 플랫폼)로 변환한 데스크톱 앱에 서명하는 방법에 대해 설명합니다. .appx 패키지를 배포하려면 먼저 인증서로 .appx 패키지에 서명해야 합니다.

## .appx에 서명

먼저 MakeCert.exe를 사용하여 인증서를 만듭니다. 암호를 입력하라는 메시지가 표시되면 “없음”을 선택합니다. 

```cmd
C:\> MakeCert.exe -r -h 0 -n "CN=<publisher_name>" -eku 1.3.6.1.5.5.7.3.3 -pe -sv <my.pvk> <my.cer>
```

다음으로, pvk2pfx.exe를 사용하여 공용 및 개인 키 정보를 인증서에 복사합니다. 

```cmd
C:\> pvk2pfx.exe -pvk <my.pvk> -spc <my.cer> -pfx <my.pfx>
```
마지막으로, SignTool.exe를 사용하여 인증서로 .appx에 서명합니다.

```cmd
C:\> signtool.exe sign -f <my.pfx> -fd SHA256 -v .\<outputAppX>.appx
``` 

자세한 내용은 [SignTool을 사용하여 앱 패키지에 서명하는 방법](https://msdn.microsoft.com/en-us/library/windows/desktop/jj835835(v=vs.85).aspx)을 참조하세요. 

위의 세 도구는 모두 Microsoft Windows 10 SDK에 포함되어 있습니다. 직접 호출하려면 명령 프롬프트에서 ```C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\Tools\VsDevCmd.bat``` 스크립트를 호출합니다.

## 일반적인 오류

### 손상되거나 잘못된 형식의 Authenticode 서명

이 섹션에서는 손상되거나 잘못된 형식의 Authenticode 서명이 포함되어 있을 수 있는 AppX 패키지의 PE(이식 가능 파일) 파일과 관련된 문제를 식별하는 방법에 대해 설명합니다. .exe, .dll, .chm 등의 이진 형식일 수 있는 PE 파일의 Authenticode 서명이 잘못된 경우 패키지가 올바르게 서명되지 않으므로 AppX 패키지에서 패키지를 배포할 수 없게 됩니다. 

PE 파일의 Authenticode 서명 위치는 선택적 헤더 데이터 디렉터리의 인증서 표 항목 및 관련된 특성 인증서 표에 의해 지정됩니다. 서명 확인 중에 이러한 구조에 지정된 정보는 PE 파일에서 서명을 찾는 데 사용됩니다. 이러한 값이 손상되면 파일이 잘못 서명된 것처럼 보일 수 있습니다. 

Authenticode 서명이 올바르려면 Authenticode 서명에 대해 다음 조건을 충족해야 합니다.

- PE 파일에 있는 **WIN_CERTIFICATE** 항목의 시작을 실행 파일의 끝을 지나도록 확장할 수 없습니다.
- **WIN_CERTIFCATE** 항목이 이미지의 끝에 위치해야 합니다.
- **WIN_CERTIFICATE** 항목의 크기는 양수여야 합니다.
- **WIN_CERTIFICATE** 항목이 32비트 실행 파일의 경우 **IMAGE_NT_HEADERS32** 구조체 뒤에서 시작해야 하고 64비트 실행 파일의 경우 IMAGE_NT_HEADERS64 구조체 뒤에서 시작해야 합니다.

자세한 내용은 [Authenticode 포털 실행 파일 사양](http://download.microsoft.com/download/9/c/5/9c5b2167-8017-4bae-9fde-d599bac8184a/Authenticode_PE.docx) 및 [PE 파일 형식 사양](https://msdn.microsoft.com/en-us/windows/hardware/gg463119.aspx)을 참조하세요. 

SignTool.exe는 AppX 패키지에 서명하려고 할 때 손상되거나 잘못된 형식의 이진 파일 목록을 출력할 수 있습니다. 이렇게 하려면 APPXSIP_LOG 환경 변수를 1로 설정하여(예: ```set APPXSIP_LOG=1```) 자세한 정보 로깅을 사용하도록 설정하고 SignTool.exe를 다시 실행합니다.

이러한 잘못된 형식의 이진 파일을 수정하려면 해당 이진 파일이 위의 요구 사항을 따르는지 확인합니다.

## 참고 항목

- [SignTool](https://msdn.microsoft.com/library/windows/desktop/aa387764(v=vs.85).aspx)
- [SignTool.exe(서명 도구)](https://msdn.microsoft.com/library/8s9b9yaz(v=vs.110).aspx)
- [SignTool을 사용하여 앱 패키지에 서명하는 방법](https://msdn.microsoft.com/en-us/library/windows/desktop/jj835835(v=vs.85).aspx)

<!--HONumber=Jun16_HO5-->


