Notatki programisty .net

### Liczby i operacje

Wartości liczbowe (nie wszystkie, tylko najpopularniejsze w użyciu):
```C#
sbyte   8 bit
byte    8 bit
short   16 bit
int     32 bit
long    64 bit
decimal 64 bit
double  64 bit
```

sprawdzenie czy nie przepełniamy wartości za pomocą konstrukcji
```C#
checked
{
    return liczba + Int32.MaxValue;
}
```
bez tego sprawdzenia 
```C#
return liczba + Int32.MaxValue;
```

dochodzi do niejawnego przekręcenia licznika (dowiesz się dopiero w wyniku).
Z sprawdzeniem przy przekroczeniu wartości trzeba użyć try - catch bo wyrzuca wyjątkiem.
```C#
checked
{
    try 
    {
    return liczba + Int32.MaxValue;
    }
    catch(System.OverflowExcepion ex)
    {
    }
    catch(Exception ex)
    {
    }
}
```

### Polimorfizm i dziedziczenie :

przesłanianie intencjonalne metod dodajemy słówko <code>new</code> //TODO dokończyć
<br>
do dokończenia (...)

### Dostęp i modyfikatory dostępu:

namespace można deklarować jako
```C#
namespace nazwaPrzestrzeni;
class someClass()
{
}
```
lub
```C#
namespace nazwaPrzestrzeni
{
    class someClass()
    {
    }
}
```
jeżeli klasa nie jest zadeklarowana explicite jako public, to ma modyfikator internal
jeżeli pola nie mają modyfikatora to stają się private

<pre>
Caller's location                           public      protected internal      protected   internal    private protected   private     &nbsp;file
Within the file                             ✔️️          ✔️                      ✔️          ✔️          ✔️                  ✔️          ✔️
Within the class                            ✔️️          ✔️                      ✔️          ✔️          ✔️                  ✔️          ❌
Derived class (same assembly)               ✔️          ✔️                      ✔️          ✔️          ✔️                  ❌          ❌
Non-derived class (same assembly)           ✔️          ✔️                      ❌          ✔️          ❌                  ❌          ❌
Derived class (different assembly)          ✔️          ✔️                      ✔️          ❌          ❌                  ❌          ❌
Non-derived class (different assembly)      ✔️          ❌                      ❌          ❌          ❌                  ❌          ❌
</pre>

### Stałe

<code>Const vs Readonly</code>
```C#
const
``` 
musi być podana wprost, jest niezmienna i musi być znana w trakcie kompilacji - dobra praktyka nakazuje używanie readonly
```C#
readonly
```
może być wyliczane i jest podstawiane w runtime, nie podczas kompilacji.

<code>_</code> podkreślenie używane jest jako wildcard np w instrukcji <code>swich</code> podobnie <code>default</code> w java, albo w string jako dowolny nieokreślony znak.
//TODO ciąg dalszy nastąpi

