# spring security和shiro的异同

##相同点

1. 认证功能
2. 授权功能
3. 加密功能
4. 会话管理
5. 缓存支持
6. rememberMe功能

##不同点

1. Spring Security 基于Spring 开发，项目若使用 Spring 作为基础，配合 Spring Security 做权限更加方便，而 Shiro 需要和 Spring 进行整合开发；
2. Spring Security 功能比 Shiro 更加丰富些，例如安全维护方面；
3. Spring Security 社区资源相对比 Shiro 更加丰富；  
Spring Security对Oauth、OpenID也有支持,Shiro则需要自己手动实现。而且Spring Security的权限细粒度更高。
spring security 接口 RequestMatcher 用于匹配路径,对路径做特殊的请求，类似于shiro的
抽象类 PathMatchingFilter，但是 RequestMatcher 作用粒度更细
 
4. Shiro 的配置和使用比较简单，Spring Security 上手复杂些；
5. Shiro 依赖性低，不需要任何框架和容器，可以独立运行.Spring Security 依赖Spring容器；
6. shiro 不仅仅可以使用在web中，还支持非web项目它可以工作在任何应用环境中。在集群会话时Shiro最重要的一个好处或许就是它的会话是独立于容器的。

&emsp;&emsp;apache shiro的话，简单，易用，功能也强大，spring官网就是用的shiro，可见shiro的强大。

 

&emsp;&emsp;spring security 和 shiro 对加密都提供了各种各样的支持 例如 BCryptPasswordEncoder 采用 SHA-256 + 随机盐 + 秘钥 对密码进行加密。shrio 的 SimpleHash 提供散列算法的支持，生成数据的摘要信息.  
&emsp;&emsp;shiro 的 AuthorizingRealm 的 doGetAuthorizationInfo方法 与 doGetAuthenticationInfo 一个 是定义 获取 用户权限信息 的方法，一 个 是 定义用户身份认证及获取用户身份的方法，而 spring security 也有 资源 角色 授权器 FilterInvocationSecurityMetadataSource，定义资源url 与 角色权限的关系 ， 决策 管理器 AccessDecisionManager 定义权限满足的规则。  

**注**：
OAuth在”客户端”与”服务提供商”之间，设置了一个授权层（authorization layer）。”客户端”不能直接登录”服务提供商”，只能登录授权层，以此将用户与客户端区分开来。”客户端”登录授权层所用的令牌（token），与用户的密码不同。用户可以在登录的时候，指定授权层令牌的权限范围和有效期。  
“客户端”登录授权层以后，”服务提供商”根据令牌的权限范围和有效期，向”客户端”开放用户储存的资料。  
OpenID 系统的第一部分是身份验证，即如何通过 URI 来认证用户身份。目前的网站都是依靠用户名和密码来登录认证，这就意味着大家在每个网站都需要注册用户名和密码，即便你使用的是同样的密码。如果使用 OpenID ，你的网站地址（URI）就是你的用户名，而你的密码安全的存储在一个 OpenID 服务网站上（你可以自己建立一个 OpenID 服务网站，也可以选择一个可信任的 OpenID 服务网站来完成注册）。  
与OpenID同属性的身份识别服务商还有ⅥeID,ClaimID,CardSpace,Rapleaf,Trufina ID Card等，其中ⅥeID通用账户的应用最为广泛。  