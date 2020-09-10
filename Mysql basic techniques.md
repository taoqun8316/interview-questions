<details>
 <summary><b>select * from table where (ID = 10) or (ID = 32) or (ID = 22) 让结果按10, 32, 22的顺序检索出来？</b></summary>

```
Select *
from user_info
Where (ID IN (10, 32, 22))

order BY FIND_IN_SET(ID, '10, 32, 22')
 ```

</details>