# Linux1  

## H1: Linux lere kennen

### Hulp zoeken  

`man passwd` = Hulp over het comando "passwd"  
`man 5 passwd`  = Hulp over het configuratiebestand /etc/passwd  
`man -k passwd` `apropos passwd` = Zoek in alle man-pages naar de string "passwd"  
- Binnen man-pages:
    - `q` - man-page verlaten  
    - `/` - zoeken bnnen de pagina  
    - `n` - ga naar volgende zoekresultaat  
    - `N` - ga naar vorige zoekresultaat  
- Secties, vb:  
    - 1 - commando's  
    - 5 - configuratiebestanden  
    - 8 - systeembeheercommando's  
    - Notatie: vb. `passwd(1)`, `passwd(5)`  

### Belangrijke man-pages  

`man hier` = directorystructiir Linux (Filesystem Hierarchy)  
`man builtins` = ingebuowde Bash commando's  
`man 7 glob` = wildcards in bestandsnamen (bv. \*.\*, [a-z])  

### Structuur van een commandoregel  

- "Onderdelen" gesceiden door **spaties**  
- 1e woord = **commando**  
- **Opties** veranderen het gedrag van een commando
    - beginnen met streepje (`-` of `--`)  
- **Argumenten** zijn de entiteiten waarop het commando uitgevoerd wordt  
`COMMANDO [OPTIES]... [ARGUMENTEN]...`  

(verder uitwerken)  

# Linux2
## Permissies (herhaling)
### Bestandspermissies

`ls -l` = een overzicht  
  
Instelbaar op niveau van:
- `u`: gebruiker, user   
- `g`: groep, group
- `o`: andere gebruikers, others  
- `a`: iedereen, all  

### Soorten permissies

- `r`: lezen,read  
- `w`: schrijven,write  
- `x`: execute  
    - bestanden: uitvoeren als commando  
    - directory: toegang met `cd`  

### Instellen met symbolische notatie

permissies instellen met `chmod`, symbolische notatie  
````
chmod MODUS FILE
chmod u+r FILE
      g-w
      o=x
      a
````  
Voorbeelden:  
- `chmod g+rw bestand`  
- `chhmod +x bestand`  
- `chmod u+rw,g+r,o-rm bestand`  
- `chmod a=r bestand`  

### Instellen met octale notatie

`stat -c %a BESTAND` = octale permissies opvragen  

````
  U       g       o
r w x   r - x   - - -
1 1 1   1 0 1   0 0 0
4+2+1   4+0+1   0+0+0
  7       5       0
````  
Voorbeelden:  
- `chmod 755 script`  
- `chmod 600 file`  
- `chmod 644 file`  
- `chmod 775 dir`  

## More Permissies
### Permissies van nieuwe bestanden: umask

Voorbeeld `umask`  
````
umask 0027

  file       directory
  0 6 6 6    0 7 7 7     basis
- 0 0 2 7  - 0 0 2 7     unmask
---------  -----------  
  0 6 4 0    0 7 5 0     permissies
````  

### Speciale permissies: SUID

- op bestanden met execute-permissies  
- Tijdens uitvoeren krijgt de gebruiker de rechtes an de eigenaar van het bestand.  
- symbolisc: `u+s`
- octaal: 4  

````
ls -l /bin/passwd
-rwsr-xr-x. 1 root root 28k 2017-02-11 12:02 /bin/passwd
````  

### Speciale permissies: SGID

- op bestanden met execute-permissies  
- tijdens uitvoeren krijgt de gebruiker de rechten van de groep van het bestand    
- symbolisch: `g+s`  
- octaal: 2  

````
ls -l /bin/passwd
-rwxr-sr-x. 1 root tty 20k 2017-09-22 10:55 /usr/bin/write
````  

### Speciale permissies: restricted deletion

- restricted deletion, of sticky bit
- toegepast op directories  
- een bestand mag in zo’n directory enkel door de eigenaar verwijderd worden  
- symbolisch: `+t`  
- octaal: 1  

````
ls -ld /tmp
drwxrwxrwt. 16 root root 360 2017-12-04 13:05 /tmp/
````  

## Beheer van gebruikers

### Commando’s voor gebruikers en groepen

- Gebruikers: `useradd`, `usermod`, `userdel`  
- Groepen: `groupadd`, `groupmod`, `groupdel`  
- Info opvragen: `who`, `groups`, `id`  

### Configuratiebestanden

- Gebruikers: `/etc/passwd`, `/etc/shadow`  
- Groepen: `/etc/group`, (`/etc/gshadow`, van weinig belang)  

### Primaire vs aanvullende groepen

- Primaire groep: 4e veld van `/etc/passwd` (group ID)  
- Aanvullende groepen: in `/etc/group`. Gebruiker staat niet vermeld in de primaire groep!  

primaire groep aanpassen  
````
sudo usermod -g groep gebruiker
````  

gebruiker toevoegen aan opgegeven groepen en verwijderen uit alle andere groepen  
````
sudo usermod -G groep1,groep2 gebruiker
````  

gebruiker toevoegen aan opgegeven groep, blijft lid van andere groepen  
````
sudo usermod -aG groep gebruiker
````  

### Eigenaar/groep veranderen:  

````
chown user file
chown user:group file
chgrp group file
````  

### root of administrator

- `root` oorspronkelijk de enige administrator van het systeem  
- `/etc/sudoers` tells you who can be admin
    - group `sudo` in Ubuntu/Debian distro  
    - group `wheel` in Fedora/RedHat distro  

### wachtwoord
eigen wachtwoord aanpassen  
````
passwd
````  

wachtwoord aanpassen van een gebruiker als administrator  
````
sudo passwd <username>
````  

wachtwoord genereren in hash vorm (zoals in /etc/shadow)  
````
openssl passwd -6 -salt <salt-string> <desired passwd>
e.g.
openssl passwd -6 -salt hogent nieuwWachtWoord
````  

## Streams, pipes, redirects
### Input en output

![Input Uotput](Images/InputUotput.png)  

### I/O Redirection

|Syntax       |Betekeinis                                        |
|-------------|--------------------------------------------------|
|`cmd > file` |schrijf uitvoer van `cmd` weg naar `file`         | 
|`cmd >> file`|voeg toe aan einde van `file`                     |
|`cmd 2> file`|schrijf foutboodschappen van `cmd` weg naar `file`|
|`cmd < file` |gebruik inhoud van `file` als invoer voor `cmd`   |
|`cmd1 | cmd2`|gebruik uitvoer van `cmd1` als invoer voor `cmd2` |  

### Combineren

````
# stdout en stderr apart wegschrijven
find / -type d > directories.txt 2> errors.txt

# stderr "negeren"
find / -type d > directories.txt 2> /dev/null

# stdout en stderr samen wegschrijven
find / -type d > all.txt 2>&1

# invoer én uitvoer omleiden
sort < unsorted.txt > sorted.txt 2> errors.txt
````  

### Foutboodschappen afdrukken

````
printf 'Error: %s is not a directory\n' "${dir}" >&2
````  

### Here documents

Als je meer dan één lijn wil afdrukken:  
````
cat << _EOF_
Usage: ${0} [OPT]... [ARG]..

OPTIONS:
  -h, --help  Print this help message

_EOF_
````  

Dit kan bv. ook:  
````
mysql -uroot -p"${db_password}" mysql << _EOF_
DROP DATABASE IF EXISTS drupal;
CREATE DATABASE drupal;
GRANT ALL PRIVILEGES ON drupal TO ${drupal_usr}@localhost
  IDENTIFIED BY ${drupal_password};
