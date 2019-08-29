# 前后端分离自定义返回json-filter
## 自定义校验工具类(非必须)
```java

import java.util.Collection;
import java.util.Map;
/**
 * 数据校验工具类
 *
 * @author: chm
 * @create: 2019-08-20
 * @verison: 1.0
 * @description: 数据校验工具类
 */
public class CheckUtils {

    public static boolean isEmpty(Object o) {
        if (o == null) {
            return true;
        } else {
            if (o instanceof String && o.toString().trim().equals("")) {        //字符串判断
                return true;
            } else if (o instanceof Iterable && ((Collection) o).size() <= 0) { //判断set和list
                return true;
            } else if (o instanceof Map && ((Map) o).size() == 0) {             //map判断
                return true;
            } else if (o instanceof Object[] && ((Object[]) ((Object[]) o)).length == 0) {//object判断
                return true;
            } else if (isEmptyBasicDataArray(o)) {              //基础数据类型数组判断
                return true;
            }
            return false;
        }
    }
    /**
     * 判断传入所有可变参数是否有一个为空
     * @param o
     * @return
     */
    public static boolean isOneEmpty(Object ... o){
        for (Object obj :o ) {
            if(isEmpty(obj)){//有一个为空，符合条件
                return true;
            }
        }
        return false;
    }

    /**
     * 判断传入所有可变参数是否都为空
     * @param o
     * @return
     */
    public static boolean isAllEmpty(Object ... o){
        for (Object obj :o ) {
            if(!isEmpty(obj)){//有一个不为空，则不符合条件
                return false;
            }
        }
        return true;
    }

    /**
     * 判断传入可变参数是否有一个参数不为空
     * @param o
     * @return
     */
    public static boolean isOneNotEmpty(Object ... o){
        for (Object obj :o ) {
            if(!isEmpty(obj)){//有一个不为空，符合条件
                return true;
            }
        }
        return false;
    }

    /**
     * 判断传入所有可变参数是否都非空
     * @param o
     * @return
     */
    public static boolean isAllNotEmpty(Object ... o){
        for (Object obj :o ) {
            if(isEmpty(obj)){//有一个为空，则不符合条件
                return false;
            }
        }
        return true;
    }
    /**
     * 判断基础数据类型是否非空
     * @param o
     * @return
     */
    public static boolean isNotEmpty(Object o ){
        return !isEmpty(o);
    }

    /**
     * 校验基础数据类型数组是否为空
     *
     * @param o
     * @return
     */
    public static boolean isEmptyBasicDataArray(Object o) {
        if (o == null) {
            return true;
        } else {
            if (o instanceof int[] && ((int[]) ((int[]) o)).length == 0) {
                return true;
            } else if (o instanceof long[] && ((long[]) ((long[]) o)).length == 0) {
                return true;
            } else if (o instanceof byte[] && ((byte[]) ((byte[]) o)).length == 0) {
                return true;
            } else if (o instanceof char[] && ((char[]) ((char[]) o)).length == 0) {
                return true;
            } else if (o instanceof double[] && ((double[]) ((double[]) o)).length == 0) {
                return true;
            } else if (o instanceof float[] && ((float[]) ((float[]) o)).length == 0) {
                return true;
            } else if (o instanceof short[] && ((short[]) ((short[]) o)).length == 0) {
                return true;
            } else if (o instanceof boolean[] && ((boolean[]) ((boolean[]) o)).length == 0) {
                return true;
            }
        }
        return false;
    }


    /**
     * 判断基础数据类型数组是否非空
     * @param o
     * @return
     */
    public static boolean isNotEmptyBasicDataArray(Object o){
        return !isEmptyBasicDataArray(o);
    }

    /**
     * 判断传入可变数组是否有一个为空
     * @param o
     * @return
     */
    public static boolean isOneEmptyBasicDataArray(Object ... o){
        for (Object obj :o ) {
            if(isEmptyBasicDataArray(obj)){//有一个符合条件即可
                return true;
            }
        }
        return false;
    }
    /**
     * 判断传入可变数组是否有一个非空
     * @param o
     * @return
     */
    public static boolean isOneNotEmptyBasicDataArray(Object ... o){
        for (Object obj :o ) {
            if(!isEmptyBasicDataArray(obj)){//有一个符合条件即可
                return true;
            }
        }
        return false;
    }

    /**
     * 判断传入可变数组是否全部为空
     * @param o
     * @return
     */
    public static boolean isAllEmptyBasicDataArray(Object ... o){
        for (Object obj :o ) {
            if(!isEmptyBasicDataArray(obj)){//有一个不为空，则不符合条件
                return false;
            }
        }
        return true;
    }
    /**
     * 判断传入可变数组是否全部为空
     * @param o
     * @return
     */
    public static boolean isAllNotEmptyBasicDataArray(Object ... o){
        for (Object obj :o ) {
            if(isEmptyBasicDataArray(obj)){//有一个不为空，则不符合条件
                return false;
            }
        }
        return true;
    }
}

```
## 自定义错误码枚举
```java

/**
 * 错误码枚举
 *
 * @author: chm
 * @create: 2019-08-20
 * @verison: 1.0
 * @description: 错误码枚举
 */
public enum ErrorCodeEnum {

    /**
     * 字典
     */
    DICT_EXISTED(400, "字典已经存在"),
    ERROR_CREATE_DICT(500, "创建字典失败"),
    ERROR_WRAPPER_FIELD(500, "包装字典属性失败"),
    ERROR_CODE_EMPTY(500, "字典类型不能为空"),

    /**
     * 文件上传
     */
    FILE_READING_ERROR(400, "FILE_READING_ERROR!"),
    FILE_NOT_FOUND(400, "FILE_NOT_FOUND!"),
    UPLOAD_ERROR(500, "上传图片出错"),

    /**
     * 权限和数据问题
     */
    DB_RESOURCE_NULL(400, "数据库中没有该资源"),
    NO_PERMITION_AUTHORITY(403, "无访问权限"),
    NO_PERMITION(405, "登录认证异常"),
//    NO_PERMITION(405, "权限异常"),
    REQUEST_INVALIDATE(400, "请求数据格式不正确"),
    INVALID_KAPTCHA(400, "验证码不正确"),
    CANT_DELETE_ADMIN(600, "不能删除超级管理员"),
    CANT_FREEZE_ADMIN(600, "不能冻结超级管理员"),
    CANT_CHANGE_ADMIN(600, "不能修改超级管理员角色"),

    /**
     * 账户问题
     */
    NOT_LOGIN(401, "当前用户未登录"),
    USER_ALREADY_REG(401, "该用户已经注册"),
    NO_THIS_USER(400, "没有此用户"),
    USER_NOT_EXISTED(400, "没有此用户"),
//    ACCOUNT_FREEZED(401, "账号被冻结"),
    OLD_PWD_NOT_RIGHT(402, "原密码不正确"),
    TWO_PWD_NOT_MATCH(405, "两次输入密码不一致"),

    /**
     * 错误的请求
     */
    MENU_PCODE_COINCIDENCE(400, "菜单编号和副编号不能一致"),
    EXISTED_THE_MENU(400, "菜单编号重复，不能添加"),
    DICT_MUST_BE_NUMBER(400, "字典的值必须为数字"),
    REQUEST_NULL(400, "请求有错误"),
    SESSION_TIMEOUT(400, "会话超时"),
    SERVER_ERROR(500, "服务器异常"),

    /**
     * token异常
     */
    TOKEN_EXPIRED(700, "token过期"),
    TOKEN_ERROR(700, "token验证失败"),

    /**
     * 签名异常
     */
    SIGN_ERROR(700, "签名验证失败"),

    /**
     * 其他
     */
    AUTH_REQUEST_ERROR(400, "账号密码错误");

    ErrorCodeEnum(int code, String message) {
        this.code = code;
        this.message = message;
    }

    private Integer code;

    private String message;

    public Integer getCode() {
        return code;
    }

    public void setCode(Integer code) {
        this.code = code;
    }

    public String getMessage() {
        return message;
    }

    public void setMessage(String message) {
        this.message = message;
    }
}

```
## 自定义返回类
```java

import lombok.Data;

import java.io.Serializable;
import java.util.HashMap;

/**
 * 响应基类
 * @package: com.yinghu.framework.exception
 * @author: chm
 * @create: 2019-08-15
 * @verison: 1.0
 * @description: 响应基类
 */

@Data
public class ResponseBo extends HashMap implements Serializable {
    private static final long serialVersionUID = -8713837118340960775L;

    private static final boolean SUCCESS = true;
    private static final boolean FAIL = false;

    public static final String MESSAGE_VALUE = "message";   //string
    public static final String DATA_VALUE = "data";      //object

    public static final String CODE_VALUE = "code";         //int
    public static final String SUCCESS_VALUE = "success";  //boolean

    public ResponseBo() {
        this.put(SUCCESS_VALUE,SUCCESS);
    }

    public ResponseBo(boolean success){
        this.put(SUCCESS_VALUE,success);
    }

    public static ResponseBo ok(){
        return new ResponseBo();
    }

    public static ResponseBo ok(String message){
        ResponseBo responseBo = new ResponseBo();
        responseBo.put(MESSAGE_VALUE,message);
        return responseBo;
    }
    public static ResponseBo error(){
        return new ResponseBo(FAIL);
    }

    public static ResponseBo error(String message){
        ResponseBo responseBo = new ResponseBo(FAIL);
        responseBo.put(MESSAGE_VALUE,message);
        return responseBo;
    }

    public static ResponseBo error(ErrorCodeEnum e){
        ResponseBo responseBo = new ResponseBo(FAIL);
        responseBo.put(CODE_VALUE,e.getCode());
        responseBo.put(MESSAGE_VALUE,e.getMessage());
        return responseBo;
    }

    public ResponseBo(Integer code, Boolean success, String message, Object data) {
        this.put(CODE_VALUE, code);
        this.put(SUCCESS_VALUE, success);
        this.put(MESSAGE_VALUE, message);
        this.put(DATA_VALUE, data);
    }

    public Integer getCode() {
        return (Integer) get(CODE_VALUE);
    }

    public void setCode(Integer code) {
        this.put(CODE_VALUE, code);
    }

    public Boolean getSuccess() {
        return (boolean) get(SUCCESS_VALUE);
    }

    public void setSuccess(Boolean success) {
        this.put(SUCCESS_VALUE, success);
    }

    public ResponseBo put(String message) {
        this.put(MESSAGE_VALUE, message);
        return this;
    }

    public ResponseBo putData(Object data) {
        this.put(DATA_VALUE, data);
        return this;
    }

    public String getMessage() {
        return (String) this.get(MESSAGE_VALUE);
    }

    /**
     * 获取字符串
     *
     * @param key
     * @return
     */
    public String getString(String key) {
        Object str = this.get(key);
        return str != null ? (String) str : "";
    }
}
```
## 自定义返回数据类
```java

import com.alibaba.fastjson.JSON;

import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

public class WebUtilsPro {

    /**
     * 是否是Ajax请求
     *
     * @param request
     * @return
     * @author SHANHY
     * @create 2017年4月4日
     */
    public static boolean isAjaxRequest(HttpServletRequest request) {
        String requestedWith = request.getHeader("x-requested-with");
        if (requestedWith != null && requestedWith.equalsIgnoreCase("XMLHttpRequest")) {
            return true;
        } else {
            return false;
        }
    }

    public static void writer(Object obj , ServletRequest request,ServletResponse response){
        HttpServletRequest req = (HttpServletRequest)request;
        HttpServletResponse resp = (HttpServletResponse) response;

        //前端Ajax请求，则不会重定向
        resp.setHeader("Access-Control-Allow-Origin",  req.getHeader("Origin"));
        resp.setHeader("Access-Control-Allow-Credentials", "true");
        resp.setContentType("application/json;charset=UTF-8");
        try {
            resp.getWriter().write(JSON.toJSONString(obj));
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    public static  void out(HttpServletResponse response, Object obj){
        PrintWriter out = null;
        try {
            response.setCharacterEncoding("UTF-8");
            response.setContentType("application/json; charset=utf-8");
            out = response.getWriter();
            out.write(JSON.toJSONString(obj));
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (out != null) {
                out.close();
            }
        }
    }
}
```

