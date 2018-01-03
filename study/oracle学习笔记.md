#### 1.replace('abcde', 'a', null) 替换函数 => bcde
#### 2.greatest() 查找最大值
#### 3.创建视图文件 
    CREATE OR REPLACE VIEW V_zwz_test as 查询1 union all 查询2 
    - union all 合并多个查询的值，可以有重复的项 union 不能有重复的
	- union 和 union all的区别 
		-union 会去重，就算单个结果集中的重复集合也会被去除，
#### 4.coalesce 返回第一个不为空的值
#### 5.子查询中，别名的使用：若
    查询条件需要使用别名来判断，需要把它设为子查询
	SELECT * FROM (SELECT USER_ID 用户id, PWD as 密码 FROM MY_USER) x WHERE x.用户id < 126;
#### 6. case when 语法
    SELECT USER_ID, PWD,
     CASE
        when pwd = '123' then '过低'
        when pwd = '456' then '过高'
        else 'ok'
    end as status FROM MY_USER WHERE USER_ID = 125;
#### 7.ORDER BY 2
    表示 以第二字段开排序  ORDER BY 3 ASC, 4 DESC ----> 多字段的排序
#### 8.WHERE ROWNUM <= 3 可以限制返回行数
#### 9.dbms_random.value() 去随机值
#### 10. 正确的取随机行的方法   --正确 （先随机取数， 然后再取出来 ）
	SELECT USER_ID, PWD FROM (select USER_ID, pwd FROM MY_USER ORDER BY dbms_random.value()) WHERE ROWNUM < = 3;
#### 11.oracle中substr的用法
- substr(字符串,截取开始位置,截取长度)
- substr(USERNAME, -2，1) 截取字符 -2表示从倒数第二的位置截取 1 表示 截取长度为1

#### 12.int 和 number 、Integer的异同 
    - oracle 中本来就没有int类型，只是为了和别的数据库进行兼容，
	- int相当于number(22)
    一般不采用这样会造成字段空间的浪费。number可以设置各种类型比如说mysql中的
    - integer 相当于Number(38, 0) 同样 如果不需要这么大的 可能会造成浪费
	- oracle 中 decimail(8, 2) 其实就是number类型 如999999.99
#### 13.Oracle to_date 用法
     create_time &lt;= to_date(':createTimeEnd'||' 23:59:59','YYYY-MM-DD hh24:mi:ss')
*注意 时间 格式化 秒的部分是 mi 不是像java中的mm 其中 || 是sql中字符拼接* 
#### 14.translate() 函数
    SELECT translate('ab 你好 bcdef', 'abcdef', '123456') as new_str from dual;
    to_string 不能为空， 为空 将没有意义
	from_string与to_string 按顺序一一对应, 若后面的没有那么则为空

    SELECT translate('clack,king,meiller,s', ',' || 'clack,king,meiller,s', ',') as str FROM dual
- 分析可得 凡是 ',' 的位置都被替换为',' 其他的部分由于没有值，则为空，   
- 结果为',,,'

#### 15.字符串拼接函数 || conact()的异同 
    -concat 只能连接两个字符， 如果有需要需要嵌套 concat(str1, concat(str2, str3)). 
    - || 可以随意拼接，没有限制
#### 16.nvl(str, -1) 
    若str不存在则为-1，存在则不变 nvl2(str, 0, -1) str为空则为0， 不为空则为1
#### 17.NULLS FIRST/LAST
    根据NULLS FIRST/LAST 来排序空值 ，当然你要对空值那一列进行order by,对没有空值的列进行排序然后进行NULLS FIRST或者FIRST 那将毫无意义
#### 18.oracle中插入多条 
    INSERT ALL INTO MY_USER (USERNAME, PWD, HEIGHT, GENDER) VALUES ('ximo', '12', '151', '0')
    INTO MY_USER(USERNAME, PWD, HEIGHT, GENDER) VALUES ('mmm', '180', '4444', '1') SELECT 1 from dual;
