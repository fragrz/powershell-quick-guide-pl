# Wstęp

## O przewodniku

Przewodnik skupia się na używaniu PowerShell do codziennych zadań takich jak poruszanie się po katalogach, używanie kontroli wersji, przenoszenie i wyszukiwanie plików.

## O powershell
Multiplatformowy, otwarto źródłowy interpreter poleceń ze wsparceim .NET, opracowany przez Microsoft.

# Instalacja
Windows 10+ posiada domyslnie zainstlowane środowisko. Aby zainstalować na linux lub mac zanjdź pomoc w google.

## Windows Terminal
Nowoczesny terminal. Wspiera karty i może działać w różnych trybach takich jak PowerShell, CMD, Bash, Azure. Warty zainstalowania ale nie wymagany.
 
## Uprawnienia administratora
Nadanie uprawnień administratora PowerShell lub Windows Terminal uniknie wielu potencjalnych problemów z wywoływaniem komand.
 
## Aktualizacja plików pomocy
Po zainstalowaniu środowiska wywołaj ```Update-Help```. Wymagany dostęp do internetu. Więcej o komendach w dalszej części.
 
## Wsparcie GIT
Moduł wyświetlający informacje o aktualnym repo, dodaje autokomplementację komend GIT.
Aby zainstalować wywołaj ```Install-Module posh-git -Scope CurrentUser -Force```. 

## Przypięcie do paska zadań
W celu łatwiejszego dostępu, można przypiąć Windows Terminal lub PowerShell do paska zadań.
Przypięcie na pierwszej pozycji pozwoli na uruchamianie go  skrótem klawiszowym ``` Win+1```.

# Środowisko

## Powszechne skróty

| Skrót | Opis |
| :---  | :--- |
| ```WT``` | Windows Terminal |
| ```PS``` | PowerShell |
| ```cmdlet``` | natywna komenta PowerShell |

## Nawigacja
| Kombinacja klawiszy| Efekt |
| :--- | :--- |
| ```tab``` | Uzupełnia komendę lub dodaje podkatalog aktualnego katalogu |
| ```ctrl+space``` | Interaktywnie uzupełnia komendę oraz jej parametry |
| ```enter``` | Wywołuje wpisaną komendę lub przechodzi do nowej linii |
| ```strzałki góra dół ``` | Wyświetlają ostanio użyte komendy |
| ```ctrl+c``` | Przerwywa proces |



# Cmdlet
Szybkie fakty:

- zwracają obiekty a nie stringi
- case insensitive
- ich parametry mogą być przekazwyane po nazwie lub pozycyjne
- nazwy mają format czasownik-rzeczownik (np. ```Get-Item```)
- wspierają stałe administracyjne takie jak GB, kb itp.

Aby przekazać dane używany jest tzw. pipieline za pomocą znaku ```|```, np:

```Get-Item Path1\* | Move-Item -Destination Path2```

Cmdlety pracują równolegle, tj. cmdlet po znaku ```|``` nie czeka na zakończenie działania tego przed, ale na otrzymanie od niego porcji danych.

Aby wywołać program z aktualnej lokalizacji poprzedź jego ścieżkę ``` .\ ```

Aby wywołać cmdlet ze spacją użyj składni ``` &'.\Program With Spaces.exe' ```

Wiele popularnych komend posiada tzw. aliasy czyli krótszą wersję. Np. ```Invoke-WebRequest``` posiada alias ```iwr```.

# Najczęściej używane cmdlet

| Cmdlet | Efekt |
| :--- | :--- |
| ``` ii ``` | uruchamia plik w jego domyślnym programie |
| ``` out-gridview ``` | przekazuje wynik do graficznej tabelki. Z parametrem ```-PassThrough``` zwraca zaznaczone elementy do pipeline |
| ``` ? <dana> -<operator porównania> ``` |  słuzy do filtrowania obiektów na podstawie parametrów |
| ``` % ``` | iteruje po kolekcjach (tablicach, listach) |
| ``` sort ``` |  sortowanie kolekcji |
| ``` (Warunek) ? "gdy PRAWDA" : "gdy FAŁSZ" ``` | Tenary operator |


