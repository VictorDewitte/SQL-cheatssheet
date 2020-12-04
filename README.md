# Databases cheatsheet

## HO7

## Werken met 1 tabel

``` sql
SELECT [Gewenste kollomen]
FROM [Tabel waar kollomen inzitten];
```
### Selectie
```sql
SELECT [Gewenste kollomen]
FROM [Tabel waar kollomen inzitten]
WHERE [Selectievoorwaarde bv. afdNr ='D11'];
```
### Vergelijkingsoperatoren

```sql
SELECT [Gewenste kollomen]
FROM [Tabel waar kollomen inzitten]
WHERE [Selectievoorwaarde bv. code = 56 , code >= 56];
```
### Gebruik van LIKE of NOT LIKE

```sql
SELECT [Gewenste kollomen]
FROM [Tabel waar kollomen inzitten]
WHERE [Selectievoorwaade] LIKE ['%P'];
```
<br>

#### Wildcards
naam beginnend met P: ’P%’ \
naam eindigend op C: ’%C’ \
naam beginnend met ABC: ’ABC%’ \
naam eindigend met XYZ: ’XYZ%’ \
naam beginnend met A en eindigend op C: ’A%C’ \
naam die ergens SON bevat: ’%SON%’ <br><br>

### AND, OR en NOT
#### AND
```sql
SELECT [Gewenste kollomen]
FROM [Tabel waar kollomen inzitten]
WHERE [Selectievoorwaade1]  AND  [Selectievoorwaade2];
```
Zowel sectievoorwaarde1 en Selectievoorwaade2 moeten gelden om gekozen te worden uit de tabel.<br>
<br>

#### OR
```sql
SELECT [Gewenste kollomen]
FROM [Tabel waar kollomen inzitten]
WHERE [Selectievoorwaade1]  OR  [Selectievoorwaade2];
```
Een van de twee sectievoorwaarden moet gelden om gekozen te worden uit de tabel.

#### Vermijd het gebruik van de NOT operator 
<br>


### BETWEEN
```sql
SELECT [Gewenste kollomen]
FROM [Tabel waar kollomen inzitten]
WHERE [Selectievoorwaade1] BETWEEN ['waarde1'] AND ['waarde2'];
```
<br>

### IN

Hierdoor kan men de inhoiud van een veld vergelijken met een lijst van waarden.
```sql
SELECT [Gewenste kollomen]
FROM [Tabel waar kollomen inzitten]
WHERE [Selectievoorwaade1] IN (['waarde1'] , ['waarde2']);
```

<br>

### Gebruik van NULL waarde

```sql
SELECT [Gewenste kollomen]
FROM [Tabel waar kollomen inzitten]
WHERE [Selectievoorwaade1] IS NULL;
```
### Combinatie voorbeeld
```sql
SELECT [Gewenste kollomen]
FROM [Tabel waar kollomen inzitten]
WHERE [Selectievoorwaade1] = [waarde]
    AND [Selectievoorwaade2] IN (waarde, waarde)
    AND [Selectievoorwaade3] BETWEEN [waarde1] AND [Waarde2]
    AND [Selectievoorwaade4] IS NULL;
```
<br>

## Formateren van het resultaat

### <u>Gebruik van ORDER BY</u>
ALGEMENE VORM

```sql 
SELECT *
FROM [Tabel waar kollomen inzitten]
ORDER BY [Kolom-Specificatie] ASC/DESC;
```

Vb.
```sql
SELECT [Gewenste kollomen]
FROM [Tabel waar kollomen inzitten]
WHERE [Selectievoorwaade1] IS NULL
ORDER BY [Selectievoorwaarden] DESC;
```
<br>

### <u>Gebruik van DISTINCT/ALL</u>

```sql
SELECT = SELECT ALL
```
```sql
SELECT DISTINCT
```
Distinct gaat alle dubbele waarden overslaan\

<br>

BELANGRIJK VOORBEELD

```sql
SELECT FNaam
FROM Werknemer
WHERE FNaam = 'De Bruyn';
```
Elke keer dat 'De Bruyn' in de kolom staat wordt getoond voornaam maakt niet uit.
<br><br>


