---
title: '방법: WebBrowser 컨트롤을 사용하여 URL로 이동'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
f1_keywords:
- WebBrowser.Navigate
helpviewer_keywords:
- WebBrowser control [Windows Forms], examples
- URLs [Windows Forms], navigating to
- WebBrowser control [Windows Forms], navigating to URLs
- examples [Windows Forms], WebBrowser control
ms.assetid: b3ec38cb-f509-4d0b-bd79-9f3611259c62
ms.openlocfilehash: f6cb26ff247bba75cc351d453314bade2d38d9f5
ms.sourcegitcommit: ee5b798427f81237a3c23d1fd81fff7fdc21e8d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84144840"
---
# <a name="how-to-navigate-to-a-url-with-the-webbrowser-control"></a>방법: WebBrowser 컨트롤을 사용하여 URL로 이동
다음 코드 예제에서는 <xref:System.Windows.Forms.WebBrowser> 컨트롤을 특정 URL로 탐색 하는 방법을 보여 줍니다.

 새 문서가 완전히 로드 되는 시기를 확인 하려면 이벤트를 처리 <xref:System.Windows.Forms.WebBrowser.DocumentCompleted> 합니다. 이 이벤트에 대 한 데모는 [방법: WebBrowser 컨트롤을 사용 하 여 인쇄](how-to-print-with-a-webbrowser-control.md)를 참조 하세요.

## <a name="example"></a>예제

```vb
Me.webBrowser1.Navigate("https://www.microsoft.com")
```

```csharp
this.webBrowser1.Navigate("https://www.microsoft.com");
```

## <a name="compiling-the-code"></a>코드 컴파일
 이 예제에는 다음 사항이 필요합니다.

- `webBrowser1`이라는 <xref:System.Windows.Forms.WebBrowser> 컨트롤

- `System` 및 `System.Windows.Forms` 어셈블리에 대한 참조

## <a name="see-also"></a>참고 항목

- <xref:System.Windows.Forms.WebBrowser>
- <xref:System.Windows.Forms.WebBrowser.DocumentCompleted?displayProperty=nameWithType>
- <xref:System.Windows.Forms.WebBrowser.Navigating?displayProperty=nameWithType>
- <xref:System.Windows.Forms.WebBrowser.Navigated?displayProperty=nameWithType>
- [WebBrowser 컨트롤](webbrowser-control-windows-forms.md)
- [방법: WebBrowser 컨트롤을 사용하여 인쇄](how-to-print-with-a-webbrowser-control.md)
