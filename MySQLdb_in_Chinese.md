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
> conn methods ���±�

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

db=_mysql.connect(host="localhost",user="joebob",
                  passwd="moonpie",db="thangs")