title: 在spring环境中进行单元测试
date: 2016-05-04 18:16:16
tags:
---


清单 5. AccountServiceOldTest.Java
```
 package service; 

 import static org.Junit.Assert.assertEquals; 

 import org.Junit.BeforeClass; 
 import org.Junit.Test; 
 import org.Springframework.context.ApplicationContext; 
 import org.Springframework.context.support.ClassPathXmlApplicationContext; 

 import domain.Account; 

 public class AccountServiceOldTest { 
         private static AccountService service; 
        
         @BeforeClass 
         public static void init() { 
                 ApplicationContext 
 context = new ClassPathXmlApplicationContext("config/Spring-db-old.xml"); 
                 service = (AccountService)context.getBean("accountService"); 
         }      
        
         @Test 
         public void testGetAcccountById() { 
 Account acct = Account.getAccount(1, "user01", 18, "M"); 
                 Account acct2 = null; 
                 try { 
 service.insertIfNotExist(acct); 
                         acct2 = service.getAccountById(1); 
                         assertEquals(acct, acct2); 
                 } catch (Exception ex) { 
                         fail(ex.getMessage()); 
                 } finally { 
                         service.removeAccount(acct); 
                 } 
 } 
 }
```


注意上面的 Junit4 注释标签，第一个注释标签 @BeforeClass，用来执行整个测试类需要一次性初始化的环境，这里我们用 Spring 的 ClassPathXmlApplicationContext 从 XML 文件中加载了上面定义的 Spring 配置文件，并从中获得了 accountService 的实例。第二个注释标签 @Test 用来进行实际的测试。<br/><br/>
测试过程：我们先获取一个 Account 实例对象，然后通过 service bean 插入数据库中，然后通过 getAccountById 方法从数据库再查询这个记录，如果能获取，则判断两者的相等性；如果相同，则表示测试成功。成功后，我们尝试删除这个记录，以利于下一个测试的进行，这里我们用了 try-catch-finally 来保证账号信息会被清除。<br/><br/>
执行测试：（在 Eclipse 中，右键选择 AccountServiceOldTest 类，点击 Run as Junit test 选项），得到的结果如下：



### 执行测试的结果
在 Eclipse 的 Junit 视图中，我们可以看到如下的结果：
图 2. 测试的结果对于这种不使用 Spring test 框架进行的单元测试，我们注意到，需要做这些工作：

* 在测试开始之前，需要手工加载 Spring 的配置文件，并获取需要的 bean 实例
* 在测试结束的时候，需要手工清空搭建的数据库环境，比如清除您插入或者更新的数据，以保证对下一个测试没有影响

另外，在这个测试类中，我们还不能使用 Spring 的依赖注入特性。一切都靠手工编码实现。好，那么我们看看 Spring test 框架能做到什么。
首先我们修改一下 Spring 的 XML 配置文件，删除 <context:annotation-config/> 行，其他不变。
清单 6. Spring-db1.xml
```
 <beans xmlns="http://www.Springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
 xsi:schemaLocation="http://www.Springframework.org/schema/beans 
 http://www.Springframework.org/schema/beans/Spring-beans-3.2.xsd"> 
 <bean id="datasource" 
 class="org.Springframework.jdbc.datasource.DriverManagerDataSource"> 
                 <property name="driverClassName" value="org.hsqldb.jdbcDriver" /> 
                 <property name="url" value="jdbc:hsqldb:hsql://localhost" /> 
                 <property name="username" value="sa"/> 
                 <property name="password" value=""/> 
         </bean> 
 <bean id="transactionManager" 
 class="org.Springframework.jdbc.datasource.DataSourceTransactionManager"> 
                 <property name="dataSource" ref="datasource"></property> 
         </bean> 
         <bean id="initer" init-method="init" class="service.Initializer"> 
         </bean> 
 <bean id="accountDao" depends-on="initer" class="DAO.AccountDao"> 
                 <property name="dataSource" ref="datasource"/> 
         </bean> 
         <bean id="accountService" class="service.AccountService"> 
         </bean> 
 </beans>
```
其中的 transactionManager 是 Spring test 框架用来做事务管理的管理器。
清单 7. AccountServiceTest1.Java
```
 package service; 
 import static org.Junit.Assert.assertEquals; 

 import org.Junit.Test; 
 import org.Junit.runner.RunWith; 
 import org.Springframework.beans.factory.annotation.Autowired; 
 import org.Springframework.test.context.ContextConfiguration; 
 import org.Springframework.test.context.Junit4.SpringJUnit4ClassRunner; 
 import org.Springframework.transaction.annotation.Transactional; 

 import domain.Account; 

 @RunWith(SpringJUnit4ClassRunner.class) 
 @ContextConfiguration("/config/Spring-db1.xml") 
 @Transactional 
 public class AccountServiceTest1 { 
         @Autowired 
         private AccountService service; 
        
         @Test 
         public void testGetAcccountById() { 
 Account acct = Account.getAccount(1, "user01", 18, "M"); 
                 service.insertIfNotExist(acct); 
                 Account acct2 = service.getAccountById(1); 
                 assertEquals(acct,acct2); 
         } 
 }
```
对这个类解释一下：

