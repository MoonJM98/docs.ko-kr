---
title: 코어응답데이터.m_ResponseHeaders 필드
ms.date: 01/29/2018
topic_type:
- apiref
api_name:
- System.Net.CoreResponseData.m_ResponseHeaders
api_location:
- System.dll
api_type:
- Assembly
author: stevewhims
ms.openlocfilehash: 723df6dc2de978695608d106e3a01bde286fc4fe
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/12/2020
ms.locfileid: "79156105"
---
# <a name="coreresponsedatam_responseheaders-field"></a>코어응답데이터.m\_응답헤더 필드

`CoreResponseData.m_ResponseHeaders`<xref:System.Net.WebHeaderCollection> 은 서버 응답과 연결된 헤더입니다.

## <a name="syntax"></a>구문
  
```csharp
public WebHeaderCollection m_ResponseHeaders
```

> [!WARNING]
> 이 API는 코드에서 직접 사용할 수 없습니다. 대신 에 를 <xref:System.Diagnostics.DiagnosticSource> 사용하여 네트워킹 코드를 연결해야 합니다. [진단 소스 사용자 가이드를](https://github.com/dotnet/runtime/blob/master/src/libraries/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md)참조하십시오.
>
> Microsoft는 어떠한 상황에서도 프로덕션 응용 프로그램에서 이 클래스의 사용을 지원하지 않습니다.

## <a name="requirements"></a>요구 사항

**네임스페이스:**<xref:System.Net>

**어셈블리:** 시스템(시스템.dll)

**.NET 프레임워크 버전:** 2.0 부터 사용할 수 있습니다.
