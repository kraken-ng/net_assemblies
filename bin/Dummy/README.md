# Dummy

Dummy is a test binary to check that the loading of NET Assemblies using Kraken modules is working correctly. This binary simply acts as an "echoer".

## Examples

An example of execution using Kraken:

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
  execute_assembly -f ~/Kraken/test/net_assemblies/badpotato_net40_x64.exe -n BadPotato -c Program -m call
  execute_assembly -f ~/Kraken/test/net_assemblies/dummy_net40_x64.exe -n BadPotato -c Program -m call -- Ping
  execute_assembly -f ~/Kraken/test/net_assemblies/dummy_net40_x64.exe -n BadPotato -c Program -m call -- Ping -h --help

(ST) IIS APPPOOL\DefaultAppPool@DESKTOP-8D0OEQR:C:/inetpub/wwwroot$ execute_assembly -f /home/secu/net_assemblies/bin/Dummy/dummy_net40_x64.exe -n Dummy -c Program -m call -- arg1 arg2 arg3
arg1<space>arg2<space>arg3

(ST) IIS APPPOOL\DefaultAppPool@DESKTOP-8D0OEQR:C:/inetpub/wwwroot$ exit
```
