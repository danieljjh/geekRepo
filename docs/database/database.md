---
title: database
type: repo
keywords: database, mysql
---

# database

## mysql

### about mysql

### installation

    在 Ubuntu 中安装 MySQL 可以通过以下步骤进行：

    1. 更新软件包列表：
    sudo apt-get update

    2. 安装 MySQL：
    sudo apt-get install mysql-server

    在安装过程中，会提示你输入 MySQL 的 root 用户的密码。

    3. 启动 MySQL 服务：
    sudo systemctl start mysql

    检查 MySQL 服务是否已经启动：
    sudo systemctl status mysql
    如果 MySQL 服务已经启动，终端输出的信息中应该包含 "Active: active (running)"。

    4. 设置 MySQL root 用户的密码：
    sudo mysql_secure_installation
    程序会提示你输入之前设置的 root 密码，然后会让你进行一些安全设置，比如是否删除匿名用户等。
    如果你需要在 Ubuntu 上安装 MySQL 客户端，可以使用以下命令：

    sudo apt-get install mysql-client

    安装完成后，你可以通过以下命令来连接到 MySQL 服务器：

    mysql -h 主机名 -u 用户名 -p

    其中，主机名可以是本地主机（localhost），用户名是你在 MySQL 中创建的用户名，密码是你在安装时设置的密码。

### mysql commands

    1. 连接 mysql 数据库：
    mysql -h 主机名 -u 用户名 -p
    例如：mysql -u root -p
    mysql -u UserName -pPassword -P port(3306)

    2. 显示所有数据库：
    show databases;

    3. 选择数据库：
    use 数据库名;
    例如：use test;

    4. 显示数据库中所有表：
    show tables;

    5. 创建数据库：
    create database 数据库名;
    CREATE DATABASE mydb CHARACTER SET utf8mb4 COLLATE utf8mb4;
    其中 CHARACTER 指定字符集， COLLATE 指定排序规则， utf8mb4 是常用的字符集
    例如：create database test_db;

    6. 删除数据库：
    drop database 数据库名;
    例如：drop database test_db;

    7. 创建表：
    create table 表名 (字段名 数据类型, 字段名 数据类型, ...);
    例如：create table student (id int, name varchar(20), age int);

    8. 删除表：
    drop table 表名;
    例如：drop table student;

    9. 插入记录：
    insert into 表名 (字段 1, 字段 2, ...) values (值 1, 值 2, ...);
    例如：insert into student (id, name, age) values (1, 'Tom', 18);

    10. 更新记录：
        update 表名 set 字段 1=值 1, 字段 2=值 2, ... where 条件;
        例如：update student set age=20 where name='Tom';

    11. 删除记录：
        delete from 表名 where 条件;
        例如：delete from student where id=1;

    12. 查询记录：
        select _ from 表名;
        例如：select _ from student;

    13. 查询特定条件的记录：
        select _ from 表名 where 条件;
        例如：select _ from student where age>18;

    14. 排序：
        select _ from 表名 order by 字段 asc/desc;
        例如： select _ from student order by age desc;

    15. 分组：
        select 字段 1, 字段 2, ... from 表名 group by 字段;
        例如：select name, count(\*) from student group by name;

    16. 连接查询：
        select _ from 表 1 join 表 2 on 条件;
        例如：select _ from student join score on student.id=score.student_id

### use mysql with python

    你可以使用 Python 的 MySQL Connector 模块来连接 MySQL 数据库并进行数据操作。下面是一个简单的示例：

    1. 安装 MySQL Connector：

    pip install mysql-connector-python

    2. 连接 MySQL 数据库：

    import mysql.connector

    cnx = mysql.connector.connect(user='username', password='password',
    host='127.0.0.1',
    database='database_name')

    3. 创建一个游标对象：

    cursor = cnx.cursor()

    4. 执行 SQL 查询：

    query = ("SELECT \* FROM table_name")
    cursor.execute(query)

    5. 获取查询结果：

    for (column1, column2, ...) in cursor: # 处理查询结果

    6. 插入数据：

    add_data = ("INSERT INTO table_name "
    "(column1, column2, ...) "
    "VALUES (%s, %s, ...)")
    data = (value1, value2, ...)
    cursor.execute(add_data, data)
    cnx.commit()

    7. 更新数据：

    update_data = ("UPDATE table_name SET column1=%s WHERE column2=%s")
    data = (new_value, condition_value)
    cursor.execute(update_data, data)
    cnx.commit()

    8. 删除数据：

    delete_data = ("DELETE FROM table_name WHERE column=%s")
    data = (value,)
    cursor.execute(delete_data, data)
    cnx.commit()

    9. 关闭游标和数据库连接：

    cursor.close()
    cnx.close()

    注意：在进行数据操作时，需要注意 SQL 注入攻击，可以使用参数化查询来避免此类攻击。例如：

    query = ("SELECT * FROM table_name WHERE column=%s")
    data = (value,)
    cursor.execute(query, data)

    这里的 %s 是一种占位符，用来表示查询条件。在执行查询时，将会使用 data 中的值来替换占位符。这样可以避免用户输入的数据被当作代码执行，从而避免 SQL 注入攻击。