## 定义未登录filter
```java

import org.apache.shiro.web.filter.authc.FormAuthenticationFilter;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;

/**
 * shiro无权限过滤器
 *
 * @package: com.yinghu.platform.filter
 * @author: chm
 * @create: 2019-08-20
 * @verison: 1.0
 * @description: shiro未登录过滤器
 */
public class SystemUnauthorizedFilter extends FormAuthenticationFilter {

    private static final Logger log = LoggerFactory.getLogger(SystemUnauthorizedFilter.class);
    @Override
    protected boolean onAccessDenied(ServletRequest request, ServletResponse response) throws Exception {
//        return super.onAccessDenied(request, response);
        if (isLoginRequest(request, response)) {
            if (isLoginSubmission(request, response)) {
                if (log.isTraceEnabled()) {
                    log.trace("Login submission detected.  Attempting to execute login.");
                }
                return executeLogin(request, response);
            } else {
                if (log.isTraceEnabled()) {
                    log.trace("Login page view.");
                }
                WebUtilsPro.writer(ResponseBo.error(ErrorCodeEnum.NOT_LOGIN),request,response);
                return false;
            }
        } else {
            WebUtilsPro.writer(ResponseBo.error(ErrorCodeEnum.NO_PERMITION_AUTHORITY),request,response);
            return false;
        }
    }
}

```