_EOF_
````  

### Pipes  

````
sudo apt install fortune cowsay lolcat figlet
echo ${USER} | figlet
fortune
fortune | cowsay
fortune | cowsay | lolcat
````  

## Filters

Ofwel bestand meegeven, ofwel stdin  
````
filter file1 file2...

filter < file

cmd | filter
````  

### cat, tac en shuf

- `cat`: wat binnenkomt op stdin afdrukken op stdout
- `tac`: idem, maar in omgekeerde volgorde
- `shuf`: in willekeurige volgorde  

````
cat file1 file2 > file3
tac file1
shuf cards.txt
````  

### Head en tail

- Toon (10) eerste/laatste regels

````
head /etc/passwd
tail -5 /etc/passwd
tail -f /var/log/syslog
````  

### cut, paste en join

- `cut`: selecteer kolommen uit gestructureerde tekst (bv. CSV)
- `paste`: voeg bestanden regel per regel samen
- `join`: voeg bestanden samen ahv gemeenschappelijke kolom

````
cut -d: -f1,3-4 /etc/passwd
paste -d';' users.txt passwords.txt

cat foodtypes.txt
1 Protein
2 Carbohydrate

cat foods.txt
1 Cheese 
2 Potato

join foodtypes.txt foods.txt
1 Protein Cheese
2 Carbohydrate Potato
````  

### sort en uniq

````
sort unsorted.txt
uniq -c sorted.txt
````  
Welke commando’s gebruik ik het vaakst?  

````
history | awk '{ print $2 }' | sort | uniq -c | sort -nr | head
````  

### fmt, nl en wc

- `fmt`: simple optimal text formatter
- `nl`: number lines of files
- `wc`: print newline, word, and byte counts for each file  

````
fmt -w40 some-file.txt
nl script.sh
wc thesis.md
wc --words thesis.md
ls /usr/bin | wc -l
````  

### grep

- `grep`: print lines that match patterns

````
grep root /etc/passwd
grep '^#' script.sh
sudo grep -i dhcp /var/log/syslog
````  

Moet je zoeken in een reeks directories? Gebruik  
`silversearcher-ag`  

````
ag TODO *.java
````  

### Buitenbeentje: tr

- TRanslate
- Teken per teken ipv lijn per lijn
- Enkel stdin, geen files

````
tr 'A-Z' 'a-z' < UPPERCASE.txt > lowercase.txt
tr -d '[:punct:]' < file.txt
````  

### De Stream EDitor, sed

`sed 's/foo/bar/'` = Zoeken en vervangen (1x per regel)  
`sed 's/foo/bar/g'` = "Globaal", meerdere keren per regel  
`sed '/^#/d'` = Regels die beginnen met '#' verwijderen  
`sed '/^$/d'` = Lege regels verwijderen  

### De AWK-taal

Wat tussen accolades staat wordt uitgevoerd op elke regel  

````
# Druk 4e kolom af (afgebakend door "whitespace")
awk '{ print $4 }'

# Enkel regels afdrukken die beginnen met #
awk '/^#/ { print $0 }'

# Druk kolom 2 en 4 af, gescheiden door ;
awk '{ printf "%s;%s", $2, $4 }'

# Druk de namen van de "gewone" gebruikers af
awk -F: '{ if($3 > 1000) print $1 }' /etc/passwd
````  

## Intro Bash scripts
### Een script schrijven

1. Maak bestand aan (bv. `script.sh`) met een teksteditor, bv.
    ````
    #! /bin/bash
    echo "Hallo ${USER}"
    ````  
2. Maak bestand uitvoerbaar: `chmod +x script.sh`
3. Voer uit: `./script.sh`  

### De “shebang”

- Eerste regel van een script
- Begint met `#!` (`#` = hash; `!` = bang)  
- Absoluut pad naar de interpreter voor het script, bv:
    - `#! /usr/bin/python`
    - `#! /usr/bin/ruby`
    - `#! /usr/bin/ruby`
        - = zoek in `${PATH}` naar `bash`  

### Tekst afdrukken

````
var="world"

echo "Hello ${var}"    # Binnen " " wordt substitutie toegepast
echo 'Hello ${var}'    # Binnen ' ' NIET!
````  

### Variabelen

Bash-variabelen zijn (meestal) strings.  

- Declaratie: `variabele=waarde`
    - geen spaties rond `=`
- Waarde v/e variabele opvragen: `${variable}`
- Gebruik in strings (met dubbele aanhalingstekens):  
    ````
    echo "Hello ${USER}"
    ````  

### Variabelen: codestijl

- Gebruik zoveel mogelijk de notatie `${var}`
    - accolades
- Gebruik dubbele aanhalingstekens: `"${var}"`
    - vermijd splitsen van woorden  

````
bestand="Mijn bestand.txt"
touch ${bestand}        # Fout
touch "${bestand}"      # Juist
````  

### Onbestaande variabelen opvragen

- Onbestaande variabele wordt beschouwd als lege string.
    - `set -o nounset` ⇒ script afbreken  

### Zichtbaarheid variabelen (scope)  

Enkel binnen zelfde “shell”, niet binnen “subshells”  

- Een script oproepen creëert een subshell
- Maak “globale”, of omgevingsvariabele met `export`:  

````
export VARIABLE1=value

VARIABLE2=value
export VARIABLE2
````  

### Variabelen: naamgeving

Conventie naamgeving:  
- Lokale variabelen: kleine letters, bv: `foo_bar`
- Omgevingsvariabelen: hoofdletters, bv. `FOO_BAR`  

## [Xtra]: vi(m) vs nano

### Hoe maak je een bestand aan?

1. Met teksteditor Vi/Vim: `vim bestand.txt`  
2. Met teksteditor Nano: `nano bestand.txt`
3. Leeg bestand: `touch bestand.txt`  

### Vim survival guide

|Taak                      |Commanod|
|--------------------------|--------|
|Normal mode -> insert mode|`i`     |
|Insert mode -> normal mode|`<Esc>` |
|Opslaan                   |`:w`    |
|Opslaan en afsluiten      |`:wq`   |
|Afsluiten zonder opslaan  |`:q!`   |  

Steep learning curve, great tool!  

````
sudo apt install vim-runtime     # Op Debian
sudo dnf install vim-enhanced    # Op RedHat
vimtutor
````  

## Nano  

- Exit: ^X -> Ctrl+X

## Software-installatie

### Debian dpkg

1. download een .deb package (manueel)
2. installeer met dpkg
    ````
    dpkg -i <package_name>.deb
    ````
3. Los eventuele depencencies manueel op (zie later)  

Overzicht van geïnstalleerde packages op Debian:  
````
dpkg -l
````  

### Debian apt

APT = Advanced Package Tool  

1. Zoek een package op de (aanvaarde) repository servers  
2. download de .deb package (automatisch)
3. controlleer depencencies, download eventuele extra packages  
4. installeert (achterliggend) met dpkg  

````
apt install <package_name>
````  

Bijwerken van info op de repo servers  

````
apt update
````  

Bijwerken van alle packages op jouw systeem  

````
apt upgrade
````  

Bijwerken van een enkele package op jouw systeem  

````
apt install <package_name> # nieuwe versie wordt geïnstalleerd
````  

## Debian repository servers

List of repository servers you use:  

````
ls /etc/apt/sources.list*
/etc/apt/sources.list

/etc/apt/sources.list.d:
official-package-repositories.list
````  

## Red Hat yum / dnf

