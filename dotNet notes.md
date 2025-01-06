Notatki programisty .net
### Ogólne

Sposób zapisu kodu znajduje się w tej konwencji
https://learn.microsoft.com/en-us/dotnet/standard/design-guidelines/capitalization-conventions

### Utworzenie projektu konsolowego z terminalu
```console
dotnet new console -n <nazwa projektu> --target-framework-override net7 --use-program-main
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

Aby móc korzystać z polimorfizmu, należy metodę bazową oznaczyć jako <code>virtual</code> lub
<code>abstract</code>. Abstract działa podobnie jak w javie.
Metody oznaczone jako <code>virtual</code> mogą być używane jako normalne metody, nie są abstrakcyjne.
Jeżeli chcemy skorzystać z polimorfizmu, przesłaniamy je przez metody oznaczone jako <code>override</code>

```C#
public class BaseClass
{
    public virtual void DisplayMessage()
    {
        Console.WriteLine("Message from BaseClass");
    }
}

public class DerivedClass : BaseClass
{
    public override void DisplayMessage()
    {
        Console.WriteLine("Message from DerivedClass");
    }
}

public class Program
{
    public static void Main(string[] args)
    {
        BaseClass obj1 = new BaseClass();
        obj1.DisplayMessage(); // Wywoła "Message from BaseClass"

        DerivedClass obj2 = new DerivedClass();
        obj2.DisplayMessage(); // Wywoła "Message from DerivedClass"
    }
}
```

przesłanianie za pomocą <code>new</code>

```C#
using System;

public class BaseClass
{
    public void DisplayMessage()
    {
        Console.WriteLine("Message from BaseClass");
    }
}

public class DerivedClass : BaseClass
{
    public new void DisplayMessage()
    {
        Console.WriteLine("Message from DerivedClass");
    }
}

public class Program
{
    public static void Main(string[] args)
    {
        BaseClass baseObj = new BaseClass();
        baseObj.DisplayMessage(); // Wywoła "Message from BaseClass"

        DerivedClass derivedObj = new DerivedClass();
        derivedObj.DisplayMessage(); // Wywoła "Message from DerivedClass"

        BaseClass baseDerivedObj = new DerivedClass();
        baseDerivedObj.DisplayMessage(); // Wywoła "Message from BaseClass"
    }
}
```

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

### Przekazywanie parametrów do metod

out - deklarujemy w metodzie. Jest paramtetrem wyjściowym, służy do zwracania wartości z metody <br>
ref - parametr wejściowo/wyjściowy. Dokonujemy zmiany poprzez referencje (NIE KOPIE). Wiec zmianie ulega zarówno warotść w metodzie jak i zmienna globalna

```C#
int a = 1;
int b = 1;
int c = 1;
Console.WriteLine($"Before: a = {a}, b = {b}, c = {c}");
// tu może być użyta zmienna "out c" z jednocześnie zadeklarowaną zmienną globalną c. Po ykonaniu kodu c zostanie nadpisana nowym wynikiem
PassParams(a, ref b, out c);
Console.WriteLine($"After: a = {a}, b = {b}, c = {c}");
// w tym przypadku "out int c", nie może być deklarowanej zmiennej globalnej c ponieważ jest deklarowana podczas przekazywania do metody !
PassParams(a, ref b, out int c);
public void PassParams(int x, ref int y, out int z)
{
  z = 11; // out musi być zadeklarowana w metodzie
  x++;
  y++;
  z++;
}

```

Partial - podziaał klasy na kilka plików
```C#
public partial class ExampleClass
```

### Elementy członkowskie wyrażeń (expression-bodied member)

```C#
public int Add(int x, int y)
{
    return x + y;
}
```
można zapisać jako
```C#
public int Add(int x, int y) => x + y;
```

Inicjacja pola
```C#
public int Age => DateTime.Now.Year - BirthYear;
```
znak równości "=" zamiast "=>" powyżej, nie zadziała bo jest to wyliczenia przed inicjają wartości
<br>
Konstruktor:
```C#
public Person(string name) => Name = name;
```

### Przeciążanie operatorów

```C#
public class Complex
{
    public double Real { get; }
    public double Imaginary { get; }

    public Complex(double real, double imaginary)
    {
        Real = real;
        Imaginary = imaginary;
    }

    // Przeciążenie operatora +
    public static Complex operator +(Complex c1, Complex c2)
    {
        return new Complex(c1.Real + c2.Real, c1.Imaginary + c2.Imaginary);
    }
}
```

```C#
Complex c1 = new Complex(1.1, 4.3);
Complex c2 = new Complex(2.0, 3.2);
Complex sum = c1 + c2; // Wynik to liczba zespolona (3.1, 7.5)
```

### Metody lokalne
Metoda może posiadać w swoim ciele metodę lokalna (zagnieżdżoną), dostępną tylko ze środka metody rodzica.

### Obiekty immutable
```C#
public class Osoba 
{
public string? Imie {get; init;}
}

Osoba ktos = new()
{
   Imie = "Pankracy";
}
ktos.imie = "nowe imie"; // nie przejdzie bo jest init czyli tylko inicjalizacja, bez możliwości zmiany
```
