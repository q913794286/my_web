---
---
关于Go语言的一系列库、框架分享，涉及机器学习、数据库等多方面
------------------------------------------------------------

作为一款网红编程语言，Go语言还十分年轻，很多程序员无法及时了解到Go语言的框架、库和软件应用。近日，Github用户avelino分享了一张非常完整且庞大的表单，包括命令行、数据库、Web框架、机器学习、自然语言处理......以下是部分内容截取，感谢avelino的分享。

#### 标准CLI

用于构建标准或基本命令行应用程序的库。

argv - 使用bash语法将库命令行字符串拆分为参数数组。

cli - 基于golang的功能丰富且易于使用的命令行程序包。

cli-init - 开始构建Golang命令行应用程序的简单方法。

climax - 具有“human face”的替代CLI。

cobra - CLI交互指挥官。

complete - 在Go + Go命令bash完成中写入bash完成。

docopt.go - 命令行参数解析器。

drive - Google Drive客户端命令行。

env - 基于标签的结构环境配置。·

flag - 简单而强大的命令行选项解析库支持Go子命令。

go-arg - 在Go中基于结构的参数解析。

go-flags - go命令行选项解析器。

kingpin - 支持子命令的命令行和标志解析器。

liner - 用于命令行接口的类似于readline的库。

mitchellh/ cli - 用于实现命令行界面的库。

mow.cli - 用于构建具有复杂标志和参数解析验证的CLI应用程序库。

pflag - 替换Go的flag包，实现POSIX/GNU-style --flags。

readline - 纯Golang实现，在MIT许可下提供GNU-Readline中的大部分功能。

sflags - 基于结构的标志生成器，用于flag, urfave/cli, pflag, cobra,
kingpin和其他库。

ukautz/ clif - 小型命令行界面框架。

urfave/ cli - 在Go（以前的codegangsta /
cli）中构建命令行应用程序的简单，快速和有趣的包。

wlog - 支持跨平台颜色和并发性的简单日志记录界面。

wmenu - 易于使用的菜单结构，用于提示用户进行选择的cli应用程序。

#### 高级控制台UI

用于构建控制台应用程序和控制台用户界面的库。

aurora - 支持fmt.Printf / Sprintf的ANSI终端颜色。

chalk - 直观的包装，用于优化终端/控制台输出。

color - 用于彩色终端输出的多功能包装。

colourize - 终端中ANSI文本颜色的Go库。

go-ataman - Go库，用于在终端中呈现ANSI彩色文本模板。

go-colorable - Windows的可着色画笔。

go-colortext - 用于在终端中输出颜色的库。

gocui - Minimalist —Go库旨在创建控制台用户界面。

gommon / color - Style终端文本。

mpb - 终端应用程序的多进度条。

termbox-go - Termbox是一个用于创建跨平台的、基于文本的界面的库。

termtables - 将Ruby库终端表的端口用于简单的ASCII表生成以及提供HTML输出。

termui - 终端仪表板，基于termbox-go，并受到blessed-contrib的启发。

uilive - 用于实时更新终端输出的库。

uiprogress - 灵活的库用于在终端应用程序中呈现进度条。

uitable - 使用表格数据提高终端应用程序的可读性。

数据结构

Go中的通用数据结构和算法。

binpacker - 二进制打包程序和解包程序可帮助用户构建自定义二进制流。

bit - Golang设置数据结构，带有加密的bit-twiddling功能。

bitset - Go包执行位组。

bloom - 在Go中实现的Bloom过滤器。

bloom - Golang Bloom过滤器实现。

boomfilters - 用于处理连续，无界流的概率数据结构。

concurrent-writer - bufio.Writer的高度并发插件替换。

count-min-log - 执行计数最小日志草图：使用近似计数器近似计数。

encoding - 整数压缩库。

go-adaptive-radix-tree - 执行自适应基数树。

go-datastructures - 收集有用的，执行的和线程安全的数据结构。

go-ef - 执行Elias-Fano编码。

go-geoindex - 内存geo索引。

go-rquad - 具有高效点位置和邻居查找的区域四叉树。

