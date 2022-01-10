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

### Algemene syntax:   
````
EXEC CICS command option(value) ...;
````  

### CICS commando voor scherm interactie  

- **RECEIVE MAP**: haalt input data op van de terminal  
  ````
  EXEC CICS    
    RECEIVE MAP(map-name)       
      [MAPSET(mapset-name)]      
      INTO(data-name)
  END-EXEC

  /*ex*/
  EXEC CICS
    RECEIVE MAP ('INQMAP1')
      MAPSET ('INQSET1')
      INTO (INQMAP1I)
  END-EXEC
  ````  
  |Option|Description                                                                     |
  |------|--------------------------------------------------------------------------------|
  |MAP   |Specifies the name of the map for the input data (one to seven characters)      |
  |MAPSET|Specifies the name of the mapset that contains the map (one to seven characters)|
  |INTO  |Specifies the name of the data area where the mapped data should be placed      |  

- **SEND MAP**: verzend info naar de terminal  
  ````
  EXEC CICS    
    SEND MAP(map-name)      
      [MAPSET(mapset-name)]       
      [FROM(data-name)]       
      [MAPONLY | DATAONLY]       
      [ERASE]       
      [ALARM]
  END-EXEC  
  ````  
  |Option          |Description                                                                           |
  |----------------|--------------------------------------------------------------------------------------|
  |MAP             |Specifies the name of the map for the output data (one to seven characters)           |
  |MAPSET          |Specifies the name of the mapset that contains the map (one to seven characters)      |
  |FROM            |Specifies the name of the area in the symbolic map that contains the data to be mapped|
  |MAPONLY/DATAONLY|MAPONLY: sends only the **constant** data from the physical map (no FROM used)        |  
  |                |DATAONLY: sends only the data from the FROM area (no constant data is sent)           |  
  |                |When neither is specified: both constant data and FROM area are sent                  |
  |ERASE           |Erases the contents of the screen before data is displayed                            |
  |ALARM           |Sounds the alarm when the map is displayed                                            |  


### CICS commando’s om controle van een programma aan een andere programma te geven.  

- **ABEND**: geeft controle aan een aband routine of geeft controle aan CICS
- **LINK**: roep een programma op in een lager level
- **RETURN**: geeft de controle terug aan CICS of het bovenliggend programma  
  ````
  EXEC CICS
    RETURN  [TRANSID(trans-id)]
            [COMMAREA(data-name)]
  END-EXEC
  ````  
  |Option|Description|
  |-|-|
  |TRANSID|Specifies the name of the transaction to be invoked when the user presses an attention key (one to four characters)|
  |COMMAREA|Specifies the name of a data area that’s stored in the CICS communication area between program executions so it can be passed to the next execution of a pseudo-conversational program. The next program execution accesses the communication area via its DFHCOMMAREA field.|  

- **XCTL**: geef controle aan een ander programma  
  ````
  EXEC CICS
    XCTL PROGRAM(program-name)
      [COMMAREA(data-name)]
  END-EXEC

  ````  

### CICS commando’s om VSAM files te processen.  

- **DELETE**: delete record van een file
- **READ**: haal een specifiek record op van een file  
  ````
  EXEC CICS
    READ FILE(file-name)
      INTO(data-name)
      RIDFLD(data-name)
      [RESP(response-code)]       
  END-EXEC

  /*Reading from a key-sequenced file:*/
  EXEC CICS
    READ FILE(‘CUSTMAS’) 
      INTO(CUSTOMER-MASTER-RECORD)
      RIDFLD(CUSTNOI) 
      RESP(RESPONSE-CODE)
  END-EXEC
  /*Testing the response code:*/
  IF RESPONSE-CODE = DFHRESP(NORMAL)

  ````  
  |Option|Description                                                                                              |
  |------|---------------------------------------------------------------------------------------------------------|
  |FILE  |Specifies the name of the file containing the record to be read                                          |
  |INTO  |Specifies the name of the data area where the input record is to be placed                               |
  |RIDFLD|For a key-sequenced file this specifies the name of the field containing the key of the record to be read|
  |RESP  |Specifies the name of the field where CICS should store the response code from the read operation        |  

- **REWRITE**: update een record in een file
- **WRITE**: voeg een record toe aan een file


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

Een physical map heeft informatie over hoe de map moet weergegeven worden(details over de attributen van alle velden in de map, hun posities en display format van de map voor een bepaalde terminal ), een symbolic map heeft enkel de variabel data en named fields om data te kunnen lezen en schrijven in een map.  

![transaction](/images/PhisicalSymbolicMap.png)
