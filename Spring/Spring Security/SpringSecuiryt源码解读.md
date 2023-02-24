SpringSecurity是的本质过滤链

- 账号密码封存在哪,除此之外还封存了哪些信息
- 过滤器怎么管理
- 认证授权逻辑在哪
- 如何自定义过滤器,可以自定义哪些过滤器
- 

账号密码过滤器:UsernamePasswordAuthenticationFilter,核心方法如下



```java

@Override	
public Authentication attemptAuthentication(HttpServletRequest request, HttpServletResponse response)
			throws AuthenticationException {
    	//先判断请求方法,只接受post方法
		if (this.postOnly && !request.getMethod().equals("POST")) {
			throw new AuthenticationServiceException("Authentication method not supported: " + request.getMethod());
		}
    	//获取username
		String username = obtainUsername(request);
		username = (username != null) ? username : "";
		username = username.trim();
		String password = obtainPassword(request);
		password = (password != null) ? password : "";
    	//将用户名密码封装进 UsernamePasswordAuthenticationToken
		UsernamePasswordAuthenticationToken authRequest = new UsernamePasswordAuthenticationToken(username, password);
		// Allow subclasses to set the "details" property
		setDetails(request, authRequest);
		return this.getAuthenticationManager().authenticate(authRequest);
}
```

