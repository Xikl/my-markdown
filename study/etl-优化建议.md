# 线程池的问题
    应该添加线程池，特别是在ScheduleTask中preShutdownExecution的部分
# 实体类中
    有的属性的默认值，应该又枚举对象获得，特别是还有枚举对象的情况，居然还能够不用，直接="sth"的形式 @see ProcessRunEntity中的status字段
- 如:
    ```
    private String status = "W";
    ```

# 注释啊 ，每个方法上就不能有一点注释么 .,就算那个方法再简单，也不要没有注释吧
