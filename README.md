## Welcome to saiorm (/saɪɔ:m/,塞翁)

Saiorm is a simple library for accessing database from the asyncio framework.

It will take you have a easy way to use SQl database. [learn more](http://saiorm.readthedocs.io/).


### Usage for select
1. only param
2. call mysql function with param(param should be str)
3. call mysql function with no param

```python

DB("table").select()

DB("table").order_by("id DESC").get() # get last one line

DB("table").where({
    "a": 1,
    "b": 2,
    "c": ("ABS({})", "2"),
    "d": "now()",
}).select("zz,xx")

DB().select("now()")
```

will transform to

```sql
SELECT * FROM table ;
SELECT * FROM table  ORDER BY id DESC LIMIT 1;
SELECT zz,xx FROM table WHERE a=1 AND b=2 AND c=ABS(2) AND d=now() ;
SELECT now();
```

### Usage for update

```python
DB("table").where({
    "a": 1,
    "b": 2,
    "c": ("ABS({})", "2"),
    "d": "now()",
}).update({
    "x": "1",
    "y": "2",
})
```

will transform to


```sql
UPDATE table SET x=%s,y=%s WHERE a=1 AND b=2 AND c=ABS(2) AND d=now() ;
```

### Usage for insert

insert function support several kinds of data

```python
# use dict 1 natural
DB("table").insert({
    "a": "1",
    "b": "2",
})

# use dict 2
DB("table").insert({
    "fields": ["a", "b"],
    "values": ["1", "2"],

})

# use natural dict in list, SQL statement will in one line
DB("table").insert_many([{
    "a": "1",
    "b": "2",
}, {
    "a": "3",
    "b": "4",
}])

# use natural dict in list, SQL statement will in many line
DB("table").insert_many([{
    "a": "1",
    "b": "2",
}, {
    "a": "3",
    "b": "4",
}], one_line=False)

# use split dict in list, SQL statement will in one line
DB("table").insert_many({
    "fields": ["a", "b"],
    "values": [
        ["1", "2"],
        ["3", "4"],
        ["5", "6"]
    ]
})

# use split dict in list, SQL statement will in many line
DB("table").insert_many({
    "fields": ["a", "b"],
    "values": [
        ["1", "2"],
        ["3", "4"],
        ["5", "6"]
    ]
}, one_line=False)
```

will transform to


```sql
INSERT INTO table (a,b) VALUES (%s,%s);
INSERT INTO table (a,b) VALUES (%s,%s);
INSERT INTO table (a,b) VALUES (%s,%s);
INSERT INTO table (a,b) VALUES (%s,%s); -- repeat
INSERT INTO table (a,b) VALUES (%s,%s,%s);
INSERT INTO table (a,b) VALUES (%s,%s,%s); -- repeat

```

### Usage for delete

By default, delete must have where condition,or you can pass strict=False when initialization.

```python
DB("table").where({
    "a": 1,
    "b": 2,
    "c": ("ABS({})", "2"),
    "d": "now()",
}).delete()

DB("table").delete()  -- will not execute
DB("table", strict=False).delete()
```

will transform to


```sql
DELETE FROM table WHERE a=1 AND b=2 AND c=ABS(2) AND d=now() ;
DELETE FROM table ;
```


### Plan

I will support MySQL first,and then PostgreSQL etc.