##### * INSERT ALL 无条件插入 同时向两张表插入
    INSERT ALL
        INTO test1(a1, b2 ,c3) VALUES (a1, b2, c3)
        INTO test2(a1, b2, d4) VALUES (a1, b2, d4)
    SELECT a1, b2, c3, d4 FROM master_test WHERE test_id in(10, 20)
##### * INSERT ALL  有条件插入
    INSERT ALL
      WHEN c3 IN ('c31', 'c32') THEN
        INTO test1(a1, b2, c3) VALUES (a1, b2, c3)
      WHEN  b2 < 456 THEN
        INTO test2(a1, b2, d4) VALUES (a1, b2, d4)
    SELECT a1, b2, c3, d4 FROM master_test;
##### * INSERT FIRST 当第一个表条件满足时，第二个表不在插入对应的行
    INSERT FIRST 
      WHEN c3 IN ('c31', 'c32') THEN
        INTO test1(a1, b2, c3) VALUES (a1, b2, c3)
      WHEN  b2 < 456 THEN
        INTO test2(a1, b2, d4) VALUES (a1, b2, d4)
    SELECT a1, b2, c3, d4 FROM master_test;
##### * 转置 insert 
    insert all 
        into test2(a1, des) values('周一'， e1)
        into test2(a1, des) values('周二'， e2)
        into test2(a1, des) values('周三'， e3)
    select e1, e2, e3 from test2;
#### 19.oracle中空字符串
    常常相当于null select '' as str from dual  但是 空字符'' 是varchar2类型 ，而null则可以为任何类型
#### 20.oracle中 or 和 union 
    二者在一定条件的时候可以互换 ，但是当结果集中有重复集的时候，就需要一定的转化
	一般遇到有重复集的时候，一般可以添加一个不重复的字段 来表示它不唯一， 然后作为一个子查询 ，在查询出来！这个值可以是唯一主键，也可以是row_id
#### 21.ROWNUM 和 rowid的区别 
    rownum 是一个伪列,且是一个唯一的，可以说是先查找数据，然后再加上去唯id
    一般只支持<、<=、!= 并非说用>,& gt;=,=,between..and 时会提示SQL语法错误，而是经常是查不出一条记录来，还会出现似乎是莫名其妙的结果来
    rowid 是物理存在的，很多rownum会出现的问题，都可以解决
#### 22.组织相关的行
##### inner join 
    SELECT L.str left_str, R.str right_str FROM L INNER JOIN R on L.v = R.v
        ORDER BY 1, 2;
    SELECT L.str left_str, R.str right_str FROM L, R WHERE L.v = R.v
        ORDER BY 1, 2;
##### left join 
    SELECT a.*, b.* from a = b(+)就是一个左连接
    等同于
    select a.*, b.* from a left join b
* 其中了left join 中的条件不能乱写，即 on 后面的条件 与 where后面的条件 是不一样的
    - on后面的是连接的条件 ，一般过滤的条件都是在这个里面
    - where 一般都是些对结果集的采集

##### right join 
    SELECT a.*, b.* from a(+) = b就是一个右连接
#### 24.oracle 中start with
    START WITH... CONNECT BY PRIOR...

- 获得一个树结构
- START WITH 子句为可选项，用来标识哪个节点作为查找树形结构的根节点。
- 条件2：是连接条件，其中用PRIOR表示上一条记录，例如CONNECT BY PRIOR STUDENT_ID = GRADE_ID    等同于
    select a.\*, b.\* from a right join b

##### FULL JOIN
    FULL JOIN 没有（+）的写法
##### 自连接
    同一个表取不同的表名，然后用表内不同的字段进行连接，
    比如说员工号和上级领导号，以此来查询某个人的直接上级的名字或者其他信息
#### 23.oracle中的查看执行计划
    explain plan for select user_id from my_user;
    select * from table(dbms_xplan.display());
#### 24.oracle 中start with
    START WITH... CONNECT BY PRIOR...
