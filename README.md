# JSONP Filter

A J2EE Servlet filter to wrap JSON responses in a JSONP callback.

## Getting Started

Include the `com.earldouglas.jsonp-filter` dependency in your _pom.xml_:

```xml
<dependencies>
  <dependency>
    <groupId>com.earldouglas</groupId>
    <artifactId>jsonp-filter</artifactId>
    <version>1.0.0</version>
  </dependency>
</dependencies>
```

## Usage

### web.xml

To use the filter with a _web.xml_ file, add the `<filter>` and the `<filter-mapping>`. For example:

```xml
<filter>
  <filter-name>jsonp-filter</filter-name>
  <filter-class>com.earldouglas.jsonpfilter.JsonPFilter</filter-class>
</filter>
<filter-mapping>
  <filter-name>jsonp-filter</filter-name>
  <url-pattern>/foo</url-pattern>
</filter-mapping>
```

### Spring Boot

To use the filter in a Spring Boot application, add a `FilterRegistrationBean` to your root Spring class. For example:

```java
public class ExampleSpringApplication {
    @Bean
    public FilterRegistrationBean<JsonPFilter> jsonPFilter() {
        FilterRegistrationBean<JsonPFilter> jsonPFilter = new FilterRegistrationBean<>();
        jsonPFilter.setFilter(new JsonPFilter());
        jsonPFilter.addUrlPatterns("/foo");

        return jsonPFilter;
    }
}
```

Now any requests to */foo* will have their responses wrapped in *callback(...)*.

## Callback Parameter

The JSONP callback parameter defaults to "callback" (e.g. http://localhost:8080/foo?callback=bar). Override the `callbackParam` filter parameter to override the default.

### web.xml

To override the _callback_ parameter in a _web.xml_ file:

```xml
<filter>
  ...
  <init-param>
    <param-name>callbackParam</param-name>
    <param-value>calleybackey</param-value>
  </init-param>
</filter>
```

**Full Example**

```xml
<filter>
  <filter-name>jsonp-filter</filter-name>
  <filter-class>com.earldouglas.jsonpfilter.JsonPFilter</filter-class>
  <init-param>
    <param-name>callbackParam</param-name>
    <param-value>calleybackey</param-value>
  </init-param>
</filter>
<filter-mapping>
  <filter-name>jsonp-filter</filter-name>
  <url-pattern>/foo</url-pattern>
</filter-mapping>
```

### Spring Boot

To override the _callback_ parameter in a Spring Boot application:

```java
Map<String, String> parameters = new HashMap<>();
parameters.put("callbackParam", "calleybackey");
jsonPFilter.setInitParameters(parameters);
```

**Full Example**

```java
public class ExampleSpringApplication {
    @Bean
    public FilterRegistrationBean<JsonPFilter> jsonPFilter() {
        FilterRegistrationBean<JsonPFilter> jsonPFilter = new FilterRegistrationBean<>();

        jsonPFilter.setFilter(new JsonPFilter());
        jsonPFilter.addUrlPatterns("/*");

        Map<String, String> parameters = new HashMap<>();
        parameters.put("callbackParam", "calleybackey");
        jsonPFilter.setInitParameters(parameters);

        return jsonPFilter;
    }
}
```

Now any requests to */foo* will have their responses wrapped in calleybackey(...).