gods-数据结构。容器，集合，列表，堆栈，地图，BidiMaps，树，HashSet等

gangang-set - 线程安全和非线程安全的高性能Go集合。

goset - Go的一个有用的集合实现。

goskiplist - Go中的 Skip list实现。

goota - 数据框架和数据争用方法实现。

hilbert - Go包，用于将值映射到空格填充曲线（如Hilbert和Peano曲线）。

hyperloglog - HyperLogLog实现与稀疏，LogLog-Beta偏差校正和TailCut空间缩减。

levenshtein - Levenshtein距离和相似性度量。

levenshtein - 在Go中计算levenshtein距离的实现。

mafsa - MA-FSA实现与最小完美哈希。

merkletree - 实现一个merkle树，提供数据结构内容的高效安全验证。

ttlcache - 内存中的LRU string-interface {}映射

willf/ bloom - 执行Bloom过滤器的包。

#### 数据库

Go中实现的数据库。

badger - 快捷键值对存储。

BigCache - 高效的键/值缓存，用于千兆字节数据。

bolt - Go的低级键/值数据库。

buntdb - 具有自定义索引和空间支持的快速可嵌入内存中的键/值数据库。

cache2go - 内存中的Key:value缓存，支持基于超时的自动无效。

cockroach - 可扩展，地理复制，事务性数据存储。

couchcache - 由Couchbase服务器支持的RESTful缓存微服务。

dgraph - 可扩展，分布式，低延迟，高吞吐量图形数据库。

diskv - 支持键值存储。

eliasdb - 具有REST API，短语搜索和类似SQL的查询语言的无依赖关系的事务图数据库。

forestdb - ForestDB绑定。

GCache - 缓存库，支持可预见的Cache，LFU，LRU和ARC。

geocache - 内存缓存，适用于基于位置的应用程序。

go-cache - 内存key:value存储/缓存（类似于Memcached）库，适用于单机应用程序。

goleveldb - 在Go中实现LevelDB键/值数据库。

groupcache - Groupcache是一个缓存和缓存填充库，用于在许多情况下替代memcached。

influxdb - 可扩展的数据存储区，用于度量，事件和实时分析。

ledisdb - Ledisdb是一个基于LevelDB的高性能NoSQL，如Redis。

levigo - Levigo是LevelDB的Go包装器。

Moss - Moss是一个简单的LSM键值存储引擎，用100％的Go语言编写。

piladb - 基于堆栈数据结构的轻量级RESTful数据库引擎。

prometheus - 监控系统和时间序列数据库。

rqlite - 构建在SQLite上的轻量级，分布式，关系型数据库。

Scribble - 微小的平面文件JSON存储。

tempdb - 临时项目的键值存储。

tidb - TiDB是一个分布式SQL数据库。灵感来自于Google F1的设计。

tiedot - 由Golang提供支持的NoSQL数据库。

Tile38 - 具有空间索引和实时地理位置的地理数据库。

数据库模式迁移

darwin - Go的数据库模式演化库。

go-fixtures - 用于Golang内置数据库/ sql库的Django样式装置。

goose - 数据库迁移工具，可以通过创建增量SQL或Go脚本来管理数据库的演进。

gormigrate - Gorm ORM的数据库模式迁移帮助器。

migrate - 数据库迁移。CLI和Golang库。

pravasan - 简单的迁移工具 -
目前用于MySQL，但计划即将支持Postgres，SQLite，MongoDB等。

soda - MySQL，PostgreSQL和SQLite的数据库迁移，创建，ORM等。

sql-migrate - 数据库迁移工具。允许使用go-bindata将迁移嵌入到应用程序中。

数据库工具

go-mysql - Go工具集来处理MySQL协议和复制。

go-mysql-elasticsearch - 自动将MySQL数据同步到弹性搜索。

kingshard - kingshard是由Golang提供的MySQL高性能代理。

myreplication - MySql二进制日志复制侦听器，支持语句和基于行的复制。

orchestrator - MySQL复制拓扑管理器和可视化器。

pgweb - 基于Web的PostgreSQL数据库浏览器。

pREST - 从任何PostgreSQL数据库提供RESTful API。

