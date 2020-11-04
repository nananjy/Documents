# Interview

## 问题
1. 变量赋值。
> Spring注解赋值 @Value("${temp}") 与代码赋值 temp = 0 调用先后次序，最后获取到的值是? 注解值。
```
测试
文件 application.yml
    test: 2020
文件 AnnocationAndCode.java
    @RestController
    @RequestMapping("/test")
    public class AnnocationAndCode {
        @Value("${test}")
        private int temp = 0;
        @GetMapping("/test")
        private int testAnnocation() {
            System.out.println("temp=" + temp);
            return temp;
        }
    }
执行结果
temp=2020
```
2. 



