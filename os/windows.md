# Windows

## Powershell

### Restart into Bios

```powershell
shutdown /r /fw /t 0
```

### Wireless

#### Wifi Passwords

```powershell
(netsh wlan show profiles) | Select-String "\:(.+)$" | %{$name=$_.Matches.Groups[1].Value.Trim(); $_} | %{(netsh wlan show profile name="$name" key=clear)} | Select-String "Key Content\W+\:(.+)$" | %{$pass=$_.Matches.Groups[1].Value.Trim(); $_} | %{[PSCustomObject]@{ PROFILE_NAME=$name;PASSWORD=$pass }} | Format-Table –Wrap
```

## WSL

### WSL2 Networking on VPN

{% hint style="info" %}
Use [wsl-vpnkit](https://github.com/sakai135/wsl-vpnkit)
{% endhint %}

```shell
wsl.exe -d wsl-vpnkit service wsl-vpnkit start
```

### Shutdown Distros

#### Shutdown WSL distro

```
wsl -t DISTRO-NAME
```

#### Shutdown all WSL Distros

```
wsl --shutdown
```

### K8s on WSL2

#### Rancher Desktop

SSH to VM (Powershell)

```
wsl -d rancher-desktop
```

or

```
rdctl shell
```

Run a Command

```
rdctl shell $CMD
```

#### Rancher Desktop (Linux/Mac)

```
limactl shell rancher-desktop
```
