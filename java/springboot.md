##### 跨域问题
```java
@Configuration
public class CORSConfiguration extends WebMvcConfigurationSupport {
    @Override
    protected void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**")
                .allowedMethods("*")
                .allowedOrigins("*")
                .allowedHeaders("*")
                .allowCredentials(true);
        super.addCorsMappings(registry);
    }
}
```
