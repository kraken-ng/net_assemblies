# NET Assemblies

<h1 align="center">
  <br>
  <img src="https://raw.githubusercontent.com/kraken-ng/Kraken/main/static/kraken-logo-background.jpg" alt="Kraken">
</h1>

<h4 align="center">Kraken, a modular multi-language webshell coded by @secu_x11.</h4>

<p align="center">
  <a href="https://github.com/kraken-ng/Kraken/wiki/Getting-Started#requirements">Requirements</a> •
  <a href="https://github.com/kraken-ng/Kraken/wiki/Support">Support</a> •
  <a href="https://github.com/kraken-ng/Kraken/wiki/Getting-Started#installation">Install</a> •
  <a href="https://github.com/kraken-ng/Kraken/wiki/Getting-Started#usage">Usage</a> •
  <a href="https://github.com/kraken-ng/Kraken/wiki/Getting-Started#advanced-usage">Advanced Usage</a> •
  <a href="https://github.com/kraken-ng/Kraken/wiki/Contribute">Contributing</a> •
  <a href="https://github.com/kraken-ng/Kraken/wiki/FAQ-&-Troubleshooting">FAQ</a> •
  <a href="https://github.com/kraken-ng/Kraken/wiki/Acknowledgments-&-References">Acknowledgments</a>
</p>

---

This repository contains the **NET Assemblies** to be used in [Kraken](https://github.com/kraken-ng/Kraken) via [execute_assembly](https://github.com/kraken-ng/modules/tree/main/execute_assembly) module.

List of available NET Assemblies:

- [**Dummy**](bin/Dummy/README.md): dummy binary to validate the loading of NET Assemblies (it simply returns the arguments passed in the NET Assembly invocation, as an "echo").
- [**BadPotato-mod**](bin/BadPotato-mod/README.md): is a modified version of the original BadPotato that allows you to elevate privileges to **NT AUTHORITY SYSTEM** by abusing **SeImpersonate** Privilege.
- [**PrinterNotifyPotato-mod**](bin/PrinterNotifyPotato-mod/README.md): is a modified version of the original PrinterNotifyPotato that allows you to elevate privileges to **NT AUTHORITY SYSTEM** by abusing **SeImpersonate** Privilege.
- [**McpManagementPotato-mod**](bin/McpManagementPotato-mod/README.md): is a modified version of the original McpManagementPotato that allows you to elevate privileges to **NT AUTHORITY SYSTEM** by abusing **SeImpersonate** Privilege.
- [**EfsPotato-mod**](bin/EfsPotato-mod/README.md): is a modified version of the original EfsPotato that allows you to elevate privileges to **NT AUTHORITY SYSTEM** by abusing **SeImpersonate** Privilege.