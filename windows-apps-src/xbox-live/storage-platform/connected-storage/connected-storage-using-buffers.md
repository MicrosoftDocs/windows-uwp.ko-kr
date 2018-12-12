---
title: 연결 된 저장소 버퍼를 사용 하 여 작업
description: 연결 된 저장소 버퍼를 사용 하는 방법을 알아봅니다.
ms.assetid: 1d9d1b52-4bfe-4cd9-8b80-50ca6c0e9ae1
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, 게임, uwp, windows 10, 연결 된 저장, xbox
ms.localizationpriority: medium
ms.openlocfilehash: 3df95e4807e8d3457143e67eebfb62011bf365cc
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8945444"
---
# <a name="working-with-connected-storage-buffers"></a>연결 된 저장소 버퍼를 사용 하 여 작업

연결 된 저장소 API는 응용 프로그램에서 데이터를 **Windows::Storage::Streams::Buffer** 인스턴스를 사용 합니다. WinRT 형식과 원시 포인터를 노출할 수 없습니다, 때문에 버퍼 인스턴스의 데이터에 대 한 액세스 **DataReader** 및 **DataWriter** 클래스를 통해 발생 합니다. 그러나 **버퍼** 는 또한 **IBufferByteAccess**, 직접 버퍼 데이터에 대 한 포인터를 가져올 수 있게 하는 COM 인터페이스를 구현 합니다.

### <a name="to-get-a-pointer-to-a-buffer-instances-data"></a>버퍼 인스턴스의 데이터에 대 한 포인터를 가져오려면

1.  **IUnknown**으로 버퍼 인스턴스를 캐스팅 **reinterpret\_cast** 를 사용 합니다.

```cpp
        IUnknown* unknown = reinterpret_cast<IUnknown*>(buffer);
```

2.  **IUnknown** 인터페이스 **IBufferByteAccess** COM 인터페이스를 쿼리 합니다.

```cpp
        Microsoft::WRL::ComPtr<IBufferByteAccess> bufferByteAccess;
        HRESULT hr = unknown->QueryInterface(_uuidof(IBufferByteAccess), &bufferByteAccess);
        if (FAILED(hr))
        return nullptr;
```

3.  **IBufferByteAccess::Buffer** 를 사용 하 여 버퍼의 데이터에 직접 포인터를 가져옵니다.

```cpp
        byte* bytes = nullptr;
        bufferByteAccess->Buffer(&bytes);
```

예를 들어 다음 코드 예제는 현재 시스템 시간이 포함 하는 버퍼를 만드는 방법을 보여 줍니다. 버퍼에 별도 용량 및 길이 값 이므로 용량 및 길이 명시적으로 설정 합니다. 기본적으로 길이 0입니다.

```cpp
    inline byte* GetBufferData(Windows::Storage::Streams::IBuffer^ buffer)
    {
        using namespace Windows::Storage::Streams;
        IUnknown* unknown = reinterpret_cast<IUnknown*>(buffer);
        Microsoft::WRL::ComPtr<IBufferByteAccess> bufferByteAccess;
        HRESULT hr = unknown->QueryInterface(_uuidof(IBufferByteAccess), &bufferByteAccess);
        if (FAILED(hr))
        return nullptr;
        byte* bytes = nullptr;
        bufferByteAccess->Buffer(&bytes);
        return bytes;
    }

    IBuffer^ WrapRawBuffer( void* ptr, size_t size )
    {
        using namespace Windows::Storage::Streams;

        //uint32 size = sizeof(FILETIME);
        Buffer^ buffer = ref new Buffer(size);
        buffer->Length = size;

        memcpy(GetBufferData(buffer),ptr,size);


        return buffer;

    };
```
