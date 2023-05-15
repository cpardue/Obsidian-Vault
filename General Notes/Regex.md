```
Use regex101.com for testing. 

(?i) - case insensitive.
[a-z0-9] - letters (lowercase) a to z, numbers 0 to 9.
{5} - match 5 characters of the previous trait. 
Such that regex (?i)[lo!]{3,4} will match lol, lol!, LOL, LOL!, Lol!, etc. 

\.[a-zA-Z0-9]*\.[comi]{2,3} match .somedomain.com or .domain.io.

(?i)[p]=[a-zA-Z]*; match p=SoMeThInG;

```