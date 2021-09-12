# Spring



# Spring 常用注解

### 配置相关

@Configuration

@ImportResource

@ComponentScan

@Bean

@ConfigurationProperties

### 定义相关

@Component、@Repository、@Service

@Controller、@RestController

@RequestMapping

### 注入相关

@Autowired、@Qualifier、@Resource

@Value

### JPA相关

**实体**

@Entity、@MappedSuperclass

@Table(name)

**主键**

@Id

@GeneratedValue(strategy, generator)

@SequenceGenerator(name, sequenceName)

**映射**

@Column(name, nullable, length, insertable, updatable)

@JoinTable(name)、@JoinColumn(name)

**关系**

@OneToOne、@OneToMany、@ManyToOne、@ManyToMany

@OrderBy

**MyBatis相关**

注解相关

@EnableCaching

@Cacheable

@CacheEvict

@CachePut

@CachePut

@Caching

@CacheConfig

**WEB**

@RequestBody、@ResponseBody、@ResponseStatus
@PathVariable、@RequestParam、@RequestHeader
HttpEntity / ResponseEntity

# Actuator

/actuator/**Endpoint**

| Endpoint URL | 作用     |
| ------------ | -------- |
| /health      | 健康检查 |
| /beans       | bean     |
| /mapping     | URL      |
| /env         | 环境     |

application.yml

```yaml
management.endpoints.web.exposure.include=[*|health,beans]
```

# Spring事务

Spring 的声明式事务本质上是通过 AOP 增强了类的功能。

AOP 的本质是为类做了代理



# JPA

- [ ] 简单使用CRUD
- [ ] 表间关系
- [ ] 接口中的方法是如何被解释的