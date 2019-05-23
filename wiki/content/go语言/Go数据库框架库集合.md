在Github中stars数最多的Go数据库框架库集合
=========================================

终于19岁 · 2017-12-19 11:56:29 · 640 次点击 · 预计阅读时间 8
分钟 · 大约2小时之前 开始浏览    

这是一个创建于 2017-12-19
11:56:29 的文章，其中的信息可能已经有所发展或是发生改变。

在Go语言世界中，[beego
orm](https://github.com/astaxie/beego/tree/master/orm)、**gorm**、**sqlx**、**gorp**、**xorm**是我已知在Github中stars数最多Go数据库框架，这几个都是Go语言世界中老牌数据库框架库。

其中[beego
orm](https://github.com/astaxie/beego/tree/master/orm)是**beego**自带的orm框架库，统计star数的时候是按[beego](https://github.com/astaxie/beego)的star数统计的（[beego](https://github.com/astaxie/beego)之前还有一个数据库框架[beedb](https://github.com/astaxie/beedb)，由于谢大在2014年就未在维护此库，所以没有出现在我的统计列表中）。

而[sqlx](https://github.com/jmoiron/sqlx)和**xorm**则是笔者最喜欢和实际开发中最多使用的Go数据库框架库。值的一提的是[sqlx](https://github.com/jmoiron/sqlx)还有3个扩展库，一个是[sqalx](https://github.com/heetch/sqalx)，它使sqlx提供对嵌套事务的支持，另外两个名字都叫sqlt，其中第一个[sqlt](https://github.com/it512/sqlt)库，使sqlx支持sql模板和类mybatis的sql配置；第二个[sqlt](https://github.com/albert-widi/sqlt)库，则使sqlx支持数据库主从数据源，读写分离；

另外一个值的一提的是[xorm](https://github.com/go-xorm/xorm)也有一个定制增强版[xormplus/xorm](https://github.com/xormplus/xorm)，使得xorm支持sql模板和类mybatis的sql配置，支持动态sql，支持嵌套事务，支持类似Java中Spring的事务传播机制，支持数据库的读写分离（master/slave）。同时它和[xorm](https://github.com/go-xorm/xorm)一样内置支持SQL
Builder。

最后还有一个比较有意思的Go数据库框架库是[argen](https://github.com/monochromegane/argen)，它在具体实现中使用了annotation方式，这在Go语言开发库中是比较少见的，对笔者而言是一个值得阅读源码和学习的库，它使Go语言又多了一种思维方式。

在实际开发中笔者经常是依据业务复杂度，数据库sql复杂度，在[sqlx](https://github.com/jmoiron/sqlx)、**xorm**和**xormplus/xorm**中进行数据库框架库选型，笔者大多数时候更倾向[xormplus/xorm](https://github.com/xormplus/xorm)。如果您也有好的Go语言数据库框架推荐，不妨在留言区留言，大家一起交流分享。

下面是我整理的Go语言数据库框架列表，其中有一些源代码很是小巧精悍（虽然star数不多，但我还是罗列出来了），非常适合学习阅读，帮助自己提高对Go语言程序库的设计实现的理解。

| **Project Name**                                              | **Stars** | **Forks** | **Description**                                                                                                                                                                                           |
|---------------------------------------------------------------|-----------|-----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [beego orm](https://github.com/astaxie/beego/tree/master/orm) | 12216     | 2814      | A powerful orm framework for go.（beego自带的orm）                                                                                                                                                        |
| [gorm](https://github.com/jinzhu/gorm)                        | 6548      | 809       | The fantastic ORM library for Golang, aims to be developer friendly                                                                                                                                       |
| [sqlx](https://github.com/jmoiron/sqlx)                       | 3244      | 276       | general purpose extensions to golang's database/sql                                                                                                                                                       |
| [gorp](https://github.com/go-gorp/gorp)                       | 2555      | 306       | Go Relational Persistence - an ORM-ish library for Go                                                                                                                                                     |
| [xorm](https://github.com/go-xorm/xorm)                       | 2273      | 341       | Simple and Powerful ORM for Go, support mysql,postgres,tidb,sqlite3,mssql,oracle [http://xorm.io](http://xorm.io/)                                                                                        |
| [xo](https://github.com/knq/xo)                               | 1255      | 103       | Command line tool to generate idiomatic Go code for SQL databases supporting PostgreSQL, MySQL, SQLite, Oracle, and Microsoft SQL Server                                                                  |
| [pg](https://github.com/go-pg/pg)                             | 1104      | 70        | PostgreSQL ORM for Golang with focus on PostgreSQL features and performance                                                                                                                               |
| [db](https://github.com/upper/db)                             | 975       | 73        | Productive data access layer for Go.                                                                                                                                                                      |
| [dbr](https://github.com/gocraft/dbr)                         | 816       | 87        | Additions to Go's database/sql for super fast performance and convenience.                                                                                                                                |
| [sqlboiler](https://github.com/volatiletech/sqlboiler)        | 755       | 64        | Generate a Go ORM tailored to your database schema.                                                                                                                                                       |
| [hood](https://github.com/eaigner/hood)                       | 659       | 51        | Database agnostic ORM for Go                                                                                                                                                                              |
| [reform](https://github.com/go-reform/reform)                 | 546       | 19        | A better ORM for Go, based on non-empty interfaces and code generation.                                                                                                                                   |
| [godb](https://github.com/samonzeweb/godb)                    | 498       | 15        | A Go query builder and struct mapper.                                                                                                                                                                     |
| [qb](https://github.com/aacanakin/qb)                         | 492       | 21        | The database toolkit for go                                                                                                                                                                               |
| [qbs](https://github.com/coocood/qbs)                         | 460       | 88        | QBS stands for Query By Struct. A Go ORM.                                                                                                                                                                 |
| [dat](https://github.com/mgutz/dat)                           | 459       | 37        | Go Postgres Data Access Toolkit                                                                                                                                                                           |
| [go-kallax](https://github.com/src-d/go-kallax)               | 443       | 26        | Kallax is a PostgreSQL typesafe ORM for the Go language.                                                                                                                                                  |
| [dotsql](https://github.com/gchaincl/dotsql)                  | 308       | 19        | A Golang library for using SQL.                                                                                                                                                                           |
| [xormplus/xorm](https://github.com/xormplus/xorm)             | 278       | 48        | Simple and Powerful ORM for Go, support mysql,postgres,tidb,sqlite3,mssql,oracle（定制增强版）                                                                                                            |
| [jet](https://github.com/eaigner/jet)                         | 199       | 21        | Jet is a super-flexible and lightweight SQL interface for Go                                                                                                                                              |
| [goyesql](https://github.com/nleof/goyesql)                   | 179       | 9         | Parse a file and associate SQL queries to a map. Useful for separating SQL from code logic.                                                                                                               |
| [ozzo-dbx](https://github.com/go-ozzo/ozzo-dbx)               | 175       | 24        | A Go (golang) package that enhances the standard database/sql package by providing powerful data retrieval methods as well as DB-agnostic query building capabilities.                                    |
| [genmai](https://github.com/naoina/genmai)                    | 144       | 17        | Simple, better and easy-to-use ORM library for Golang                                                                                                                                                     |
| [sqlt](https://github.com/it512/sqlt)                         | 143       | 23        | like mybatis see README-zh.md                                                                                                                                                                             |
| [squalor](https://github.com/square/squalor)                  | 132       | 21        | Go SQL utility library                                                                                                                                                                                    |
| [argen](https://github.com/monochromegane/argen)              | 122       | 8         | An ORM code-generation tool for Go, provides ActiveRecord-like functionality for your types.                                                                                                              |
| [sqalx](https://github.com/heetch/sqalx)                      | 76        | 4         | Nested transactions for sqlx                                                                                                                                                                              |
| [goSQL](https://github.com/quintans/goSQL)                    | 64        | 1         | a ORM like library in Go (golang) that makes SQL easier to use.                                                                                                                                           |
| [gomodel](https://github.com/cosiner/gomodel)                 | 55        | 7         | A lightweight, fast, orm-like library helps interactive with database                                                                                                                                     |
| [vivom](https://github.com/oguzbilgic/vivom)                  | 53        | 1         | a powerful Go ORM library                                                                                                                                                                                 |
| [orm](https://github.com/server-nado/orm)                     | 45        | 11        | golang ORM , mysql , sqllite3 , hash redis                                                                                                                                                                |
| [ngorm](https://github.com/ngorm/ngorm)                       | 29        | 2         | Neo GORM: The modern fork of gorm The fantastic ORM( Object Relational Mapper ) for Go                                                                                                                    |
| [gatsby](https://github.com/c9s/gatsby)                       | 22        | 1         | Gatsby Database Toolkit For Go (ORM, SQL Builder and SQLUtils)                                                                                                                                            |
| [go-ormtools](https://github.com/oreillymedia/go-ormtools)    | 20        | 1         | A package with helper functions                                                                                                                                                                           |
| [orange](https://github.com/gernest/orange)                   | 19        | 4         | A lightweight Object Relational Mapper for Go                                                                                                                                                             |
| [goql](https://github.com/rgamba/goql)                        | 17        | 0         | Generate Golang database query code, spirit from mybatis/ibatis.                                                                                                                                          |
| [light](https://github.com/arstd/light)                       | 16        | 2         | Generate Golang database query code, spirit from mybatis/ibatis.                                                                                                                                          |
| [osm](https://github.com/yinshuwei/osm)                       | 15        | 9         | go object sql mapping and template,A simple ORM.simplified mybaits.                                                                                                                                       |
| [sqlt](https://github.com/albert-widi/sqlt)                   | 7         | 10        | Sqlt is a wrapper package for jmoiron/sqlx.This wrapper build based on tsenart/nap master-slave and its load-balancing configuration with some modification                                               |
| [orm](https://github.com/issue9/orm)                          | 4         | 1         | 简单小巧的golang版orm                                                                                                                                                                                     |
| [huge](https://github.com/cxr29/huge)                         | 3         | 1         | Go huge CRUD package and SQL builder                                                                                                                                                                      |
| [gobatis](https://github.com/mitiger/gobatis)                 | 2         | 3         | an orm like ibatis (java) for golang                                                                                                                                                                      |
| [db](https://github.com/webx-top/db)                          | 2         | 0         | The upper.io/db.v3 package for Go is a productive data access layer for Go that provides a common interface to work with different data sources such as PostgreSQL, MySQL, SQLite, MSSQL, QL and MongoDB. |
| [go-bed](https://github.com/issue9/orm)                       | 1         | 1         | a high performance and lack use reflect and assertion golang framework.（带有一个orm框架）                                                                                                                |

以上数据库框架中，很多库还大量使用SQL
Builder来作为数据库框架辅助库使用，下面也介绍一些我已知的SQL Builder开发库。

| **Project Name**                                      | **Stars** | **Forks** | **Description**                                  |
|-------------------------------------------------------|-----------|-----------|--------------------------------------------------|
| [squirrel](https://github.com/Masterminds/squirrel)   | 1199      | 97        | Fluent SQL generation for golang                 |
| [goqu](https://github.com/doug-martin/goqu)           | 336       | 32        | SQL builder and query library for golang         |
| [sqrl](https://github.com/elgris/sqrl)                | 79        | 97        | Fluent SQL generation for golang                 |
| [sqlm](https://github.com/shuoli84/sqlm)              | 66        | 2         | A minimalist sql builder for Golang              |
| [go-xorm/builder](https://github.com/go-xorm/builder) | 25        | 6         | Lightweight and fast SQL builder for Go and XORM |
| [sqlabble](https://github.com/minodisk/sqlabble)      | 2         | 2         | SQL query builder with type support.             |

下面是[Golang sql builder
benchmark](https://github.com/elgris/golang-sql-builder-benchmark)中对部分SQL
Build库的性能测试对比数据

Benchmarks
==========

go test -bench=. -benchmem \| column -t on 2.6 GHz i5 Macbook Pro:

BenchmarkDbrSelectSimple 500000 2610 ns/op 864 B/op 14 allocs/op

BenchmarkDbrSelectConditional 500000 3808 ns/op 1031 B/op 19 allocs/op

BenchmarkDbrSelectComplex 200000 11585 ns/op 3323 B/op 53 allocs/op

BenchmarkDbrSelectSubquery 200000 10025 ns/op 2851 B/op 40 allocs/op

BenchmarkDbrInsert 500000 3717 ns/op 1136 B/op 19 allocs/op

BenchmarkDbrUpdateSetColumns 300000 4106 ns/op 1038 B/op 24 allocs/op

BenchmarkDbrUpdateSetMap 300000 5396 ns/op 1388 B/op 26 allocs/op

BenchmarkDbrDelete 1000000 2150 ns/op 482 B/op 13 allocs/op

BenchmarkGoquSelectSimple 100000 15180 ns/op 3282 B/op 46 allocs/op

BenchmarkGoquSelectConditional 100000 19655 ns/op 4258 B/op 61 allocs/op

BenchmarkGoquSelectComplex 30000 50628 ns/op 11414 B/op 215 allocs/op

BenchmarkSqrlSelectSimple 500000 3555 ns/op 952 B/op 15 allocs/op

BenchmarkSqrlSelectConditional 300000 4377 ns/op 1112 B/op 20 allocs/op

BenchmarkSqrlSelectComplex 100000 24040 ns/op 4751 B/op 100 allocs/op

BenchmarkSqrlSelectSubquery 100000 26203 ns/op 3560 B/op 67 allocs/op

BenchmarkSqrlSelectMoreComplex 30000 47018 ns/op 7256 B/op 150 allocs/op

BenchmarkSqrlInsert 200000 7773 ns/op 1304 B/op 25 allocs/op

BenchmarkSqrlUpdateSetColumns 200000 8633 ns/op 1369 B/op 32 allocs/op

BenchmarkSqrlUpdateSetMap 200000 15786 ns/op 1788 B/op 36 allocs/op

BenchmarkSqrlDelete 500000 3669 ns/op 496 B/op 12 allocs/op

BenchmarkSquirrelSelectSimple 100000 14934 ns/op 2737 B/op 52 allocs/op

BenchmarkSquirrelSelectConditional 100000 18034 ns/op 4023 B/op 84 allocs/op

BenchmarkSquirrelSelectComplex 20000 63096 ns/op 12742 B/op 283 allocs/op

BenchmarkSquirrelSelectSubquery 30000 48956 ns/op 9954 B/op 206 allocs/op

BenchmarkSquirrelSelectMoreComplex 20000 83842 ns/op 17153 B/op 386 allocs/op

BenchmarkSquirrelInsert 100000 14517 ns/op 3356 B/op 75 allocs/op

BenchmarkSquirrelUpdateSetColumns 100000 23995 ns/op 4787 B/op 108 allocs/op

BenchmarkSquirrelUpdateSetMap 50000 27141 ns/op 5203 B/op 112 allocs/op

BenchmarkSquirrelDelete 100000 16728 ns/op 2815 B/op 67 allocs/op
