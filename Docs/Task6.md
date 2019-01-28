# Zadanie 6

W ramach tego zadania należy stworzyć własną bibliotkę (rozszerzenie U-SQL). W ramach biblioteki należy stworzyć funkcję która zwraca listę zainstalowanych bibliotek na vertex'ie 

```c#
 public static string GetInstalledApps()
        {
            var sb = new StringBuilder();
            ManagementObjectSearcher mos = new ManagementObjectSearcher("SELECT * FROM Win32_Product");
            foreach (ManagementObject mo in mos.Get())
            {
                sb.AppendLine(mo["Name"].ToString());
            }
            return sb.ToString();
        }
```

Następnie należy stworzyć skrypt U-SQL, który zostanie wykonany na Azure (ADLU=1). Wynikiem wykonania powinna być lista zainstalowanych aplikacji.