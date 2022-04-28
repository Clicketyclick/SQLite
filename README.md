# SQLite
Tips and tricks on SQLite

- [Extract nested JSON from SQLite](SQLite2JSON_hash/) (Based on [Stackoverflow](https://stackoverflow.com/a/61004015/7485823) )
- [UUID as default value for id](DefaultUUID/) (Based on [Stackoverflow: SQLite - Generate GUID/UUID on SELECT INTO statement](https://stackoverflow.com/a/66625212) )

## TIMESTAMP vs. ISO8601 dates

SQLite has an internal timestamp with the syntax `YYYY-MM-DD hh:mm:ss`. This is NOT a valid timestamp as defined in ISO8601 where the syntax is `YYYY-MM-DD`**T**`hh:mm:ssZ` - with the letter `T` as delimiter (between date an time) and a timezone  (Zulu for UTC).

```sql
DROP TABLE times;
CREATE TABLE times (
  sqlite    TIMESTAMP  DEFAULT CURRENT_TIMESTAMP,
  iso8601   TEXT       DEFAULT (strftime('%Y-%m-%dT%H:%M:%SZ')),
  sometext  TEXT
 );
```

Testing this using:

```sql
REPLACE INTO times(sometext) values("hello world");
SELECT * FROM times;
```
Should produce:
```console
2022-04-27 18:49:07|2022-04-27T18:49:07Z|hello world
```
Please note that both timestamps are in UTC timezone (Local time is 20:52)
