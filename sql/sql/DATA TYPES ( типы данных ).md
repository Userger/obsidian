
### Numbers


|                  | name                          | bytes    | range                                                           |
| :--------------- | ----------------------------- | -------- | --------------------------------------------------------------- |
| Integral Numbers | smallint                      | 2        | 2^16 -> -32.768 to 32.767                                       |
|                  | integer                       | 4        | 2^32 -> -2.147.483.648 to 2.147.483.647                         |
|                  | bigint                        | 8        | 2^64 -> -9.223.372.036.854.775.808 to 9.223.372.036.854.775.807 |
|                  | smallserial                   | 2        | 1 to 32.767                                                     |
|                  | serial                        | 4        | 1 to 2.147.483.647                                              |
|                  | bigserial                     | 8        | 1 to 9.223.372.036.854.775.807                                  |
| Real Numbers     | decimal/numeric               | variable | -3.4 * 10^38 to 3.4 * 10^38                                     |
|                  | real/float4                   | 4        | 6 decimal digits precision                                      |
|                  | double precision/float8/float | 8        | 15 decimal digits precision                                     |

### Strings

|            | name    | bytes    | range | description                                                                          |
| :--------- | ------- | -------- | ----- | ------------------------------------------------------------------------------------ |
| Characters | char    | variable |       | char(10) -> если строка будет меньше 10 символов, то недостаток заполнится пробелами |
|            | varchar | variable |       | varchar(10) -> если меньше 10, то не заполнится пробелами                            |
|            | text    | variable |       |                                                                                      |

### Logical

|         | name         | bytes | range      |
| :------ | ------------ | ----- | ---------- |
| Logical | boolean/bool | 1     | true/false |


### Temporal ( DATES )

|          | name        | bytes | range                        | description             |
| :------- | ----------- | ----- | ---------------------------- | ----------------------- |
| temporal | date        | 4     | 4713 B.C. -> 294.276 AD      | only date               |
|          | time        | 8     | 00:00:00 -> 24:00:00         | only time               |
|          | timestamp   | 8     | 4713 B.C. -> 294.276 AD      | date & time             |
|          | interval    | 16    | -178.000.000 -> +178.000.000 | diff between timestamps |
|          | timestamptz | 8     | 4713 B.C. -> 294.276 AD + tz | timestamps + timezone   |