- `rpm` = RedHat Package Manager (equivalent van `dpkg`)
- `yum` = Yellowdog Update Manager
    - equivalent van `apt`
-  `dnf` = DaNdiFied yum 
    - Vervangt `yum` vanaf RedHat Enterprise Linux 7
    - Ook in Fedora  

Installatie package  

````
dnf install <package_name>
````  

Bijwerken van info op de repo servers; aftoetsen van wat kan bijgewerkt worden:  

````
dnf check-update
````  

Bijwerken van alle packages op jouw systeem  

````
dnf upgrade
````  

Bijwerken van een enkele package op jouw systeem  

````
dnf upgrade <package_name>
````  

### Andere handige commando’s

Lijst geïnstalleerde packages  

````
dnf list installed
````  

Lijst beschikbare packages  

````
dnf list available
````  

Met welke package kan ik het commando `fortune` installeren?  

````
dnf provides *bin/fortune
````  

Wat zijn de dependencies van `curl`?  

````
dnf deplist curl
````  

### RedHat repository servers

````
[admin@server ~]$ ls /etc/yum.repos.d/
almalinux-ha.repo  almalinux-powertools.repo  almalinux.repo  epel-modular.repo
epel-playground.repo  epel-testing-modular.repo  epel-testing.repo  epel.repo
[admin@server ~]$ cat /etc/yum.repos.d/almalinux.repo
````  

## Netwerkconfiguratie

### Netwerkinstellingen controleren

1. IP-adres en subnetmasker
2. Default gateway
3. DNS-server

### Netwerkinstellingen opvragen

1. IP-adress/netmask: `ip address` (`ip a`)
2. Default gateway: `ip route` (`ip r`)
3. DNS-server: 
    - EL: `cat /etc/resolv.conf`
    - Debian, Fedora: `resolvectl status <interface>`  


### Wat is het IP-adres van…?

````
nslookup www.hogent.be
dig www.hogent.be
````  

Wat is mijn publiek IP-adres?  

````
curl icanhazip.com
````  

### Controleer eerst netwerkinstellingen

````
ip -4 a
````  

### Netwerkinstellingen

- `lo` (loopback): 127.0.0.1/8
- `eth0`/`enp0s3 `= 1e VirtualBox adapter (NAT): 10.0.2.15/24
- `eth1`/`enp0s8` = 2e VirtualBox adapter (Host-only): 192.168.76.2/24  

### Netwerkinstellingen aanpassen (RedHat)

````
/etc/sysconfig/network-scripts/ifcfg-<interface_name>
````  

````
NM_CONTROLLED=yes
BOOTPROTO=none
ONBOOT=yes
IPADDR=192.168.76.2
NETMASK=255.255.255.0
DEVICE=eth1
PEERDNS=no
````  

Na aanpassingen, netwerk herstarten:  

````
sudo systemctl restart network
````  

## Let’s install DHCP!
### Installatie

Zoek de naam van de package om ISC DHCP te installeren!  

### Configuratie

- Configbestand: `/etc/dhcp/dhcpd.conf`
- Zie voorbeeld: `/usr/share/doc/dhcp-server/dhcpd.conf.example`

### Opstarten systemctl

- `sudo systemctl start dhcpd`
- `systemctl status dhcpd`
- `sudo systemctl restart dhcpd`
    - Na elke wijziging config!
- `sudo systemctl enable dhcpd`
    - Start altijd bij booten

## Webserver installatie
### Doelstelling

- LAMP-stack: **L**inux + **A**pache + **M**ariaDB + **P**HP  

### Installatie software

````
sudo dnf install httpd mariadb-server php
````  

### Belangrijke directories

- `/etc/httpd/`: configuratie Apache
    - `/etc/httpd/conf/httpd.conf`
    - `/etc/httpd/conf.d/*.conf`
- `/var/www/html/`: Apache DocumentRoot
- `/var/log/httpd/`: logbestanden
    - `access_log`
    - `error_log`  

### Services opstarten

````
sudo systemctl start mariadb
sudo systemctl enable mariadb
sudo systemctl start httpd
sudo systemctl enable httpd
````  

- `start`: nu opstarten
- `enable`: automatisch opstarten bij booten  

### Test de services

````
systemctl status httpd
systemctl status mariadb
````  

- CLI webbrowser op de VM  
    - surf naar `http://localhost/`
    ````
    curl localhost
    curl 127.0.0.1
    ```` 
-  PHP testen: maak bestand `/var/www/html/info.php`
-  Surf naar: `http://localhost/info.php`
    ```` php 
    <?php phpinfo(); ?>
    ````  

### Toon sockets/poorten in gebruik

Show sockets: `ss`  

- `netstat` is obsolete, replaced by `ss`
    - `netstat` uses `/proc/net/tcp`
    - `ss` directly queries the kernel

### ss Options

|Task                |Command             |
|--------------------|--------------------|
|Show server sockets |`ss -l, --listening`|
|Show TCP sockets    |`ss -t, --tcp`      |
|Show UDP sockets    |`ss -u, --udp`      |
|Show port numbers(*)|`ss -n, --numeric`  |
|Show process(†)     |`ss -p, --processes`|  

(*) instead of service names from `/etc/services`  
(†) root permissions required  

### Example

````
sudo ss -tlnp
````  

### Logbestanden

Voorbeeld voor Apache:  

````
sudo journalctl
sudo journalctl -u httpd
sudo journalctl -flu httpd
sudo tail -f /var/log/httpd/access_log
sudo tail -f /var/log/httpd/error_log
````  

### journalctl

- `journalctl` requires root permissions
    - Or, add user to group `adm` or `systemd-journal`
 - Some “traditional” text-based log files still exist (for now?):  
    - `/var/log/messages` (gone in Fedora!)
    - `/var/log/httpd/access_log` and `error_log`

### Options

|Action                              |Command                                |
|------------------------------------|---------------------------------------|
|Show latest log and wait for changes|`journalctl -f, --follow`              |
|Show only log of SERVICE            |`journalctl -u SERVICE, --unit=SERVICE`|
|Match executable, e.g. `dhclient`   |`journalctl /usr/sbin/dhclient`        |
|Match device node, e.g. `/dev/sda`  |`journalctl /dev/sda`                  |
|Show auditd logs                    |`journalctl _TRANSPORT=audit`          |
|Show log since last boot            |`journalctl -b, --boot`                | 
|Kernel messages (like `dmesg`)      |`journalctl -k, --dmesg`               |
|Reverse output (newest first)       |`journalctl -r, --reverse`             |
|Show only errors and worse          |`journalctl -p err, --priority=err`    |
|Since yesterday                     |`journalctl --since=yesterday`         |  

Filter on time (example):  
````
journalctl --since=2018-06-00 \
           --until="2018-06-07 12:00:00"
````  

### Website vanaf GUI Linux VM bekijken

- Controleer IP-adres VM: `ip a`
    - waarschijnlijk 192.168.76.2
        - verifieer dat je GUI Linux VM een IP-adres in dit netwerk heeft
        - ping ?!
- Open webbrowser op fysiek systeem
    - surf naar `http://192.168.76.2/`  

### Database beveiligen

````
sudo mysql_secure_installation
````  

- Volg de instructies!
- kies MariaDB root-wachtwoord
    - ≠ wachtwoord Linux root!
- bevestig andere vragen (ENTER)

### Database testen: root

````
mysql -uroot -padmin mysql
...
MariaDB [mysql]> SHOW DATABASES;
MariaDB [mysql]> SELECT user,password from user;
MariaDB [mysql]> quit
````  

