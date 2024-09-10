# MyBatis Plus分页查询

### 1. 引入MyBatis- plus依赖
### 2. 配置分页插件

在你的配置类中添加分页插件。MyBatis-Plus 提供了一个分页拦截器 `PaginationInterceptor`，你需要将它配置为一个 Bean。

``` JAVA
@Configuration
public class MybatisPlusConfig {
    @Bean
    public PaginationInterceptor paginationInterceptor() {
        return new PaginationInterceptor();
    }
}

```

### 3. 使用分页

在你的Mapper接口中，你可以直接使用 MyBatis-Plus 提供的 `selectPage` 方法进行分页查询。通常，你需要传入一个 `Page` 对象作为查询参数，`Page` 对象包含了分页需要的信息如当前页码和每页显示的数量。

``` JAVA
public interface UserMapper extends BaseMapper&lt;User&gt; {
    // 其他CRUD操作
}

@Service
public class UserService {
    @Autowired
    private UserMapper userMapper;
    
    public IPage&lt;User&gt; selectUserPage(Page&lt;User&gt; page, Integer state) {
        return userMapper.selectPage(page, new QueryWrapper&lt;User&gt;().eq(&#34;state&#34;,state));
    }
}

```

在调用时，你只需要创建一个 `Page` 对象，设置好相应的页码和每页数量，然后将它作为参数传递给方法即可。

### 4. 处理结果

`selectPage` 方法返回的 `IPage` 对象包含了分页查询的结果，你可以从中获取到查询到的记录以及分页的详细信息，例如总页数、总记录数等。

---

> Author: Deequoique  
> URL: http://localhost:1313/mybatis-plus%E5%88%86%E9%A1%B5%E6%9F%A5%E8%AF%A2/  

