---
title: '방법: 백그라운드 스레드를 사용하여 파일 검색'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Multithreaded Windows Forms Control sample [Windows Forms]
- custom controls [Windows Forms], multithreading
- threading [Windows Forms], custom controls
- custom controls [Windows Forms], samples
ms.assetid: 7fe3956f-5b8f-4f78-8aae-c9eb0b28f13a
ms.openlocfilehash: a58792ef6356c84d7d0c195eed21269e4036aacc
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/12/2020
ms.locfileid: "79182084"
---
# <a name="how-to-use-a-background-thread-to-search-for-files"></a>방법: 백그라운드 스레드를 사용하여 파일 검색
<xref:System.ComponentModel.BackgroundWorker> 구성 요소는 <xref:System.Threading> 네임스페이스를 대체하고 기능을 추가합니다. 그러나 네임스페이스는 <xref:System.Threading> 이전 버전과의 호환성과 이후 사용모두에 대해 유지됩니다(원하는 경우). 자세한 내용은 [백그라운드 작업자 구성 요소 개요를](backgroundworker-component-overview.md)참조하십시오.

 Windows Forms는 본질적으로 아파트 스레드가 있는 기본 Win32 창을 기반으로 하기 때문에 단일 스레드 아파트(STA) 모델을 사용합니다. STA 모델은 모든 스레드에서 창을 만들 수 있지만 일단 생성된 스레드를 전환할 수 없으며 모든 함수 호출이 생성 스레드에서 발생해야 함을 의미합니다. Windows Forms 외부에서 .NET 프레임워크의 클래스는 무료 스레딩 모델을 사용합니다. .NET 프레임워크의 스레딩에 대한 자세한 내용은 [스레딩](../../../standard/threading/index.md)을 참조하십시오.

 STA 모델은 컨트롤의 생성 스레드 외부에서 호출해야 하는 컨트롤의 메서드를 컨트롤의 생성 스레드에 마샬링해야 합니다. 기본 <xref:System.Windows.Forms.Control> 클래스는 이<xref:System.Windows.Forms.Control.Invoke%2A>목적을 <xref:System.Windows.Forms.Control.BeginInvoke%2A>위해 <xref:System.Windows.Forms.Control.EndInvoke%2A>여러 메서드 (, 및) 를 제공합니다. <xref:System.Windows.Forms.Control.Invoke%2A>동기 메서드 호출을 만듭니다. <xref:System.Windows.Forms.Control.BeginInvoke%2A> 비동기 메서드 호출을 만듭니다.

 리소스 를 많이 사용하는 작업에 컨트롤에서 다중 스레딩을 사용하는 경우 리소스 집약적인 계산이 백그라운드 스레드에서 실행되는 동안 사용자 인터페이스는 응답성을 유지할 수 있습니다.

 다음 샘플`DirectorySearcher`() 은 백그라운드 스레드를 사용하여 지정된 검색 문자열과 일치하는 파일에 대한 디렉터리를 재귀적으로 검색한 다음 목록 상자를 검색 결과와 함께 채우는 다중 스레드 Windows Forms 컨트롤을 보여 준다. 샘플에서 설명한 주요 개념은 다음과 같습니다.

- `DirectorySearcher`검색을 수행할 새 스레드를 시작합니다. 스레드는 도우미 `ThreadProcedure` `RecurseDirectory` 메서드를 호출하여 실제 검색을 수행하고 목록 상자를 채우는 메서드를 실행합니다. 그러나 목록 상자를 채우려면 다음 두 글머리 기호 항목에 설명된 대로 스레드 간 호출이 필요합니다.

- `DirectorySearcher`목록 상자에 `AddFiles` 파일을 추가하는 방법을 정의합니다. 그러나 `RecurseDirectory` 을 만든 `DirectorySearcher`STA `AddFiles` `AddFiles` 스레드에서만 실행할 수 있으므로 직접 호출할 수 없습니다.

- `RecurseDirectory` 호출할 `AddFiles` 수 있는 유일한 방법은 스레드 간 호출(즉, <xref:System.Windows.Forms.Control.BeginInvoke%2A> `DirectorySearcher`를 `AddFiles` 의 생성 스레드로 호출하거나 <xref:System.Windows.Forms.Control.Invoke%2A> 마샬링하는 것)을 사용하는 것입니다. `RecurseDirectory`호출을 비동기적으로 만들 수 있도록 사용합니다. <xref:System.Windows.Forms.Control.BeginInvoke%2A>