- `-uroot`: inloggen als MariaDB-root
- `-padmin`: gekozen wachtwoord
    - (GEEN spatie na `-p`)
- `mysql`: inloggen op database `mysql`  

## Scripting (vervolg)
### Positionele parameters

Bij uitvoeren van script zijn opties en argumenten beschikbaar via variabelen, positionele parameters  

|Variabele        |Betekenis                                 |
|-----------------|------------------------------------------|
|`${0}`           |Naam script                               |
|`${1}`, `${2}`, …|Eerste, tweede, … argument                |
|`${10}`          |Tiende argument (accolades verplicht!)    |
|`{*}`            |Alle argumenten: `${1} ${2} ${3}...`      |
|`${@}`           |Alle argumenten: `"${1}" "${2}" "${3}"...`|
|`${#}`           |Aantal positionele parameters             |  

### Positionele parameters: shift

Het commando `shift` schuift positionele parameters op naar links:  

- `${1}` verdwijnt
- `${2}` wordt `${1}`

### Positionele parameters instellen

````bash
set par1 par2 par3
echo "${1}"  #  => par1
echo "${2}"  #  => par2
echo "${3}"  #  => par3
echo "${4}"  #  =>       (lege string)
````  

### Exit-status

- Elk commando heeft een exit-status, numerieke waarde
    - Opvragen met `echo "$?"`
    - **0** => commando geslaagd, logische **true**
    - **1-255** => commando gefaald, logische **false**
- Logische operatoren in Bash zijn gebaseerd op exit-status
- Booleaanse variabelen bestaan niet  

### if

````bash
if EXPR
then
  # ...
elif EXPR
  # ...
else
  # ...
fi
````  

````bash
if [ "${#}" -gt '2' ]; then
  printf 'Expected at most 2 arguments, got %d\n' "${#}" >&2
  exit 1
fi
````  

### While-lus

````bash
while EXPR; do
  # ...
done
````  

### Until-lus

````bash
until EXPR; do
  # ...
done
````   

### While lus met teller

````bash
counter=0

while [ "${counter}" -le '10' ]; do
  echo "${counter}"
  counter=$((counter + 1))
done
````  
`$(( ... ))` is arithmetic expansion  

### For-lus

Itereren over een lijst  

````bash
for ITEM in LIST; do
  # ...
done
````  
````bash
for file in *.md; do
  printf 'Processing file %s\n' "${file}"
  # ...
done
````  

### For-lus met teller

````bash
for i in {1..10}; do
  echo "${i}"
done
````  

````bash
for i in {2..20..2}; do
  echo "${i}"
done
````  

````bash
for i in $(seq 1 10); do
  echo "${i}"
done
````  

````bash
for i in $(seq 2 2 20); do
  echo "${i}"
done
````  

“Klassieke” for-lus, komt slechts uitzonderlijk voor  

````bash
for ((i=0; i<=10; i++)); do
  echo "${i}"
done
````  

### Itereren over positionele parameters (while)

````bash
while [ "$#" -gt 0 ]; do
  printf 'Arg: %s\n' "${1}"
  # ...
  shift
done
````  

### Itereren over positionele parameters (for)

````bash
for arg in "${@}"; do
  printf 'Arg: %s\n' "${arg}"
  # ...
done
````  

## Firewall-configuratie

### Firewall-instellingen aanpassen

````
sudo systemctl status firewalld  # is de firewall actief?
sudo firewall-cmd --list-all     # = toon firewall-regels
sudo firewall-cmd --add-service http --permanent
sudo firewall-cmd --add-service https --permanent
sudo firewall-cmd --reload
````  

### Zones

- Zone = lijst van regels voor een specifieke situatie 
    - bv. in een publieke ruimte (standaard), thuisnetwerk, werk, …
- Netwerkkaarten worden toegekend aan een zone
- Vooral nuttig voor laptop
    - Server: `public` zone is meestal voldoende  

|Task                           |Command                             |
|-------------------------------|------------------------------------|
|Toon alle zones                |`firewall-cmd --get-zones`          |
|Actieve zones                  |`firewall-cmd --get-active-zones`   |
|Voeg IFACE toe aan actieve zone|`firewall-cmd --add-interface=IFACE`|
|Toon huidige regels            |`firewall-cmd --list-all`           |  

Voor `firewall-cmd` moet je root-rechten hebben!  

### Firewall-regels instellen

|Task                         |Command                           |
|-----------------------------|----------------------------------|
|Laat service toe             |`firewall-cmd --add-service=http` |
|Toon beschikbare services    |`firewall-cmd --get-services`     |
|Laat poort toe               |`firewall-cmd --add-port=8080/tcp`|
|Firewall-regels herladen     |`firewall-cmd --reload`           |
|Alle netwerkverkeer blokkeren|`firewall-cmd --panic-on`         |
|Paniekmodus uitschakelen     |`firewall-cmd --panic-off`        |  

### Persistente wijzigingen

- `--permanent` optie wordt niet onmiddellijk toegepast!
- Twee werkwijzen: 
    1. Commando 2x uitvoeren, 1x zonder, 1x mét `--permanent`
    2. Commando enkel met `--permanent` uitvoeren, firewall herladen
    ````
    sudo firewall-cmd --add-service=http --permanent
    sudo firewall-cmd --add-service=https --permanent
    sudo firewall-cmd --reload
    ````  

## Security Enhanced Linux
### SELinux

- SELinux: Security Enhanced Linux
    - “Mandatory Access Control”
    - Ingebouwd in de Linux kernel
    - Vooral op RedHat!
- AppArmor: 
    - equivalent op Debian/Ubuntu
    - niet behandeld in deze cursus

### Staat SELinux aan?

````
getenforce 
````  

Let op! Dit werkt niet op de Linux Mint-VM!  

### Hoofdconfiguratiebestand

````
cat /etc/selinux/config
````  

### 3 soorten SELinux-instellingen

- Booleans: `getsebool`, `setsebool`
- Contexts, labels: `ls -Z`, `chcon`, `restorecon`
- Policy modules: `sepolicy`

In de meeste gevallen is configuratie van booleans/context voldoende!  

### Controleer de context van een bestand

- Wat is de SELinux-context van een bestand?
    - `ls -lZ`
- Wat wordt de SELinux-context bij aanmaken van een nieuw bestand? 
    - `/etc/selinux/targeted/contexts/files/files_contexts`
- Standaard-context herstellen: 
    - `sudo restorecon -R /var/www/`
- Context instellen naar een bepaalde waarde: 
    - `sudo chcon -t httpd_sys_content_t test.php`   

### Check booleans

`getsebool -a | grep http`  

- Know the relevant booleans! (RedHat manuals)  
- Enable boolean: 
    - `sudo setsebool -P httpd_can_network_connect_db on`  

### Hoe weet ik wat SELinux tegenhoudt?

````
sudo tail -f /var/log/audit/audit.log
sudo grep denied /var/log/audit/audit.log
````  

## Scripting (verolg)

### Fouten opsporen  

- Syntax check: `bash -n script.sh`  
- ShellCheck: `shellcheck script.sh`  
- Druk veel info af (`printf` of `echo`)  
- Debug-mode:
  - `bash -x script.sh`
  - In het script: `set -x` en `set +x`  

### Fouten voorkomen

Begin elk script met:  

````bash
set -o errexit   # abort on nonzero exitstatus
set -o nounset   # abort on unbound variable
set -o pipefail  # don't hide errors within pipes
````  

