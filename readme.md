
sqlite base http

Fork https://github.com/proofrock/ws4sqlite

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

# gorm sql
```
import (
  "gorm.io/gorm"
  "gorm.io/driver/sqlite"
)

type Product struct {
  gorm.Model
  Code  string
  Price uint
}

func main() {
  db, err := gorm.Open(sqlite.Open("test.db"), &gorm.Config{})
  if err != nil {
    panic("failed to connect database")
  }

  // 迁移 schema
  db.AutoMigrate(&Product{})

  // Create
  db.Create(&Product{Code: "D42", Price: 100})

  // Read
  var product Product
  db.First(&product, 1) // 根据整型主键查找
  db.First(&product, "code = ?", "D42") // 查找 code 字段值为 D42 的记录

  // Update - 将 product 的 price 更新为 200
  db.Model(&product).Update("Price", 200)
  // Update - 更新多个字段
  db.Model(&product).Updates(Product{Price: 200, Code: "F42"}) // 仅更新非零值字段
  db.Model(&product).Updates(map[string]interface{}{"Price": 200, "Code": "F42"})

  // Delete - 删除 product
  db.Delete(&product, 1)
}
```


> 47.44.0.127:8007/xieyuhua

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