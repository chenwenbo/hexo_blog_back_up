title: Spring异常体系
date: 2016-01-19 09:32:22
tags:
---

> 总结：Spring的异常体系会将所有的checkException转换为RunTimeException,而DataAccessException就是这类runTimeException的顶级异常类。所以我们的代码中不用显示的进行try catch处理。只需要在合适的地方进行catch处理。这种方式减少了代码的侵入性。

通常在 Dao 层将所有异常都转嫁到 Spring 的 RuntimeException 体系中来　－DataAccessException

 Spring的DAO框架没有抛出与特定技术相关的异常，例如SQLException或HibernateException，抛出的异常都是 与特定技术无关的org.springframework.dao.DataAccessException类的子类，避免系统与某种特殊的持久层实现耦 合在一起。
 
 DataAccessException是RuntimeException，是一个无须检测的异常，不要求代码去处理这类异常，遵循了 Spring的一般理念：异常检测会使代码到处是不相关的catch或throws语句，使代码杂乱无章；并且 NestedRuntimeException的子类，是可以通过NestedRuntimeException的getCause（）方法获得导致该异 常的另一个异常。
 
 ---

 Spring的异常分类有：

| 异常        |何时抛出   |
| --------   | -----:  |
| CleanupFailureDataAccessException     | 一项操作成功地执行，但在释放数据库资源时发生异常（例如，关闭一个Connection） | 
| DataAccessResourceFailureException        |   数据访问资源彻底失败，例如不能连接数据库   | 
| DataIntegrityViolationException        |    Insert或Update数据时违反了完整性，例如违反了惟一性限制    | 
| DataRetrievalFailureException        |    某些数据不能被检测到，例如不能通过关键字找到一条记录    | 
| DeadlockLoserDataAccessException        |    当前的操作因为死锁而失败    | 
| IncorrectUpdateSemanticsDataAccessException        |    Update时发生某些没有预料到的情况，例如更改超过预期的记录数。当这个异常被抛出时，执行着的事务不会被回滚    | 
| InvalidDataAccessApiusageException        |    一个数据访问的JAVA API没有正确使用，例如必须在执行前编译好的查询编译失败了    | 
| invalidDataAccessResourceUsageException        |    错误使用数据访问资源，例如用错误的SQL语法访问关系型数据库    | 
| OptimisticLockingFailureException        |   乐观锁的失败。这将由ORM工具或用户的DAO实现抛出    | 
| TypemismatchDataAccessException        |    Java类型和数据类型不匹配，例如试图把String类型插入到数据库的数值型字段中    |
| UncategorizedDataAccessException        |    有错误发生，但无法归类到某一更为具体的异常中    | 


Spring的DAO异常层次是如此的细致缜密，服务对象能够精确地选择需要捕获哪些异常，捕获的异常对用户更有用的信息，哪些异常可以让她继续在调用堆栈中向上传递。

于是，我们在dao中只需要抛出这个运行时异常，我们就可以在
```
 /**
 *  根据时间获取日KPI数据
 * @param date 日期
 * @return 
 */
public List<KPIDataBean> getKPIOfDayDataByDate(String date) throws DataAccessException;
```
并在它的实现类中也抛出这么个异常。
这样，在调用这个方法的时候，我们捕获这个异常即可：
```
try {
    list = kpiDao.getKPIOfDayDataByDate(date);
} catch(DataAccessException e) {
    System.out.println("test:" + e.getMessage());
}
```
这样就可以捕获相应的异常了。
这是打印出来的信息
```
test:nested exception is org.apache.ibatis.exceptions.PersistenceException: 
Error querying database.  Cause: org.springframework.jdbc.CannotGetJdbcConnectionException: Could not get JDBC Connection; nested exception is org.apache.commons.dbcp.SQLNestedException: Cannot create PoolableConnectionFactory (Communications link failure
```