### Booleans in Bash
### Logische operatoren

````bash
if COMMANDO; then
  # A
else
  # B
fi
````  

- `A`-blok wordt uitgevoerd als exit-status van `COMMANDO` 0 is (geslaagd, TRUE)  
- `B`-blok wordt uitgevoerd als exit-status van `COMMANDO` verschillend is van 0 (gefaald, FALSE)  

### Toepassing

````bash
if ! getent passwd "${user}" > /dev/null 2>&1; then
  echo "Adding user ${user}"
  adduser "${user}"
else
  echo "User ${user} already exists"
fi
````  

### Operatoren && en ||

`command1 && command2`  

`command2` wordt enkel uitgevoerd als `command1` succesvol was (exit 0)  

`command1 || command2`  

`command2` wordt enkel uitgevoerd als `command1` niet succesvol was (exit ≠ 0)  

### Het commando test

- Alias voor `test` is `[`

````bash
# Fout:
if[$#-eq 0]; then
  echo "Expected at least one argument"
fi

# Juist:
if [ "${#}" -eq "0" ]; then
  echo "Expected at least one argument"
fi
````  

## Automatiseren webserverinstallatie

### Installeer Vagrant

- Windows: `choco install vagrant`
- MacOS: `brew install vagrant`
- Linux: `apt install vagrant` (of `dnf`)  

## Overzicht repo

````
$ tree
.
├── LICENSE.md
├── provisioning
│   ├── common.sh
│   ├── db.sh
│   └── web.sh
├── README.md
├── Vagrantfile
└── vagrant-hosts.yml

1 directory, 7 files
````  

- `Vagrantfile`: configuratiebestand van Vagrant
  - hoef je niet aan te komen
- `vagrant-hosts.yml`: overzicht VMs in de opstelling 
  - incl. eigenschappen zoals IP-adres
- `provisioning/`: installatiescripts voor VMs  

### Werken met Vagrant

````
vagrant status
Current machine states:

db                        not created (virtualbox)
web                       not created (virtualbox)

This environment represents multiple VMs. The VMs are all listed
above with their current state. For more information about a specific
VM, run `vagrant status NAME`.
````  

### Start de db VM op

````
$ vagrant up db
Bringing machine 'db' up with 'virtualbox' provider...
==> db: Importing base box 'bento/almalinux-8'...
==> db: Matching MAC address for NAT networking...
==> db: Checking if box 'bento/almalinux-8' version '202109.10.0' is up to date...
==> db: Setting the name of the VM: linux-2122-automation-bertvv_db_1635848839364_33506
==> db: Clearing any previously set network interfaces...
==> db: Preparing network interfaces based on configuration...
    db: Adapter 1: nat
    db: Adapter 2: intnet
==> db: Forwarding ports...
    db: 22 (guest) => 2222 (host) (adapter 1)
==> db: Running 'pre-boot' VM customizations...
==> db: Booting VM...
==> db: Waiting for machine to boot. This may take a few minutes...
    db: SSH address: 127.0.0.1:2222
    db: SSH username: vagrant
    db: SSH auth method: private key
    db: 
    db: Vagrant insecure key detected. Vagrant will automatically replace
    db: this with a newly generated keypair for better security.
    db: 
    db: Inserting generated public key within guest...
    db: Removing insecure key from the guest if it's present...
    db: Key inserted! Disconnecting and reconnecting using new SSH key...
==> db: Machine booted and ready!
==> db: Checking for guest additions in VM...
==> db: Setting hostname...
==> db: Configuring and enabling network interfaces...
==> db: Mounting shared folders...
    db: /vagrant => /home/bert/Documents/Vakken/Linux/21-22/linux-2122-automation-bertvv
==> db: Running provisioner: shell...
    db: Running: /tmp/vagrant-shell20211102-9694-yq9qn5.sh
    db: [LOG]  === Starting common provisioning tasks ===
    db: [LOG]  Ensuring SELinux is activePermissivePermissivePermissive
    db: [LOG]  Installing useful packages
    [...]
    db: [LOG]  Securing the database
    db: [LOG]  Creating database and user
    db: [LOG]  Creating database table and add some data
````  

### Inloggen

````
vagrant ssh db

This system is built by the Bento project by Chef Software
More information can be found at https://github.com/chef/bento
[vagrant@db ~]$ ip a show dev eth1
3: eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:1f:c4:a7 brd ff:ff:ff:ff:ff:ff
    inet 192.168.76.3/24 brd 192.168.76.255 scope global noprefixroute eth1
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fe1f:c4a7/64 scope link 
       valid_lft forever preferred_lft forever
[vagrant@db ~]$
````  

### Belangrijkste commando’s

|Commando              |Taak                        |
|----------------------|----------------------------|
|`vagrant status`      |Overzicht omgeving          |
|`vagrant up VM`       |start VM op                 |
|`vagrant ssh VM`      |inloggen op VM              |
|`vagrant halt VM`     |VM uitschakelen             |
|`vagrant reload VM`   |VM rebooten                 |
|`vagrant provision VM`|Installatie-script uitvoeren|
|`vagrant destroy VM`  |VM vernietigen              |  


## Complexere scripts

### Communicatie script/omgeving

Informatie uitwisselen tussen script en omgeving:  
- I/O: `stdin`, `stdout`, `stderr`
- Positionele parameters: `$1`, `$2`, enz
- Exit-status (0-255)
- Omgevingsvariabelen, vb:
  ````
  VAGRANT_LOG=debug vagrant up
  ````  

### Functies in Bash

````bash
functie_naam() {
    # code
}
````  

Een functie gedraagt zich als een script!  
- oproepen: `functie_naam arg1 arg2 arg3`
- positionele parameters: `${1}`, `${2}`, enz.
- `return STATUS` ipv `exit`  

### Scope variabelen bij functies - global

````bash
#! /usr/bin/env bash
var_a=a

foo() {
  var_b=b
  echo "${var_a} ${var_b}"
}

foo

echo "${var_a} ${var_b}"
````  

### Scope variabelen bij functies - local

````bash
#! /usr/bin/env bash
var_a=a

foo() {
  local var_b=b
  echo "${var_a} ${var_b}"
}

foo

echo "${var_a} ${var_b}"
````  

### Functies in Bash: voorbeeld

````bash
# Usage: copy_iso_to_usb ISO_FILE DEVICE
# Copy an ISO file to a USB device, showing progress with pv (pipe viewer)
# e.g. copy_iso_to_usb FedoraWorkstation.iso /dev/sdc
copy_iso_to_usb() {
  local iso="${1}"
  local destination="${2}"
  local iso_size

  iso_size=$(stat -c '%s' "${iso}")

  printf "Copying %s (%'dB) to %s\n" \
    "${iso}" "${iso_size}" "${destination}"

  dd if="${iso}" \
    | pv --size "${iso_size}" \
    | sudo dd of="${destination}"
}
````  

### Case

````bash
case EXPR in
  PATROON1)
    # ...
    ;;
  PATROON2)
    # ...
    ;;
  *)
    # ...
    ;;
esac
````  

````bash
option="${1}"

case "${option}" in
  -h|--help|'-?')
    usage
    exit 0
    ;;
  -v|--verbose)
    verbose=y
    shift
    ;;
  *)
    printf 'Unrecognized option: %s\n' "${option}"
    usage
    exit 1
    ;;
esac
````  

## Plannen van systeembeheertaken: cronjobs

### Processen op de achtergrond

````
$ vi test.txt
Ctrl+Z