- 获得一个树结构
- START WITH 子句为可选项，用来标识哪个节点作为查找树形结构的根节点。
- 条件2：是连接条件，其中用PRIOR表示上一条记录，例如CONNECT BY PRIOR STUDENT_ID = GRADE_ID

#### 25.oracle 子查询
##### - not in
    select count(*) from user where usename NOT IN (select u.username from my_user u) 
 如果子查询中包含空值，所以 NOT IN(空值) 返回空 故count(*)为零条所以必须加上（子查询中）where u.username IS NOT NULL;
```
    select count(*) from user where usename NOT IN (select u.username from my_user u where u.username IS NOT NULL) 
```
##### - not exist
    create table t1 (c1 number,c2 number);  
    create table t2 (c1 number,c2 number);  
      
    insert into t1 values (1,2);  
    insert into t1 values (1,3);  
    insert into t2 values (1,2);  
    insert into t2 values (1,null);  

    t1: c1 c2   t2: c1 c2
        1  2        1  2
        1  3        1  null
      
    select * from t1 where c2 not in (select c2 from t2);  
    no rows found  
    select * from t1 where not exists (select 1 from t2 where t1.c2=t2.c2);  
    c1 c2  
    1 3  
    UPDATE MY_USER t SET t.USERNAME = '你才是智障'
    WHERE NOT exists(SELECT null FROM MY_USER_BONUS d WHERE d.user_id = t.USER_ID)
#### 26.oracle 中表的复制有两种方法
##### *将test复制到test2中， 整张表复制，数据加结构
    CREATE TABLE test2 as SELECT * from test;
##### *将test表格的定义复制到test2中，即空表没有数据
    create TABLE test2 as SELECT * FROM test WHERE 1 = 2;
#### 27.oracle中的约束CONSTRAINT 
    ALTER table emp add CONSTRAINT ch_sal CHECK (sal > 0)  
#### 28.WITH CHECK OPTION -- 被看做视图
    里面的条件要满足才能继续操作,下面的会执行失败
    insert into (select a, b from user where a != b with check option) values ('tset', 'test2');
#### 29. 为表添加一个新的字段
    alter table my_table add new_type varchar2(16) default 'noname';
#### 30.oracle中merge的用法
    MERGE INTO table1 t1 USING test2 t2 on (t1.id = t2.id)
    WHEN MATCHED THEN
      UPDATE SET t1.name = t2.name
      DELETE WHERE t1.name = t2.name
    WHEN NOT MATCHED THEN
      INSERT VALUES (t2.id, t2.name, t2.age);
#### 31. oracle 中查询版本号
    select * from v$version;  
    select * from product_component_version;
#### 32. oracle中delete语句
    delete from person where id = :id;
#### 33.oracle 建立主键的约束(主键也尽量加上名字)
    - ALTER TABLE MY_USER_BONUS ADD CONSTRAINT pk_my_user_bonus_userno PRIMARY KEY (USERNO)
#### 34.可查询的大表 ods_nbgl2.khgl_dwkh_coreinfo
#### 35.删除子表中与主表中数据不一致的值（很有用有木有！）
    -- 子表中删除与主表中不一致的值 在添加外键的时候会导致一定的影响
    DELETE FROM emp WHERE NOT EXISTS(SELECT null FROM dept WHERE emp.id = dept.id)
#### 36.oracle中删除同一张表中重复的数据
##### - 通过name相同 id不同的方式来判定 需要建立联合索引
    create index idx_name_id on my_user (name, id);
###### 留user_id小的值
    DELETE FROM MY_USER a WHERE
      exists(SELECT NULL FROM MY_USER b WHERE a.USERNAME = b.USERNAME AND a.USER_ID < b.USER_ID)
###### 留user_id大的值
    DELETE FROM MY_USER a WHERE
      exists(SELECT NULL FROM MY_USER b WHERE a.USERNAME = b.USERNAME AND a.USER_ID > b.USER_ID)
##### - 通过name相同 行号rowid不同的方式来判定 只需要建立单个id索引
    主键是有索引的 加个鬼哦 
    create index idx_id on my_user (user_id)
