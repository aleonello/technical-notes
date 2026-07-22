# IIS

## Links

* [IIS Official Documentation](https://learn.microsoft.com/en-us/iis/get-started/introduction-to-iis/introduction-to-iis-architecture)
* [IIS Configuration Reference](https://learn.microsoft.com/en-us/iis/configuration/)
* [Application Request Routing (ARR) Documentation](https://learn.microsoft.com/en-us/iis/extensions/planning-for-arr/application-request-routing-version-2-overview)

## Recycle Application Pool via Command Line

**Windows Server 2003:**

```sh
cscript.exe %WINDIR%\system32\iisapp.vbs /a %APPPOOL% /r
```

**Windows Server 2008:**

```sh
%windir%\system32\inetsrv\appcmd recycle apppool /apppool.name:%APPPOOL%
```

## List Running Application Pools and PIDs

```sh
C:\Windows\system32\inetsrv\appcmd.exe list wp
```

(Windows Server 2008 only)

Reference: [appcmd list wp](http://technet.microsoft.com/en-us/library/cc771273%28v=ws.10%29.aspx)

## Set Folder Permissions for Application Pool Identity (icacls)

```sh
icacls C:\inetpub\wwwroot\MyApp /grant "IIS APPPOOL\MyApp":(OI)(CI)(RX)
```

Reference: [How to assign permissions to ApplicationPoolIdentity](http://serverfault.com/questions/81165/how-to-assign-permissions-to-applicationpoolidentity-account)

## Fix ReportViewer in IIS 7

Add a Managed Handler in IIS with the following values:

| Field | Value |
|---|---|
| Request Path | `Reserved.ReportViewerWebControl.axd` |
| Type | `Microsoft.Reporting.WebForms.HttpHandler` |
| Name | `Reserved-ReportViewerWebControl-axd` |

## Fix Excel File Blocked by IIS via Interop

**Error:**
```
System.Runtime.InteropServices.COMException (0x800A03EC): Microsoft Office Excel cannot access the file
```

Create the Desktop directory for the IIS process user:

- 64-bit Windows: `C:\Windows\SysWOW64\config\systemprofile\Desktop`
- 32-bit Windows: `C:\Windows\System32\config\systemprofile\Desktop`

Set Full Control permissions for `IIS AppPool\DefaultAppPool` on that folder.

## NGINX References

* [NGINX Tech Blog](http://nginx.com/category/tech/)
* [NGINX guide (pt-BR)](http://klauslaube.com.br/2011/12/19/nginx-poderoso-rapido-facil.html)
