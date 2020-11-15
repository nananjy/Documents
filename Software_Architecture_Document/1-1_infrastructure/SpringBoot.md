# SpringBoot

### springboot注解设置事务回滚数据操作
- @Transactional(rollbackFor = Exception.class) 当你的方法中抛出异常时，它将会事务回滚，数据库中的数据将不会改变，回到进入此方法前的状态
- 参考：https://blog.csdn.net/ginkgo_leaf/article/details/38352881

### Could not autowire. there is more than one bean of '' type
- 接口A有两个以上实现类，不能使用@Autowired注解，要加Qulifier注解，或类前加Primary
><br/>如@Qulifier(name="xxx")，或者直接替换为Resource(name="xxx")
- 可能是引入类有含参构造，没有无参构造，再加一个无参构造函数
```
	private int id;
    private String name;
    private double price;

    public JavaCourse() {
    }

    public JavaCourse(int id, String name, double price) {
        this.id = id;
        this.name = name;
        this.price = price;
    }
```
- 实现类加了@Service和@Component两个注解，把@Component去掉
><br/>@service引用了@component注解

