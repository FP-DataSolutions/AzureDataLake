# Zadanie 2

W ramach tego zadania należy w Visual Studio stworzyć projekt U-SQL oraz uruchomić lokalnie skrypt U-SQL

```mssql
@sample =
    SELECT *
    FROM(
        VALUES
        (
            1,
            new DateTime(2017, 01, 01, 05, 00, 00),
            new DateTime(2017, 01, 01, 06, 00, 00),
            100.00
        )
        ) AS T (id, begin, end, value);

OUTPUT @sample
TO "sample.csv"
USING Outputters.Csv(outputHeader : true);
```

