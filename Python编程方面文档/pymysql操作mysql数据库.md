# pymysql速查手册(自建)



## 什么是 PyMySQL？

PyMySQL 是在 Python3.x 版本中用于连接 MySQL 服务器的一个库，Python2中则使用mysqldb。



## PyMySQL 安装

```shell
pip3 install PyMySQL
```

## 

## 数据库连接

连接数据库前，请先确认以下事项：

- 您已经创建了数据库 TESTDB.
- 在TESTDB数据库中您已经创建了表 EMPLOYEE
- EMPLOYEE表字段为 FIRST_NAME, LAST_NAME, AGE, SEX 和 INCOME。
- 连接数据库TESTDB使用的用户名为 "testuser" ，密码为 "test123",你可以可以自己设定或者直接使用root用户名及其密码，Mysql数据库用户授权请使用Grant命令。



## 导入数据库操作包pymysql

`import pymysql`



## 连接数据库

参数依次是 ip地址, 用户名, 密码, 数据库名

例子: db = pymysql.connect("localhost",'root',"3534365","world" )

官方文档参数说明

```python
class pymysql.connections.Connection(host=None, user=None, password='', database=None, port=0, unix_socket=None, charset='', sql_mode=None, read_default_file=None, conv=None, use_unicode=None, client_flag=0, cursorclass=<class 'pymysql.cursors.Cursor'>, init_command=None, connect_timeout=10, ssl=None, read_default_group=None, compress=None, named_pipe=None, autocommit=False, db=None, passwd=None, local_infile=False, max_allowed_packet=16777216, defer_connect=False, auth_plugin_map=None, read_timeout=None, write_timeout=None, bind_address=None, binary_prefix=False, program_name=None, server_public_key=None)
```

**Parameters** 参数说明

- **host** – Host where the database server is located
- **user** – Username to log in as
- **password** – Password to use.
- **database** – Database to use, None to not use a particular one.
- **port** – MySQL port to use, default is usually OK. (default: 3306)
- **bind_address** – When the client has multiple network interfaces, specify the interface from which to connect to the host. Argument can be a hostname or an IP address.
- **unix_socket** – Optionally, you can use a unix socket rather than TCP/IP.
- **read_timeout** – The timeout for reading from the connection in seconds (default: None - no timeout)
- **write_timeout** – The timeout for writing to the connection in seconds (default: None - no timeout)
- **charset** – Charset you want to use.
- **sql_mode** – Default SQL_MODE to use.
- **read_default_file** – Specifies my.cnf file to read these parameters from under the [client] section.
- **conv** – Conversion dictionary to use instead of the default one. This is used to provide custom marshalling and unmarshaling of types. See converters.
- **use_unicode** – Whether or not to default to unicode strings. This option defaults to true for Py3k.
- **client_flag** – Custom flags to send to MySQL. Find potential values in constants.CLIENT.
- **cursorclass** – Custom cursor class to use.
- **init_command** – Initial SQL statement to run when connection is established.
- **connect_timeout** – Timeout before throwing an exception when connecting. (default: 10, min: 1, max: 31536000)
- **ssl** – A dict of arguments similar to mysql_ssl_set()’s parameters. For now the capath and cipher arguments are not supported.
- **read_default_group** – Group to read from in the configuration file.
- **compress** – Not supported
- **named_pipe** – Not supported
- **autocommit** – Autocommit mode. None means use server default. (default: False)
- **local_infile** – Boolean to enable the use of LOAD DATA LOCAL command. (default: False)
- **max_allowed_packet** – Max size of packet sent to server in bytes. (default: 16MB) Only used to limit size of “LOAD LOCAL INFILE” data packet smaller than default (16KB).
- **defer_connect** – Don’t explicitly connect on contruction - wait for connect call. (default: False)
- **auth_plugin_map** – A dict of plugin names to a class that processes that plugin. The class will take the Connection object as the argument to the constructor. The class needs an authenticate method taking an authentication packet as an argument. For the dialog plugin, a prompt(echo, prompt) method can be used (if no authenticate method) for returning a string from the user. (experimental)
- **server_public_key** – SHA256 authenticaiton plugin public key value. (default: None)
- **db** – Alias for database. (for compatibility to MySQLdb)
- **passwd** – Alias for password. (for compatibility to MySQLdb)
- **binary_prefix** – Add _binary prefix on bytes and bytearray. (default: False)



### 其他操作

- `autocommit_mode`*= None*

  specified autocommit mode. None means use server default.

- `begin`()

  Begin transaction.

- `close`()

  Send the quit message and close the socket.See [Connection.close()](https://www.python.org/dev/peps/pep-0249/#Connection.close) in the specification.Raises:**Error** – If the connection is already closed.

- `commit`()

  Commit changes to stable storage.See [Connection.commit()](https://www.python.org/dev/peps/pep-0249/#commit) in the specification.

- `cursor`(*cursor=None*)

  Create a new cursor to execute queries with.Parameters:**cursor** – The type of cursor to create; one of `Cursor`, `SSCursor`, `DictCursor`, or `SSDictCursor`. None means use Cursor.

- `open`

  Return True if the connection is open

- `ping`(*reconnect=True*)

  Check if the server is alive.Parameters:**reconnect** – If the connection is closed, reconnect.Raises:**Error** – If the connection is closed and reconnect=False.

- `rollback`()

  Roll back the current transaction.See [Connection.rollback()](https://www.python.org/dev/peps/pep-0249/#rollback) in the specification.

- `select_db`(*db*)

  Set current db.Parameters:**db** – The name of the db.

- `show_warnings`()

  Send the “SHOW WARNINGS” SQL command.

## 创建一个新指针

`cursor = db.cursor()`

#### 也可以设置指针为字典类型

`cursor = db.cursor(cursor=pymysql.cursors.DictCursor)`

#### 移动指针

`cursor.scroll(1,mode='relative') # 相对当前位置移动`

`cursor.scroll(2,mode='absolute') # 相对绝对位置移动`

## 执行SQL语句--重点

cursor.execute('select * from city;')



## 获取一行数据

cursor.fetchone()



## 获取指定数量数据

cursor.fetchmany(5)



## 获取查询到的全部数据

cursor.fetchall()



## 查询结束后关闭连接

db.close()



## 对数据库进行改变操作之后, 需要提交改变

db.commit()



## 标准的执行语法

```python
# SQL 插入语句
sql = """INSERT INTO EMPLOYEE(FIRST_NAME,
         LAST_NAME, AGE, SEX, INCOME)
         VALUES ('张', '老三', 44, '男', 7000)"""
try:
   # 执行sql语句
   cursor.execute(sql)
   # 提交到数据库执行
   db.commit()
except:
   # 如果发生错误则回滚
   db.rollback()
 
# 关闭数据库连接
db.close()
```

