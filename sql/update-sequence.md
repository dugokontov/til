## Update Sequence in PSQL

### How?
To see current last sequence value run:

```sql
SELECT last_value FROM sequence_name;
```

To update to new 

```sql
SELECT setval('sequence_name', 20, true);  # next value will be 21
```

### Reference
 * How to retrieve last value: https://dba.stackexchange.com/a/78228
 * How to set new value: https://stackoverflow.com/a/8745101/521373
