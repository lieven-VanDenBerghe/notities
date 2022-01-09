Run een programma met optie ex voor de member.  

# Voorbeelden  

## H1 Introduction to REXX
```
/*REXX*/
/*naam hoofdletter gevoelig*/
SAY "What's your name?"
PARSE PULL name
SAY "Hello," name
EXIT
```  

## H2 Writing and Running a REXX EXEC

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

Run.  
````
Run explicitly:
    exec 'hog0002.examples.rexx(test)' exec                   /*ga f3 tot REDY in rood dan enter comando*/

Run implicitly:
    alloc f(sysuexec) dsn('hog0002.examples.rexx') shr reuse  /*Alocate to file name SYSUEXEC*/

    altlib activate user(exec)                                /*Activate the user library*/

    %test                                                     /*run*/


```` 

## H3 Using Variables and Expressions    

### Aritmetic Operators  

|Operator|Meaning                      |  |Operator|Meaning      |
|--------|-----------------------------|--|--------|-------------|
|+       |Add                          |  |-       |Subtract     |
|*       |Multiply                     |  |/       |Divide       |
|%       |Divide (Wole number rest)    |  |//      |Remainder    |
|**      |Raise to a whole number power|  |-number |Negate number|
|+number |Add number to 0              |  

|Priority|Operator|Meaning                    |
|--------|--------|---------------------------|
|1       |+-      |Prefix (unary)             |
|2       |**      |Power                      |
|3       |* / % //|Multiplication and division|
|4       |+-      |Addition and subtraction   |  

````
/*REXX*/
SAY 7 + 2    /* answer is 9 */
SAY 7 ­- 2    /* answer is 5 */
SAY 7 * 2    /* answer is 14 */
SAY 7 ** 2   /* answer is 49 */
SAY 7 ** 2.5 /* answer is an error */

SAY 7.5 / 2  /* answer is 3.75 */
SAY 7 / 2    /* answer is 3.5, thus no integer division */

SAY 7.5 % 2  /* answer is 3, integer division even for reals */
SAY 7 % 2    /* answer is 3 */

SAY 7.5 // 2 /* answer is 1.5, remainder may not be integer */
SAY 7 // 2   /* answer is 1 */

EXIT
````  
### Comparison Operators  

|Oper|Meaning           |Oper|Meaning           |
|----|------------------|----|------------------|
|==  |Strictly Equal    |=   |Equal             |
|\\==|Not Strictly Equal|\\= |Not Equal         |
|>   |Greater tan       |<   |Lesser than       |
|<>  |Not equal         |>=  |Greater or equal  |
|\\< |Not less than     |<=  |Less than or equal|
|\\> |Not greater than  |  

(gives 0 for False and 1 for True)  

````
/*REXX*/
SAY "'ABC' == 'abc' gives" ('ABC' == 'abc')    /* false */
SAY "'abc' = ' abc ' gives" ('abc' = ' abc ')  /* true */
SAY "'ABC' = ' abc ' gives" ('ABC' = ' abc ')  /* false */
SAY "'DEF' > 'ABC' gives" ('DEF' > 'ABC')      /* true */
SAY "' DEF' >> 'ABC'" gives" (' DEF' >> 'ABC') /* false */

EXIT
````  

### Boolean Operators

|Precedence|Operator|Meaning     |
|----------|--------|------------|
|1         |\       |Not         |
|2         |&       |And         |
|3         |\|      |Or          |
|3         |&&      |Exclusive or|  

````
/*REXX*/
PARSE ARG season snowing broken_leg
IF ((season = ’winter’) | (snowing =’yes’)) & (broken_leg =’no’) THEN
SAY ’Go skiing.’
ELSE
SAY ’Stay home.’

EXIT
````  

### Concatenation Operators

|Operator |Meaning                                    |
|---------|-------------------------------------------|
|*blank*  |Concatenate terms with no blank in between |
|\|\|     |Concatenate terms with no blanks in between|
|*abuttal*|Concatenate terms with no blanks in between|  

````
/*REXX*/
SAY hello          world    /* (multiple) blanks */   /*HELLO WORLD*/
SAY hello   ||       world  /* || operator */         /*HELLOWORLD*/
SAY 'hello'world            /* abuttal */             /*helloWORLD*/

EXIT
````  

## H4 Flow Control  

### IF / THEN / ELSE ...  

