# Zadanie 4

W ramach tego zadania należy dla podanego DataSet (UK-Crimes), wyznaczyć statystki przestępstw dla danego miasta.

Statystyka powinna zawierać 

| Nazwa pola  | Opis               |
| ----------- | ------------------ |
| CrimeType   | Typ przestępstwa   |
| City        | Miasto             |
| TotalCrimes | Liczba przestępstw |

Dane wynikowe powinny zostać zapisane do pliku csv z nagłówkiem, posortowane po największej liczbie przestępstw dla danego miasta.

Lista pól w plikach źródłowych

| Pole                | Typ    |
| ------------------- | ------ |
| CrimeID             | string |
| Month               | string |
| ReportedBy          | string |
| FallsWithin         | string |
| Longitude           | string |
| Latitude            | string |
| Location            | string |
| LSOACode            | string |
| LSOAName            | string |
| CrimeType           | string |
| LastOutcomeCategory | string |
| Context             | string |

Struktura danych źrodłowych

![](../Imgs/UkCrimes.png)

### Zadanie dodatkowe

Należy funkcjonalność ładowania przenieść do funkcji tabelarycznej.