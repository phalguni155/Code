Code
====

Source code of the project:



$large
Dim A As Byte
Dim Addr As Byte
Dim Datain[addr] As Byte
Dim Text As String * 12
Dim Sms As String * 6
Dim H As Byte
Dim W As Byte
Dim V As Integer
Dim P As Bit

Declare Sub Adc
Cursor Off
P2 = 255

P3.7 = 0
P3.6 = 0

Cls
Lcd "   Automated"
Lowerline
Lcd "   Irrigation"
Wait 5

Print "AT+CMGF=1"
Wait 2

Print "AT+CMGD=1"
Wait 2

A1:
Print "AT+CMGR=1"

Cls

Do

A = Inkey

If A = 44 Or A = 34 Then
Text = ""
Cls
End If

If A > 47 And A < 123 Then
Text = Text + Chr(a)
Lcd Chr(a)
End If

If A = 79 Then
Call Adc
If P = 1 Then
If H > 150 Or W = 0 Or V < 180 Or V > 260 Then
P3.7 = 0
P = 0
Call Sms2
End If
End If

Sms = ""
Sms = Mid(text , 1 , 4)
Lowerline
Lcd "H:" ; H ; " W:" ; W ; " V:" ; V

If Sms = "45P1" Then
Call Adc

If H < 150 And W = 1 And V > 180 And V < 260 Then
P3.7 = 1
P = 1
Cls
Lcd Sms ; "(S)"
Lowerline
Lcd "H:" ; H ; " W:" ; W ; " V:" ; V
Call Sms1

Else
P3.7 = 0
P = 0
Cls
Lcd Sms ; "(F)"
Lowerline
Lcd "H:" ; H ; " W:" ; W ; " V:" ; V
Call Sms2
End If
End If

If Sms = "45P0" Then
Call Adc
P3.7 = 0
P = 0
Cls
Lcd Sms ; "(F)"
Lowerline
Lcd "H:" ; H ; " W:" ; W ; " V:" ; V
Call Sms2
End If

Wait 3
Goto A1
End If

Loop




Sub Sms1
Print "AT+CMGD=1"
P3.6 = 1
Wait 2
P3.6 = 0
Text = ""
Sms = ""
Print "AT+CMGS=" ; Chr(34) ; "09870154954" ; Chr(34)
Wait 1
Print "SMS: " ; Sms ; "Pump On    ";
Print "H:" ; H ; " W:" ; W ; " V:" ; V ; Chr(26)
Wait 7
End Sub



Sub Sms2
Print "AT+CMGD=1"
P3.6 = 1
Wait 4
P3.6 = 0
Text = ""
Sms = ""
Print "AT+CMGS=" ; Chr(34) ; "09870154954" ; Chr(34)
Wait 1
Print "SMS: " ; Sms ; "Pump Off    ";
Print "H:" ; H ; " W:" ; W ; " V:" ; V ; Chr(26)
Wait 7
End Sub




Sub Adc
For Addr = 0 To 2

P1 = Addr
Waitms 1
P1.3 = 1
Waitms 1
P1.3 = 0
Waitms 1

Datain[addr] = 0

If P2.0 = 1 Then
Datain[addr] = Datain[addr] + 128
End If
If P2.1 = 1 Then
Datain[addr] = Datain[addr] + 64
End If
If P2.2 = 1 Then
Datain[addr] = Datain[addr] + 32
End If
If P2.3 = 1 Then
Datain[addr] = Datain[addr] + 16
End If
If P2.4 = 1 Then
Datain[addr] = Datain[addr] + 8
End If
If P2.5 = 1 Then
Datain[addr] = Datain[addr] + 4
End If
If P2.6 = 1 Then
Datain[addr] = Datain[addr] + 2
End If
If P2.7 = 1 Then
Datain[addr] = Datain[addr] + 1
End If

If Addr = 0 Then
H = Datain[addr]
End If

If Addr = 1 Then
W = Datain[addr]
If W > 50 Then
W = 1
Else
W = 0
End If
End If

If Addr = 2 Then
V = Datain[addr]
V = V * 4
End If

Next

Lowerline
Lcd "                       "
Lowerline
Lcd "H:" ; H ; " W:" ; W ; " V:" ; V


End Sub