# Odkrywanie
Ważną cechą PS jest łatwe poznawanie środowiska.

## Znajdowanie potrzebnej komendy

Cmdlet ```gcm``` znajduje komendę zawierającą dane słowo kluczowe

## Informacje o komendzie 

Aby wyświetlić pomoc dotyczącą cmdlet, użyj ```Get-Help```. Uwaga! ```help``` nie działa identycznie, dzieli wynik na strony.

## Informacje o obiekcie

Inormacje o obiekcie np. zwracanym przez cmdlet można uzyska używając ```gm```. Wyświetla listę metod i parametrów obiektu.
Jeśli chcesz sprawdzić samą kolekcję a nie jej członków dopisz parametr ``` -InputObject ```

## Symulacja wykoania komendy

Cmdlety modyfikujące dane wspierają parametr ```-WhatIf```, który dodany do cmdlet spowoduje symulację jego wykonania. 

## Aliasy

Wiele często używanych komend posiada krótsze, alternatywne nazwy czyli aliasy. Użyj ```gal``` aby wyświetlić ich listę.
Aby sprawdzić pojedynczy alias użyj ```gal <alias>```. Aby znaleźć alias cmdlet wpisz ```gal -definition <cmdlet>```

# Historia sesji

## Szybkie przeszukiwanie historii sesji
Poprzedź słowo kluczowe znakiem ```#```

## Wyświetlenie historii
Każda sesja posiada pamięć wpisanych cmdlet, ```h``` ją wyświetli.

## Nagrywanie sesji
Rzopcznij nagrywanie sesji ```Start-Transcript```, zakończ ```Stop-Transcript```. Pozwala np. zapisać sesję do pliku.


# Zmienne
Szybkie fakty:

- operator przypisania to znak równości
- można przechowywać w nich wszystko
- zaczynają się od ```$```
- dane wyjściowe poprzedniego cmdlet znajdują się w zmiennej ```$_```

Istnieje wiele predefiniowanych zmiennych. Użyj ```Get-Help about_Automatic_Variables``` aby je wyświetlić.

W celu użycia znaków specjalnych w nazwie zmiennej uzyj składni ``` ${nazwa ze znakami specjalnymi} ```


## Scope zmiennych

Jeśli blok kodu wchodzi do kolejnego bloku, PS tworzy nowy Scope, tzw. Child Scope; stary to Parent Scope.
Child Scope ma dostęp do zmiennych w Parent Scope ale ich modyfikacja ma efekty tylko we własnym Scope. 

# Stringi

