# vba

native stuff mainly without a need to import anything

## create file

    Dim fso as Object
    Set fso = CreateObject("Scripting.FileSystemObject")
    Dim oFile as Object
    Set oFile = FSO.CreateTextFile(strPath)
    oFile.WriteLine "test" 
    oFile.Close
    Set fso = Nothing
    Set oFile = Nothing 

## download a file

    Public Function DownloadFileB(ByVal URL As String, ByVal DownloadPath As String, ByRef Username As String, ByRef Password, Optional Overwrite As Boolean = True) As Boolean
        On Error GoTo Failed

        Dim WinHttpReq          As Object: Set WinHttpReq = CreateObject("Microsoft.XMLHTTP")

        WinHttpReq.Open "GET", URL, False, Username, Password
        WinHttpReq.send

        If WinHttpReq.Status = 200 Then
            Dim oStream         As Object: Set oStream = CreateObject("ADODB.Stream")
            oStream.Open
            oStream.Type = 1
            oStream.Write WinHttpReq.responseBody
            oStream.SaveToFile DownloadPath, Abs(CInt(Overwrite)) + 1
            oStream.Close
            DownloadFileB = Len(Dir(DownloadPath)) > 0
            Exit Function
        End If

    Failed:
        DownloadFileB = False
    End Function
