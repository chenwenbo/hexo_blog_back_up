title: quarz使用指南
date: 2016-01-19 10:08:06
tags: JAVA
categories: 编程
---


##环境配置（spring集成）

 - 依赖jar包

    **spring-webmvc.jar
    quartz.jar**

 - 配置 
在spring的dtd声明文件中加入：
xmlns:task=http://www.springframework.org/schema/task
http://www.springframework.org/schema/task 
http://www.springframework.org/schema/task/spring-task-3.1.xsd

在spring配置文件中加入 `<task:annotation-driven/>` 驱动配置

 - 示例代码

    public class DelayDataHandleTask extends QuartzJobBean {
    
    /* 业务实现 */
    //@Scheduled(cron = "0 0/3 *  * * ? ") // 每3分钟执行一次
    @Scheduled(cron="0 0 0/3 * * ? ") //每3小时执行一次
    public void work() {
    	try {
    		taskService.handleDelayData();
    	} catch (Exception e) {
    		logger.error(e.getMessage(), e);
    	}
    }




##时间表达式配置
 - 配置说明
![](http://ww1.sinaimg.cn/large/8a0ce11egw1f04l671ov3j20em0ae761.jpg)
 - 配置示例
"0 0 12 * * ?" 每天中午12点触发
"0 15 10 ? * *" 每天上午10:15触发
"0 15 10 * * ?" 每天上午10:15触发
"0 15 10 * * ? *" 每天上午10:15触发
"0 15 10 * * ? 2005" 2005年的每天上午10:15触发
"0 * 14 * * ?" 在每天下午2点到下午2:59期间的每1分钟触发
"0 0/5 14 * * ?" 在每天下午2点到下午2:55期间的每5分钟触发
"0 0/5 14,18 * * ?" 在每天下午2点到2:55期间和下午6点到6:55期间的每5分钟触发
"0 0-5 14 * * ?" 在每天下午2点到下午2:05期间的每1分钟触发
"0 10,44 14 ? 3 WED" 每年三月的星期三的下午2:10和2:44触发
"0 15 10 ? * MON-FRI" 周一至周五的上午10:15触发
"0 15 10 15 * ?" 每月15日上午10:15触发
"0 15 10 L * ?" 每月最后一日的上午10:15触发
"0 15 10 ? * 6L" 每月的最后一个星期五上午10:15触发
"0 15 10 ? * 6L 2002-2005" 2002年至2005年的每月的最后一个星期五上午10:15触发
"0 15 10 ? * 6#3" 每月的第三个星期五上午10:15触发
