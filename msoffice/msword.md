# MS Word #

## Dividir un documento en páginas ##

Fuente: https://www.extendoffice.com/documents/word/5415-split-word-document-every-x-pages.html

Nota: Falla con algún tipo de formado y con el salto de página final.

Código VBA
----------
    Sub DocumentSplitter()
        Dim xDoc As Document, xNewDoc As Document
        Dim xSplit As String, xCount As Long, xLast As Long
        Dim xRngSplit As Range, xDocName As String, xFileExt As String
        Dim xPageCount As Integer
        Dim xShell As Object, xFolder As Object, xFolderItem As Object
        Dim xFilePath As String
        On Error Resume Next
        Set xDoc = Application.ActiveDocument
        Set xShell = CreateObject("Shell.Application")
        Set xFolder = xShell.BrowseforFolder(0, "Select a Folder:", 0, 0)
        If TypeName(xFolder) = "Nothing" Then Exit Sub
        Set xFolderItem = xFolder.Self
        xFilePath = xFolderItem.Path & "\"
        Application.ScreenUpdating = False
        Set xNewDoc = Documents.Add(Visible:=False)
        xDoc.Content.WholeStory
        xDoc.Content.Copy
        xNewDoc.Content.PasteAndFormat wdFormatOriginalFormatting
        With xNewDoc
            xPageCount = .ActiveWindow.Panes(1).Pages.Count
    L1:     xSplit = InputBox("The document contains " & xPageCount & " pages." & _
                     vbCrLf & vbCrLf & " Please enter the page count you want to split:", "Kutools for Word", xSplit)
            If Len(Trim(xSplit)) = 0 Then Exit Sub
            If VBA.Int(xSplit) >= xPageCount Then
                MsgBox "The number is greater than the document number." & vbCrLf & "Please re-enter", vbInformation, "Kutools for Word"
                GoTo L1
            End If
            xDocName = xDoc. Name
            xFileExt = VBA.Right(xDocName, Len(xDocName) - InStrRev(xDocName, ".") + 1)
            xDocName = Left(xDocName, InStrRev(xDocName, ".") - 1) & "_"
            xFilePath = xFilePath & xDocName
            For xCount = 0 To Int(xPageCount / xSplit)
                xPageCount = .ActiveWindow.Panes(1).Pages.Count
                If xPageCount > xSplit Then
                    xLast = xSplit
                Else
                    xLast = xPageCount
                End If
                Set xRngSplit = .GoTo(What:=wdGoToPage, Name:=xLast)
                Set xRngSplit = xRngSplit.GoTo(What:=wdGoToBookmark, Name:="\page")
                xRngSplit.Start = .Range.Start
                xRngSplit.Cut
                Documents.Add
                Selection.Paste
                ActiveDocument.SaveAs FileName:=xFilePath & xCount + 1 & xFileExt, AddToRecentFiles:=False
                ActiveWindow.Close
            Next xCount
            Set xRngSplit = Nothing
            xNewDoc.Close wdDoNotSaveChanges
            Set xNewDoc = Nothing
        End With
        Application.ScreenUpdating = True
    End Sub


### Posible mejora (por estudiar) ###
https://excelsignum.com/2019/02/22/combinar-correspondencia-y-guardar-documentos-independientes/