[1]+  Stopped                 gvim -v test.txt
$ bg
[1]+ sleep 30 &

$ find / -type f > all-files.txt 2>&1 &
[2] 4321
````  

- `Ctrl+Z` zet de uitvoer van een proces stil (nog niet afgesloten!)
- `bg` start het proces terug op, maar in de achtergrond
- `&` op het einde van een regel start proces op de achtergrond = combinatie van `Ctrl+Z` en `bg`  

### Achtergrondprocessen beheren

|Commando |Betekenis                                |
|---------|-----------------------------------------|
|`jobs`   |Lijst van achtergrondprocessen           |
|`jobs -l`|Idem, toon ook Process ID (PID)          |
|`fg NUM` |Breng proces op voorgrond                |
|`bg NUM` |Herstart stilgelegd proces op achtergrond|  

### Processen eenmalig plannen

````
$ at now + 2 minutes
warning: commands will be executed using /bin/sh
at Mon Nov 15 15:47:00 2021
at> date > /tmp/date.txt     
at> <Ctrl+D>
job 9 at Mon Nov 15 15:47:00 2021
$ watch cat /tmp/date.txt
````  

- `at` zal binnen 2 minuten het opgegeven commando uitvoeren
- Met `watch` herbekijken we elke 2s de inhoud van het doelbestand  

Nog `at` voorbeelden:
- `at 3:03 AM`
- `at midnight`
- `at 1am tomorrow`
- `at now + 3 weeks`  

### Overzicht

|Commando  |Betekenis                                     |
|----------|----------------------------------------------|
|`at`      |Voer commando’s uit op specifiek tijdstip     |
|`atq`     |Geeft lijst van geplande taken                |
|`atrm NUM`|Verwijder taak met id NUM                     |
|`batch`   |Voer taak uit wanneer systeem minder belast is|  

### Processen herhaald plannen: cron

- Bekijk `/etc/crontab`
- Bevat taken die regelmatig gepland worden: 
  - tijdsaanduiding
  - commando
- Crontab per gebruiker: 
  - tonen: `crontab -l`
  - bewerken: `crontab -e`  

### Tijdsaanduiding

|Veld|Beschrijving    |Waarden|
|----|----------------|-------|
|MIN |Minuten         |0-59   |
|HOUR|Uren            |0-23   |
|DOM |Dag van de maand|1-31   |
|MON |Maand           |1-12   |
|DOW |Dag van de week |0-7    |
|CMD |Commando        |       |  

Dag van de week: zo = 0/7, ma = 1, di = 2, …  

### Voorbeelden

````bash
# use /bin/sh to run commands, no matter what /etc/passwd says
SHELL=/bin/sh
# mail any output to `paul', no matter whose crontab this is
MAILTO=paul
# Set time zone
CRON_TZ=Japan
# run five minutes after midnight, every day
5 0 * * *       $HOME/bin/daily.job >> $HOME/tmp/out 2>&1
# run at 2:15pm on the first of every month -- output mailed to paul
15 14 1 * *     $HOME/bin/monthly
# run at 10 pm on weekdays, annoy Joe
0 22 * * 1-5    mail -s "It's 10pm" joe%Joe,%%Where are your kids?%
23 0-23/2 * * * echo "run 23 minutes after midn, 2am, 4am ..., everyday"
5 4 * * sun     echo "run at 5 after 4 every sunday"
````  

## Troubleshooting, SSH: Introduction

### Agenda

- Bottom-up approach
- Network access (Link) layer
- Internet layer
- Transport
- Application Layer
- SELinux  

### Use a bottom-up approach

|Layer         |Protocols             |Keywords             |
|--------------|----------------------|---------------------|
|Application   |HTTP, DNS, SMB, FTP, …|sockets, port numbers|
|Transport     |TCP, UDP              |sockets, port numbers|
|Internet      |IP, ICMP              |routing, IP address  |
|Network access|Ethernet              |switch, MAC address  |
|Physical      |                      |cables               |  

## Network Access Layer

### Network Access Layer

- bare metal:
  - test the cable(s)
  - check switch/NIC LEDs
- VM (e.g. VirtualBox): 
  - check virtual network adapter type & settings
- `ip link`  

## Internet Layer

### Checklist: Internet Layer

1. Local network configuration
2. Routing within the LAN  

Know the expected values!  

Checking Local network configuration:  

1. IP address: `ip a`
2. Default gateway: `ip r`
3. DNS service: `/etc/resolv.conf`  

### Local configuration: ip address

- IP address?
- In correct subnet?
- DHCP or fixed IP?
- Check configuration: `/etc/sysconfig/network-scripts/ifcfg-*`  

Example: DHCP  

````
$ cat /etc/sysconfig/network-scripts/ifcfg-enp0s3 
TYPE=Ethernet
BOOTPROTO=dhcp
NAME=enp0s3
DEVICE=enp0s3
ONBOOT=yes
[...]
````  

Example: Static IP  

````
$ cat /etc/sysconfig/network-scripts/ifcfg-enp0s8
BOOTPROTO=none
ONBOOT=yes
IPADDR=192.168.76.73
NETMASK=255.255.255.0
DEVICE=enp0s8
[...]
````  

### Common causes (DHCP)

- No IP
  - DHCP unreachable
  - DHCP won’t give an IP
- 169.254.x.x 
  - No DHCP offer, “link-local” address
- Unexpected subnet 
  - Bad config (fixed IP set?)

Watch the logs: `sudo journalctl -f`  

### Common causes (Fixed IP)

- Unexpected subnet 
  - Check config
- Correct IP, “network unreachable”
  - Check network mask  

### Local configuration: ip route

- Default GW present?
- In correct subnet?
- Check network configuration  

### DNS server: /etc/resolv.conf

- `nameserver` option present?
- Expected IP?  

### Checklist: Internet Layer

Checking routing within the LAN:  

- Ping between hosts  
- Ping default GW/DNS 
- Query DNS (`dig`, `nslookup`, `getent`)  

### LAN connectivity: ping

- GUI-VM-> VM: `ping 192.168.76.72`
- VM -> GUI-VM: `ping 192.168.76.101`
- VM -> NAT-GW: `ping 10.0.2.2`
- VM -> NAT-DNS: `ping 10.0.2.3`  

Remark: some routers block ICMP!  

### LAN connectivity: DNS

- `dig icanhazip.com`
- `nslookup icanhazip.com`
- `getent ahosts icanhazip.com`  

### LAN connectivity

Next step: routing beyond GW  

## Transport Layer

### Checklist: Transport Layer

1. Service running? `sudo systemctl status SERVICE`
2. Correct port/inteface? `sudo ss -tulpn`
3. Firewall settings: `sudo firewall-cmd --list-all`  

### Is the service running?

`systemctl status httpd.service`  

- `active (running)` vs. `inactive (dead)`
  - `systemctl start httpd`  
  - Fail? See below (Application layer)
- Start at boot: `enabled` vs. `disabled`
  - `systemctl enable httpd`  

### Firewall settings

`sudo firewall-cmd --list-all`  

- Is the service or port listed?
- Use `--add-service` if possible 
  - Supported: `--get-services`
- Don’t use both `--add-service` and `--add-port`
- Add `--permanent`
- `--reload` firewall rules  

````
$ sudo firewall-cmd --add-service=http --permanent
$ sudo firewall-cmd --add-service=https --permanent
$ sudo firewall-cmd --reload
````  

### Correct ports/interfaces?

- Use `ss` (not `netstat`)
  - TCP service: `sudo ss -tlnp`
  - UDP service: `sudo ss -ulnp`