## 定义无权限filter
```java

import org.apache.shiro.subject.Subject;
import org.apache.shiro.util.StringUtils;
import org.apache.shiro.web.filter.AccessControlFilter;
import org.apache.shiro.web.filter.authz.AuthorizationFilter;
import org.apache.shiro.web.util.WebUtils;

import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.util.regex.Matcher;
import java.util.regex.Pattern;

/**
 * shiro无权限过滤器
 *
 * @package: com.yinghu.platform.shiro.filter
 * @author: chm
 * @create: 2019-08-20
 * @verison: 1.0
 * @description: shiro无权限过滤器
 */
public class SystemAuthorizationFilter extends AuthorizationFilter {
    @Override
    protected boolean isAccessAllowed(ServletRequest request, ServletResponse response, Object o) throws Exception {
        HttpServletRequest httpRequest = (HttpServletRequest) request;
        HttpServletResponse httpResponse = (HttpServletResponse) response;
        String url = httpRequest.getRequestURI();
        String reqUrl = httpRequest.getRequestURL().toString();
        System.out.println("请求地址："+reqUrl);
        System.out.println("参数地址："+url);

        String re = "\\/(.*?)\\/";
        Pattern p = Pattern.compile(re);
        Matcher m = p.matcher(url);
        String server="";
        int n=0;
        while(m.find()){
            server=m.group(1);
            if(n==1){
                break;
            }
            n++;
        }

        Subject subject = getSubject(request, response);
        System.out.println("访问服务名："+server+"；用户角色是否存在："+subject.hasRole(server));
        if(subject.hasRole(server)){
            //进一步看是否是访问分类
            String rsearch_cid=request.getParameter("search_cid");
            if(rsearch_cid!=null){
                try{
                    subject.checkPermission(server+"&"+rsearch_cid);
                }catch(Exception e){
                    //抛出异常表示无权限
//                    WebUtilsPro.writer(ResponseBo.error(ErrorCodeEnum.NO_PERMITION_AUTHORITY),request,response);
                    return false;
                }
            }
            return true;
        }

//        WebUtilsPro.writer(ResponseBo.error(ErrorCodeEnum.NO_PERMITION_AUTHORITY),request,response);
        return false;
    }
    @Override
    protected boolean onAccessDenied(ServletRequest request, ServletResponse response) throws IOException {
//        WebUtilsPro.writer(ResponseBo.error(ErrorCodeEnum.NO_PERMITION_AUTHORITY),request,response);
        WebUtilsPro.out((HttpServletResponse) response, ResponseBo.error(ErrorCodeEnum.NO_PERMITION_AUTHORITY));//自定义的返回数据
        return Boolean.FALSE;
    }
}

```

