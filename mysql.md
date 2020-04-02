# biji_2333

> 1.TIMESTAMP创建: 
  CREATE TABLE test
  (
    id            INT(10) UNSIGNED PRIMARY KEY   NOT NULL,

    updatedAt     TIMESTAMP  DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    createdAt     TIMESTAMP
  );

> 2. [distict、group by 和 row_number() over](去重： https://www.cnblogs.com/171207xiaohutu/p/11520759.html)