- 메서드를 마샬링하려면 함수 포인터 또는 콜백과 동일해야 합니다. 이 작업은 .NET 프레임워크의 대리자를 사용하여 수행됩니다. <xref:System.Windows.Forms.Control.BeginInvoke%2A>대리자를 인수로 합니다. `DirectorySearcher``FileListDelegate`따라서 대리자 ()를 `AddFiles` 정의합니다)는 `FileListDelegate` 생성자의 인스턴스에 바인딩하고 이 <xref:System.Windows.Forms.Control.BeginInvoke%2A>대리자 인스턴스를 에 전달합니다. `DirectorySearcher`또한 검색이 완료될 때 마샬링되는 이벤트 대리자를 정의합니다.

```vb
Option Strict
Option Explicit

Imports System.IO
Imports System.Threading
Imports System.Windows.Forms

Namespace Microsoft.Samples.DirectorySearcher
   ' <summary>
   '      This class is a Windows Forms control that implements a simple directory searcher.
   '      You provide, through code, a search string and it will search directories on
   '      a background thread, populating its list box with matches.
   ' </summary>
   Public Class DirectorySearcher
      Inherits Control
      ' Define a special delegate that handles marshaling
      ' lists of file names from the background directory search
      ' thread to the thread that contains the list box.
      Delegate Sub FileListDelegate(files() As String, startIndex As Integer, count As Integer)

      Private _listBox As ListBox
      Private _searchCriteria As String
      Private _searching As Boolean
      Private _deferSearch As Boolean
      Private _searchThread As Thread
      Private _fileListDelegate As FileListDelegate
      Private _onSearchComplete As EventHandler

      Public Sub New()
         _listBox = New ListBox()
         _listBox.Dock = DockStyle.Fill

         Controls.Add(_listBox)

         _fileListDelegate = New FileListDelegate(AddressOf AddFiles)
         _onSearchComplete = New EventHandler(AddressOf OnSearchComplete)
      End Sub

      Public Property SearchCriteria() As String
         Get
            Return _searchCriteria
         End Get
         Set
            ' If currently searching, abort
            ' the search and restart it after
            ' setting the new criteria.
            '
            Dim wasSearching As Boolean = Searching

            If wasSearching Then
               StopSearch()
            End If

            _listBox.Items.Clear()
            _searchCriteria = value

            If wasSearching Then
               BeginSearch()
            End If
         End Set
      End Property

      Public ReadOnly Property Searching() As Boolean
         Get
            Return _searching
         End Get
      End Property

      Public Event SearchComplete As EventHandler

      ' <summary>
      ' This method is called from the background thread.  It is called through
      ' a BeginInvoke call so that it is always marshaled to the thread that
      ' owns the list box control.
      ' </summary>
      ' <param name="files"></param>
      ' <param name="startIndex"></param>
      ' <param name="count"></param>
      Private Sub AddFiles(files() As String, startIndex As Integer, count As Integer)
         While count > 0
            count -= 1
            _listBox.Items.Add(files((startIndex + count)))
         End While
      End Sub

      Public Sub BeginSearch()
         ' Create the search thread, which
         ' will begin the search.
         ' If already searching, do nothing.
         '
         If Searching Then
            Return
         End If

         ' Start the search if the handle has
         ' been created. Otherwise, defer it until the
         ' handle has been created.
         If IsHandleCreated Then
            _searchThread = New Thread(New ThreadStart(AddressOf ThreadProcedure))
            _searching = True
            _searchThread.Start()
         Else
            _deferSearch = True
         End If
      End Sub

      Protected Overrides Sub OnHandleDestroyed(e As EventArgs)
         ' If the handle is being destroyed and you are not
         ' recreating it, then abort the search.
         If Not RecreatingHandle Then
            StopSearch()
         End If
         MyBase.OnHandleDestroyed(e)
      End Sub

      Protected Overrides Sub OnHandleCreated(e As EventArgs)
         MyBase.OnHandleCreated(e)
         If _deferSearch Then
            _deferSearch = False
            BeginSearch()
         End If
      End Sub

      ' <summary>
      ' This method is called by the background thread when it has
      ' finished the search.
      ' </summary>
      ' <param name="sender"></param>
      ' <param name="e"></param>
      Private Sub OnSearchComplete(sender As Object, e As EventArgs)
         RaiseEvent SearchComplete(sender, e)
      End Sub

      Public Sub StopSearch()
         If Not _searching Then
            Return
         End If

         If _searchThread.IsAlive Then
            _searchThread.Abort()
            _searchThread.Join()
         End If

         _searchThread = Nothing
         _searching = False
      End Sub

      ' <summary>
      ' Recurses the given path, adding all files on that path to
      ' the list box. After it finishes with the files, it
      ' calls itself once for each directory on the path.
      ' </summary>
      ' <param name="searchPath"></param>
      Private Sub RecurseDirectory(searchPath As String)
         ' Split searchPath into a directory and a wildcard specification.
         '
         Dim directoryPath As String = Path.GetDirectoryName(searchPath)
         Dim search As String = Path.GetFileName(searchPath)

         ' If a directory or search criteria are not specified, then return.
         '
         If directoryPath Is Nothing Or search Is Nothing Then
            Return
         End If

         Dim files() As String

         ' File systems like NTFS that have
         ' access permissions might result in exceptions
         ' when looking into directories without permission.
         ' Catch those exceptions and return.
         Try
            files = Directory.GetFiles(directoryPath, search)
         Catch e As UnauthorizedAccessException
            Return
         Catch e As DirectoryNotFoundException
            Return
         End Try

         ' Perform a BeginInvoke call to the list box
         ' in order to marshal to the correct thread. It is not
         ' very efficient to perform this marshal once for every
         ' file, so batch up multiple file calls into one
         ' marshal invocation.
         Dim startingIndex As Integer = 0
         While startingIndex < files.Length
            ' Batch up 20 files at once, unless at the
            ' end.
            '
            Dim count As Integer = 20
            If count + startingIndex >= files.Length Then
               count = files.Length - startingIndex
            End If
            ' Begin the cross-thread call. Because you are passing
            ' immutable objects into this invoke method, you do not have to
            ' wait for it to finish. If these were complex objects, you would
            ' have to either create new instances of them or
            ' wait for the thread to process this invoke before modifying
            ' the objects.
            Dim r As IAsyncResult = BeginInvoke(_fileListDelegate, New Object() {files, startingIndex, count})
            startingIndex += count
         End While
         ' Now that you have finished the files in this directory, recurse
         ' for each subdirectory.
         Dim directories As String() = Directory.GetDirectories(directoryPath)
         Dim d As String
         For Each d In  directories
            RecurseDirectory(Path.Combine(d, search))
         Next d
      End Sub

      '/ <summary>
      '/ This is the actual thread procedure. This method runs in a background
      '/ thread to scan directories. When finished, it simply exits.
      '/ </summary>
      Private Sub ThreadProcedure()
         ' Get the search string. Individual
         ' field assigns are atomic in .NET, so you do not
         ' need to use any thread synchronization to grab
         ' the string value here.
         Try
            Dim localSearch As String = SearchCriteria

            ' Now, search the file system.
            '
            RecurseDirectory(localSearch)
         Finally
            ' You are done with the search, so update.
            '
            _searching = False

            ' Raise an event that notifies the user that
            ' the search has terminated.
            ' You do not have to do this through a marshaled call, but
            ' marshaling is recommended for the following reason:
            ' Users of this control do not know that it is
            ' multithreaded, so they expect its events to
            ' come back on the same thread as the control.
            BeginInvoke(_onSearchComplete, New Object() {Me, EventArgs.Empty})
         End Try
      End Sub
   End Class
End Namespace
```