** @RunWith ** 注释标签是 Junit 提供的，用来说明此测试类的运行者，这里用了 SpringJUnit4ClassRunner，这个类是一个针对 Junit 运行环境的自定义扩展，用来标准化在 Spring 环境中 Junit4.5 的测试用例，例如支持的注释标签的标准化

** @ContextConfiguration ** 注释标签是 Spring test context 提供的，用来指定 Spring 配置信息的来源，支持指定 XML 文件位置或者 Spring 配置类名，这里我们指定 classpath 下的 /config/Spring-db1.xml 为配置文件的位置

** @Transactional ** 注释标签是表明此测试类的事务启用，这样所有的测试方案都会自动的 rollback，即您不用自己清除自己所做的任何对数据库的变更了

** @Autowired ** 体现了我们的测试类也是在 Spring 的容器中管理的，他可以获取容器的 bean 的注入，您不用自己手工获取要测试的 bean 实例了
 testGetAccountById 是我们的测试用例：注意和上面的 AccountServiceOldTest 中相同的测试方法的对比，这里我们不用再 try-catch-finally 了，事务管理自动运行，当我们执行完成后，所有相关变更会被自动清除

执行结果在 Eclipse 的 Junit 视图中，我们可以看到如下的结果：
图 3. 执行结果小结如果您希望在 Spring 环境中进行单元测试，那么可以做如下配置：

* 继续使用 Junit4 测试框架，包括其 @Test 注释标签和相关的类和方法的定义，这些都不用变

* 您需要通过 @RunWith(SpringJUnit4ClassRunner.class) 来启动 Spring 对测试类的支持

* 您需要通过 @ContextConfiguration 注释标签来指定 Spring 配置文件或者配置类的位置

* 您需要通过 @Transactional 来启用自动的事务管理

* 您可以使用 @Autowired 自动织入 Spring 的 bean 用来测试

另外您不再需要：

* 手工加载 Spring 的配置文件

* 手工清理数据库的每次变更

* 手工获取 application context 然后获取 bean 实例

Spring 测试注释标签我们已经看到利用 Spring test framework 来进行基于 Junit4 的单元测试是多么的简单，下面我们来看一下前面遇到的各种注释标签的一些可选用法。

### @ContextConfiguration 和 @Configuration 的使用

刚才已经介绍过，可以输入 Spring xml 文件的位置，Spring test framework 会自动加载 XML 文件，得到 application context，当然也可以使用 Spring 3.0 新提供的特性 @Configuration，这个注释标签允许您用 Java 语言来定义 bean 实例，举个例子：

现在我们将前面定义的 Spring-db1.xml 进行修改，我们希望其中的三个 bean：initer、accountDao、accountService 通过配置类来定义，而不是 XML，则我们需要定义如下配置类：

注意：如果您想使用 @Configuration，请在 classpath 中加入 cglib 的 jar 包（cglib-nodep-2.2.3.jar），否则会报错。<br>
清单 8. SpringDb2Config.Java
```
 package config; 

 import org.Springframework.beans.factory.annotation.Autowired; 
 import org.Springframework.context.annotation.Bean; 
 import org.Springframework.context.annotation.Configuration; 
 import org.Springframework.jdbc.datasource.DriverManagerDataSource; 

 import service.AccountService; 
 import service.Initializer; 
 import DAO.AccountDao; 

 @Configuration 
 public class SpringDb2Config { 
         private @Autowired DriverManagerDataSource datasource; 
         @Bean 
         public Initializer initer() { 
                 return new Initializer(); 
         } 
        
         @Bean 
         public AccountDao accountDao() { 
 AccountDao DAO = new AccountDao(); 
 DAO.setDataSource(datasource); 
 return DAO; 
         } 
        
         @Bean 
         public AccountService accountService() { 
 return new AccountService(); 
         } 
 }
```
注意上面的注释标签：

** @Configuration ** ：表明这个类是一个 Spring 配置类，提供 Spring 的 bean 定义，实际效果等同于 XML 配置方法<br>
** @Bean **：表明这个方法是一个 bean 的定义，缺省情况下，方法名称就是 bean 的 Id<br>
** @Autowired **：这个 datasource 采用自动注入的方式获取

