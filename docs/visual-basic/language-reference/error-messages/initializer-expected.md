---
title: 이니셜라이저가 필요합니다.
ms.date: 07/20/2015
f1_keywords:
- vbc30996
- bc30996
helpviewer_keywords:
- BC30996
ms.assetid: 6e183fe0-8888-43ed-a062-01571079455f
ms.openlocfilehash: 13fa8917f228661fc44f5e0920d91c596e250c38
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84402839"
---
# <a name="initializer-expected"></a>이니셜라이저가 필요합니다.
다음 예제와 같이 초기화 목록이 비어 있는 개체 이니셜라이저를 사용 하 여 클래스의 인스턴스를 선언 하려고 했습니다.  
  
 `' Not valid.`  
  
 `' Dim aStudent As New Student With {}`  
  
 다음 예제와 같이 이니셜라이저 목록에서 하나 이상의 필드 또는 속성을 초기화 해야 합니다.  
  
 `Dim aStudent As New Student With {.year = "Senior"}`  
  
 **오류 ID:** BC30996  
  
## <a name="to-correct-this-error"></a>이 오류를 해결하려면  
  
1. 이니셜라이저에서 하나 이상의 필드 또는 속성을 초기화 하거나 개체 이니셜라이저를 사용 하지 않습니다.  
  
## <a name="see-also"></a>참고 항목

- [개체 이니셜라이저: 명명된 형식 및 무명 형식](../../programming-guide/language-features/objects-and-classes/object-initializers-named-and-anonymous-types.md)
- [방법: 개체 이니셜라이저를 사용하여 개체 선언](../../programming-guide/language-features/objects-and-classes/how-to-declare-an-object-by-using-an-object-initializer.md)