````
/*REXX*/
IF expression THEN
    DO
        instruction
    END
ELSE NOP   /*NOP = no operation*/

EXIT
````  

### SELECT / WHEN / OTHERWISE / END  

````
/*REXX*/
SELECT
    WHEN expression THEN 
        DO
            instruction
        END
    WHEN expression THEN 
        DO
            instruction
        END
    WHEN expression THEN 
        DO
            instruction
        END
    OTHERWISE
        NOPs
END

EXIT
````  

### Loops  

#### Repetitive Loops  

````
/*REXX*/
DO 3
    SAY 'Hello!'
END

EXIT

Geeft:
Hello!
Hello!
Hello!
````  

````
/*REXX*/
DO i = 1 to 3      /* DO i = 1 to 6 BY 2 ==== in stappen an 2*/
    SAY 'Hello!'   /*Met ITERATE kan je 1 van de 3 stappen overslaan*/
END

EXIT

Geeft:
Hello!
Hello!
Hello!
````  
#### Infinite Loops  

````
/*REXX*/
DO FOREVER
    SAY "What's your name? (blank to quit)"
    PARSE PULL name
    IF name = '' THEN
        EXIT     /*Met LEAVE ga je uit de loop zonder programma te stoppen*/
    ELSE
        SAY 'Hello,' name
END
SAY 'Goodbye' /* This will never be shown! */

EXIT
````  

#### Conditional Loops  

````
/*REXX*/
DO WHILE expression /* expression must be true */
    instruction(s)

END

EXIT
````   

#### Do Until

````
/*REXX*/
DO UNTIL expression /* expression must be false */
    instruction(s)
END

EXIT
````  

## H5 Using Functions  

````
/*REXX*/
SAY 7 + max(­2,3,1) /* max is a built­in arithmetic function */

EXIT
````  

### Built-in Functions  

- Arithmetic functions  
    **ABS**: Returns the absolute value of the input number.  
    **MAX**: Returns the largest number from the list specified.  
    **MIN**: Returns the smallest number from the list specified.  
    **RANDOM**: Returns a quasi­random, non­negative whole number in the range specified.  
    ````
    /*REXX*/
    sequence = RANDOM(1, 6,12345) /* 12345 is the seed */
    DO 9
        sequence = sequence RANDOM(1,6) /* Do not use seed here! */
    END
    SAY sequence  /*resultaten van 1 tot en met 6*/

    EXIT
    ````  

- Comparison functions  
    **COMPARE**: Returns 0 if the two input strings are identical. Otherwise, returns the position of the first character that does not match.  
    **DATATYPE**: Returns a string indicating the input string has a particular data type, such as a number or character string.  

    |Letter|Meaning     |Letter|Meaning    |
    |------|------------|------|-----------|
    |A     |Alpanumeric |B     |Binary     |
    |L     |Lowercase   |M     |Mixed case |
    |N     |Number      |U     |Uppercase  |
    |W     |Whole number|X     |heXadecimal|  

    ````
    /*REXX*/
    SAY DATATYPE('12')           /*NUM*/
    SAY DATATYPE('123*')         /*CHAR*/
    SAY DATATYPE('Fred', 'M')    /*1*/
    SAY DATATYPE('Fred', 'L')    /*0*/
    SAY DATATYPE(' Fred ', 'A')  /*0*/

    EXIT
    ````  

- Conersion functions  
    ````
    /*REXX*/
    SAY D2X(42)         /* Decimal to Hexadecimal */        /*2A*/
    SAY X2D(2A)         /* Hexadecimal to Decimal */        /*42*/
    SAY C2X(' 123 ')    /* Character to Hexadecimal */      /*40F1F2F340*/
    
    EXIT
    ````

- Formatting functions  
    **CENTER**: centers a given string in a string of the specified length with padding characters as necessary.  
    **COPIES**: returns the given number of copies of the input string.  
    **JUSTIFY**: justifies a string to a specified length.  
    **LEFT/RIGT**: returns a string of the specified length, truncated or padded on the right/left as needed.  
    ````
    /*REXX*/
    SAY COPIES('*', 11) 
    SAY CENTER(' Hello ', 11, '*')
    SAY COPIES('*', 11)
    
    EXIT

    geeft:
    ***********
    ** Hello **
    ***********

    ````  

