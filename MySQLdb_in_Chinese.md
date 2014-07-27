### Introduction ����

MySQLdb is an thread-compatible interface to the popular MySQL database server that provides the Python database API.

MySQLdb �����е�Mysql ���ݿ���̼߳��ݽӿڣ����ṩ��python�����ݿ�API��


### Installation ��װ

The README file has complete installation instructions.

README�ļ��������İ�װ˵����


### _mysql

If you want to write applications which are portable across databases, use MySQLdb, and avoid using this module directly. _mysql provides an interface which mostly implements the MySQL C API. For more information, see the MySQL documentation. The documentation for this module is intentionally weak because you probably should use the higher-level MySQLdb module. If you really need it, use the standard MySQL docs and transliterate as necessary.

�������дһ��app������ ��������ݿ⣬�� MySQLdb��������ֱ��ʹ��_mysql��. _mysql �ṩ�Ľӿ� ʵ���˴󲿷� Mysql��C����API. �������飬�Լ���MySQL�ĵ�. ��ƪ���� _mysql���ĵ����������ˣ���Ϊ����ܸ����ø߲��װ�� MySQLdb�⡣����������� _mysql��������Ҫ�鿴��׼ MySQL�ĵ� �� ���롣


### MySQL C API translation MySQL��C����API����

The MySQL C API has been wrapped in an object-oriented way. The only MySQL data structures which are implemented are the MYSQL (database connection handle) and MYSQL_RES (result handle) types. In general, any function which takes MYSQL *mysql as an argument is now a method of the connection object, and any function which takes MYSQL_RES *result as an argument is a method of the result object. Functions requiring none of the MySQL data structures are implemented as functions in the module. Functions requiring one of the other MySQL data structures are generally not implemented. Deprecated functions are not implemented. In all cases, the mysql_ prefix is dropped from the name. Most of the conn methods listed are also available as MySQLdb Connection object methods. Their use is non-portable.


MySQL��C����API �Ѿ�����װ�����������Ψһʵ���˵�MySQL ���ݽṹ�� MYSQL(database connection handle)���� �� MYSQL_RES(result handel)����. 
�ܵ�������
- �κ�һ�� ���� `MYSQL *mysql` ��Ϊ������function ���ڶ���ʵ�ֳ��� һ��connecition object�����method����, 
- ���� �κ�һ������ `MYSQL_RES *result`��Ϊ������function ���ڶ���ʵ�ֳ��� һ��result object�����method.
- ��Щ��ҪMySQL ���������ݽṹ��Functions ������ûʵ��. �ѹ�ʱ��functions û��ʵ��. �����ϣ�ǰ׺`mysql_`�Ѿ����������Ƴ�. ������г��� conn methods ͬ������ MySQLdb connection object methods ��ʹ��. ���������������(��䷭����Ǹ�p).


> ��ã���������ע�� k9���뷢��Ů�绰�� yan95207396@126.com
> 1. database connection handle(����ע: ��������mysql�ķ���, ���þ�������)
> 2. result handel(����ע: �²��� ��sql��ѯ ת����� list, tuple֮���python���ݽṹ)
> 3. �������Ǻ���, ������ô��ע�� Orz. ��ϲ����Ů�������� yan95207396@126.com. 1024.
> 4. MYSQL *mysql = int *p, ����ָmysql�ٷ��ĵ���. ��������׼ȷ�ز²⣬��Ҫ����cָ��. ����´���ӭָ�� Orz.
> 5. conn methods ���±�


### MySQL C API function mapping ���ձ�
| C API | _mysql | Orz ע��
| :------------------- | --------:| --------: | 
|mysql_affected_rows() |	conn.affected_rows()| |
|mysql_autocommit()	| conn.autocommit()|
|mysql_character_set_name()	| conn.character_set_name()|
|mysql_close()	| conn.close()|
|mysql_commit()	| conn.commit()|
|mysql_connect() |	_mysql.connect()|
|mysql_data_seek() |	result.data_seek()|
|mysql_debug() |	_mysql.debug()|
|mysql_dump_debug_info |	conn.dump_debug_info()|
|mysql_escape_string() |	_mysql.escape_string()|
|mysql_fetch_row() |	result.fetch_row()|
|mysql_get_character_set_info() |	conn.get_character_set_info()|
|mysql_get_client_info() |	_mysql.get_client_info()|
|mysql_get_host_info() |	conn.get_host_info()|
|mysql_get_proto_info() |	conn.get_proto_info()||
|mysql_get_server_info() |	conn.get_server_info()|
|mysql_info() |	conn.info()|
|mysql_insert_id() |	conn.insert_id()|
|mysql_num_fields() | result.num_fields()|
|mysql_num_rows() |	result.num_rows()|
|mysql_options() |	various options to _mysql.connect()|
|mysql_ping() |	conn.ping()|
|mysql_query() |	conn.query()|
|mysql_real_connect() |	_mysql.connect()|
|mysql_real_query() |	conn.query()|
|mysql_real_escape_string() |	conn.escape_string()|
|mysql_rollback() |	conn.rollback()|
|mysql_row_seek() |	result.row_seek()|
|mysql_row_tell() |	result.row_tell()|
|mysql_select_db() |	conn.select_db()|
|mysql_set_character_set() |	conn.set_character_set()|
|mysql_ssl_set() | ssl option to _mysql.connect()|
|mysql_stat() |	conn.stat()|
|mysql_store_result() |	conn.store_result()|
|mysql_thread_id() |	conn.thread_id()|
|mysql_thread_safe_client() |	conn.thread_safe_client()|
|mysql_use_result() |	conn.use_result()|
|mysql_warning_count() | conn.warning_count()|
|CLIENT_* |	MySQLdb.constants.CLIENT.*|
|CR_* |	MySQLdb.constants.CR.*|
|ER_* |	MySQLdb.constants.ER.*|
|FIELD_TYPE_* |	MySQLdb.constants.FIELD_TYPE.*|
|FLAG_* |	MySQLdb.constants.FLAG.*|

 