Szybkie fakty:

 - escape char to ``` ` ``` a nie ``` \ ``` jak w innych powłokach. Ma to na celu ułatwienie wpisywania ścieżek.

## Expandable i literal

``` "Tekst" ``` Zmienne wewnątrz rozszerzane są na ich wartości.

``` 'Tekst' ``` Traktowany jest jako dosłowny string.

Aby wstawić ' lub " w powyższych, należy go podwoić np ``` "przed ""TEST"" po" ```

## Tekst wieloliniowy

Tzw. _Here string_. Składnia wygląda następująco:

````
@"
Pierwsza linia
Druga linia
"@
````

```@"``` oraz ```"@``` **MUSZĄ** być w osobnych liniach!

## Łączenie stringów

Łączenie stringów można wykonać:

- Za pomocą operatora ```+```
- Używając unary join: ```-join ("A","B","C")```
- Używając binary join: ```("A","B","C") -join "`r`n"```
- Dłuże stringi można wydajnie łączyć .NETowym StringBuilderem. Więcej o wywoływaniu .NET w dalszej części.


## Zastpępowanie

Zastępowanie tekstu wewnątrz string wygląda następująco:

``` "Tekst do zastąpienia" -replace '<regexp>','tekst docelowy' ```

Tekst docelowy może zawierać znaczniki grup ```$1``` itd.

W regexpach przeważnie są znaki specjalne używane przez PS, aby uniknąć problemów przepuść wyrazenie przez ``` [Regex]::Escape() ```

## Rozdzielanie 

Jeżeli string rozdzielany jest znakiem tak jak np. csv użyj ```"a-b-c-d-e-f" -split "<regexp>"```. Wynik to lista.

## Reformatowanie

Najłatwiej uzyć reformatowania na podstawie przykładów.

``` Convert-String -example <"5551212=(425) 555-1212","4524587=(425) 452-4587"> ```

## Tworzenie obiektów ze stringa

Szczególnie przydatne jeśli program zwraca jeden wielki string a chcemy na nim operować w PS.

Aby utworzyć obiekt na podstawie znaku rozdzielającego: ```ConvertFrom-String -Delimiter``` 

Obiekt można utworzyć równiż na podstawie przykładowego stringa za pomocą ```ConvertFrom-String -Template```
Wystarczy otagować dane w przykładowym stringu: ```{Nazwa:Wartość}```

# Operacje matematyczne

# Wprowadzanie liczb
Szybkie fakty:

- Separatorem dziesiętnym jest kropka
- Domyslny format wprowdzania to liczby dziesiętne
- Poprzedź liczbę ```0x``` aby była interpretowana jako szesnastkowa
- Poprzedź liczbę ```0b``` aby była interpretowana jako binarna

## Porównywanie

### Operatory logiczne:

| Operator |
| :--- |
| ```-and``` |
| ```-or```  |
| ```-xor``` |
| ```-not``` |

Aby wykonać operacje logiczne na poszczególnych bitach, dodaj ```b``` na początku np. ```-band```

### Operatory porównań matematyczncyh:

Jeżeli porównywane są stringi, poniższe operatory są case-insensitive, dodaj ``` c ``` na początku aby były case sensitive.

| Operator | Operacja |
| :--- | :--- |
| ```-eq```          | = |
| ```-ne```          | != |
| ```-ge```          | >= |
| ```-gt```          | > |
| ```-lt```          | < |
| ```-le```          | <= |

### Operatory porównań tekstowych:

Istnieją również zanegowane warianty operatorów, wystarczy dodać na początku ```not```.

| Operator | Operacja |
| :--- | :--- |
| ```-like```        | Wildcard |
| ```-match```       | Wyrażenie regularne |

### Operatory porównań obiektów i kolekcji:

Istnieją również zanegowane warianty operatorów, wystarczy dodać na początku ```not```.

| Operator | Operacja |
| :--- | :--- |
| ```-in```          | Wartość znajduje się w kolekcji |
| ```-contains```    | Kolekcja zawiera wartość |
| ```-is```          | Obiekty są tego samego typu |

# Operacje matematyczne

## Podstawowe

PS wspiera podsawowe operacje na podstawie znaku. Wspierana jest poprawna kolejność wykonywania działań, również z nawiasami.

| Operator | Operacja |
| :--- | :--- |
| ```+```          | Dodawanie |
| ```-```          | Odejmowanie |
| ```*```          | Mnożenie |
| ```/```          | Dzielenie |
| ```%```          | Modulo |

## Zaawansowane

Zaawansowane operacje matematyczne nie są wspierane natywnymi cmdlet. Użyj .NETowej biblioteki ``` [Math] ```

## Statystyki obiektu

Aby wyśiwetlić numeryczne stastyki obiektu np. rozmiar, wartość średnia, suma, wartość maksymala i minimalna użyj ```Measure-Object```

# Formatowanie obiektów

## Format-Table

Większość cmdletów które coś listują zwraca dane w formie tabeli.

Podaj nazwy kolumn które chcesz wyświetlić (np. ```Format-Table Name,WS```)

Parametr ```-Auto``` zwiększa czytelność ale zmniejsza wydajność

Nie należy używać wewnątrz pipeline!

## Fromat-List

Listuje parametry obiektu w formie ```<Nazwa>:<Wartość>```.

Dodaj ```*``` aby wylsitować wszystkie parametry. Domyslnie lisotowane są tylko predefiniowane parametry określone w konfiguracji.

Nie należy używać wewnątrz pipeline!

[](## Format-Wide)

# Katalogi

## Nawigacja

### Przejście do lokalizacj

```cd <pełna ścieżka>``` lub ```cd .\<względna ścieżka>``` 

### Przejdź do katalogu użytkownika

``` cd ~\ ```

### Stos ścieżek

Dodaj aktualną ścieżkę na stos za pomocą ``` pushd ``` lub pobierz ją używając ``` popd```.

[](### Wstecz i dalej)

[](Wróć do poprzedniej ścieżki ``` cd -```; przejdź do następnej ``` cd + ```. Tak jak wstecz i dalej w exploreże albo przeglądarce)

## Operacje na katalogach

### Tworzenie

```md <nazwa katalogu>```

### Usuwanie

```del``` W przypadku katalogów, jeśli coś w nich jest, PS zapyta czy na pewno usunąć. Aby tego uniknąć wpisz ```-Recursive```

### przenoszenie

```move <co> -destination <gdzie>```

### Wypakowanie archiwum

```Expand-Archive <ścieżka archiwum> -DestinationPath <ścieżka docelowa>```

W przypadku pominięcia ```-DestinationPath``` zostanie utworozny katalog obok archiwum o takiej samej nazwie jak archiwum.

# Integracja z .NET
Cmdlety są de-facto wrapperami .NETowych klas, więc integracja jest bardzo dobra.

## Wywoływanie metody / property
``` [<NazwaKlasy>]::<Property>lub<Metoda(Parametry)>  ``` lub ``` $ReferencjaObiektu.<Property>lub<Metoda(Parametry)>  ```

## Sprawdzenie przeciązęń metody
Aby sprawdzić przeciązęnia metodty, wpisz metodę bez ```()``` np. ``` [System.Date.Time]::ToString ```

### Instancjonowanie klasy

Za pomocą bezparametrowgo konstruktora: ``` New-Object ``` 

Za pomocą metody new ``` [System.Drawing.Bitmap]::new("$pwd\source.gif") ``` 

Jednoczesne instancjonowanie i uzycie bez zapisywania referencji ``` (New-Object Net.WebClient).DownloadString("http://live.com") ``` 


## Namespace

Jeżeli korzystamy z kliku klas tego samego namespace, możliwe jest użycie komendy wskazującej skąd będą używane obiekty:

```using namespace System.Collections```

## Generyki

Wspierane są generyki. Typ definiuje się poprzez wpisanie go w nawiasie kwadratowym np:

``` New-Object System.Collections.ObjectModel.Collection[Int] ```


## Tworzenie własnych obiektów

Najczęściej tworzone są obiekt klasy ```PSCustomObject```, które używane są jako tzw. "property bags":

``` [PSCustomObject] @{'User' = 'DOMAIN\User';'Quota' = 100MB;'ReportDate' = Get-Date;} ```

Aby dodać funkcjonalność do klasy trzeba utworzyć skrypt.

# Dostosowywanie środowiska

## Aliasy

Aby utworzyć alias uzyj ``` Set-Alias <nazwa> '<tutaj treść>' ```. Aby go usunąć: ``` del alias:\new ```

Aliasy nie mogą przyjmować parametrów.

## Funkcje
W ``` $profile ``` umieszczone są funkcje uzytkownika. Otwórz w dowolnym edytorze i dopisz swoje funkcje.
Nazywanie funkcji jest bardzo ważne w PS. Użyj ```get-verb``` aby otrzymać listę 
standartowych czasowników z ktorych najlepiej  wybrać początek nazwy własnej funkcji.

Jeżeli funkcja jest czasochłonna uzyj ```Write-Progress``` aby wyświetlić postęp jej wykonaywania.
