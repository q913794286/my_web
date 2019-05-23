---
Title: golang操作postgreSQL(增删改查实例)
---
### 一、PostgreSQL介绍

* PostgreSQL is a powerful, open source object-relational database system.
PostgreSQL是一个功能强大的开源对象关系数据库管理系统(ORDBMS)。用于安全地存储数据;支持最佳做法，并允许在处理请求时检索它们。

特点

* PostgreSQL可在所有主要操作系统(即Linux，UNIX(AIX，BSD，HP-UX，SGI IRIX，Mac OS X，Solaris，Tru64)和Windows等)上运行
* PostgreSQL支持文本，图像，声音和视频，并包括用于C/C++，Java，Perl，Python，Ruby，Tcl和开放数据库连接(ODBC)的编程接口
* PostgreSQL支持SQL的许多功能，例如复杂SQL查询，SQL子选择，外键，触发器，视图，事务，多进程并发控制(MVCC)，流式复制(9.0)，热备(9.0))
* 在PostgreSQL中，表可以设置为从“父”表继承其特征
* 可以安装多个扩展以向PostgreSQL添加附加功能
 

 

### 二、安装PostgreSQL

下载地址：[https://www.postgresql.org/download/](https://www.postgresql.org/download/)

安装客户端程序pgAdmin或pgweb(golang写的)等

 

 

### 三、常用的golang驱动

*  https://github.com/bmizerany/pq支持database/sql驱动，纯Go写的

*  https://github.com/jbarham/gopgsqldriver支持database/sql驱动，纯Go写的

*  https://github.com/lxn/go-pgsql支持database/sql驱动，纯Go写的

采用了第一个驱动，因为它目前使用的人最多，在github上也比较活跃，包括一些框架支持（xorm）

 

### 四、示例

注意：需要导入postgreSQL驱动

```
package main
 
import (
	"database/sql"
	_ "github.com/lib/pq"
	"fmt"
	"log"
)
 
 
 
const (
	host = "localhost"
	port = 5432
	user = "postgres"
	password = "your_password"
	dbname="your_database_name"
)
 
func main() {
	db := getDB()
	if db != nil {
		UpdateUser(db)
	}
}
 
 
 
func getDB() *sql.DB{
	psqlInfo := fmt.Sprintf("host=%s port=%d user=%s password=%s dbname=%s sslmode=disable",host,port,user,password,dbname)
	db,err := sql.Open("postgres",psqlInfo)
	if err != nil {
		log.Fatal(err)
		return nil
	}
	err = db.Ping()
	if err != nil {
		log.Fatal(err)
		return nil
	}
	fmt.Println("successfull connected!")
	return db
}
 
type User struct {
	ID    					int
	Sex						int
	UserName		string
	Info 					string
}
 
//查询
func  SelectUser(db *sql.DB)  {
	rows,err := db.Query("select * from user_tbl")
	if err != nil {
		log.Fatal(err)
	}
	user := User{}
	for rows.Next() {
		err := rows.Scan(&user.UserName,&user.Sex,&user.ID,&user.Info)
		if err != nil {
			log.Fatal(err)
		}
	}
	fmt.Println(user)
 
}
 
//添加用户
func InsertUser(db *sql.DB)  {
	stmt,err := db.Prepare("insert into user_tbl(id,username,sex,info) values($1,$2,$3,$4)")
	if err != nil {
		log.Fatal(err)
	}
	_,err = stmt.Exec(6,"Windows",1,"操作系统")
	if err != nil {
		log.Fatal(err)
	}else {
		fmt.Println("insert into user_tbl success")
	}
}
 
func InsertUser2(db *sql.DB) {
	 db.QueryRow("INSERT INTO user_tbl(id,username,sex,info) VALUES ($1,$2,$3,$4)",7,"Linux",0,"dd")
}
 
//删除用户
func DeleteUser(db *sql.DB) {
	stmt,err := db.Prepare("DELETE  FROM user_tbl WHERE  id=$1")
	if err != nil {
		log.Fatal(err)
	}
 
	_,err = stmt.Exec(7)
	if err != nil {
		log.Fatal(err)
	}else {
		fmt.Println("delete form user_tbl success")
	}
}
 
//更新用户
func UpdateUser(db *sql.DB) {
	stmt,err := db.Prepare("UPDATE  user_tbl  set username=$1 WHERE  id=$2")
	if err != nil {
		log.Fatal(err)
	}
	_,err = stmt.Exec("Linux",6)
	if err != nil {
		log.Fatal(err)
	}else {
		fmt.Println("udpate user_tbl success")
	}
 
}

```
### 五：缺点

只用驱动操作硬编码多，而且操作起来非常繁琐。

建议使用框架来操作,可以参考以下链接

[http://blog.csdn.net/skh2015java/article/details/78814595](http://blog.csdn.net/skh2015java/article/details/78814595)