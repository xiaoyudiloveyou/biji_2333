# biji_2333

> 1.TIMESTAMP创建: 
  CREATE TABLE test
  (
    id            INT(10) UNSIGNED PRIMARY KEY   NOT NULL,

    updatedAt     TIMESTAMP  DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    createdAt     TIMESTAMP
  );

> 2. [distict、group by 和 row_number() over](去重： https://www.cnblogs.com/171207xiaohutu/p/11520759.html)

##### 3.分页
https://www.cnblogs.com/bbgasj/archive/2012/11/06/2756567.html
http://blog.sina.com.cn/s/blog_65d50aab0102yt5u.html
https://www.cnblogs.com/net064/p/11061106.html
##### 4.SQL去重distinct方法解析
https://www.cnblogs.com/lixuefang69/p/10420186.html
##### 5.jion in
https://www.w3school.com.cn/sql/sql_join_left.asp

select * INTO做批量插入   https://bbs.csdn.net/topics/391006394?list=3485944
增量抽取
SELECT * FROM fenye fy WHERE NOT EXISTS (SELECT * FROM fenye_his fh WHERE fy.id = fh.id )


其他信息：
205026 	员工姓名	余佳乐 
分公司	西安软通动力网络技术有限公司 	部门	西安HWGTS实施三部0136 
职位	工程师B-MAG 	参加工作时间 	  2015-09-12


https://hellogithub.com/

https://blog.csdn.net/qing_gee/article/details/104709079



