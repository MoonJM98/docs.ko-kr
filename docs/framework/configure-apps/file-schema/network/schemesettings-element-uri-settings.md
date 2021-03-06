---
title: <schemeSettings> 요소(URI 설정)
ms.date: 03/30/2017
ms.assetid: 0ae45c6e-8c4c-4c0d-8b9f-a93824648890
ms.openlocfilehash: c745c90bb61b9ee393687d7f6db4fd11565c7dc7
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/06/2020
ms.locfileid: "79154649"
---
# <a name="schemesettings-element-uri-settings"></a>\<schemeSettings> 요소(URI 설정)
특정 체계에 대해 <xref:System.Uri>가 구문 분석되는 방법을 지정합니다.  
  
[**\<configuration>**](../configuration-element.md)  
&nbsp;&nbsp;[**\<uri>**](uri-element-uri-settings.md)  
&nbsp;&nbsp;&nbsp;&nbsp;**\<schemeSettings>**  
  
## <a name="syntax"></a>구문  
  
```xml  
<schemeSettings>
</schemeSettings>  
```  
  
## <a name="attributes-and-elements"></a>특성 및 요소  
 다음 섹션에서는 특성, 자식 요소 및 부모 요소에 대해 설명합니다.  
  
### <a name="attributes"></a>특성  
 None  
  
### <a name="child-elements"></a>자식 요소  
  
|**요소**|**설명**|  
|-----------------|---------------------|  
|[추가](add-element-for-schemesettings-uri-settings.md)|구성표 이름에 대 한 구성표 설정을 추가 합니다.|  
|[해제](clear-element-for-schemesettings-uri-settings.md)|기존 구성표 설정을 모두 지웁니다.|  
|[remove](remove-element-for-schemesettings-uri-settings.md)|구성표 이름에 대 한 구성표 설정을 제거 합니다.|  
  
### <a name="parent-elements"></a>부모 요소  
  
|**요소**|**설명**|  
|-----------------|---------------------|  
|[uri](uri-element-uri-settings.md)|.NET Framework Uri (uniform resource identifier)를 사용 하 여 표현 된 웹 주소를 처리 하는 방법을 지정 하는 설정을 포함 합니다.|  
  
## <a name="remarks"></a>설명  
 기본적으로 클래스는 <xref:System.Uri?displayProperty=nameWithType> 경로 압축을 실행 하기 전에 인코딩된 경로 구분 기호 비율을 이스케이프 해제 합니다. 이는 다음과 같은 공격에 대 한 보안 메커니즘으로 구현 되었습니다.  
  
 `http://www.contoso.com/..%2F..%2F/Windows/System32/cmd.exe?/c+dir+c:\`  
  
 % 인코딩된 문자를 올바르게 처리 하지 않는 모듈에이 URI가 전달 되 면 서버에서 다음 명령을 실행 하 게 될 수 있습니다.  
  
 `c:\Windows\System32\cmd.exe /c dir c:\`  
  
 따라서 <xref:System.Uri?displayProperty=nameWithType> 클래스는 먼저 경로 구분 기호를 이스케이프 해제 한 후 경로 압축을 적용 합니다. 위의 악성 URL을 클래스 생성자에 전달 하면 <xref:System.Uri?displayProperty=nameWithType> 다음 URI가 생성 됩니다.  
  
 `http://www.microsoft.com/Windows/System32/cmd.exe?/c+dir+c:\`  
  
 이 기본 동작은 특정 스키마에 대 한 schemeSettings 구성 옵션을 사용 하 여 인코딩된 경로 구분 기호를 이스케이프 해제 하지 않도록 수정할 수 있습니다.  
  
## <a name="configuration-files"></a>구성 파일  
 이 요소는 애플리케이션 구성 파일 또는 컴퓨터 구성 파일(Machine.config)에서 사용할 수 있습니다.  
  
## <a name="example"></a>예제  
 다음 예제에서는 <xref:System.Uri> http 체계의 백분율 인코딩된 경로 구분 기호를 이스케이프 하지 않도록 지원 하기 위해 클래스에서 사용 하는 구성을 보여 줍니다.  
  
```xml  
<configuration>  
  <uri>  
    <schemeSettings>  
      <add name="http" genericUriParserOptions="DontUnescapePathDotsAndSlashes"/>  
    </schemeSettings>  
  </uri>  
</configuration>  
```  
  
## <a name="element-information"></a>요소 정보  
  
|||
|-|-|  
|네임스페이스|System|  
|Schema Name||  
|유효성 검사 파일||  
|비워 둘 수 있음||  
  
## <a name="see-also"></a>참고 항목

- <xref:System.Configuration.SchemeSettingElement?displayProperty=nameWithType>
- <xref:System.Configuration.SchemeSettingElementCollection?displayProperty=nameWithType>
- <xref:System.Configuration.UriSection?displayProperty=nameWithType>
- <xref:System.Configuration.UriSection.SchemeSettings%2A?displayProperty=nameWithType>
- <xref:System.GenericUriParserOptions?displayProperty=nameWithType>
- <xref:System.Uri?displayProperty=nameWithType>
- [네트워크 설정 스키마](index.md)
