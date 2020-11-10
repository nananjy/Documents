# SpringBoot

### springboot注解设置事务回滚数据操作
- @Transactional(rollbackFor = Exception.class) 当你的方法中抛出异常时，它将会事务回滚，数据库中的数据将不会改变，回到进入此方法前的状态
- 参考：https://blog.csdn.net/ginkgo_leaf/article/details/38352881