```csharp
namespace Microsoft.Samples.DirectorySearcher
{
   using System;
   using System.IO;
   using System.Threading;
   using System.Windows.Forms;

   /// <summary>
   ///      This class is a Windows Forms control that implements a simple directory searcher.
   ///      You provide, through code, a search string and it will search directories on
   ///      a background thread, populating its list box with matches.
   /// </summary>
   public class DirectorySearcher : Control
   {
      // Define a special delegate that handles marshaling
      // lists of file names from the background directory search
      // thread to the thread that contains the list box.
      private delegate void FileListDelegate(string[] files, int startIndex, int count);

      private ListBox listBox;
      private string  searchCriteria;
      private bool searching;
      private bool deferSearch;
      private Thread searchThread;
      private FileListDelegate fileListDelegate;
      private EventHandler onSearchComplete;

      public DirectorySearcher()
      {
         listBox = new ListBox();
         listBox.Dock = DockStyle.Fill;

         Controls.Add(listBox);

         fileListDelegate = new FileListDelegate(AddFiles);
         onSearchComplete = new EventHandler(OnSearchComplete);
      }

      public string SearchCriteria
      {
         get
         {
            return searchCriteria;
         }
         set
         {
            // If currently searching, abort
            // the search and restart it after
            // setting the new criteria.
            //
            bool wasSearching = Searching;

            if (wasSearching)
            {
               StopSearch();
            }

            listBox.Items.Clear();
            searchCriteria = value;

            if (wasSearching)
            {
               BeginSearch();
            }
         }
      }

      public bool Searching
      {
         get
         {
            return searching;
         }
      }

      public event EventHandler SearchComplete;

      /// <summary>
      /// This method is called from the background thread. It is called through
      /// a BeginInvoke call so that it is always marshaled to the thread that
      /// owns the list box control.
      /// </summary>
      /// <param name="files"></param>
      /// <param name="startIndex"></param>
      /// <param name="count"></param>
      private void AddFiles(string[] files, int startIndex, int count)
      {
         while(count-- > 0)
         {
            listBox.Items.Add(files[startIndex + count]);
         }
      }

      public void BeginSearch()
      {
         // Create the search thread, which
         // will begin the search.
         // If already searching, do nothing.
         //
         if (Searching)
         {
            return;
         }

         // Start the search if the handle has
         // been created. Otherwise, defer it until the
         // handle has been created.
         if (IsHandleCreated)
         {
            searchThread = new Thread(new ThreadStart(ThreadProcedure));
            searching = true;
            searchThread.Start();
         }
         else
         {
            deferSearch = true;
         }
      }

      protected override void OnHandleDestroyed(EventArgs e)
      {
         // If the handle is being destroyed and you are not
         // recreating it, then abort the search.
         if (!RecreatingHandle)
         {
            StopSearch();
         }
         base.OnHandleDestroyed(e);
      }

      protected override void OnHandleCreated(EventArgs e)
      {
         base.OnHandleCreated(e);
         if (deferSearch)
         {
            deferSearch = false;
            BeginSearch();
         }
      }

      /// <summary>
      /// This method is called by the background thread when it has finished
      /// the search.
      /// </summary>
      /// <param name="sender"></param>
      /// <param name="e"></param>
      private void OnSearchComplete(object sender, EventArgs e)
      {
         if (SearchComplete != null)
         {
            SearchComplete(sender, e);
         }
      }

      public void StopSearch()
      {
         if (!searching)
         {
            return;
         }

         if (searchThread.IsAlive)
         {
            searchThread.Abort();
            searchThread.Join();
         }

         searchThread = null;
         searching = false;
      }

      /// <summary>
      /// Recurses the given path, adding all files on that path to
      /// the list box. After it finishes with the files, it
      /// calls itself once for each directory on the path.
      /// </summary>
      /// <param name="searchPath"></param>
      private void RecurseDirectory(string searchPath)
      {
         // Split searchPath into a directory and a wildcard specification.
         //
         string directory = Path.GetDirectoryName(searchPath);
         string search = Path.GetFileName(searchPath);

         // If a directory or search criteria are not specified, then return.
         //
         if (directory == null || search == null)
         {
            return;
         }

         string[] files;

         // File systems like NTFS that have
         // access permissions might result in exceptions
         // when looking into directories without permission.
         // Catch those exceptions and return.
         try
         {
            files = Directory.GetFiles(directory, search);
         }
         catch(UnauthorizedAccessException)
         {
            return;
         }
         catch(DirectoryNotFoundException)
         {
            return;
         }

         // Perform a BeginInvoke call to the list box
         // in order to marshal to the correct thread. It is not
         // very efficient to perform this marshal once for every
         // file, so batch up multiple file calls into one
         // marshal invocation.
         int startingIndex = 0;

         while(startingIndex < files.Length)
         {
            // Batch up 20 files at once, unless at the
            // end.
            //
            int count = 20;
            if (count + startingIndex >= files.Length)
            {
               count = files.Length - startingIndex;
            }

            // Begin the cross-thread call. Because you are passing
            // immutable objects into this invoke method, you do not have to
            // wait for it to finish. If these were complex objects, you would
            // have to either create new instances of them or
            // wait for the thread to process this invoke before modifying
            // the objects.
            IAsyncResult r = BeginInvoke(fileListDelegate, new object[] {files, startingIndex, count});
            startingIndex += count;
         }

         // Now that you have finished the files in this directory, recurse for
         // each subdirectory.
         string[] directories = Directory.GetDirectories(directory);
         foreach(string d in directories)
         {
            RecurseDirectory(Path.Combine(d, search));
         }
      }

      /// <summary>
      /// This is the actual thread procedure. This method runs in a background
      /// thread to scan directories. When finished, it simply exits.
      /// </summary>
      private void ThreadProcedure()
      {
         // Get the search string. Individual
         // field assigns are atomic in .NET, so you do not
         // need to use any thread synchronization to grab
         // the string value here.
         try
         {
            string localSearch = SearchCriteria;

            // Now, search the file system.
            //
            RecurseDirectory(localSearch);
         }
         finally
         {
            // You are done with the search, so update.
            //
            searching = false;

            // Raise an event that notifies the user that
            // the search has terminated.
            // You do not have to do this through a marshaled call, but
            // marshaling is recommended for the following reason:
            // Users of this control do not know that it is
            // multithreaded, so they expect its events to
            // come back on the same thread as the control.
            BeginInvoke(onSearchComplete, new object[] {this, EventArgs.Empty});
         }
      }
   }
}
```