##### - 留user_id的最小值
    DELETE FROM MY_USER a WHERE
    exists(SELECT null FROM MY_USER b WHERE a.USERNAME = b.USERNAME AND a.ROWID > b.ROWID)
##### - oracle 中组内排序
    SELECT row_number() OVER
    (PARTITION BY USERNAME ORDER BY HEIGHT DESC ) id, USER_ID, GENDER, USERNAME FROM MY_USER

    4444  1   1007    1   mmm
    160   1   1006    0   ximo
    4444  1   1009    1   你才是智障
    151   2   1008    0   你才是智障
    null  1   1005    0   朱文赵
    175   2   1000    0   朱文赵

    DELETE FROM MY_USER WHERE ROWID IN (
    SELECT rid from (SELECT ROWID as rid, row_number() OVER (PARTITION BY USERNAME ORDER BY HEIGHT ASC ) seq FROM MY_USER)
    WHERE seq > 1)
#### 37.oracle中LEVEL的使用, 用于遍历字符串 level <= (str)
    SELECT LEVEL from dual CONNECT BY LEVEL <= 4;
    SELECT LEVEL, substr('天天向上', LEVEL, 1) as fun FROM dual CONNECT BY LEVEL <= length('天天向上');
#### 38. oracle 中字符串中保留单引号
    SELECT 'g''day mate' qmarks FROM dual UNION ALL
    SELECT 'beavers'' teeth' FROM dual UNION ALL
    SELECT '''' FROM dual
       str
    1  g'day mate
    2  beavers' teeth
    3  '
#### 39. oracle中q-quote的使用
- 界定符可以是tab，空格，回车外的任何单字节或者多字节字符
- 界定符可以是\[], {}, <>, ()且必须成对出现
```
    SELECT q'[']' FROM dual
```

#### 40.oracle中regexp_count() 注：11g特有的函数 dataGrip会标黄
    SELECT regexp_count(str, ',') + 1 as count FROM 
    (SELECT 'clack,king,meiller' as str from dual)
#### 41.oracle中字符串中有多个分隔符时， 使用translate要 除以 分隔符的长度
    SELECT length(translate(str, '$#' || str, '$#')) / length('$#') + 1 as count from (SELECT 'ab$#sss$#dsa' as str FROM dual)
- 使用 regexp_count() 来分割，就不需要注意分隔符的长度
```
    SELECT regexp_count(str, '\$#') + 1 as count FROM
    (SELECT 'ab$#sss$#dsa' as str FROM dual)
```

#### 42.删除字符串中不需要的值
- 如现在需要删除名字中的aeiou 元音值
    + 一般做法
``` 
    replace(translate(ename, 'aeiou', 'aaaaa'), 'a', '')
```
    + 进阶做法 不需要嵌套
```
    translate(ename, '1aeiou', '1')
```
    + 正则方法 将所要替换的值置为空
```
    regexp_replace(ename, '[aeiou]')
```

#### 43.oracle 中没有top limit关键字（很绝望） 
# 新的一天 新的开始 2018-01-02
#### 44.oracle中正则表达式的使用 regexp_replace
    regexp_replace(data, '[0-9]', '');
- 此处代表替换非字符串 也可以用translate关键字

#### 45.oracle 中的 regexp_like
- regexp_like(data, '\[abc]') 相当于(like or %a% or like %b% or like %c%);
- regexp_like(data, '^a')相当于 like 'a%'没有了前模糊查询
- regexp_like(data, 'a$')相当于 like '%a' 没有了后模糊查询
- regexp_like(data, '^a$')相当于 like 'a' 没有了模糊查询 精准查询a
- regexp_like(data, '\[0-9a-zA-Z]+')相当于(like or %数字% or like %小写字母% or like %大写字母%);
- ^\[0-9a-zA-Z]+$ 其中^ 在[]外表示字符串开始，$ 表示字符串结束
- \[^0-9]表示非数字 ^ 在[]里面表示非
- 正则表达式中
    + '+'表示匹配前面的式子一次或者多次，如regexp_like(data, '16+')相当于like 16%
    + '\*'表示匹配前面的式子零次或者多次，如regexp_like(data, '16*')相当于like 1%

#### 46.oracle中如果想要用字符串中的数字进行排序，那么方法就是先用regexp_replace(data, '\[^0-9]', '')或者translate(data, '0123456789' || data, '0123456789') 将非数字替换为''

#### 47.oracle中listagg用法
    SELECT ID, listagg(NAME, ',') within group(ORDER BY ID) AS NAMES
      FROM DEMO T 
     GROUP BY ID
- 具体请参见 http://blog.csdn.net/baojiangfeng/article/details/62237522
- 具体请参见 oracle查询优化改写 P106的部分
- 分析 ORDER BY USERNAME DESC 中表示 里面按照 名字的降序来排列 如 B,C,D
- 以pwd 密码为分组，将密码相同的人的名字放在一起，很实用的方法
```
    SELECT PWD, listagg(username, ',') WITHIN GROUP(ORDER BY USERNAME DESC) as new_name FROM MY_USER GROUP BY PWD 
```
- listagg的高级用法 和 level来使用
    + 将my_user中的username 去重 加排序
```
    SELECT USERNAME, (
      SELECT LISTAGG(MIN(SUBSTR(USERNAME, LEVEL, 1))) WITHIN GROUP (ORDER BY MIN(SUBSTR(USERNAME, LEVEL, 1)))
        FROM DUAL
      CONNECT BY LEVEL <= LENGTH(USERNAME) GROUP BY SUBSTR(USERNAME, LEVEL, 1)
    ) AS NEW_NAME
      FROM MY_USER;
```

#### 48.oracle中 instr的用法
- 可以使用instr函数对某个字符串进行判断，判断其是否含有指定的字符。 
- instr(sourceString,destString,start,appearPosition) 
- instr（'源字符串' , '目标字符串' ,'开始位置','第几次出现'） 

#### 49.oracle中regexp_substr的用法
- regexp_substr(name, '\[^,]+', 1, 2)
    + 其中参数2： '\[^,]+'表示匹配多个非','一次或者多次
    + 参数3： 1表示从位置1开始
    + 参数4： 2表示下一个匹配的字符串

#### 50.oracle 中level关键字的用法 伪列 1，2, 3，4，5……
    SELECT DBMS_RANDOM.VALUE, level
       FROM dual
      CONNECT BY LEVEL < 10
- level中将username 拆分为单个字符
```
    SELECT my_name, substr(my_name, level, 1) as c FROM (SELECT 'username' as my_name FROM dual)
    CONNECT BY LEVEL < =length(my_name)
```
- 使用listagg讲这些字符连接起来
```
    SELECT my_name, listagg(substr(my_name, level, 1)) WITHIN GROUP(ORDER BY substr(my_name, level, 1)) as new_name
      FROM (SELECT 'username' my_name from dual)
    CONNECT BY LEVEL <= length(my_name)
    GROUP BY my_name
```
- 使用 listagg level min 几个关键字来实现 去重 排序
```
     SELECT USERNAME, (
      SELECT LISTAGG(MIN(SUBSTR(USERNAME, LEVEL, 1))) WITHIN GROUP (ORDER BY MIN(SUBSTR(USERNAME, LEVEL, 1)))
        FROM DUAL
      CONNECT BY LEVEL <= LENGTH(USERNAME) GROUP BY SUBSTR(USERNAME, LEVEL, 1)
    ) AS NEW_NAME
      FROM MY_USER;
```

#### 51.判断可以使用可以作为数字的字符串 TRANSLATE, is not null
    SELECT TO_NUMBER(MIXED) AS MIXED FROM (
      SELECT TRANSLATE(MIXED, '0123456789' || MIXED, '0123456789') AS MIXED FROM TEST
    ) WHERE MIXED IS NOT NULL
#### 52.