- Correct port number? 
  - See `/etc/services`
- Correct interface? 
  - Only loopback?  

## Application Layer

### Checklist: Application Layer

- Check the logs: `journalctl`
- Validate config file syntax
- Use (command line) client tools 
  - e.g. `curl`, `smbclient` (Samba), `dig` (DNS), etc.
  - Netcat (`ncat`, `nc`)
- Other checks are application dependent
  - Read the reference manuals!  

### Check the log files

- Either journalctl: `journalctl -f -u httpd.service`
- Or `/var/log/`:
  - `tail -f /var/log/httpd/error_log`  

### Check config file syntax

- Application dependent, for Apache: `apachectl configtest`  

### Read the fine manual!

- RedHat Manuals: 
  - System Administrator’s Guide
  - Networking guide
  - SELinux guide
- Reference manuals, e.g.: 
  - https://httpd.apache.org/docs/2.4/configuring.html
- Man pages 
  - smb.conf(5), dhcpd.conf(5), named.conf(5), …  

## SELinux troubleshooting

### SELinux

- SELinux is Mandatory Access Control in the Linux kernel
- Settings: 
  - Booleans: `getsebool`, `setsebool``
  - Contexts, labels: `ls -Z`, `chcon`, `restorecon`
  - Policy modules: `sepolicy`  

### Check file context

- Is the file context as expected? 
  - `ls -Z /var/www/html`
- Set file context to default value
  - `sudo restorecon -R /var/www/`
- Set file context to specified value 
  - `sudo chcon -t httpd_sys_content_t test.php`  

### Check booleans

`getsebool -a | grep http`  

- Know the relevant booleans! (RedHat manuals)
- Enable boolean:
  - `sudo setsebool -P httpd_can_network_connect_db on`  

## General guidelines

- Back up config files before changing
- Be systematic, bottom-up
- Be thorough, don’t skip steps
- Do not assume: test
- Know your environment
- Know your log files
- Read The F*** Error Message!
- Open logs in separate terminal
- Small steps
- Validate the syntax of config files
- Reload service after config change
- Verify each change
- Keep a cheat sheet/checklist
  - E.g. https://github.com/bertvv/cheat-sheets
- Use a configuration management system
- Automate tests
  - E.g. https://github.com/HoGentTIN/elnx-sme/blob/master/test/pu001/lamp.bats
- Don’t ping Google!  

## Mount: Pre knowledge  

### Disk devices

Sata disks devices:  

````
$ ls /dev/sd*
/dev/sda    /dev/sdb
````  

### Disk partitions

Every disk:  

- maximum 4 primary/extended partitions 
- one extended partition can host further logical (sub)partitions  

|Partition Type  |naming|
|----------------|------|
|Primary (max 4) |1-4   |
|Extended (max 1)|1-4   |
|Logical         |5-    |  

### fdisk

Display and modify partitions of a disk.  

````
$ sudo fdisk -l 
Disk /dev/sda: 64 GiB, 68719476736 bytes, 134217728 sectors
[...]

Device     Boot   Start       End   Sectors  Size Id Type
/dev/sda1          2048   4501503   4499456  2.1G 82 Linux swap / Solaris
/dev/sda2  *    4501504 134217727 129716224 61.9G 83 Linux
````   

### File systems

Linux file systems (common):  

- ext2
- ext3 - with journaling  
- ext4 - latest version, with journaling  
- xfs  

Other file systems:  

- vfat
- ntfs
- iso9660  

### Formating a partition

mkfs = MaKe FileSystem  

````
$ sudo mkfs -t ext3 /dev/sdb3
mke2fs 1.45.5 (07-Jan-2020)
Creating filesystem with 244224 4k blocks and 61056 inodes
Filesystem UUID: 396d698d-eac6-44f3-9b95-34db7d461664
Superblock backups stored on blocks:
    32768, 98304, 163840, 229376

Allocating group tables: done
Writing inode tables: done
Creating journal (4096 blocks): done
Writing superblocks and filesystem accounting information: done
````  

### Changing defaults of a file system

Every partition has e.g. reserved blocks for root user only:  

````
$ sudo tune2fs -l /dev/sdb3 | grep -i "block count"
Block count:              104388
Reserved block count:     5219
````  

Update with `tune2fs` e.g. reduce to 3% reserved blocks  

````
$ sudo tune2fs -m3 /dev/sdb3 "
tune2fs 1.45 (Jan-2020)
Setting reserved blocks percentage to 3 (3131 blocks)
````  

## Mount

### Manual mount

mount = Making a partition available in the file tree  

1. make a mount point ~ a mount directory, e.g.
  ````
  $ sudo mkdir /mnt/newmountpoint
  ````  
2. bind the partition to the mount point
  ````
  $ sudo mount -t ext3 /dev/sdb3 /mnt/newmountpoint
  ````  

### Display mount points

````
$ mount | grep sd
/dev/sda1 on / type ext4 (rw,relatime,errors=remount-ro)
/dev/sda4 on /home type ext4 (rw,relatime)
````  

### Permanent mounts

Partitions which will be mounted at boot: `/etc/fstab`  

````
$ cat /etc/fstab
# /etc/fstab: static file system information.
#
# <file system> <mount point>   <type>  <options>       <dump>  <pass>
/dev/sda1           /               ext4    errors=remount-ro 0       1
/dev/sda2           /boot           ext4    defaults        0       2
/dev/sda4           /home           ext4    defaults        0       2
/dev/sda3           none            swap    sw              0       0
````  

### Mount options

Mount has some useful options:  

- ro - read-only
- rw - read-write
- remount - mount an already mounted device with new options  

````
$ mount | grep boot 
/dev/sda2 on /boot type ext4 (rw,relatime)
$ sudo mount -o remount,ro /boot/
$ mount | grep boot 
/dev/sda2 on /boot type ext4 (ro,relatime)
````  

## UUID

### UUID def

UUID = universally unique identifier  

- 128 bits
- generated while formating   

### lookup UUID

````
$ sudo tune2fs -l /dev/sda2 | grep UUID
Filesystem UUID:          fd5db924-4be6-4fee-9a92-ca9db8fe2b9c
$ blkid /dev/sda2
/dev/sda2: UUID="fd5db924-4be6-4fee-9a92-ca9db8fe2b9c" TYPE="ext4" PARTUUID="97584a69-02"
````  

### fstab with UUID

unique indication of partition in case e.g. sda and sdb get switched when booting  

````
$ cat /etc/fstab
# /etc/fstab: static file system information.
#
# <file system> <mount point>   <type>  <options>       <dump>  <pass>
# / was on /dev/sda1 during installation
UUID=757350f2-9cb2-4ce5-86bc-4528dfe9d9ac /               ext4    errors=remount-ro 0       1
# /boot was on /dev/sda2 during installation
UUID=fd5db924-4be6-4fee-9a92-ca9db8fe2b9c /boot           ext4    defaults        0       2
# /home was on /dev/sda4 during installation
UUID=502c37ca-255c-4a38-9294-9802a5fb5941 /home           ext4    defaults        0       2
# swap was on /dev/sda3 during installation
UUID=b559d746-ef9d-45bc-acfb-faa036b3418f none            swap    sw              0       0
````  

## DNS-server met BIND

### Agenda

- DNS
- BIND op Enterprise Linux
- Configuratie
- Zonebestanden  

http://www.zytrax.com/books/dns/  

zie h10  



















# Spiek brief  

### Man pages:  

`Q` = sluit  
`/` = zoek  
`n` = volggende  
`N` = vorige  
`Man hier` = mapen  
`Man builtins` = bash comando's  
`Man 7 glob` = wildcards  

### Zip map:  

`Tar cjf "naam".tar "wat moet gezipt worden"`  
`czf` = gzip(.gz)  
`Tar xf "naamOntzippen".tar -C "locatie"`  
`Tar -xvf "zipFile" "bestand"` = bestand uit zip halen  
Opties:  
`C` = maak  
`x` = uitpakken  
`j` = compres met bzip2  
`Z` = compres met gzip  
`f` = naam bestand  
`V` = toon uitgepakte  

### Vim / Nano:  

Vim:  
`Vim "naam".txt` = bestand maken  
`Vimtutor` = wat hulp voor vim  
`i` = invoer mode  
esc toets = normal mode  
`:w` =opslaan  
`:wq` = sluit/opslaan  
`:q!` = sluit  

Nano:  
`nano "naam".txt` = bestand maken  
^x -> ctrl+X = exit  

### Mappen en bestanden:  

`Mkdir -p` = maak map en volgende  
`Rmdir` = verwijder map  
`Rm -r "naam"` = verwijder volle map  
`Pwd` = huidige directory  
`Ls` = lijst  
`Cd` = verplaats  
`Cp` = kopieer  
`Mv` = verplaats/hernoem  
`Rm` = verwijder  
`Touch "naam".txt` =maak  
`Tree` = tree vorm  
/root = home root  
`Pushd` = directory op stapel  
`Popd` = directory van stapel  

### File inhoud:  

`Head "naam"` = eerste 10 lijnen file  
`tail "naam"` = laatste 10 lijnen file  
`Cat` = inhoud bestand  
`Cat > "naam".txt` = bestnad maken en in schrijven (Stop door ctrl+d)  
`Cat > "naam".txt <<"stop woord"`  
`Less` = inhoud per pagina  
`Echo "txt" >"bestand"` = tekst in bestand zetten  
`Tac` = print de txt in omgekeerde volgorde  

### Tekst:  

`Echo "aa bb"`  
`Echo "${user}"` = variabele  
`Echo '${user}'` = ${USER}  
`Printf 'aaa bbb *s \n' "${USER}"`  
`Cmd > file` = uitkomst cmd in flie zetten  
`Cmd >> file` = op einde file zetten  
`Cmd 2> file` = errors naar file  
`Cmd < file` = inhoud file als invoer cmd  
`Cmd1 | cmd2` = uitvoer 1 als invoer 2  
`Cat "bestand" | Grep "tekst"` = filteren van tekst uit bestand  
	Of  
`Grep "tekst" "bestand"`  
`Grep -i` = houd geen rekening met hoofdletters  
`Grep -v` = strings die niet overeenkommen  
`Cut`   
`| Tr 'vorg' 'nieuw'` = vervangt vorig door nieuw  
`| Tr – d "karakter"` = delet karakter  
`Wc "bestand"` = telt woord lijn karakter  
`Wc -l` `wc -w` `wc -c`  
`Sort` = standaart abc  
`Sort -n` = 0-9  
`Sort "bestand" | uniq` = dubele woorden verwijderen  
`Sort "bestand" | uniq -c` = telt voorkomen woord  
`Comm` = inhoud files vergelijken  

### Comando's / argumenten  

`Alias print` = `cat` = alternatief maken  
`Unalias cat` = alternatief verwijderen  
`;` geld als een afbreking  
`&&` = and `||` = or  
`#` = begin comentaar regel  
`$PATH` = waar uitvoert comando’s  
`Export "variabele"`  
`!!` = herhaal vorig comando  

