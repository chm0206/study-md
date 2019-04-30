## 一、foreach退出循环  
**关键字：end**  
**引入组件：**`<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>`  
标签解释：  
	items-要循环的列表  
	var-列表中获取的对象  
	varStatus-foreach自带监控次数插件  
	end-判断是否结束  
```js
<c:forEach var="role" items="${roles}" varStatus = "status" end="${exitId}">
     <c:if test="${status.index == 10}"><!--等于10的时候退出循环体-->
         <c:set var="exitId" value="0"></c:set>
     </c:if>
 </c:forEach>
```
## 二、获取序号  
**关键字：varStatus,index**  
**引入组件：**`<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>`  
标签解释：  
	items-要循环的列表  
	var-列表中获取的对象  
	varStatus-foreach自带监控次数插件  
	index-获取varStatus当前索引值（从0开始） 
```js
<c:forEach var="role" items="${roles}" varStatus = "status">
     ${status.index+1}
 </c:forEach>
```
## 三、if...else if...else  
**关键字：c:when  c:otherwise**  
**引入组件：**`<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>`  
```js
<c:choose> 
   <c:when test='${a==100}'>
   满分
   </c:when> 
   <c:when test='${a60}'>
   及格
   </c:when> 
   <c:otherwise>
   不及格
   </c:otherwise> 
</c:choose> 
```
## 四、时间格式化  
**关键字：formatDate、pattern**  
**引入组件：**`<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>`  
标签注释：  
formatDate:时间格式化  
pattern：需要转换的时间格式  

```js
<fmt:formatDate value="${dateValue}" pattern="MM/dd/yyyy HH:mm:ss.SSS"/><!-- SSS的个数表示精确的毫秒数 -->
```
