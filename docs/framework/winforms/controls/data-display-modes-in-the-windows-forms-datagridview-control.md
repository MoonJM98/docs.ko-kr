---
title: DataGridView 컨트롤의 데이터 디스플레이 모드
ms.date: 03/30/2017
helpviewer_keywords:
- data [Windows Forms], display modes
- data grids [Windows Forms], display modes
- DataGridView control [Windows Forms], display modes
ms.assetid: 9755a030-3f3f-4705-a661-ba5a48a81875
ms.openlocfilehash: ad6be647e3a36904b055fd6771f539df28938fab
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76744014"
---
# <a name="data-display-modes-in-the-windows-forms-datagridview-control"></a>Windows Forms DataGridView 컨트롤의 데이터 디스플레이 모드
<xref:System.Windows.Forms.DataGridView> 컨트롤은 바인딩, 바인딩 해제 및 가상의 세 가지 다른 모드로 데이터를 표시할 수 있습니다. 요구 사항에 따라 가장 적합 한 모드를 선택 합니다.  
  
## <a name="unbound"></a>바인딩 안 됨  
 바인딩되지 않은 모드는 프로그래밍 방식으로 관리 하는 상대적으로 적은 양의 데이터를 표시 하는 데 적합 합니다. <xref:System.Windows.Forms.DataGridView> 컨트롤을 바인딩된 모드에서 데이터 원본에 직접 연결 하지 않습니다. 대신, 일반적으로 <xref:System.Windows.Forms.DataGridViewRowCollection.Add%2A?displayProperty=nameWithType> 메서드를 사용 하 여 컨트롤을 직접 채워야 합니다.  
  
 바인딩되지 않은 모드는 정적, 읽기 전용 데이터에 특히 유용 하거나 외부 데이터 저장소와 상호 작용 하는 사용자 고유의 코드를 제공 하려는 경우에 유용 합니다. 그러나 사용자가 외부 데이터 소스와 상호 작용 하도록 하려는 경우 일반적으로 바인딩된 모드를 사용 합니다.  
  
 읽기 전용 바인딩되지 않은 <xref:System.Windows.Forms.DataGridView>를 사용 하는 예제는 [방법: 바인딩되지 않은 Windows Forms DataGridView 컨트롤 만들기](how-to-create-an-unbound-windows-forms-datagridview-control.md)를 참조 하세요.  
  
## <a name="bound"></a>Bound  
 바인딩된 모드는 데이터 저장소와의 자동 상호 작용을 사용 하 여 데이터를 관리 하는 데 적합 합니다. <xref:System.Windows.Forms.DataGridView.DataSource%2A> 속성을 설정 하 여 <xref:System.Windows.Forms.DataGridView> 컨트롤을 해당 데이터 소스에 직접 연결할 수 있습니다. 컨트롤이 데이터 바인딩될 때 데이터 행은 사용자가 명시적으로 관리할 필요 없이 푸시되 고 끌어옵니다. <xref:System.Windows.Forms.DataGridView.AutoGenerateColumns%2A> 속성이 `true`되 면 데이터 원본의 각 열에 해당 하는 열이 컨트롤에 생성 됩니다. 고유한 열을 만들려면이 속성을 `false` 설정 하 고 <xref:System.Windows.Forms.DataGridViewColumn.DataPropertyName%2A> 속성을 사용 하 여 각 열을 구성할 때 바인딩합니다. 이는 기본적으로 생성 되는 형식 이외의 열 형식을 사용 하려는 경우에 유용 합니다. 자세한 내용은 [Windows Forms DataGridView 컨트롤의 열 형식](column-types-in-the-windows-forms-datagridview-control.md)을 참조 하세요.  
  
 바인딩된 <xref:System.Windows.Forms.DataGridView> 컨트롤을 사용 하는 예제는 [연습: Windows Forms DataGridView 컨트롤의 데이터 유효성 검사](walkthrough-validating-data-in-the-windows-forms-datagridview-control.md)를 참조 하세요.  
  
 바인딩되지 않은 열을 바인딩 모드의 <xref:System.Windows.Forms.DataGridView> 컨트롤에 추가할 수도 있습니다. 사용자가 특정 행에 대 한 작업을 수행할 수 있도록 하는 단추 또는 링크의 열을 표시 하려는 경우에 유용 합니다. 바인딩된 열에서 계산 된 값이 있는 열을 표시 하는 데도 유용 합니다. <xref:System.Windows.Forms.DataGridView.CellFormatting> 이벤트에 대 한 처리기에서 계산 된 열에 대 한 셀 값을 채울 수 있습니다. 그러나 <xref:System.Data.DataSet> 또는 <xref:System.Data.DataTable>를 데이터 원본으로 사용 하는 경우에는 <xref:System.Data.DataColumn.Expression%2A?displayProperty=nameWithType> 속성을 사용 하 여 계산 열을 대신 만들 수 있습니다. 이 경우 <xref:System.Windows.Forms.DataGridView> 컨트롤은 데이터 원본의 다른 열과 마찬가지로 계산 된 열을 처리 합니다.  
  
 바인딩된 모드로 바인딩되지 않은 열을 기준으로 정렬 하는 것은 지원 되지 않습니다. 사용자가 편집 가능한 값을 포함 하는 바인딩 모드에서 바인딩되지 않은 열을 만드는 경우 바인딩된 열을 기준으로 컨트롤을 정렬할 때 이러한 값을 유지 하려면 가상 모드를 구현 해야 합니다.  
  
