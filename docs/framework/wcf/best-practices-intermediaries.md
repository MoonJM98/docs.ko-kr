---
title: '최선의 구현 방법: 중간'
ms.date: 03/30/2017
ms.assetid: 2d41b337-8132-4ac2-bea2-6e9ae2f00f8d
ms.openlocfilehash: a7c037b5942a404b1ff790638979a8e430394151
ms.sourcegitcommit: 628e8147ca10187488e6407dab4c4e6ebe0cac47
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72320792"
---
# <a name="best-practices-intermediaries"></a>최선의 구현 방법: 중간
매개자의 서비스 쪽 채널이 올바르게 닫히도록 하려면 매개자를 호출할 때 오류를 올바르게 처리하도록 주의해야 합니다.  
  
 다음과 같은 시나리오를 생각해 볼 수 있습니다. 클라이언트가 매개자를 호출하고 매개자는 백 엔드 서비스를 호출합니다.  백 엔드 서비스는 아무 오류 계약도 정의하지 않으므로, 이 서비스에서 throw되는 모든 오류는 형식화되지 않은 오류로 처리됩니다.  백 엔드 서비스는 <xref:System.ApplicationException>를 throw 하 고 WCF는 서비스 쪽 채널을 올바르게 중단 합니다. 그런 다음 <xref:System.ApplicationException>은 매개자에 throw된 <xref:System.ServiceModel.FaultException>으로 표시됩니다. 매개자는 <xref:System.ApplicationException>을 다시 throw합니다. WCF는 이를 매개자에서 발생한 형식화되지 않은 오류로 해석하고 이를 클라이언트에 전달합니다. 오류를 수신한 매개자와 클라이언트는 클라이언트 쪽 채널에서 오류를 발생시킵니다. 하지만 WCF에서 이 오류가 치명적인 오류로 인식되지 않기 때문에 매개자의 서비스 쪽 채널은 계속 열린 상태로 유지됩니다.  
  
 이 시나리오에서 가장 좋은 방법은 서비스에서 발생한 오류가 치명적인지 확인하고, 치명적인 오류인 경우 다음 코드 조각에 표시된 것처럼 매개자가 해당 서비스 쪽 채널에 오류를 발생시키는 것입니다.  
  
```csharp  
catch (Exception e)  
{  
    bool faulted = service.State == CommunicationState.Faulted;  
    service.Abort();  
    if (faulted)  
    {  
        throw new ApplicationException(e.Message);  
    }  
    else  
    {  
        throw;  
    }  
}  
```  
  
## <a name="see-also"></a>참조

- [WCF 오류 처리](wcf-error-handling.md)
- [계약 및 서비스에서 오류 지정 및 처리](specifying-and-handling-faults-in-contracts-and-services.md)
