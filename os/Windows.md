# Windows

## Links

* [PowerShell Documentation](https://learn.microsoft.com/en-us/powershell/)
* [Windows Commands Reference](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/windows-commands)
* [Sysinternals Suite](https://learn.microsoft.com/en-us/sysinternals/downloads/sysinternals-suite)
* [Windows Terminal Documentation](https://learn.microsoft.com/en-us/windows/terminal/)
* [winget (Windows Package Manager) Documentation](https://learn.microsoft.com/en-us/windows/package-manager/winget/)

## PowerShell - List logged users on a remote Windows Server

```powershell
qwinsta /server:<server_name>
```

## PowerShell - Map a local folder as a network drive

```powershell
subst w: c:\folder
```

## PowerShell - Kill a process by name or PID

Using `pskill` (Sysinternals):

```powershell
# Local
pskill -t excel.exe
pskill -t <PID>

# Remote
pskill -t \\SERVER excel.exe
pskill -t \\SERVER <PID>
```

Using `taskkill` (built-in):

```powershell
# Local
taskkill /f /t /im EXCEL.exe

# Remote
taskkill /s SERVER /f /t /im EXCEL.exe
```

## PowerShell - Copy regex matches from a file to a new file

```powershell
# All matches
Select-String -Path input.txt -Pattern "[0-9a-zA-Z ]*" -AllMatches | % { $_.Matches } | Select-Object Value > output.txt

# Unique, sorted
Select-String -Path input.txt -Pattern "[0-9a-zA-Z ]" -AllMatches | % { $_.Matches } | Select-Object Value -Unique | Sort-Object Value > output.txt
```

## PowerShell - Find and replace a string in a file

```powershell
$filePath = 'C:\path\to\file.ini'
$oldValue = 'OLD_VALUE'
$newValue = 'NEW_VALUE'

$file = Get-Content $filePath
$file = $file -replace $oldValue, $newValue
$file | Set-Content $filePath
```

## PowerShell - Convert XLT/XLTM spreadsheets to XLS

```powershell
[CmdletBinding(SupportsShouldProcess=$True)]
param (
    [Parameter(Mandatory=$true)]
    [string] $basedir = $null
)

function Set-Culture([System.Globalization.CultureInfo] $culture)
{
    [System.Threading.Thread]::CurrentThread.CurrentUICulture = $culture
    [System.Threading.Thread]::CurrentThread.CurrentCulture = $culture
}

function SaveAsXLS($target)
{
    $baseFile = Get-Item -path $target
    $excelApplication = New-Object -comobject Excel.Application
    $ci = New-Object system.globalization.cultureinfo("en-US")
    Set-Culture $ci
    $excelApplication.Visible = $True
    $workBooks = $excelApplication.Workbooks.Open($baseFile.FullName)
    $workBooks.SaveAs($baseFile.DirectoryName + $baseFile.BaseName + ".xls", 56)
    $excelApplication.Quit()
}

function Get-Sheets($list, $path = $pwd, [string[]]$exclude)
{
    foreach ($item in Get-ChildItem $path)
    {
        if ($exclude | Where {$item -like $_}) { continue }

        if (Test-Path $item.FullName -PathType Container)
        {
            Get-Sheets $list $item.FullName $exclude
        }
        else
        {
            if ($item.FullName.ToLower().EndsWith("xlt") -Or $item.FullName.ToLower().EndsWith("xltm"))
            {
                $i = $list.Add($item.FullName)
            }
        }
    }
}

$sheets = New-Object System.Collections.ArrayList
Get-Sheets $sheets $basedir

foreach ($sheet in $sheets) {
    SaveAsXLS($sheet)
}
```

## CMD - Check which application is using a specific port

```powershell
netstat -a -n -b -p tcp
```

## CMD - Export list of running processes to a text file

```powershell
WMIC /OUTPUT:C:\ProcessList.txt PROCESS get Caption,Commandline,Processid
```

## IIS - Recycle Application Pool from command line

Windows Server 2003:

```powershell
cscript.exe %WINDIR%\system32\iisapp.vbs /a %APPPOOL% /r
```

Windows Server 2008+:

```powershell
%windir%\system32\inetsrv\appcmd recycle apppool /apppool.name:%APPPOOL%
```

## IIS - Set permissions for DefaultAppPool on a web application folder

Reference: [How to assign permissions to ApplicationPoolIdentity account](https://serverfault.com/questions/81165/how-to-assign-permissions-to-applicationpoolidentity-account)

```powershell
icacls C:\inetpub\wwwroot\MyApp /grant "IIS APPPOOL\MyApp":(OI)(CI)(RX)
```

## IIS - List application pools with their PIDs

```powershell
C:\Windows\system32\inetsrv\appcmd.exe list wp
```

## Active Directory - List users in a group

```powershell
net group /domain <group_name>
```

## Active Directory - Query with ldifde

Extract information from a specific group:

```powershell
ldifde -d "CN=GROUP_NAME,OU=Groups,OU=SITE,DC=domain,DC=com" -f C:\output.txt
```

Extract all groups:

```powershell
ldifde -d "OU=Groups,OU=SITE,DC=domain,DC=com" -f C:\output.txt
```

Search by specific field:

```powershell
ldifde -d "DC=domain,DC=com" -r "(mail=user@domain.com)" -f log.txt
ldifde -d "DC=domain,DC=com" -r "(telephoneNumber=+55 11*)" -f log.txt
```

Alternative with `dsquery`:

```powershell
dsquery user -name Arthur*
```

## Windows Registry - Identify 32-bit vs 64-bit applications

- 32-bit applications: `HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node`
- 64-bit applications: `HKEY_LOCAL_MACHINE\SOFTWARE`

## Register a .NET Assembly in the Global Assembly Cache (GAC)

```powershell
# Run as Administrator
gacutil /i <path\assembly.dll>
```

Tool path: `C:\Program Files (x86)\Microsoft Visual Studio 8\SDK\v2.0\Bin\gacutil.exe`