### Some _mysql examples

Okay, so you want to use _mysql anyway. Here are some examples.
Okay, ����������������`_mysql`. ����һЩ����.

The simplest possible database connection is:
��򵥵����ݿ�����:

``` python
import _mysql
db=_mysql.connect()
```
This creates a connection to the MySQL server running on the local machine using the standard UNIX socket (or named pipe on Windows), your login name (from the USER environment variable), no password, and does not USE a database. Chances are you need to supply more information.

�������еı���, ʹ�� ��׼ UNIX socket (or suck windows)������һ����MySQL ���ݿ������, ��ĵ�¼��(���û���������), û������, ���Ҳ�ʹ�����ݿ�. ����Ҫ�ṩ�����������Ϣ��
``` python
db=_mysql.connect("localhost","joebob","moonpie","thangs")
```

This creates a connection to the MySQL server running on the local machine via a UNIX socket (or named pipe), the user name "joebob", the password "moonpie", and selects the initial database "thangs".

ͨ�� UNIX socket (or suck windows) ������һ���������� MySQL ������������, �û��� "joebob", ���� "moonpie", ���� ʹ���� "thangs"Ϊ�������ݿ�.


We haven't even begun to touch upon all the parameters connect() can take. For this reason, I prefer to use keyword parameters:

���ǻ�û�нӴ��� `connect()`�����в���. ���ڴ�, ���˸�����ʹ�� �ؼ��ֲ�����

```python
# �Ƽ�
db=_mysql.connect(host="localhost",user="joebob",
passwd="moonpie",db="thangs")
```

This does exactly what the last example did, but is arguably easier to read. But since the default host is "localhost", and if your login name really was "joebob", you could shorten it to this:
���������ȫ������������ͬ, ����, ����˵���׶�. ������Ϊ `host` ��Ĭ��ֵ����`localhost` , ���ң������ĵ�½������ `joebobo`, �����ѡ�����̵�д����

> �ؼ��ֲ��� vs λ�ò���, �Բ�

```python
# ���߲��Ƽ��� explicit is better than no-explicit.
db=_mysql.connect(passwd="moonpie",db="thangs")
```

UNIX sockets and named pipes don't work over a network, so if you specify a host other than localhost, TCP will be used, and you can specify an odd port if you need to (the default port is 3306):
UNIX sockets �� suck windows ������ͨ�����繤��, �����ָ���Ĳ���`localhost`, TCP Э���ʹ��, ���������ָ�������ӿ�, �������Ҫ(Ĭ��ֵ��3306)

> 1. ����: localhost�� 127.0.0.1, ����, /etc/hosts
> 2. mysqlĬ�ϼ���3306, �Ǻǣ��Լ���ǰ������ɵ��

```python
# = =! Why is moonpie...? Is this from the big theory?
db=_mysql.connect(host="outhouse",port=3307,passwd="moonpie",db="thangs")
```

If you really had to, you could connect to the local host with TCP by specifying the full host name, or 127.0.0.1.
Generally speaking, putting passwords in your code is not such a good idea:
����������Ҫ�������ͨ��ָ��ȫhost name, ���� 127.0.0.1��������TCP���ӱ����ˡ�
ͨ����˵��������ŵ���������Ǹ������⡣���ߣ���Ȼ�����������Ĺ�˾������������ĳ������ְԱ��Ҳûʲô��Ӧ��setting�ļ�Ȩ��Ҳ����666.

```python
db=_mysql.connect(host="outhouse",db="thangs",read_default_file="~/.my.cnf")
```

This does what the previous example does, but gets the username and password and other parameters from ~/.my.cnf (UNIX-like systems). Read about option files for more details.

