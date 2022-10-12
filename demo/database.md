# 数据库操作

```go 
package main

import (
	"database/sql"
	"errors"
	"fmt"
	_ "github.com/mattn/go-sqlite3"
	"time"
)

type UserInfo struct {
	uid        int64
	userName   string
	departName string
	created    string
}

// 创建数据库
func migrateTable(db *sql.DB) error {
	UserInfo := `
	create table  if not exists 'userinfo' (
	'uid' integer primary key autoincrement,
	'username' varchar(64) null,
	'departname' varchar(64) null,
	'created' date null
	);
	`
	_, err := db.Exec(UserInfo)
	if err != nil {
		return err
	}

	return nil
}

//插入数据
func insertUserInfo(db *sql.DB, user *UserInfo) (int64, error) {
	stmt, err := db.Prepare("INSERT INTO userinfo(username, departname, created) values(?,?,?)")
	if err != nil {
		return 0, err
	}
	res, err := stmt.Exec(user.userName, user.departName, "2012-12-09")
	if err != nil {
		return 0, err
	}
	id, err := res.LastInsertId()
	if err != nil {
		return 0, err
	}
	return id, nil
}

func queryUserInfo(db *sql.DB) ([]UserInfo, error) {
	rows, err := db.Query("SELECT * FROM userinfo")
	if err != nil {
		return nil, err
	}

	out := make([]UserInfo, 0, 10)
	for rows.Next() {
		var user UserInfo
		err = rows.Scan(&user.uid, &user.userName, &user.departName, &user.created)
		if err == nil {
			out = append(out, user)
		}
	}
	return out, nil
}

func updateUsrInfo(db *sql.DB, user *UserInfo) error {
	//更新数据
	stmt, err := db.Prepare("update userinfo set username=? where uid=?")
	if err != nil {
		return err
	}

	res, err := stmt.Exec(user.userName, user.uid)
	if err != nil {
		return err
	}

	affect, err := res.RowsAffected()
	if err != nil {
		return err
	}

	if affect == 0 {
		return errors.New("no row update")
	}
	return nil
}

func deleteUserInfo(db *sql.DB, uid int64) error {
	//删除数据
	stmt, err := db.Prepare("delete from userinfo where uid=?")

	_, err = stmt.Exec(uid)
	if err != nil {
		return err
	}

	return nil
}

func printUserInfo(users []UserInfo) {
	fmt.Printf("id       姓名     部门     时间\n")
	for _, user := range users {
		fmt.Printf("%-6d%-15s%-12s%-12s\n", user.uid, user.userName, user.departName, user.created)
	}
}

func main() {
	db, err := sql.Open("sqlite3", "./foo.db")
	defer db.Close()

	err = migrateTable(db)
	if err != nil {
		fmt.Println("migrate error:", err)
		return
	}

	users := []UserInfo{
		{
			uid:        0,
			userName:   "zhangsan",
			departName: "it",
			created:    time.Now().Format(time.RFC3339),
		},
		{
			uid:        0,
			userName:   "lisi",
			departName: "it",
			created:    time.Now().Format(time.RFC3339),
		},
	}
	for _, user := range users {
		if _, err := insertUserInfo(db, &user); err != nil {
			fmt.Println("insert error:", err)
			continue
		}
	}

	queryRes, err := queryUserInfo(db)
	if err != nil {
		fmt.Println("query error:", err)
	} else {
		printUserInfo(queryRes)
	}

	if len(queryRes) > 0 {
		upUser := queryRes[0]
		upUser.userName = "update test"
		if err := updateUsrInfo(db, &upUser); err != nil {
			fmt.Println("update error:", err)
		}
		if err := deleteUserInfo(db, upUser.uid); err != nil {
			fmt.Println("delete error:", err)
		}
	}
}
```
[orm框架](https://gorm.io/zh_CN/docs/)