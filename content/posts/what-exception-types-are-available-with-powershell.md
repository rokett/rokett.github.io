+++
date = "2016-12-29T15:19:59Z"
title = "What exception types are available with PowerShell"

+++

Throwing exceptions when an error occurs is a useful way of providing context to whatever the problem is and therefore allowing the consumer to get meaning from the error.  Ideally an exception would be accompanied with an exception type, but how do you know what exception types are available in PowerShell?

```powershell
[System.AppDomain]::CurrentDomain.GetAssemblies() | ForEach-Object {
    Try {
        $_.GetExportedTypes() | Where-Object {
            $_.Fullname -match 'Exception'
        }
    } Catch {}
} | Sort-Object -Property FullName | Select-Object -Property FullName
```

The above code will output all of the available exception types for you to choose the most appropriate.  You may need to look up exactly what information they are meant to convey, but it's a good start.