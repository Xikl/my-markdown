# 线程池的问题
    应该添加线程池，特别是在ScheduleTask中preShutdownExecution的部分
# 实体类中
    有的属性的默认值，应该又枚举对象获得，特别是还有枚举对象的情况，居然还能够不用，直接="sth"的形式 @see ProcessRunEntity中的status字段
- 如:
    ```
    private String status = "W";
    ```

# 注释啊 ，每个方法上就不能有一点注释么 .,就算那个方法再简单，也不要没有注释吧
####表设计
- 表名:mail_info
- 表字段： 
    + mail_id  邮件id 是否需要自增 还是 需要一个独立生成的方法 来生成唯一的id  如时间+随机数等（可能会有并发）
    + mail_status 邮件状态 0 代表未发送， 1 发送成功 2 发送失败 3 发送中
    + retry_count 发送次数 最大两次 
    + mail_title 邮件标题  
    + mail_content 邮见内容  
    + attachment_files  附件
    + create_time 创建时间
    + modified_time 修改时间