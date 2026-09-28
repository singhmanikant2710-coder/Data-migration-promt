Sub DumpFormats()
  Dim o As AccessObject, c As Control, n As String
  Open "C:\temp\bcat_formats.txt" For Output As #1
  For Each o In CurrentProject.AllForms
    If o.Name Like "frm0*" Then
      DoCmd.OpenForm o.Name, acDesign, , , , acHidden
      For Each c In Forms(o.Name).Controls
        If c.ControlType = acTextBox Then
          If c.ControlSource Like "*CustomField*" Or c.ControlSource Like "*Covenant*" Then
            Print #1, o.Name & " | " & c.Name & " | " & c.ControlSource & " | " & c.Format
          End If
        End If
      Next
      DoCmd.Close acForm, o.Name, acSaveNo
    End If
  Next
  For Each o In CurrentProject.AllReports
    If o.Name Like "rpt0*" Then
      DoCmd.OpenReport o.Name, acViewDesign, , , acHidden
      For Each c In Reports(o.Name).Controls
        If c.ControlType = acTextBox Then
          If c.ControlSource Like "*CustomField*" Or c.ControlSource Like "*Covenant*" Then
            Print #1, o.Name & " | " & c.Name & " | " & c.ControlSource & " | " & c.Format
          End If
        End If
      Next
      DoCmd.Close acReport, o.Name, acSaveNo
    End If
  Next
  Close #1
End Sub