## <a name="using-the-multithreaded-control-on-a-form"></a>양식에서 다중 스레드 컨트롤 사용
 다음 예제에서는 양식에서 다중 스레드 `DirectorySearcher` 컨트롤을 사용할 수 있는 방법을 보여 주입니다.

```vb
Option Explicit
Option Strict

Imports System.Collections
Imports System.ComponentModel
Imports System.Data
Imports System.Drawing
Imports System.Windows.Forms
Imports Microsoft.Samples.DirectorySearcher

Namespace SampleUsage

   ' <summary>
   '      Summary description for Form1.
   ' </summary>
   Public Class Form1
      Inherits System.Windows.Forms.Form
      Private WithEvents directorySearcher As DirectorySearcher
      Private searchText As System.Windows.Forms.TextBox
      Private searchLabel As System.Windows.Forms.Label
      Private WithEvents searchButton As System.Windows.Forms.Button

      Public Sub New()
         '
         ' Required for Windows Forms designer support.
         '
         InitializeComponent()
         '
         ' Add any constructor code after InitializeComponent call here.
         '
      End Sub

      #Region "Windows Form Designer generated code"
      ' <summary>
      '      Required method for designer support. Do not modify
      '      the contents of this method with the code editor.
      ' </summary>
      Private Sub InitializeComponent()
         Me.directorySearcher = New Microsoft.Samples.DirectorySearcher.DirectorySearcher()
         Me.searchButton = New System.Windows.Forms.Button()
         Me.searchText = New System.Windows.Forms.TextBox()
         Me.searchLabel = New System.Windows.Forms.Label()
         Me.directorySearcher.Anchor = System.Windows.Forms.AnchorStyles.Top Or System.Windows.Forms.AnchorStyles.Bottom Or System.Windows.Forms.AnchorStyles.Left Or System.Windows.Forms.AnchorStyles.Right
         Me.directorySearcher.Location = New System.Drawing.Point(8, 72)
         Me.directorySearcher.SearchCriteria = Nothing
         Me.directorySearcher.Size = New System.Drawing.Size(271, 173)
         Me.directorySearcher.TabIndex = 2
         Me.searchButton.Location = New System.Drawing.Point(8, 16)
         Me.searchButton.Size = New System.Drawing.Size(88, 40)
         Me.searchButton.TabIndex = 0
         Me.searchButton.Text = "&Search"
         Me.searchText.Anchor = System.Windows.Forms.AnchorStyles.Top Or System.Windows.Forms.AnchorStyles.Left Or System.Windows.Forms.AnchorStyles.Right
         Me.searchText.Location = New System.Drawing.Point(104, 24)
         Me.searchText.Size = New System.Drawing.Size(175, 20)
         Me.searchText.TabIndex = 1
         Me.searchText.Text = "c:\*.cs"
         Me.searchLabel.ForeColor = System.Drawing.Color.Red
         Me.searchLabel.Location = New System.Drawing.Point(104, 48)
         Me.searchLabel.Size = New System.Drawing.Size(176, 16)
         Me.searchLabel.TabIndex = 3
         Me.ClientSize = New System.Drawing.Size(291, 264)
         Me.Controls.AddRange(New System.Windows.Forms.Control() {Me.searchLabel, Me.directorySearcher, Me.searchText, Me.searchButton})
         Me.Text = "Search Directories"
      End Sub
      #End Region

      ' <summary>
      '    The main entry point for the application.
      ' </summary>
      <STAThread()> _
      Shared Sub Main()
         Application.Run(New Form1())
      End Sub

      Private Sub searchButton_Click(sender As Object, e As System.EventArgs) Handles searchButton.Click
         directorySearcher.SearchCriteria = searchText.Text
         searchLabel.Text = "Searching..."
         directorySearcher.BeginSearch()
      End Sub

      Private Sub directorySearcher_SearchComplete(sender As Object, e As System.EventArgs) Handles directorySearcher.SearchComplete
         searchLabel.Text = String.Empty
      End Sub
   End Class
End Namespace
```

