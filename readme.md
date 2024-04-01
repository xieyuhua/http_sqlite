
# 安装
```
[root@VM-16-5-centos http_sqlite]# go mod tidy
[root@VM-16-5-centos http_sqlite]# go build
[root@VM-16-5-centos http_sqlite]# ./sqlite_http -db "xieyuhua.db" -port 8007
sqlite_http v0.0.0, based on sqlite v3.45.1
- Serving database 'xieyuhua' from xieyuhua.db?_pragma=journal_mode(WAL)
  + No valid config file specified, using defaults
  + File not present, it will be created
  + Using WAL
- Web Service listening on 0.0.0.0:8007

```

> http://47.44.0.127:8007/xieyuhua

> Content-Type:  application/json

```
{
    "resultFormat": "map",
    "transaction": [
        {
            "statement": "CREATE TABLE TEST_TABLE  ( id int, val string, val2 string );"
        },
        {
            "statement": "INSERT INTO TEST_TABLE (ID, VAL, VAL2) VALUES (:id, :val, :val2)",
            "values": {
                "id": 3,
                "val": "he24524242llo",
                "val2": null
            }
        },
        {
            "query": "SELECT * FROM TEST_TABLE where id>1 and val like '%524%' "
        }
    ]
}
```