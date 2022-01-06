Run een programma met optie ex voor de member.  

# Voorbeelden
## H1
```
/*REXX*/
/*naam hoofdletter gevoelig*/
SAY "What's your name?"
PARSE PULL name
SAY "Hello," name
EXIT
```  

## H2

```
/*REXX*/
/*Som berekenen*/
SAY 'Please enter a number.'
PULL number1
SAY 'Now enter a number to add to the first number.'
PULL number2
sum = number1 + number2
SAY 'The sum of the two numbers is' sum'.'
EXIT
```  
````
/*REXX*/
SAY 'This is',
'a string.'

SAY 'This is' 'a string.'

/*uitkomst is*/
/*This is a string.*/

/*********************************/

SAY 'This is' ||,
'a string.'

SAY 'This is' || 'a string.'

/*uitkomst is*/
/*This isa string.*/

EXIT
````  

````
/*REXX*/
/*vraagt tijd en geef dan tijd weer met TIME comand*/
Game1: /*is een lable*/
SAY 'What time is it?'
PULL usertime /* Put the user's response into a variable
called "usertime" */
IF usertime = '' THEN /* User didn't enter a time */
SAY "O.K. Game's over."
ELSE
DO
SAY "The computer says:"
/* TSO system */ TIME /* command */
END
EXIT
````  

# H3  