## 初始化filter
```java
import org.apache.commons.lang3.StringUtils;
import org.apache.shiro.mgt.SecurityManager;
import org.apache.shiro.codec.Base64;
import org.apache.shiro.session.SessionListener;
import org.apache.shiro.spring.LifecycleBeanPostProcessor;
import org.apache.shiro.spring.security.interceptor.AuthorizationAttributeSourceAdvisor;
import org.apache.shiro.spring.web.ShiroFilterFactoryBean;
import org.apache.shiro.web.mgt.CookieRememberMeManager;
import org.apache.shiro.web.mgt.DefaultWebSecurityManager;
import org.apache.shiro.web.servlet.SimpleCookie;
import org.apache.shiro.web.session.mgt.DefaultWebSessionManager;
import org.crazycake.shiro.RedisCacheManager;
import org.crazycake.shiro.RedisManager;
import org.crazycake.shiro.RedisSessionDAO;
import org.springframework.aop.framework.autoproxy.DefaultAdvisorAutoProxyCreator;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.DependsOn;

import javax.servlet.Filter;
import java.util.*;

/**
 * Shiro 配置类
 *
 */
@Configuration
public class ShiroConfig {

    @Autowired
    private FebsProperties febsProperties;

    @Autowired
    private ShiroAuthMapper authMapper;

    @Value("${spring.redis.host}")
    private String host;

    @Value("${spring.redis.port}")
    private int port;

    @Value("${spring.redis.password}")
    private String password;

    @Value("${spring.redis.timeout}")
    private int timeout;

    /**
     * shiro 中配置 redis 缓存
     *
     * @return RedisManager
     */
    private RedisManager redisManager() {
        RedisManager redisManager = new RedisManager();
        // 缓存时间，单位为秒
        //redisManager.setExpire(febsProperties.getShiro().getExpireIn()); // removed from shiro-redis v3.1.0 api
        redisManager.setHost(host);
        redisManager.setPort(port);
        if (CheckUtils.isNotEmpty(password))
            redisManager.setPassword(password);
        redisManager.setTimeout(timeout);
        return redisManager;
    }

    private RedisCacheManager cacheManager() {
        RedisCacheManager redisCacheManager = new RedisCacheManager();
//        redisCacheManager.setExpire(1000*30);//设置过期时间30秒
        redisCacheManager.setRedisManager(redisManager());
        return redisCacheManager;
    }

    /**
     * filter工厂
     * @param securityManager
     * @return
     */
    @Bean
    public ShiroFilterFactoryBean shiroFilterFactoryBean(SecurityManager securityManager) {
        System.out.println("ShiroConfiguration.shirFilter()");
        ShiroFilterFactoryBean shiroFilterFactoryBean = new ShiroFilterFactoryBean();
        // 设置 securityManager
        shiroFilterFactoryBean.setSecurityManager(securityManager);
        addFilter(shiroFilterFactoryBean);//将自定义 的FormAuthenticationFilter注入shiroFilter中

        LinkedHashMap<String, String> filterChainDefinitionMap = new LinkedHashMap<>();
        // 设置免认证 url
        String[] anonUrls = StringUtils.splitByWholeSeparatorPreserveAllTokens(febsProperties.getShiro().getAnonUrl(), ",");
        if(CheckUtils.isNotEmpty(anonUrls)) {
            for (String url : anonUrls) {
                filterChainDefinitionMap.put(url, "anon");
            }
        }

        List<Auth> alist = authMapper.findNeedAuth();
        for (Auth auth:alist) {
            if(StringUtils.isNotBlank(auth.getUrl())&&StringUtils.isNotBlank(auth.getPerms()) ){
                filterChainDefinitionMap.put(auth.getUrl(),"perms["+auth.getPerms()+"]");
            }
        }

        // 配置退出过滤器，其中具体的退出代码 Shiro已经替我们实现了
        filterChainDefinitionMap.put(febsProperties.getShiro().getLogoutUrl(), "logout");
//        filterChainDefinitionMap.put("/","authc");
        // 除上以外所有 url都必须认证通过才可以访问，未通过认证自动访问 LoginUrl
        filterChainDefinitionMap.put("/**", "user");

        shiroFilterFactoryBean.setFilterChainDefinitionMap(filterChainDefinitionMap);

        return shiroFilterFactoryBean;
    }

    /**
     * 安全管理器
     * @return
     */
    @Bean
    public SecurityManager securityManager() {
        DefaultWebSecurityManager securityManager = new DefaultWebSecurityManager();
        // 配置 rememberMeCookie
        securityManager.setRememberMeManager(rememberMeManager());

        // 配置 缓存管理类 cacheManager
        securityManager.setCacheManager(cacheManager());


        securityManager.setSessionManager(sessionManager());

        // 配置 SecurityManager，并注入 shiroRealm
        securityManager.setRealm(shiroRealm());
        return securityManager;
    }



    @Bean(name = "lifecycleBeanPostProcessor")
    public static LifecycleBeanPostProcessor lifecycleBeanPostProcessor() {
        // shiro 生命周期处理器
        return new LifecycleBeanPostProcessor();
    }

    @Bean
    public ShiroRealm shiroRealm() {

        return new ShiroRealm();
    }
    private void addFilter(ShiroFilterFactoryBean shiroFilterFactoryBean){
        Map<String, Filter> filters = shiroFilterFactoryBean.getFilters();//获取filters
        filters.put("perms", authorizationFilter());//将自定义 的FormAuthenticationFilter注入shiroFilter中
        filters.put("loginF", unauthorizedFilter());//将自定义 的FormAuthenticationFilter注入shiroFilter中
        shiroFilterFactoryBean.setFilters(filters);
    }

    @Bean("authorizationFilter")
    public SystemAuthorizationFilter authorizationFilter() {
        return new SystemAuthorizationFilter();
    }

    @Bean("unauthorizedFilter")
    public SystemUnauthorizedFilter unauthorizedFilter(){
        return new SystemUnauthorizedFilter();
    }


    /**
     * rememberMe cookie 效果是重开浏览器后无需重新登录
     *
     * @return SimpleCookie
     */
    private SimpleCookie rememberMeCookie() {
        // 设置 cookie 名称，对应 login.html 页面的 <input type="checkbox" name="rememberMe"/>
        SimpleCookie cookie = new SimpleCookie("rememberMe");
        // 设置 cookie 的过期时间，单位为秒，这里为一天
        cookie.setMaxAge(febsProperties.getShiro().getCookieTimeout());
        return cookie;
    }

    /**
     * cookie管理对象
     *
     * @return CookieRememberMeManager
     */
    private CookieRememberMeManager rememberMeManager() {
        CookieRememberMeManager cookieRememberMeManager = new CookieRememberMeManager();
        cookieRememberMeManager.setCookie(rememberMeCookie());
        // rememberMe cookie 加密的密钥
        cookieRememberMeManager.setCipherKey(Base64.decode("4AvVhmFLUs0KTA3Kprsdag=="));
        return cookieRememberMeManager;
    }

    /**
     * DefaultAdvisorAutoProxyCreator 和 AuthorizationAttributeSourceAdvisor 用于开启 shiro 注解的使用
     * 如 @RequiresAuthentication， @RequiresUser， @RequiresPermissions 等
     *
     * @return DefaultAdvisorAutoProxyCreator
     */
    @Bean
    @DependsOn({"lifecycleBeanPostProcessor"})
    public DefaultAdvisorAutoProxyCreator advisorAutoProxyCreator() {
        System.out.println("lifecycleBeanPostProcessor");
        System.out.println(febsProperties.getClass());
        DefaultAdvisorAutoProxyCreator advisorAutoProxyCreator = new DefaultAdvisorAutoProxyCreator();
        advisorAutoProxyCreator.setProxyTargetClass(true);
        return advisorAutoProxyCreator;
    }

    @Bean
    public AuthorizationAttributeSourceAdvisor authorizationAttributeSourceAdvisor(SecurityManager securityManager) {
        AuthorizationAttributeSourceAdvisor authorizationAttributeSourceAdvisor = new AuthorizationAttributeSourceAdvisor();
        authorizationAttributeSourceAdvisor.setSecurityManager(securityManager);
        return authorizationAttributeSourceAdvisor;
    }



    @Bean
    public RedisSessionDAO redisSessionDAO() {
        RedisSessionDAO redisSessionDAO = new RedisSessionDAO();
        redisSessionDAO.setRedisManager(redisManager());
        return redisSessionDAO;
    }

    /**
     * session 管理对象
     *
     * @return DefaultWebSessionManager
     */
    @Bean
    public DefaultWebSessionManager sessionManager() {
//        DefaultWebSessionManager sessionManager = new DefaultWebSessionManager();
        DefaultWebSessionManager sessionManager = new StatelessSessionManager();
        Collection<SessionListener> listeners = new ArrayList<>();
        listeners.add(new ShiroSessionListener());
        // 设置session超时时间，单位为毫秒
        sessionManager.setGlobalSessionTimeout(febsProperties.getShiro().getSessionTimeout());
        sessionManager.setSessionListeners(listeners);
        sessionManager.setSessionDAO(redisSessionDAO());
        sessionManager.setSessionIdUrlRewritingEnabled(false);
        return sessionManager;
    }
}

```