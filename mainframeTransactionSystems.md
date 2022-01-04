# Work based learning transaction systems

## 1. Wat is een transactie?

Een transactie is een sequentie van gerelateerde operaties die een functie uitvoeren (vb. geef account balans weer)

## 2. Wat is het verschil tussen batch en transacties?

Batch programma’s hebben geen interactie met een gebruiker en zijn op voorhand ingeplant om uitgevoerd te worden op een specifiek tijdstip en transacties worden onmiddellijk afgerond en geven de output onmiddellijk.  

## 3. Wat is CICS?

CICS: Customer Information Control Center  
Cics is een online transactie processing systeem.   
Middleware tussen operating system en de business applications.  
User interface.  
Data ophalen en bewerken.  

## 4. Wat wordt bedoeld met pseudo-conversational processing?

Als een programma input van een gebruiker nodig heeft zal het programma stoppen tot de gebruiker iets ingeeft, dan zal het programma opnieuw opgestart worden. Hierdoor blijft het programma niet lopen op de achtergrond.  

![pseudoConversationalProcessnig](/images/pseudoConversationalProcessnig.png)  

![pseudoConversationalProcessnig](/images/pseudoConversationalProcessnig1.png)

## 5. Wat is het CICS command?

Algemene syntax:   
````
EXEC CICS command option(value) ...;
````  

CICS commando voor scherm interactie  
- Receive map: haalt input data op van de terminal  
  ````
  EXEC CICS
    RECEIVE MAP ('INQMAP1')
            MAPSET ('INQSET1')
            INTO (INQMAP1I)
  END-EXEC
  ````
- Send map: verzend info naar de terminal
- 

## 6. Hoeveel karakters telt een transactienaam?

Een transactie heeft een ID bestaande uit 4 karakters.  

## 7. Geef twee eigenschappen van een taak

•	Een taak is een instantie van een transactie.  
•	Taken kunnen gelijktijdig lopen.  
•	Gebruikers kunnen dezelfde transactie gelijktijdig gebruiken.     

![transaction](/images/transaction.png)

## 8. Wat is een physical/symbolic map? Geef het verschil

**Physical map**: Is een load module in de load library die informatie bijhoud over hoe de map getoond moet worden.  
**Symbolic map**: Is een copy book in de library. Het copy book wordt gebruikt door CICS applicatie programma’s om informatie te verzenden en ontvangen van de terminal.   

Een physical map heeft informatie over hoe de map moet weergegeven worden(details over de attributen van alle velden in de map en hun posities, display format van de map voor een bepaalde terminal ), een symbolic map heeft enkel de variabel data en named fields om data te kunnen lezen en schrijven in een map.  

![transaction](/images/PhisicalSymbolicMap.png)
