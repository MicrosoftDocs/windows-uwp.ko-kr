---
title: BasicReaderWriter의 전체 코드
description: 일반적으로 이진 데이터 파일을 읽고 쓰기 위한 클래스 및 메서드의 전체 코드입니다.
ms.assetid: af968edd-df5c-b8e6-479e-bfa9689380fc
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 게임, basicreaderwriter
ms.localizationpriority: medium
ms.openlocfilehash: 60c2bf74de1e56004f1f705f317bc6bd1bb34b26
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8690455"
---
# <a name="complete-code-for-basicreaderwriter"></a>BasicReaderWriter의 전체 코드



일반적으로 이진 데이터 파일을 읽고 쓰기 위한 클래스 및 메서드의 전체 코드입니다. [BasicLoader](complete-code-for-basicloader.md) 클래스에서 사용됩니다.

이 항목은 다음과 같은 섹션으로 구성되어 있습니다.

-   [기술](#technologies)
-   [요구 사항](#requirements)
-   [코드 보기(C++)](#view-the-code-c)


## <a name="download-location"></a>다운로드 위치

이 샘플은 다운로드할 수 없습니다.


## <a name="technologies"></a>기술

**프로그래밍 언어** -  C++  
**프로그래밍 모델** - Windows 런타임


## <a name="requirements"></a>요구 사항

 **지원되는 최소 클라이언트** - Windows 10       
 **지원되는 최소 서버** - Windows Server 2016 Technical Preview 

## <a name="view-the-code-c"></a>코드 보기(C++)


## <a name="basicreaderwriterh"></a>BasicReaderWriter.h


```cpp
//// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
//// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
//// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
//// PARTICULAR PURPOSE.
////
//// Copyright (c) Microsoft Corporation. All rights reserved

#pragma once

#include <ppltasks.h>

// A simple reader/writer class that provides support for reading and writing
// files on disk. Provides synchronous and asynchronous methods.
ref class BasicReaderWriter
{
private:
    Windows::Storage::StorageFolder^ m_location;
    Platform::String^ m_locationPath;

internal:
    BasicReaderWriter();
    BasicReaderWriter(
        _In_ Windows::Storage::StorageFolder^ folder
        );

    Platform::Array<byte>^ ReadData(
        _In_ Platform::String^ filename
        );

    concurrency::task<Platform::Array<byte>^> ReadDataAsync(
        _In_ Platform::String^ filename
        );

    uint32 WriteData(
        _In_ Platform::String^ filename,
        _In_ const Platform::Array<byte>^ fileData
        );

    concurrency::task<void> WriteDataAsync(
        _In_ Platform::String^ filename,
        _In_ const Platform::Array<byte>^ fileData
        );
};
```

## <a name="basicreaderwritercpp"></a>BasicReaderWriter.cpp


```cpp
using namespace Microsoft::WRL;
using namespace Windows::Storage;
using namespace Windows::Storage::FileProperties;
using namespace Windows::Storage::Streams;
using namespace Windows::Foundation;
using namespace Windows::ApplicationModel;
using namespace concurrency;

BasicReaderWriter::BasicReaderWriter()
{
    m_location = Package::Current->InstalledLocation;
    m_locationPath = Platform::String::Concat(m_location->Path, "\\");
}

BasicReaderWriter::BasicReaderWriter(
    _In_ Windows::Storage::StorageFolder^ folder
    )
{
    m_location = folder;
    Platform::String^ path = m_location->Path;
    if (path->Length() == 0)
    {
        // Applications are not permitted to access certain
        // folders, such as the Documents folder, using this
        // code path.  In such cases, the Path property for
        // the folder will be an empty string.
        throw ref new Platform::FailureException();
    }
    m_locationPath = Platform::String::Concat(path, "\\");
}

Platform::Array<byte>^ BasicReaderWriter::ReadData(
    _In_ Platform::String^ filename
    )
{
    CREATEFILE2_EXTENDED_PARAMETERS extendedParams = {0};
    extendedParams.dwSize = sizeof(CREATEFILE2_EXTENDED_PARAMETERS);
    extendedParams.dwFileAttributes = FILE_ATTRIBUTE_NORMAL;
    extendedParams.dwFileFlags = FILE_FLAG_SEQUENTIAL_SCAN;
    extendedParams.dwSecurityQosFlags = SECURITY_ANONYMOUS;
    extendedParams.lpSecurityAttributes = nullptr;
    extendedParams.hTemplateFile = nullptr;

    Wrappers::FileHandle file(
        CreateFile2(
            Platform::String::Concat(m_locationPath, filename)->Data(),
            GENERIC_READ,
            FILE_SHARE_READ,
            OPEN_EXISTING,
            &extendedParams
            )
        );
    if (file.Get() == INVALID_HANDLE_VALUE)
    {
        throw ref new Platform::FailureException();
    }

    FILE_STANDARD_INFO fileInfo = {0};
    if (!GetFileInformationByHandleEx(
        file.Get(),
        FileStandardInfo,
        &fileInfo,
        sizeof(fileInfo)
        ))
    {
        throw ref new Platform::FailureException();
    }

    if (fileInfo.EndOfFile.HighPart != 0)
    {
        throw ref new Platform::OutOfMemoryException();
    }

    Platform::Array<byte>^ fileData = ref new Platform::Array<byte>(fileInfo.EndOfFile.LowPart);

    if (!ReadFile(
        file.Get(),
        fileData->Data,
        fileData->Length,
        nullptr,
        nullptr
        ))
    {
        throw ref new Platform::FailureException();
    }

    return fileData;
}

task<Platform::Array<byte>^> BasicReaderWriter::ReadDataAsync(
    _In_ Platform::String^ filename
    )
{
    return task<StorageFile^>(m_location->GetFileAsync(filename)).then([=](StorageFile^ file)
    {
        return FileIO::ReadBufferAsync(file);
    }).then([=](IBuffer^ buffer)
    {
        auto fileData = ref new Platform::Array<byte>(buffer->Length);
        DataReader::FromBuffer(buffer)->ReadBytes(fileData);
        return fileData;
    });
}

uint32 BasicReaderWriter::WriteData(
    _In_ Platform::String^ filename,
    _In_ const Platform::Array<byte>^ fileData
    )
{
    CREATEFILE2_EXTENDED_PARAMETERS extendedParams = {0};
    extendedParams.dwSize = sizeof(CREATEFILE2_EXTENDED_PARAMETERS);
    extendedParams.dwFileAttributes = FILE_ATTRIBUTE_NORMAL;
    extendedParams.dwFileFlags = FILE_FLAG_SEQUENTIAL_SCAN;
    extendedParams.dwSecurityQosFlags = SECURITY_ANONYMOUS;
    extendedParams.lpSecurityAttributes = nullptr;
    extendedParams.hTemplateFile = nullptr;

    Wrappers::FileHandle file(
        CreateFile2(
            Platform::String::Concat(m_locationPath, filename)->Data(),
            GENERIC_WRITE,
            0,
            CREATE_ALWAYS,
            &extendedParams
            )
        );
    if (file.Get() == INVALID_HANDLE_VALUE)
    {
        throw ref new Platform::FailureException();
    }

    DWORD numBytesWritten;
    if (
        !WriteFile(
            file.Get(),
            fileData->Data,
            fileData->Length,
            &numBytesWritten,
            nullptr
            ) ||
        numBytesWritten != fileData->Length
        )
    {
        throw ref new Platform::FailureException();
    }

    return numBytesWritten;
}

task<void> BasicReaderWriter::WriteDataAsync(
    _In_ Platform::String^ filename,
    _In_ const Platform::Array<byte>^ fileData
    )
{
    return task<StorageFile^>(m_location->CreateFileAsync(filename, CreationCollisionOption::ReplaceExisting)).then([=](StorageFile^ file)
    {
        FileIO::WriteBytesAsync(file, fileData);
    });
}
```

 

 