```sql
SELECT DISTINCT FNaam
FROM Werknemer
WHERE FNaam = 'De Bruyn';
```
Nu kom je 1 keer 'De Bruyn' uit ook al zijn er 2 werknemers met achternaam 'De Bruyn' en andere voormaan. 

<br>

Oplossing
```sql
SELECT DISTINCT FNaam , VNaam
FROM Werknemer
WHERE FNaam = 'De Bruyn';
```

<br>

### <u>Gebruik van Aliassen </u>
```sql
SELECT DISTINCT [Kolom allias] AS ['KolomNaam']
FROM [Tabel waar kollomen inzitten];
```

<br>

### Enkele handige functies
<u><b>Datum en tijd functies</b></u>
```sql
CURDATE();
```
Huidige datum JJJJ-MM-DD.
```sql
CURTIME();
```
Huidige tijd UU-MM-SS.

```sql
DATEDIFF(expr1, expr2)
```
Geeft vershil expr1 - expr2 in aantal dagen.
```sql
DATE_FORMAT(Datum, Format);
```
```Sql
SELECT DATE_FORMAT('2017-10-15' , '%W %d %M %Y')
```
<br><br>
```sql
DAY();
```
Geeft de dag van de maand weer. getal tussen 1-31.

```sql
NOW()
```
Geeft huidige datum en tijd weer in formaat YYYY-MM-DD HH:MM:SS

<br>

<u><b>Control flow functies</b></u>
```sql
SELECT *
CASE 
    WHEN [Selectievoorwaade1] IS NULL 
        THEN 'Result'
    WHEN [Selectievoorwaade1] = 'value' 
        THEN 'Result'
    ELSE 'result'
END AS []
FROM []
ORDER BY []
```
```sql
IF(expr1, expr2,expr3)
```
Geeft expr2 indien expr1 waar is en veschillend van NULL zoniet is het resultaat expr3
```sql
IFNULL
```
```sql
NULLIF
```

### <u>Statiche Functies </u>
```sql 
AVG
```
Gemiddelde
```sql
SUM
MIN, MAX
```
<br><br>

```sql
COUNT(*) : Telt Totaal aantal rijen
COUNT(KolomNaam) : Telt aantal velden in kolom
COUNT(DISTINCT KolomNaam)
```

<br>

### <u>Gebruik van GROUP BY </u>

Vb. Geef het maximum, het minimum en het gemiddelde salaris van alle werknemers per afdeling.

```sql
SELECT Afd, MAX(Salaris), MIN(Salaris), AVG(Salaris)
FROM Werknemer
GROUP BY Afd
```

<br>

<br>

## HO8
## Werken met meedere Tabellen

### <b><u>JOIN van verschillende tabellen</b></u>
<b>INNER JOIN van vershillende tabllen</b>\
2 manieren
```sql
SELECT [Kol1], [kol2], [Kol3]
FROM [Tabel1] JOIN [Tabel2] ON [Tabel1.Kol2] = [Tabel2.Kol3]
```
De ON Clause specifieert dan een bepaalde relatie tussen de rijen die moeten samengevoegd worden

<br>

```sql
SELECT [KOL1], [Kol2]
FROM [Tabel1] ,  [Tabel2]
WHERE [Tabel1].[Kol1] = [Tabel2].[Kol2];
```
### <u> Join van een tabel met <b>Zichzelf</b></u>

```sql
SELECT X.[KOL1], Y.[Kol2]
FROM [Tabel1]X JOIN [Tabel1]Y ON X.[KOL1] = Y.[KOL1]
WHERE X.[KOL2] >= Y.[KOl2]+5;
```
<br>

### <u>OUTER JOIN</u>
Er zijn 3 types van outer join
```sql
LEFT (OUTER) JOIN
```
Retourneert alle rijen van de eerst genoemde tabel in de FROM clause
```sql
RIGHT (OUTER) JOIN
```
Retourneert alle rijen van de tweede tabel in de FROM clause
```sql
FULL JOIN
```
Retourneert ook rijen uit de eerste en tweede tabel die geen corresponderende entry hebben in andere tabel. De resultest bevat steeds alle rijden van de eerste tabel en alle rijen van de tweede tabel.

<br>