注意，我们采用的是 XML+config bean 的方式进行配置，这种方式比较符合实际项目的情况。相关的 Spring 配置文件也要做变化，如下清单所示：
清单 9. Spring-db2.xml
```
 <beans xmlns="http://www.Springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
 xmlns:context="http://www.Springframework.org/schema/context"
 xsi:schemaLocation="http://www.Springframework.org/schema/beans 
 http://www.Springframework.org/schema/beans/Spring-beans-3.0.xsd 
 http://www.Springframework.org/schema/context 
 http://www.Springframework.org/schema/context/Spring-context-3.0.xsd"> 
 <context:annotation-config/> 
 <bean id="datasource" 
 class="org.Springframework.jdbc.datasource.DriverManagerDataSource"> 
                 <property name="driverClassName" value="org.hsqldb.jdbcDriver" /> 
                 <property name="url" value="jdbc:hsqldb:hsql://localhost" /> 
                 <property name="username" value="sa"/> 
                 <property name="password" value=""/> 
         </bean> 
 <bean id="transactionManager" 
          class="org.Springframework.jdbc.datasource.DataSourceTransactionManager"> 
                 <property name="dataSource" ref="datasource"></property> 
         </bean> 
        
         <bean class="config.SpringDb2Config"/> 
 </beans>
```
注意里面的 context 命名空间的定义，如代码中黑体字所示。另外还必须有 <context:annotaiton-config/> 的定义，这个定义允许采用注释标签的方式来控制 Spring 的容器，最后我们看到 beans 已经没有 initer、accountDao 和 accountService 这些 bean 的定义，取而代之的是一个 SpringDb2Config bean 的定义，注意这个 bean 没有名称，因为不需要被引用。<br><br>
现在有了这些配置，我们的测试类只要稍稍修改一下，即可实现加载配置类的效果，如下：<br>
 ```
 @ContextConfiguration("/config/Spring-db2.xml")
 ```

通过上面的配置，测试用例就可以实现加载 Spring 配置类，运行结果也是成功的 green bar。

### @DirtiesContext

缺省情况下，Spring 测试框架一旦加载 applicationContext 后，将一直缓存，不会改变，但是，
由于 Spring 允许在运行期修改 applicationContext 的定义，例如在运行期获取 applicationContext，然后调用 registerSingleton 方法来动态的注册新的 bean，这样的情况下，如果我们还使用 Spring 测试框架的被修改过 applicationContext，则会带来测试问题，我们必须能够在运行期重新加载 applicationContext，这个时候，我们可以在测试类或者方法上注释：@DirtiesContext，作用如下：

* 如果定义在类上（缺省），则在此测试类运行完成后，重新加载 applicationContext
* 如果定义在方法上，即表示测试方法运行完成后，重新加载 applicationContext

### @TransactionConfiguration 和 @Rollback
缺省情况下，Spring 测试框架将事务管理委托到名为 transactionManager 的 bean 上，如果您的事务管理器不是这个名字，那需要指定 transactionManager 属性名称，还可以指定 defaultRollback 属性，缺省为 true，即所有的方法都 rollback，您可以指定为 false，这样，在一些需要 rollback 的方法，指定注释标签 @Rollback（true）即可。

###对 Junit4 的注释标签支持

看了上面 Spring 测试框架的注释标签，我们来看看一些常见的基于 Junit4 的注释标签在 Spring 测试环境中的使用方法。

### @Test(expected=...)
此注释标签的含义是，这是一个测试，期待一个异常的发生，期待的异常通过 xxx.class 标识。例如，我们修改 AccountService.Java 的 insertIfNotExist 方法，对于传入的参数如果为空，则抛出 IllegalArgumentException，如下：
```
 public void insertIfNotExist(Account account) { 
         if(account==null) 
                 throw new IllegalArgumentException("account is null"); 
         Account acct = accountDao.getAccountById(account.getId()); 
         if(acct==null) { 
                 log.debug("No "+account+" found,would insert it."); 
                 accountDao.saveAccount(account); 
         } 
         acct = null; 
 }
```

然后，在测试类中增加一个测试异常的方法，如下：

```
 @Test(expected=IllegalArgumentException.class) 
 public void testInsertException() { 
         service.insertIfNotExist(null); 
 }
```
运行结果是 green bar。
### @Test(timeout=...)
可以给测试方法指定超时时间（毫秒级别），当测试方法的执行时间超过此值，则失败。
比如在 AccountService 中增加如下方法：
```
 public void doSomeHugeJob() { 
         try { 
                 Thread.sleep(2*1000); 
         } catch (InterruptedException e) { 
         } 
 }
```
上述方法模拟任务执行时间 2 秒，则测试方法如下：
```
 @Test(timeout=3000) 
 public void testHugeJob() { 
         service.doSomeHugeJob(); 
 }
```
上述测试方法期待 service.doSomeHugeJob 方法能在 3 秒内结束，执行测试结果是 green bar。
### @Repeat
通过 @Repeat，您可以轻松的多次执行测试用例，而不用自己写 for 循环，使用方法：<br>
```
 @Repeat(3) 
 @Test(expected=IllegalArgumentException.class) 
 public void testInsertException() { 
         service.insertIfNotExist(null); 
 }
```
这样，testInsertException 就能被执行 3 次
