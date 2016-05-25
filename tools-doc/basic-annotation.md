# 基础注解

### Bean 声明注解
* @Service 业务逻辑层
* @Component 组件
* @Repository 数据访问层
* @Controller Spring mvc 展现层
* @Configurable 声明当前类是一个配置类！！！
* @ComponentScan("com.reachauto.cxn.book.test")

        设置自动扫描包下面所有的
        @Service @Component @Repository @Controller

* @EnableAsync
        开启异步任务支持

* @PropertySource("classpath:demo.properties")
```java
@Component
@PropertySource("classpath:demo.properties")
public class Demo {

    @Value("${kk.name}")
    private String aaa;
```

* @EnableScheduling 注解开启对计划任务的支持
* 


### Bean 注入注解
* @Autowired Spring 提供
* @Resource JSR-250
* @Value("xxxx") 注入普通字符串
* @Value("${xxx.xxx}") 注入配置文件中字符串
* @PostConstruct 标注在方法上，在构造函数执行完毕后执行
* @PreDestroy Bean 标注在方法上，销毁前执行
* @Async 异步方法表明，若是在class上则全是
* @Scheduled 声明方法是计划任务
* @Conditional() 条件注解，当满足某条件时
 


### Spring MCV
* @RequestMapping

        用于映射Web请求 返回体，编码格式都可以在此处设置
        produces = {} 设置返回值json/xml charset 等
* @RestController

        这是个组合注解，组合了@Controller和@ResponseBody

* @ResponseBody

        支持返回体放入response体中，而不是直接返回一个页面，
        此注解可以放在返回值或者方法体上
* @RequestBody

        允许参数在request体里，而不是在地址栏后面
* @PathVariable

        用来接收路径参数，api/{id}
*
* 
*  




