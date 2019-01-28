# Zadanie 4

Rozwiązanie 

W pierwszym kroku tworzymy nową bazę

```mssql
CREATE DATABASE IF NOT EXISTS [ADLUTraining];
```

Następnie tworzymy funkcję TVF na wcześniej stworzonej bazie

```mssql
USE [ADLUTraining];
DROP FUNCTION IF EXISTS dbo.tvf_GetCrimes;
CREATE FUNCTION dbo.tvf_GetCrimes(@City string = "London")
RETURNS (@crimes, @crimesTypes)
AS
BEGIN
//Change Path
    DECLARE @inputCrimes = @"d:\AppData\BIGDATA\UKCrimes\{Date:yyyy}-{Date:MM}\{Input}-{City}-street.csv";
    @crimes =
        EXTRACT CrimeID string,
                Month string,
                ReportedBy string,
                FallsWithin string,
                Longitude string,
                Latitude string,
                Location string,
                LSOACode string,
                LSOAName string,
                CrimeType string,
                LastOutcomeCategory string,
                Context string,
                Date DateTime,
                Input string,
                City string
        FROM @inputCrimes
        USING Extractors.Csv(silent : false, skipFirstNRows : 1);

IF @City !="*" THEN
    @crimes =
        SELECT *
        FROM @crimes
        WHERE City.ToUpper().StartsWith(@City.ToUpper());
    END;

    @crimesTypes =
        SELECT DISTINCT CrimeType
        FROM @crimes;
END;
```

Mając stworzoną funkcję tworzymy skrypt do generowania statystyk

```mssql
USE [ADLUTraining];

DECLARE @baseOutput string = @"d:\Repos\FP-DataSolutions\AzureDataLake\Results\";
(@crimes, @crimesTypes) = dbo.tvf_GetCrimes("*");

@crimesStats =
    SELECT City,
           CrimeType,
           COUNT( * ) AS TotalCrimes
    FROM @crimes
    GROUP BY City,
             CrimeType;

(@crimesLondon, @crimesTypesLondon) = dbo.tvf_GetCrimes("London");


@crimesStatsLondon =
    SELECT City,
           CrimeType,
           COUNT( * ) AS TotalCrimes
    FROM @crimesLondon
    GROUP BY City,
             CrimeType;

OUTPUT @crimesTypes
TO @baseOutput+"crimesTypes.csv"
ORDER BY CrimeType
USING Outputters.Csv(outputHeader:true);

OUTPUT @crimesStats
TO @baseOutput+"CitiesCrimes.csv"
ORDER BY CrimeType,
         City,
         TotalCrimes DESC
USING Outputters.Csv(outputHeader:true);


OUTPUT @crimesStatsLondon
TO @baseOutput+"LondonCrimes.csv"
ORDER BY CrimeType,
         City,
         TotalCrimes DESC
USING Outputters.Csv(outputHeader:true);


//Dead code
OUTPUT @crimesTypesLondon
TO @baseOutput+"LondonCrimesTypes.csv"
ORDER BY CrimeType
USING Outputters.Csv(outputHeader:true);
```