## <a name="virtual"></a>가상  
 가상 모드를 사용 하 여 고유한 데이터 관리 작업을 구현할 수 있습니다. 바인딩된 열을 기준으로 컨트롤을 정렬 하는 경우 바인딩 모드에서 바인딩되지 않은 열의 값을 유지 하는 데 필요 합니다. 그러나 가상 모드를 주로 사용 하는 것은 많은 양의 데이터와 상호 작용할 때 성능을 최적화 하는 것입니다.  
  
 관리 하는 캐시에 <xref:System.Windows.Forms.DataGridView> 컨트롤을 연결 하 고, 데이터 행을 푸시한 후 끌어올 때 코드를 제어 합니다. 메모리 사용 공간을 작게 유지 하기 위해 캐시는 현재 표시 되는 행 수와 크기가 비슷해야 합니다. 사용자가 새 행을 뷰로 스크롤하면 코드가 캐시에서 새 데이터를 요청 하 고 필요에 따라 메모리에서 이전 데이터를 플러시합니다.  
  
 가상 모드를 구현할 때 새 행의 추가를 롤백하는 경우와 데이터 모델에 새 행이 필요한 시기를 추적 해야 합니다. 이 기능의 정확한 구현은 데이터 모델의 구현 및 데이터 모델의 트랜잭션 의미 체계에 따라 달라 집니다. 커밋 범위가 셀 또는 행 수준에 있는지 여부입니다.  
  
 가상 모드에 대 한 자세한 내용은 [Windows Forms DataGridView 컨트롤의 가상 모드](virtual-mode-in-the-windows-forms-datagridview-control.md)를 참조 하세요. 가상 모드 이벤트를 사용 하는 방법을 보여 주는 예제는 [연습: Windows Forms DataGridView 컨트롤에서 가상 모드 구현](implementing-virtual-mode-wf-datagridview-control.md)을 참조 하세요.  
  
## <a name="see-also"></a>참고 항목

- <xref:System.Windows.Forms.DataGridView>
- <xref:System.Windows.Forms.DataGridView.DataSource%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DataGridView.VirtualMode%2A?displayProperty=nameWithType>
- <xref:System.Windows.Forms.BindingSource>
- <xref:System.Windows.Forms.DataGridViewColumn.DataPropertyName%2A?displayProperty=nameWithType>
- [Windows Forms DataGridView 컨트롤에서 데이터 표시](displaying-data-in-the-windows-forms-datagridview-control.md)
- [Windows Forms DataGridView 컨트롤의 열 형식](column-types-in-the-windows-forms-datagridview-control.md)
- [연습: 바인딩되지 않은 Windows Forms DataGridView 컨트롤 만들기](walkthrough-creating-an-unbound-windows-forms-datagridview-control.md)
- [방법: Windows Forms DataGridView 컨트롤에 데이터 바인딩](how-to-bind-data-to-the-windows-forms-datagridview-control.md)
- [Windows Forms DataGridView 컨트롤의 가상 모드](virtual-mode-in-the-windows-forms-datagridview-control.md)
- [연습: Windows Forms DataGridView 컨트롤에서 가상 모드 구현](implementing-virtual-mode-wf-datagridview-control.md)
