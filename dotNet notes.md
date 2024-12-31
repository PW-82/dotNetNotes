Notatki programisty .net

### Utowrzenie projektu konsolowego z terminala
```console
dotnet new console -n <project name> --target-framework-override net7 --use-program-main
```
Powyższe polecenie tworzy projekt konsolowy w wersji .net7

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

### Krotki (tuples)

```C#
namespace tuples;
public class exampleClass
{
  public (string, int) tuple1()
  {
    return ("Something", 3);
  }
  public (int, int) tuple2 = (1,2);
}
```

użycie :

```C#
exampleClass example = new();
Console.WriteLine(example.tuple1().Item1);
Console.WriteLine(example.tuple2.Item1);
```
Dekonstrukcja krotki :
```C#
(string exampleString, int exampleInt) = class.MethodReturnsStringAndInt();
```

Dekonstrukaja typów :

```C#
public void Deconstruct(out string fname, out string lname)
{
    fname = FirstName; //fname zmiennna deklarowana globalnie w klasie
    lname = LastName;  //lname zmiennna deklarowana globalnie w klasie
}

var p = new Person("John", "Quincy", "Adams", "Boston", "MA"); //konstrukcja obiektu prze konstruktor
var (fName, lName, city, state) = p; //dekonstruktor wywołanie automatyczne po typach zadeklarowanych i ustawienie zmiennych przez "out"
```