```csharp
namespace SampleUsage
{
   using System;
   using System.Collections;
   using System.ComponentModel;
   using System.Data;
   using System.Drawing;
   using System.Windows.Forms;
   using Microsoft.Samples.DirectorySearcher;

   /// <summary>
   ///      Summary description for Form1.
   /// </summary>
   public class Form1 : System.Windows.Forms.Form
   {
      private DirectorySearcher directorySearcher;
      private System.Windows.Forms.TextBox searchText;
      private System.Windows.Forms.Label searchLabel;
      private System.Windows.Forms.Button searchButton;

      public Form1()
      {
         //
         // Required for Windows Forms designer support.
         //
         InitializeComponent();

         //
         // Add any constructor code after InitializeComponent call here.
         //
      }

      #region Windows Form Designer generated code
      /// <summary>
      ///      Required method for designer support. Do not modify
      ///      the contents of this method with the code editor.
      /// </summary>
      private void InitializeComponent()
      {
         this.directorySearcher = new Microsoft.Samples.DirectorySearcher.DirectorySearcher();
         this.searchButton = new System.Windows.Forms.Button();
         this.searchText = new System.Windows.Forms.TextBox();
         this.searchLabel = new System.Windows.Forms.Label();
         this.directorySearcher.Anchor = (((System.Windows.Forms.AnchorStyles.Top | System.Windows.Forms.AnchorStyles.Bottom)
            | System.Windows.Forms.AnchorStyles.Left)
            | System.Windows.Forms.AnchorStyles.Right);
         this.directorySearcher.Location = new System.Drawing.Point(8, 72);
         this.directorySearcher.SearchCriteria = null;
         this.directorySearcher.Size = new System.Drawing.Size(271, 173);
         this.directorySearcher.TabIndex = 2;
         this.directorySearcher.SearchComplete += new System.EventHandler(this.directorySearcher_SearchComplete);
         this.searchButton.Location = new System.Drawing.Point(8, 16);
         this.searchButton.Size = new System.Drawing.Size(88, 40);
         this.searchButton.TabIndex = 0;
         this.searchButton.Text = "&Search";
         this.searchButton.Click += new System.EventHandler(this.searchButton_Click);
         this.searchText.Anchor = ((System.Windows.Forms.AnchorStyles.Top | System.Windows.Forms.AnchorStyles.Left)
            | System.Windows.Forms.AnchorStyles.Right);
         this.searchText.Location = new System.Drawing.Point(104, 24);
         this.searchText.Size = new System.Drawing.Size(175, 20);
         this.searchText.TabIndex = 1;
         this.searchText.Text = "c:\\*.cs";
         this.searchLabel.ForeColor = System.Drawing.Color.Red;
         this.searchLabel.Location = new System.Drawing.Point(104, 48);
         this.searchLabel.Size = new System.Drawing.Size(176, 16);
         this.searchLabel.TabIndex = 3;
         this.ClientSize = new System.Drawing.Size(291, 264);
         this.Controls.AddRange(new System.Windows.Forms.Control[] {this.searchLabel,
                                                        this.directorySearcher,
                                                        this.searchText,
                                                        this.searchButton});
         this.Text = "Search Directories";

      }
      #endregion

      /// <summary>
      ///    The main entry point for the application.
      /// </summary>
      [STAThread]
      static void Main()
      {
         Application.Run(new Form1());
      }

      private void searchButton_Click(object sender, System.EventArgs e)
      {
         directorySearcher.SearchCriteria = searchText.Text;
         searchLabel.Text = "Searching...";
         directorySearcher.BeginSearch();
      }

      private void directorySearcher_SearchComplete(object sender, System.EventArgs e)
      {
         searchLabel.Text = string.Empty;
      }
   }
}
```

## <a name="see-also"></a>참고 항목

- <xref:System.ComponentModel.BackgroundWorker>
- [.NET Framework에서 사용자 지정 Windows Forms 컨트롤 개발](developing-custom-windows-forms-controls.md)
- [이벤트 기반 비동기 패턴 개요](../../../standard/asynchronous-programming-patterns/event-based-asynchronous-pattern-overview.md)
