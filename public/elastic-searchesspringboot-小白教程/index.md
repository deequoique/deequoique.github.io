# Elastic Search（ES）Springboot 小白教程

Elasticsearch 是一个开源的、高度可扩展的、全文搜索和分析引擎，它能够快速、近实时地存储、搜索和分析大量数据。Elasticsearch 常用于日志和事件数据分析、全文搜索、安全智能分析以及业务智能等场景。
&lt;!--more--&gt;
## 1. 添加依赖
maven:
``` xml
&lt;dependencies&gt;
    &lt;!-- Spring Data Elasticsearch --&gt;
    &lt;dependency&gt;
        &lt;groupId&gt;org.springframework.boot&lt;/groupId&gt;
        &lt;artifactId&gt;spring-boot-starter-data-elasticsearch&lt;/artifactId&gt;
    &lt;/dependency&gt;
    &lt;!-- Other dependencies --&gt;
&lt;/dependencies&gt;
```

## 2. 配置连接信息
在`application.properties`或`application.yml`中配置Elasticsearch的连接信息。
``` properties
# application.properties
spring.elasticsearch.rest.uris=http://localhost:9200
spring.elasticsearch.rest.username=user # 如果需要的话
spring.elasticsearch.rest.password=pass # 如果需要的话
```

## 3. 定义实体类
创建一个实体类，使用`@Document`注解标记它，以表示它是一个Elasticsearch文档。
这里文档可以理解成和MySQL中entity一样的东西。
例如
``` java
import org.springframework.data.annotation.Id;
import org.springframework.data.elasticsearch.annotations.Document;

@Document(indexName = &#34;blog&#34;, type = &#34;article&#34;)
public class ESIdeaPO {
    @Id
    private String address;
    private String title;
    private String content;
    private String type;
}

```

## 4. 创建Repository接口
创建一个继承`ElasticsearchRepository`的接口，Spring Data Elasticsearch将自动实现基本的CRUD操作。
就像Mybatis-plus的`IService`一样
``` java
import org.springframework.data.elasticsearch.repository.ElasticsearchRepository;

public interface ArticleRepository extends ElasticsearchRepository&lt;ESIdeaPO, String&gt; {
    // 可以添加自定义的查询方法
}

```
比较神奇的点是在，定义一个方法后不需要实现它，只要按特定格式写就可以。
比如方法名叫做：`findByTitle`，那么它就知道你是根据title查询，然后自动帮你完成，无需写实现类。
## 5. 编写服务层
可以使用这个`Respository`编写需要的crud了
```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class ArticleService {

    @Autowired
    private ArticleRepository articleRepository;

    public void save(Article article) {
        articleRepository.save(article);
    }

    public Iterable&lt;Article&gt; findByTitle(String title) {
        return articleRepository.findByTitle(title);
    }

    // 其他业务方法
}

```
# 总结
在`SpringBoot`的加持下，使用感和各种数据库大差不差。初步了解像是那种可以专门用来搜索，且不用担心效率和索引的数据库。

---

> Author: Deequoique  
> URL: http://localhost:1313/elastic-searchesspringboot-%E5%B0%8F%E7%99%BD%E6%95%99%E7%A8%8B/  