### File globbing  

`*` = aanvulling (file*)(file1 file22)  
`?` = aanvulling (file?)(file1)  
`[]` =match alles er in([5A])(FileA)  
`a-z 0-9` = (`[a-z]`= enkel alfabet klein)   

### Scripts:  

`Vim "naam".sh` = script maken nog niet uitvoerbaar  
`Chmod +x "naam".sh` = uitvoerbaar maken  
`./"naam".sh` = uitvoeren  
`set -o noclobber` = kan een bestand niet  
`+o` = uit  
`>|` = noclobber negeren  
`2>&1` `&>`= error en output in document  
  
```` bash
#!/bin/bash
#korte beschrijving
Set -o errexit
Set -o nounset
Set -o pipefail
overschrijven
````  
  
-eq..== -ni..!= -lt..< -le..=< -gt..> -ge..=>  
Test  10 -gt 55 = true of false (1 is faal 0 is geslaagd)  
	Of [10 -gt 55]  
  
`${0}` = naam script  
`${*}` = alle argumenten ${1} ${2} …  
`${@}` = alle argumenten “${1}” “${2}” …  
`${#}` = aantal positionele parameters  
`Shift` =  schuift alles op 1 verwijdert 2 wordt 1 ….  
`Set aaa bbb ccc` = `${1}` = aaa …  
`Echo "$?"` = exit status(0is true 1-225=false)  
  
```` bash
If [commando]; then       #(als “$?”=0)
	#a
Else
	#b
fi
````  
  
```` bash
while []; do
#
Done
````  
  
```` bash
Until []; do
#
Done
````  
  
```` bash
For [item in list]; do 
#(vb file in *.md of i in {1..10} of i in $(seq 2 2 20) van 2 stap 2 tot 20) of ((i=0; i<=10; i++))
#
Done
````  
  
```` bash
Case “expr” in #(switch)
  “patroon1”)
 #
;;
  “patroon1”)
#
;;
*)
#
;;
esac
````  
  
```` bash
functie_naam(){
#code
}

functie_naam #(uitvoer)
````


### Permissies:  
  
-rw-rw-r—(vorm)  
u-g-o    a= user,group,other en all  
r=lees w=schrijf x=uitvoer  
`chmod (u+x)(777)` = verandert  
-rwxrwxrwx=777 (binair)(4r2w1x)  
`Stat -c %a "naam"` = octaal opvragen permisie  
`Unmask`  
`Chown “user” “bestand”` = eigenaar veranderen  
`Chgrep group file` = groep veranderen  
|first character|file type       |
|:-------------:|----------------|
|-              |normal file     |
|d              |directory       |
|l              |symbolic link   |
|p              |named pipe      |
|b              |block device    |
|c              |character device|
|s              |socket          |  

### webserver:  

`Ip address (ip a)` = ip address opvragen  
`Ip router (ip r)` = default gateway  
`Cat /etc/resolv.conf` = dns server  
`Nslookup "site"` = ip adres opzoeken  
`Dig "site"`     
`Curl icanhazip.com` = mijn publiek ip  
`Enp0s3` = nat 8 =host-only  

### Gebruikers groep:  
  
`Useradd "naam"` = maakt een gebruiker  
`Passwd "naam"` = een wachtwoord maken  
`Usermod` = instellingen  
`Userdel` = verwijder user  
`Groupadd`  
`Groupmod`  
`Groupdel`  
`Who`  
`Groups`  
`Id`  
`Su – "naam"` = gebruiker wisselen  
`Usermod -aG "groep" "user"` = gebruiker aan groep toevoegen blijft lid van andere groep  
`Usermod -G "groep" "user"` = blijft niet bij vorige groep  
`Usermod -g "groep" "user"` = primaire groep veranderen  
  
### Links:  

`ln -s "doel" "naam link"` = symbolic link  
(verwijst naar bestand)  
Ln "orgineel" "naam link" = hard link  