- String manipulating functions  
    **ABBREV**: checks is one string has a minimum length and is the start of another string.  
    **LENGTH**: returns the length of the input string.  
    **POS**: obtains the position of one string in another.  
    **REVERSE**: reverses a string.  
    **STRIP**:  returns a string which has the leading and/or trailing characters of the input string removed.  
    **SUBSTRING**: returns a substring of the input string.  
    **SUBWORD**: return a substring starting at the specified word number.  
    **VERIFY**:  checks whether the input string consists solely of reference characters.  
    **WORD**: returns a word from the input string.  
    ````
    /*REXX*/
    SAY "Give a word starting with a capital letter."
    PARSE PULL input /* Preserve case with parse pull. */
    input = strip(input,'L') /* Remove leading blanks. */
    capitals = "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
    IF verify(substr(input, 1, 1), capitals) = 0 THEN
        SAY "OK, your input starts with a capital letter."
    ELSE
        SAY "Hey! Please read the instructions!"

    EXIT
    ```` 
    
- Miscellaneous functions  
    **ADDRESS**: gives the name of the environment to which commands are currently being sent.  
    **ARG**: returns an argument string or information about the argument strings to a program or internal routine. (is niet arg instructie)  
    **DATE**: returns the current date or performs conversion between dates.  
    **QUEUED**: returns the number of lines on the data stack.  
    **TIME**: returns the local time.  
    ````
    /*REXX*/
    SAY "Are you American or European?"
    PULL input /* input converted to uppercase */
    SELECT
        WHEN abbrev("AMERICAN", input, 1) THEN DO
            SAY "Today's date is" date('U') || "."
            END
        WHEN abbrev("EUROPEAN", input, 1) THEN DO
            SAY "Today's date is" date('E') || "."
            END
        OTHERWISE
            SAY "Could not understand where you are from",
                "but today's date is" date('N') || "."
    END

    EXIT
    ````  

## H6 Writing Subroutines and Functions  

````
/*REXX*/
CALL subroutine_name ar1, arg2  /*subroutine*/

x = subroutine_name(arg1,arg2)  /*function*/

EXIT

subroutine_name:
    ARG argument1, argument2
    doSomting
RETURN  /*RETURN 2*(1+5)  return waarde is opgeslagen in variabele RESULT*/
````  

## H7 Manipulating Data  

### Compound Variable  

````
/**/
FRED.5
Array.Row.Col        /* simulates multiple dimensions */
employee.name.phone  /* an associative array */

EXIT
````  

````
/**/
FRED.5
employee.7 = "Mister Pink"
new = 7
SAY employee.new
employee.new = "Mister Brown"
SAY employee.7
employee = "Mister Orange" /* 'ordinary' variable */
SAY employee.1


EXIT

gives: 
Mister Pink
Mister Brown
EMPLOYEE.1

````  

Initialiseren van een compound variabele en vullen.  
````
/*REXX*/
DO i = 1 TO 4
    SAY 'Enter an employee name.'
    PARSE PULL employee.i
END


EXIT
````  

## H8 Entering Commands from an EXEC  

Programma in REXX op locatie userid.examples.exec.  
````
/*REXX*/
PARSE ARG s
SAY s

EXIT
````  

Commando dat REXX oproept.  
````
EXEC examples(targ) 'rexx.input' EXEC
````  

Resultaat is:  
````
rexx.input
````  

## H9 Debugging EXECs  

````
TRACE I
```` 
Zetten om trace te activeren.  

Error handeling.  
````
/*REXX*/
signal on error 
signal on failure 
signal on syntax 


/*code*/


signal off error 
signal off failure
signal off syntax 
EXIT

error: failure: syntax:

say 'An error has occured' 
say rc 
say signal
````  

## H10 Using TSO/E External Functions  

Some external functions.  
- **LISTDSI**: Lists data set information in specified variables (and indicates success or failure). Some variables set by LISTDSI.  

|Variable  |Contents                                                                |
|----------|------------------------------------------------------------------------|
|SYSDSNAME |Data set name                                                           |
|SYSVOLUME |Volume Serial ID                                                        |
|SYSUNIT   |Device unite on witch volume resides                                    |
|SYSDSORG  |Data set organization: PS, PO, VS, ...                                  |
|SYSRECFM  |Record format: F, FB, U, V, VB, ....                                    |
|SYSLRECL  |Logical Record Length                                                   |
|SYSBLKSIZE|Block Size                                                              |
|SYSUDIRBLK|Directory blocks used ­ returned only for PDS when DIRECTORY is specified|
|SYSMEMBERS|Number of members ­ returned only for PDS when DIRECTORY is specified    |
|SYSREASON |LISTDSI reason code                                                     |  
  

