---
title: '방법: 활성 MDI 자식으로 데이터 전송'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- child forms
- MDI [Windows Forms], sending data to forms
- Clipboard [Windows Forms], pasting
- Clipboard [Windows Forms], getting data from
ms.assetid: 1047d2fe-1235-46db-aad9-563aea1d743b
ms.openlocfilehash: 563be8494cb84dc74b45985d3ba74e4b6a07eb8a
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/12/2020
ms.locfileid: "79182491"
---
# <a name="how-to-send-data-to-the-active-mdi-child"></a>방법: 활성 MDI 자식으로 데이터 전송
종종 [MDI(다중 문서 인터페이스) 응용 프로그램의](multiple-document-interface-mdi-applications.md)컨텍스트 내에서 사용자가 클립보드의 데이터를 MDI 응용 프로그램에 붙여넣는 경우와 같이 활성 자식 창으로 데이터를 보내야 합니다.  
  
> [!NOTE]
> 포커스가 있는 하위 창을 확인하고 해당 내용을 클립보드로 보내는 자세한 내용은 [활성 MDI 자식 확인](how-to-determine-the-active-mdi-child.md)을 참조하십시오.  
  
### <a name="to-send-data-to-the-active-mdi-child-window-from-the-clipboard"></a>클립보드에서 활성 MDI 자식 창으로 데이터를 보내려면  
  
1. 메서드 내에서 클립보드의 텍스트를 활성 자식 양식의 활성 컨트롤에 복사합니다.  
  
    > [!NOTE]
    > 이 예제에서는 컨트롤을 포함하는 하나`Form1`이상의 MDI 자식 창이 있는 <xref:System.Windows.Forms.RichTextBox> MDI 상위 양식()이 있다고 가정합니다. 자세한 내용은 [MDI 상위 양식 만들기](how-to-create-mdi-parent-forms.md)를 참조하십시오.  
  
    ```vb  
    Public Sub mniPaste_Click(ByVal sender As Object, _  
       ByVal e As System.EventArgs) Handles mniPaste.Click  
  
       ' Determine the active child form.  
       Dim activeChild As Form = Me.ParentForm.ActiveMDIChild  
  
       ' If there is an active child form, find the active control, which  
       ' in this example should be a RichTextBox.  
       If (Not activeChild Is Nothing) Then  
          Try  
             Dim theBox As RichTextBox = Ctype(activeChild.ActiveControl, RichTextBox)  
             If (Not theBox Is Nothing) Then  
                ' Create a new instance of the DataObject interface.  
                Dim data As IDataObject = Clipboard.GetDataObject()  
                ' If the data is text, then set the text of the
                ' RichTextBox to the text in the clipboard.  
                If (data.GetDataPresent(DataFormats.Text)) Then  
                   theBox.SelectedText = data.GetData(DataFormats.Text).ToString()  
                End If  
             End If  
          Catch  
             MessageBox.Show("You need to select a RichTextBox.")  
          End Try  
       End If  
    End Sub  
    ```  
  
    ```csharp  
    protected void mniPaste_Click (object sender, System.EventArgs e)  
    {  
      // Determine the active child form.  
       Form activeChild = this.ParentForm.ActiveMdiChild;  
  
       // If there is an active child form, find the active control, which  
       // in this example should be a RichTextBox.  
       if (activeChild != null)  
       {  
          try
          {  
             RichTextBox theBox = (RichTextBox)activeChild.ActiveControl;  
             if (theBox != null)  
             {  
                // Create a new instance of the DataObject interface.  
                IDataObject data = Clipboard.GetDataObject();  
                // If the data is text, then set the text of the
                // RichTextBox to the text in the clipboard.  
                if (data.GetDataPresent(DataFormats.Text))  
                {  
                   theBox.SelectedText = data.GetData(DataFormats.Text).ToString();
                }  
             }  
          }  
          catch
          {  
             MessageBox.Show("You need to select a RichTextBox.");  
          }  
       }  
    }  
    ```  
  
## <a name="see-also"></a>참고 항목

- [MDI(다중 문서 인터페이스) 애플리케이션](multiple-document-interface-mdi-applications.md)
- [방법: MDI 상위 폼 만들기](how-to-create-mdi-parent-forms.md)
- [방법: MDI 자식 폼 만들기](how-to-create-mdi-child-forms.md)
- [방법: 활성 MDI 자식 확인](how-to-determine-the-active-mdi-child.md)
- [방법: MDI 자식 양식 정렬](how-to-arrange-mdi-child-forms.md)
