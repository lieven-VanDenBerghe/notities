# Mainframe Work based learning system administration

## 1. Wat is het verschil tussen CICS en batch?

Batch programma’s hebben geen interactie met een gebruiker en zijn op voorhand ingeplant om uitgevoerd te worden op een specifiek tijdstip, en CICS worden onmiddellijk afgerond en geven de output onmiddellijk.   

## 2. Wat is een mainframe?

Een mainframe is een data server die ontworpen is om commerciële databanken, transactie servers en programma’s te hosten die een hoge graat van beschikbaarheid en beveiliging nodig hebben. Mainframes kunnen biljoenen transacties snel uitvoeren aan een hoge  graat van beschikbaarheid en beveiliging.

## 3. Wat zijn de grote voordelen van een mainframe (RAS)?

**RAS**: Reliability, availability, servicability  
•	Security  
•	Scalability  
•	Één centraal controle punt  
•	Werk management  
•	Compatability  
•	Constant evoluerende architectuur  
•	Duizenden mensen kunnen gelijktijdig werken

## 4. Wat is z/OS?

z/OS is het meest gebruikte besturingssysteem voor de mainframe. Het is het perfecte systeem voor heel grote jobs en een grote werkdruk van een grote hoeveelheid gebruikers tegelijkertijd.

## 5. Geef 4 karakteristieken van z/OS

•	**Multiprogramming**: De mogelijkheid om meerdere programma’s tegelijkertijd uit te voeren.  
•	**Multiprocessing**: Meerdere processors die tegelijkertijd werken en verschillende hardware resources gebruiken.   
•	Gebruikt address spaces om te verzekeren dat het privé geheugen geïsoleerd is.  
•	Zorgt voor data integriteit ondanks het groot aantal gebruikers.   
•	Verwerkt een grote hoeveelheid batch jobs en de werkdruk automatisch verdelen.  
•	Laat meerdere communicatie subsystemen toe.  
•	Beveiliging is geïntegreerd in applicaties, sources en profielen.   
•	Kan gemengde taken verwerken (online en batch).  
•	Zorgen dat SLA(service level agreements) kan behaalt worden.  

## 6. Wat is het verschil tussen multiprogramming en multiprocessing?

Multiprogramming is de mogelijkheid om meerdere programma’s gelijktijdig uit te voeren.  
Multiprocessing wil zeggen dat er meerdere processors zijn die gelijktijdig werken en verspillende hardware bronnen gebruiken.

## 7. Leg virtueel geheugen uit: begrippen te gebruiken: real – virtual – auxilary

Om de mainframe een extreem groot geheugen te geven wordt er een virtueel geheugen gebruikt, dit virtueel geheugen bestaat uit de combinatie van het effectieve geheugen van de mainframe en auxiliary storage. z/OS houd enkel het gebruikte geheugen van het programma op het interne geheugen, de rest wordt in page data sets bijgehouden op auxiliary storage.   

![VirtualStorageMainframe](/images/VirtualStorageMainframe.png)

## 8. Geef minstens 2 andere operating systems dan z/OS op mainframe

•	z/VM: z/Virtual Macine -  kan andere OS   
•	VSE: z/Virtual Storage Extended – voor kleinere mainfram computers  
•	Linux for zSeries: linux op de mainframe  
•	z/TPF: z/Transaction Processing Facility – voor bedrijven die veel transacties moeten verwerken


## 9. Wat is een LPAR?

Een LPAR is een opdeling van de mainframe in meerdere stukken (kleinere mainframes), deze hebben elk hun eigen operating system. Men kan processors specifiek toewijzen aan 1 LPAR of hem delen over meerdere LPARs. Deze LPARs gebruiken cannels om te communiceren met de data.  

![LPARs](/images/LPARs1.png)  

![channels](/images/LPARs2.png)  



## 10. Wat is een Coupling facility?

Een coupling facility wordt gebruikt om een parallel sysplex te maken. Een coupling facility verzorgt aan een hoge performance multisystem data sharing.  

•	High-speed caching  
•	List processing  
•	Locking functions in a sysplex  

![couplingFacility](/images/couplingFacility.png)   

## 11. Leg parallel sysplex uit

Parallel sysplex is een clustering technologie die toestaat meerdere kopieën van z/OS als één operating system te zien met behulp van coupling facilitys. Dit zorgt er voor dat er een bijna continue availability is.

## 12. Wat is DASD?

**DASD**: Direct Access Storage Device
DASD is een term voor magnetic disk storage devices.


## 13. Wat is een IPL?

IPL is het opstarten van het systeem.
