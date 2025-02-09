Testing Python as a client for a Remoting Powershell
---


## Objective

Open a Powershell Session using a Windows box to run remote commands in a SSH host.

## Constraints

* Python client Powershell Session (From MACOS or Linux) to Windows **cannot use SSH**.


## Known limitations

When running an SSH call from PowerShell (WinRM), 
    SSH fails to attach PStty (Pseudo Terminal) to the powershell session. <more details later>

To work around that one can, still run non-interactive command call through SSH. 


## Virtual Environment

The virtual environment running structure has:

1. A Linux Host (target Host)
2. A windows Server (Bastion)
3. MacOs source/client host. 

## Other Tests 


### Win To Win
A few specifics, 

both Windows are of a Domain, therefore I use Basic User/Password Authentication. 
    By default, that happens over HTTP (insecure).
To encrypt I created a local self-signed Certificate in the Bastion Host, then. run the Session with `-UseSSL` and export credentials. 
<add-link from tutoral reviewed>

1. client is a windows host. 
   * This was to validate the ssh from Powershell (without the initial SSH support)
   
    In this test WinA Enter-PSSesion ...
   * WinB (Bastion),
2. Once Session is estabilished
   3. Run commands through ssh against Linux (Target Host). 
      * Not critical, but setting up key authentication, speeds up command execution
### Mac to Win

Initial road bumps:
    
* After installing powershell, fix OpenSSL libraries <links>
* Setup SSL on Windows Bastion Hosts (-UseSSL) and set HTTPS firewall rule. (Required on MACOS) connections
* Add Ips to TrustedHosts
---



## Draft & saved commands


```powershell

winrm create winrm/config/Listener?Address=*+Transport=HTTPS 
    @{Hostname=”<your_server_dns_name_or_whatever_you_like>”; 
    CertificateThumbprint=”<certificate_thumbprint_from powershell>”}
```