��������������ͬ, ������ͨ�� `~/.my.cnf` (��UNIX ϵͳ) ��ȡ�û����������Լ���������. �Ķ�ѡ���ļ���ø�����Ϣ.

So now you have an open connection as db and want to do a query. Well, there are no cursors in MySQL, and no parameter substitution, so you have to pass a complete query string to db.query():
��ô������������һ����db�����ӣ�Ȼ������ȥִ�в�ѯ. Well, ��MySQL ��û�� cursors�ĸ���, ����û��������Ĳ���, �����㲻�ò���һ�������Ĳ�ѯ���� `db.query()`:

> ���߲�����, cursor���������ݿ��У���һ�����и����𣿺ܺ����� MySQLdb�ڳ��������ʵ����cursor��.

```python
db.query("""SELECT spam, eggs, sausage FROM breakfast
WHERE price < 5""")
```

There's no return value from this, but exceptions can be raised. The exceptions are defined in a separate module, _mysql_exceptions, but _mysql exports them. Read DB API specification PEP-249 to find out what they are, or you can use the catch-all MySQLError.

�����ѯû�з���ֵ, ���ǿ����׳��쳣. �쳣�Ķ���������һ��module��, `_mysql_exceptions`, ���� `_mysql` ���������. �Ķ� DB API�淶PEP-249������������ʲô, �����������`MySQLError` ȥ���������쳣.

> [PEP-249](http://legacy.python.org/dev/peps/pep-0249/), Ҫ������Ҫ������������ҲҪ��һ����~

At this point your query has been executed and you need to get the results. You have two options:

Ŀǰ��Ĳ�ѯ�Ѿ���ִ�У�Ȼ������Ҫ��ò�ѯ���. ��������ѡ��:

```python
r=db.store_result()
# ...or...
r=db.use_result()
```

Both methods return a result object. What's the difference? store_result() returns the entire result set to the client immediately. If your result set is really large, this could be a problem. One way around this is to add a LIMIT clause to your query, to limit the number of rows returned. The other is to use use_result(), which keeps the result set in the server and sends it row-by-row when you fetch. This does, however, tie up server resources, and it ties up the connection: You cannot do any more queries until you have fetched all the rows. Generally I recommend using store_result() unless your result set is really huge and you can't use LIMIT for some reason.

����method ���᷵��һ�� result object. ��ʲô�����أ� `store_result()` �������� ȫ���Ľ���� ���ͻ���. �����Ľ�����ǳ���, �����������. ���ڴ˵�һ�����������`LIMIT`Լ��, �Դ������㷵�ؽ��������. ��һ����������`user_result()`, ���ܱ��������ڷ�������, ���� ����`fetch` ��ȡ�����ʱ��һ��һ�еط���. ����, ��� �󶨷�������Դ, ���� ���������: �㲻������������ѯֱ����`fetch`��ȡ��������. ͨ�����Ƽ�ʹ��`store_result()` ������Ľ������ķǳ��޴� ���� ����ΪĳЩԭ����ʹ��`LIMIT`.

Now, for actually getting real results:

����, �����ػ�ȡ���:

```python
>>> r.fetch_row()
(('3','2','0'),)
```

This might look a little odd. The first thing you should know is, fetch_row() takes some additional parameters. The first one is, how many rows (maxrows) should be returned. By default, it returns one row. It may return fewer rows than you asked for, but never more. If you set maxrows=0, it returns all rows of the result set. If you ever get an empty tuple back, you ran out of rows.

����ܿ������е����. ������Ҫ֪������, `fetch_row()` ����һЩ����Ĳ���. ��һ����, ������ (`maxrows` �������). Ĭ��ֵ, ���᷵��һ��. �����ܻ᷵��������Ҫ�������, ������Զ�������. ��������� `maxrows=0`, �����ؽ������������. һ������һ����Ԫ�����, ��ͱ�����������.

The second parameter (how) tells it how the row should be represented. By default, it is zero which means, return as a tuple. how=1 means, return it as a dictionary, where the keys are the column names, or table.column if there are two columns with the same name (say, from a join). how=2 means the same as how=1 except that the keys are always table.column; this is for compatibility with the old Mysqldb module.

�ڶ������� (`how`) ������ �е�չ�ַ�ʽ. 
> 1. Ĭ��ֵ, `how` ��0, ����ζ�ŷ���ֵ�� tupleԪ��. 
> 2. `how=1`, ����ֵ�� dict�ֵ�, key ���� �ֶ���(����), ������ `table.column` �������ֵ������ ��ͬ���ֶ���(����, ���ϲ�ѯ). 
> 3. `how=2`, ��`how=1` ������ͬ, ���� key ������ `table.column` ��ʽ. ����Ϊ����ɵ� MySQLdb�� ����.



