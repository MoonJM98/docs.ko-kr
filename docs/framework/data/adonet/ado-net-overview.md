---
title: 개요
description: .NET Framework에서 ADO.NET의 개요를 읽고 자세한 설명 및 예제는 리소스 정보를 참조 하세요.
ms.date: 03/30/2017
ms.assetid: ee3bc1d8-11db-4be4-89eb-c708cf04117d
ms.openlocfilehash: 2ff3b7ad197bfe1e1c12e382f3a59bd470c57a75
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84287157"
---
# <a name="adonet-overview"></a>ADO.NET 개요
ADO.NET은 OLE DB 및 ODBC를 통해 노출되는 데이터 소스, SQL Server 및 XML과 같은 데이터 소스에 대한 일관성 있는 액세스를 제공합니다. 데이터 공유 소비자 애플리케이션은 ADO.NET을 통해 이러한 데이터 소스에 연결하여 포함된 데이터를 검색, 처리 및 업데이트할 수 있습니다.  
  
 ADO.NET은 데이터 조작에 따른 데이터 액세스를 각각의 구성 요소로 구분하여 개별적으로 또는 차례대로 사용할 수 있게 해 줍니다. ADO.NET에는 데이터베이스에 연결하고 명령을 실행하며 결과를 검색하는 데 사용되는 .NET Framework 데이터 공급자가 포함됩니다. 검색된 결과는 곧바로 처리되거나 ADO.NET <xref:System.Data.DataSet> 개체에 저장되어 특별한 방식으로 사용자에게 노출되거나 여러 소스의 데이터와 함께 사용되거나 계층 간에 전달됩니다. `DataSet` 개체는 또한 .NET Framework 데이터 공급자를 독립적으로 사용하여 애플리케이션에 로컬인 데이터 또는 XML에서 가져온 데이터를 관리할 수 있습니다.  
  
 ADO.NET 클래스는 System.Data.dll에 있으며 System.Xml.dll에 있는 XML 클래스와 통합됩니다. 데이터베이스에 연결 하 고 해당 데이터에서 데이터를 검색 한 다음 해당 데이터를 콘솔 창에 표시 하는 샘플 코드는 [ADO.NET 코드 예제](ado-net-code-examples.md)를 참조 하세요.  
  
 ADO.NET은 관리 코드를 작성하는 개발자에게 ADO(ActiveX Data Objects)에서 네이티브 COM(Component Object Model) 개발자를 대상으로 제공하는 기능과 유사한 기능을 제공합니다. .NET 애플리케이션에서 데이터에 액세스할 때 ADO가 아니라 ADO.NET을 사용하는 것이 좋습니다.  
  
 ADO.NET은 .NET Framework 내에서 가장 직접적인 데이터 액세스 방법을 제공합니다. 응용 프로그램이 기본 저장소 모델 대신 개념적 모델에 대해 작동할 수 있도록 하는 더 높은 수준의 추상화를 위해 [ADO.NET Entity Framework](./ef/index.md)를 참조 하세요.  
  
 **개인 정보 취급 방침**: System.object, OracleClient, system.data.sqlserverce, system.string, System.string, 및 System.xml 어셈블리는 사용자의 개인 데이터와 개인 데이터를 구분 하지 않습니다. 이러한 어셈블리는 사용자의 개인 데이터와 개인 데이터를 구분 하지 않습니다 .이는 사용자의 개인 데이터와 개인 데이터를 구분 하지 않습니다.  이러한 어셈블리는 사용자의 개인 데이터를 수집, 저장 및 전송하지 않습니다. 하지만 타사 애플리케이션에서 이러한 어셈블리를 사용하여 사용자의 개인 데이터를 수집, 저장 및 전송할 수 있습니다.  
  
## <a name="in-this-section"></a>섹션 내용  
 [ADO.NET 아키텍처](ado-net-architecture.md)  
 ADO.NET의 구조와 구성 요소에 대해 간략하게 설명합니다.  
  
 [ADO.NET 기술 옵션 및 지침](ado-net-technology-options-and-guidelines.md)  
 엔터티 데이터 플랫폼에 포함된 제품과 기술에 대해 설명합니다.  
  
 [LINQ 및 ADO.NET](linq-and-ado-net.md)  
 LINQ(Language-Integrated Query)가 ADO.NET에 어떻게 구현되었는지 설명하고 관련 항목에 대한 링크를 제공합니다.  
  
 [.NET Framework 데이터 공급자](data-providers.md)  
 ADO.NET에 포함된 하나 이상의 .NET Framework 데이터 공급자에 대한 디자인 개요를 제공합니다.  
  
 [ADO.NET 데이터 세트](ado-net-datasets.md)  
 `DataSet` 디자인 및 구성 요소의 개요를 제공합니다.  
  
 [ADO.NET에서 Side-by-Side 실행](side-by-side-execution.md)  
 ADO.NET 버전의 차이를 설명하고 이 차이가 side-by-side 실행 및 애플리케이션 호환성에 미치는 영향에 대해 살펴봅니다.  
  
 [ADO.NET 코드 예제](ado-net-code-examples.md)  
 ADO.NET 데이터 공급자를 사용하여 데이터를 검색하는 코드 샘플을 제공합니다.  
  
## <a name="related-sections"></a>관련 단원  
 [ADO.NET의 새로운 기능](whats-new.md)  
 ADO.NET에 새로 추가된 기능을 소개합니다.  
  
 [ADO.NET 애플리케이션 보안](securing-ado-net-applications.md)  
 ADO.NET을 사용할 때 보안 코드를 작성하는 방법에 대해 설명합니다.  
  
 [ADO.NET에서 데이터 형식 매핑](data-type-mappings-in-ado-net.md)  
 .NET Framework 데이터 형식과 .NET Framework 데이터 공급자 간의 데이터 형식 매핑에 대해 설명합니다.  
  
 [ADO.NET에서 데이터 검색 및 수정](retrieving-and-modifying-data.md)  
 데이터 소스에 연결하고, 데이터를 검색하고, 데이터를 수정하는 방법을 설명합니다. 여기에는 `DataReaders` 및 `DataAdapters`가 포함됩니다.  
  
## <a name="see-also"></a>참고 항목

- [ADO.NET](index.md)
- [Visual Studio에서 데이터 액세스](/visualstudio/data-tools/accessing-data-in-visual-studio)
