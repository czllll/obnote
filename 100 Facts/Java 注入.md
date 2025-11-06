
| domain  | layer | type  |
| ------- | ----- | ----- |
| #javase | #     | #fact |



### autowired和resource区别 ？
* 来源
	* Spring
	* JSR-250
* 注入类型
	* autowired默认byType注入，如果同一个类型有多个实现，需要配合 `@Qualifier("beanName")` 指定，如果不指定会抛异常；
	* resource默认byName注入，找不到与字段同名的Bean，会byType注入，也可指定name或者Type
* 支持的注入方式
	* Autowired:构造器，Setter，字段
	* Resource：Setter，字段
* 导致行为差异的原因？
	* 所依赖的 **注解处理机制（解析器）不同**
	* Autowired 依赖 spring框架下的 `AutowiredAnnotationBeanPostProcessor`
	* Resource 依赖 spring 框架下的`CommonAnnotationBeanPostProcessor` 用来支持 JSR-250 标准注解（如 `@Resource`, `@PostConstruct`, `@PreDestroy`）的注入与生命周期回调