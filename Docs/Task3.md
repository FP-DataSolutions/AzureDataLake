# Zadanie 3

W ramach tego zadania należy  uruchomić z portalu Azure skrypt U-SQL (**ADLU=1**)

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

