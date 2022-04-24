# Spring Security 原理

## 简介:

安全框架Spring Security可以很好的和Spring Boot 进行整合,主要完成两个任务:认证和授权,认证即常说的登录,授权即为权限控制

##  原理:

### 过滤器:

过滤器为servlet中9大内置对象之一



![](D:\Note\Spring\Spring Security\filter.png)

web.xml配置

```xml
<!--过滤器的xml配置  -->
<filter>
	<!--名称-->
	<filter-name>sf</filter-name>
	<!--过滤器类全称-->
	<filter-class>com.qf.web.filter.SecondFilter</filter-class>
</filter>
<!--映射路径配置-->
<filter-mapping>
    <!--名称-->
    <filter-name>sf</filter-name>
    <!--过滤的url匹配规则和Servlet的一模一样-->
    <url-pattern>/*</url-pattern>
</filter-mapping>

```

filter代码

```java
@WebFilter("/*")
public class demoFilter implements Filter {

	//销毁--执行一次
	public void destroy() {
		// TODO Auto-generated method stub
	}
	//进行过滤时执行的方法
	public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
		// TODO Auto-generated method stub
		// 过滤前
	chain.doFilter(request, response);//过滤条件通过后，进入下一个过滤器（有过滤器）/到达目标请求
		//过滤后
	}
	//初始化--执行一次,servlet(tomcat容器)启动时启动
	public void init(FilterConfig fConfig) throws ServletException {
		// TODO Auto-generated method stub
	}

}
```

![](D:\Note\Spring\Spring Security\filter2.png)



### 过滤器链:

​	Spring Security 是基于过滤器链实现授权和认证等功能的,过滤器链可通过实现多个过滤器组成过滤器链;过滤器链执行的顺序为web.xml配置的顺序(从上往下)

在启动类通过debug卡住,并且如图点击Evaluate Expression(ALT + F8)按钮 中,从Spring 容器中获取 到SecurityFilterChain实例,注意不要添加SecurityConfig.class配置类等信息

可看出其中的过滤器链信息

![](D:\Note\Spring\Spring Security\security filters.png)



Spring Security Filter 顺序如下:

- ChannelProcessingFilter
- WebAsyncManagerIntegrationFilter
- SecurityContextPersistenceFilter
- HeaderWriterFilter
- CorsFilter
- CsrfFilter
- LogoutFilter
- OAuth2AuthorizationRequestRedirectFilter
- Saml2WebSsoAuthenticationRequestFilter
- X509AuthenticationFilter
- AbstractPreAuthenticatedProcessingFilter
- CasAuthenticationFilter
- OAuth2LoginAuthenticationFilter
- Saml2WebSsoAuthenticationFilter
- [`UsernamePasswordAuthenticationFilter`](https://docs.spring.io/spring-security/reference/servlet/authentication/passwords/form.html#servlet-authentication-usernamepasswordauthenticationfilter)
- OpenIDAuthenticationFilter
- DefaultLoginPageGeneratingFilter
- DefaultLogoutPageGeneratingFilter
- ConcurrentSessionFilter
- [`DigestAuthenticationFilter`](https://docs.spring.io/spring-security/reference/servlet/authentication/passwords/digest.html#servlet-authentication-digest)
- BearerTokenAuthenticationFilter
- [`BasicAuthenticationFilter`](https://docs.spring.io/spring-security/reference/servlet/authentication/passwords/basic.html#servlet-authentication-basic)
- RequestCacheAwareFilter
- SecurityContextHolderAwareRequestFilter
- JaasApiIntegrationFilter
- RememberMeAuthenticationFilter
- AnonymousAuthenticationFilter
- OAuth2AuthorizationCodeGrantFilter
- SessionManagementFilter
- [`ExceptionTranslationFilter`](https://docs.spring.io/spring-security/reference/servlet/architecture.html#servlet-exceptiontranslationfilter)
- [`FilterSecurityInterceptor`](https://docs.spring.io/spring-security/reference/servlet/authorization/authorize-requests.html#servlet-authorization-filtersecurityinterceptor)
- SwitchUserFilter



UsernamePasswordAuthenticationFilter负责账号密码接收和封装