- **MSG**: Controls the display of TSO/E messages.  
- **OUTTRAP**: Traps lines of TSO/E command output into a specified series of variables.  
- **SYSDSN**: Checks whether a specified data sets exists.  
- **SYSVAR**: Can return information about the user, terminal, language, etc..  

````
/*REXX*/
x = LISTDSI(examples.rexx)
x = LISTDSI('examples.rexx') /* Looks for PREFIX.examples.rexx */

/* Looks for PREFIX.examples.rexx */
x = LISTDSI("'prefix.examples.rexx'")
x = LISTDSI('''prefix.examples.rexx''')

var = "'sys1.proclib'" /* Note use of ' inside " */
x = LISTDSI(var) /* Looks for SYS1.PROCLIB */
var = "examples.rexx"
x = LISTDSI(var) /* Looks for PREFIX.EXAMPLES.REXX */
EXIT
````  

## H11 Storing Information in the Data Stack  

````
/*REXX*/
elem1 = 'String 1 for the data stack.'
PUSH elem1                                  /* put at the top */
elemA = 'String A for the data stack.'
QUEUE elemA                                 /* put at the bottom */
PULL item                                   /* retrieve top and translate to uppercase */
SAY item
PARSE PULL item                             /* retrieve top preserving case */
SAY item

EXIT

gives: 
STRING 1 FOR THE DATA STACK.
String A for the data stack.
````  

````
number = QUEUED() /* Gets the number of elements on the data stack. */
````  

````
TSO/E REXX DELSTACK /*delete all data on stack*/
````

## H12 Input/Output Processing  

EXECI0 to read all lines from a data set allocates to the DD name MYINDD:  
````
"EXECIO * DISKR myindd (FINIS"
````  
\* = all records  
DISKR = will read (DISKRU read and update/ only last line update)  
FINIS = closes file after operation  

````
"EXECIO 25 DISKR ... " /* Reads 25 records */

"EXECIO 0 DISKR myindd (OPEN" /* Opens the data set witout readingg it. */

"ALLOC DA(io.data) F(myindd) SHR REUSE" /*allocate dataset*/

"EXECIO * DISKR myindd 100 (FINIS" /*start read at line 100*/

"EXECIO * DISKR myindd (STEM newvar." /*read all lines in a stem var*/
/*newvar.0 = nr lines, newvar.1 = first line*/

"EXECIO * DISKW myoutdd (FINIS" /*write all data from stack to data set*/

"EXECIO 25 DISKW ... " /* Writes 25 records */

"EXECIO 0 DISKW myindd (OPEN" /* Opens the data set. without writing*/

"EXECIO 0 DISKW myindd (FINIS" /*empty data set, follows the last command*/

"ALLOC DA(io.data) F(myoutdd) OLD REUSE" /*alocate dataset, Note the use of OLD! */
````  

Example: Copying a Data Set
````
/*REXX*/
/* This EXEC copies all records from PREFIX.my.input to */
/* a new dataset called PREFIX.my.output. */
/* Note: additional error checking would usually be done. */
/*****************************************************************/
"ALLOC DA(my.input) F(datain) SHR REUSE"
"ALLOC DA(my.output) F(dataout) LIKE(my.input) NEW"
"EXECIO * DISKR datain (FINIS"
n = QUEUED()
"EXECIO" n "DISKW dataout (FINIS"
"FREE F(datain dataout)"
EXIT
````  
# Oefeningen  

## 1 General Exercises  

1. Write a “Hello World” program. Your program should display the string “Hello from the Mainframe” when executed.  
Try to execute your EXEC both explicitly and implicitly.  

````
/*REXX*/         
SAY 'Hello World'

EXIT             
````  

````
Run explicitly:
    exec 'hog0002.examples.rexx(test)' exec                   /*ga f3 tot REDY in rood dan enter comando*/

Run implicitly:
    alloc f(sysuexec) dsn('hog0002.examples.rexx') shr reuse  /*Alocate to file name SYSUEXEC*/

    altlib activate user(exec)                                /*Activate the user library*/

    %test                                                     /*run*/


