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

einde h5  













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