vitess - 提供服务器和工具，便于MySQL数据库扩展大型Web服务。

SQL查询构建器，用于构建和使用SQL库

dat - Postgres数据访问工具包。

Dotsql - 将sql文件保存在一个地方并轻松使用。

goqu - 惯用SQL构建器和查询库。

igor - PostgreSQL抽象层，支持高级功能，并使用类似gorm的语法。

ozzo-dbx - 强大的数据检索方法以及与数据库无关的查询构建能力。

scaneo - 生成Go代码将数据库行转换为任意结构。

sqrl - SQL查询生成器，性能提升。

Squirrel - 构建SQL查询的库。

xo - 根据现有架构定义或支持PostgreSQL，MySQL，SQLite，Oracle和Microsoft SQL
Server的自定义查询，为数据库生成惯用Go代码。

#### 机器学习

###### 机器学习库

bayesian - Go语言的朴素贝叶斯分类。

CloudForest - 以纯Go为机器学习的快速，灵活，多线程的决策树组合。

gago - 灵活并行的遗传算法。

go-fann - 快速人工神经网络（FANN）库的绑定。

gogo galib - Go / golang编写的遗传算法库。

go-pr - Golang中的图像识别包。

gobrain - Go语言写的神经网络。

godist - 各种概率分布和相关方法。

goga - Go的遗传算法库。

GoLearn - Go的通用机器学习库。

golinear - Go的liblinear绑定。

goml - 在线机器学习。

goRecommend - 使用Go编写的推荐算法库。

gorgonia - 基于图形的计算库，如Theano for
Go，为构建各种机器学习和神经网络算法提供原始数据。

goscore - 获取PMML的API。

libsvm - libsvm golang版本派生工作基于LIBSVM 3.14。

mlgo - 该项目旨在提供Go中的简约机器学习算法。

neat -NeuroEvolution增强拓扑（NEAT）的即插即用并行Go框架。

neural-go —Go中实施的多层感知网络，通过反向传播进行培训。

probab - 概率分布函数。

regommend - 推荐和协同过滤引擎。

shield - 贝叶斯文本分类器，具有灵活的标记器和Go存储后端。

#### 自然语言处理

dpar - 基于过渡的统计依赖解析器。

go-eco - 相似性，不相似性和距离矩阵;多样性，公平和不平等的措施;物种丰富度估计;
coenocline模型。

go-i18n - 使用本地化文本的软件包和随附工具。

go-mystem - CGo绑定到Yandex.Mystem - 俄语形态分析器。

go-nlp - 使用离散概率分布和其他可用于执行NLP工具的实用程序。

go-stem - porter stemming算法实现。

go-unidecode - Unicode文本的ASCII音译。

go2vec - word2vec嵌入式阅读器和效用函数。

gojieba - 这是一个Go执行的jieba中文分词算法。

gounidecode - 用于Go的Unicode音译（也称为unidecode）。

icu - Cgo绑定icu4c C库检测和转换功能。保证与版本50.1兼容。

libtextcat - 用于libtextcat C库的Cgo绑定。保证与版本2.2的兼容性。

MMSEGO - 这是一个中文分词算法MMSEG的GO实现。

nlp - 从字符串中提取值并使用nlp填充结构体。

nlp - 自然语言处理库支持LSA（潜在语义分析）。

paicehusk - Golang实施Paice / Husk Stemming算法。

porter - 这是一个非常简单的 Martin Porter实现Porter干扰算法的端口。

prose - 支持标记化，词性标注，命名实体提取等的文本处理库。

RAKE.go - 快速自动关键词提取算法（RAKE）的端口。

stemmer - 用于Go的Stemmer包。包括英语和德语词干。

textcat - 基于n-gram的文本分类Go包，支持utf-8和原始文本。

whatlanggo -
Go的自然语言检测包。支持84种语言和24种脚本（写作系统，如拉丁语，西里尔字体等）。

when - 具有可插拔规则的自然EN和RU语言日期/时间解析器。

如果你觉得这些还不过瘾，可以去Github页面（项目源地址：
<https://github.com/avelino/awesome-go\#web-frameworks>）与众多Go语言程序员互动。