````  

2. Write a personalised “Hello World” program.  
    (a) In the first version the user’s name is asked interactively. The program displays “Hello from the Mainframe, name” when executed. The name should be displayed  exactly as it was entered.  

    (b) In the second version the user’s name is passed on the command line. The effect is identical as before.  

    (c) In the third version the program first checks whether something has been passed on the command line. When nothing is passed on the command line the user’s name is asked interactively, otherwise the argument on the command line is assumed to be the user’s name.  

    Thus   
%hello Watson yields   
Hello from the Mainframe, Watson  
while  
%hello  
leads to an interactive session.  

````
(a)
/*REXX*/                                   
SAY 'Enter your name.'                      
PARSE PULL name                            
SAY 'Hello from the Mainframe,' name || '.'
EXIT 

(b)
/*REXX*/                                   
SAY 'Ener your name.'                      
PARSE ARG name                             
SAY 'Hello from the Mainframe,' name || '.'
EXIT                                       

%hello Watson

(c)
/*REXX*/                                         
PARSE ARG name                                   
IF name = '' THEN                                
   DO                                            
      SAY 'Ener your name.'                      
      PARSE PULL name                            
      SAY 'Hello from the Mainframe,' name || '.'
   END                                           
ELSE                                             
   DO                                            
      SAY 'Hello from the Mainframe,' name || '.'
   END                                           
EXIT                                             

````  

3. Write a REXX procedure that checks whether or not an email address is valid. An email address is valid when it contains exactly one ’@’ character. Before (user name) and after (domain) the ’@’ character there should only be alphabetic characters (i.e. letters and digits) and periods (.). The procedure should be called as follows:  
call checkemail email  
The variable RESULT is 0 when the email address is valid. In this case the data stack should contain the user name and domain from the email address. Thus, pulling an item from the data stack yields the user name, doing a second pull should yield the domain. 2 REXX Exercises September 19, 2019When the email address is not valid, then the RESULT variable should indicate what is wrong with the email address. The data stack should be unchanged in this case. Test the procedure by calling it from a main program that repeatedly asks for an email address (until the user decides to quit).  

````
/*REXX*/                                                      
SAY 'Enter a email to check.'                                 
SAY 'Enter 0 to quit'                                         
DO FOREVER                                                    
   PARSE PULL email                                           
      IF email = 0 THEN                                       
         LEAVE                                                
   CALL checkemail email                                      
   SAY 'Enter a email to check.'                              
   SAY 'Enter 0 to quit'                                      
END                                                           
EXIT                                                          
                                                              
checkemail:                                                   
 PARSE ARG adres                                              
 PARSE VAR adres v1"@"v2"@"v3 .                               
 IF v2='' | v3<>'' THEN                                       
    DO                                                        
       SAY 'A valid email address contains one @'             
       RETURN                                                 
    END                                                       
 alpha = 'abcdefghijklmnopqrstuvwxyzABCDEFGIJKLMNOPQRSTUVWXYZ'
 nrAndPoint = '0123456789.'                                   
                                                              
 alphanr = alpha || nrAndPoint                                
 IF VERIFY(v1, alphanr)<>0 | VERIFY(v2, alphanr)<>0 THEN      
    DO                                                        
       SAY 'Email address must consist of a-z A-Z 0-9 or . .' 
       RETURN                                                 
    END                                                       
 PUSH v2                                                      
 PUSH v1                                                      
 PARSE PULL item                                              
 SAY 'User name:' item                                        
 PARSE PULL item                                              
 SAY 'Domain:' item                                                          
 SAY 'Email address:' adres
 SAY ''                    
RETURN                                                
````  

4.  (a) Write a program to calculate a person’s age in days. The person’s birthday is entered interactively in the format dd/mm/jjjj. Hint: Look up the date function.  
(b) Enhance your program so that it first checks whether the given birthday is valid. In particular you will need to check that the three constituents of the given date are indeed whole numbers. Further, you should check that the number of days in the month is valid. Please consider leap years correctly:  
• any year not divisible by four is not a leap year  
• a year divisible by four is a leap year, however  
• if the year is divisible by 100 it is not a leap year, however  
• if the year is divisible by 400 it is a leap year.  
Thus, 1996 is a leap year, 2000 is a leap year, but 1900 is not a leap year.  

````
````  
