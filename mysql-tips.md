# MySQL tips & tricks

## Set db to read-only mode

```sql
flush tables with read lock;
```

## Transfer data from one db to another

```bash
# create database
mysqladmin create mydestinationdb
# transfer data from mysourcedb into mydestinationdb
mysqldump -h my-source-host -umyuser -pmypassword mysourcedb --compress --verbose | mysql mydestinationdb
# transfer mytable data from mysourcedb into mydestinationdb
mysqldump -h my-source-host -umyuser -pmypassword mysourcedb --tables mytable --compress --verbose | mysql mydestinationdb
```

## Find out mysql databases size

```mysql
SELECT table_schema "DB Name", 
Round(Sum(data_length + index_length) / 1024 / 1024, 1) "DB Size in MB" 
FROM information_schema.tables 
GROUP BY table_schema;
```

or specific database

```mysql
SELECT table_schema "DB Name", 
Round(Sum(data_length + index_length) / 1024 / 1024, 1) "DB Size in MB" 
FROM information_schema.tables
WHERE table_schema='schema name'
GROUP BY table_schema;
```

Sample output:
```
+--------------------+---------------+
| DB Name            | DB Size in MB |
+--------------------+---------------+
| gbuildserver       |         264.3 | 
| information_schema |           0.0 | 
| mysql              |           0.5 | 
+--------------------+---------------+
3 rows in set (0.35 sec)
```