```sql
SELECT afdnr, afdNaam , count(FNaam)
FROM afdeling left outer join werknemer ON afd = afdnr
GROUP BY afdnr, afdNaam
ORDER BY afdnr;
```
<br>

### <u>CROSS JOIN</u>
```sql
SELECT [Kollomen]
FROM [Tabel] CROSS JOIN [tabel2] ON [kol1] = [kol2]
```
Een CROSS JOIN is vergelijkbaar met het cartesisch product. Het aantal rijen in de resultaattabel is gelijk aan het aantal rijen in de eerste tabel maal het aantal rijen in de tweede tabel.\
Deze join wordt in de praktijk zelden gebruikt, maar kan handig zijn on testdata te genereren

<br>

### <u>UNION</u>
Union combineer 2 of meer tabellen tot 1.

```sql
 SELECT [kol1], [kol2]
 FROM [Tabel1]
UNION
 SELECT [kol1], [kol2]
 FROM [Tabel2]
 ```

 <br>

## HO9
## Wijzigen van de inhoud van tabellen
### <u>INSERT</u>
### 1 Rij

```sql
INSERT INTO [tabel]
VALUES('nieuwe waarde', 'nieuwe waarde', 'nieuwe waarde');
```
* Ofwel vermeld je in de lijst van kolomnamen, enkel de kolomnamen, die eenwaarde zullen krijgen. De niet vermelde kolomnamen krijgen dan automatischNULL-waarden toegewezen.
* Je vermeldt geen lijst van kolomnamen en gebruikt het NULL-sleutelwoord voorde onbekende velden

NR | Vnaam | Init | Fnaam | Afd| Tel | Indienst| Gesl | Salaris
--- | --- | ---   | ---   |--- |---  |---      |---   |---
(1) | (2) | (3)   |  (4)  | (5)| (6) | (7)     | (8)  | (9)

<br>

### Meerdere rijen
```sql
CREATE TABLE [New Name]
    [kol1] CHAR NOT NULL,
    VNaam VARCHAR(12) NOT NULL,
    (2)Init CHAR(1) NOT NULL,
    (3)FNaam VARCHAR(15) NOT NULL,
    (4)Afd CHAR(3) NOT NULL,
    (5)Tel CHAR(4),
    InDienst INT,
    Gesl CHAR(1),
    Salaris DECIMAL(8,2);
```
Vul new table nu op met data van 2 bedienden

```sql
INSERT INTO AFDD41(Nr, VNaam, Init, FNaam, Afd, Indienst, Gesl)
SELECT Nr, VNaam, Init, FNaam, Afd, Indienst, Gesl
FROM Werknemer
WHERE NR LIKE ’4’OR Nr IN (SELECT ManNr FROM Afdeling WHERE AfdNr = ’D41’);
```

<br>


<br>


### <u>UPDATE</u>
```sql
UPDATE [Table]
SET [kol]= 'value', [Kol2]= 'value'
WHERE [kol3]='value'
```
Indien er geen WHERE vermeld wordt, zullen alle rijen in de opgegeven tabel gewijzigd worden!!!

### <u>DELETE</u>
```sql
DELETE
FROM [Table]
WHERE [kol] ='value'
```
DELETE met Subselect
```sql
DELETE
FROM [Table]
WHERE [Kol] IN (SELECT [Kol] FROM [Table])
```

<br><br>

## HO10
## Aanmaken, wijzigen en verwijderen van tabellen


### <u>Creatie van een Table  </u>  
```sql
CREATE TABLE AFDD41(
    NR CHAR(3) NOT NULL,
    VNaam VARCHAR(12) NOT NULL,
    Init CHAR(1) NOT NULL,
    FNaam VARCHAR(15) NOT NULL,
    Afd CHAR(3) NOT NULL,
    Tel CHAR(4) NULL,
    InDienst INT NULL,
    Gesl CHAR(1) NULL,
    Salaris DECIMAL(8,2) NULL;
```  
### <u>Wijzigen van de tabelStructuur  </u>   

```sql
ALTER TABLE tabelnaam
ADD (Kolomnaam type[, kolomnaam type])
```
### <u>DROP : Verwijderen van een volledige tabel  </u> 
```sql
DROP TABLE [Tabelnaam];
```



