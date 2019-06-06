# 敏感词算法及实现

## 1. 敏感词判断算法
&emsp;&emsp;dfa算法实现[原理](./dfa算法实现原理.md)
```java

/**
 * 数据格式化类
 * @author chm
 */
public class FilterSet{

	private final long[] elements;

	
	public FilterSet() {
		 elements = new long[1 + (65535 >>> 6)];
	}
	
	public void add(final int no) {
		elements[no >>> 6] |= (1L << (no & 63));
	}
	
	public void add(final int... no) {
		for(int currNo : no)
			elements[currNo >>> 6] |= (1L << (currNo & 63));
	}
	
	public void remove(final int no) {
		elements[no >>> 6] &= ~(1L << (no & 63));
	}
	
	/**
	 * 
	 * @param no
	 * @return true:添加成功	false:原已包含
	 */
	public boolean addAndNotify(final int no) {
		int eWordNum = no >>> 6;
		long oldElements = elements[eWordNum];
		elements[eWordNum] |= (1L << (no & 63));
		boolean result = elements[eWordNum] != oldElements;
//		if (result)
//			size++;
		return result;
	}
	
	/**
	 * 
	 * @param no
	 * @return true:移除成功	false:原本就不包含
	 */
	public boolean removeAndNotify(final int no) {
		int eWordNum = no >>> 6;
		long oldElements = elements[eWordNum];
		elements[eWordNum] &= ~(1L << (no & 63));
		boolean result = elements[eWordNum] != oldElements;
//		if (result)
//			size--;
		return result;
	}
	
	public boolean contains(final int no) {
        return (elements[no >>> 6] & (1L << (no & 63))) != 0;
    }

	public boolean containsAll(final int... no) {
		if(no.length==0)
			return true;
		for(int currNo : no)
			if((elements[currNo >>> 6] & (1L << (currNo & 63))) == 0)
				return false;
		return true;
	}
	
	/**
	 * 不如直接循环调用contains
	 * @param no
	 * @return
	 */
	public boolean containsAll_ueslessWay(final int... no) {
		long[] elements = new long[this.elements.length];
		for(int currNo : no){
			elements[currNo >>> 6] |= (1L << (currNo & 63));
		}//这一步执行完跟循环调用contains差不多了
		
		for (int i = 0; i < elements.length; i++)
			if ((elements[i] & ~this.elements[i]) != 0)
				return false;
		return true;
	}
	
	/**
	 * 目前没有去维护size，每次都是去计算size
	 * @return
	 */
	public int size() {
		int size = 0;
		for (long element : elements)
			size += Long.bitCount(element);
		return size;
	}
	
	public static void main(String[] args) {
		FilterSet oi = new FilterSet();
		System.out.println(oi.elements.length);
	}

}
```
2. 敏感词存储对象
```java
import java.util.LinkedList;
import java.util.List;

/**
 * 敏感词存储对象
 */
public class WordNode {

	private int value;
	
	private List<WordNode> subNodes;
	
	private boolean isLast;//默认false

//	public WordNode(int value){
//		this.value = value;
//	}
	
	public WordNode(int value, boolean isLast){
		this.value = value;
//		if(!this.isLast){//如果已经是某一个敏感词的last，那就是last
		this.isLast = isLast;
//		}
		
	}
	
	/**
	 * 
	 * @param subNode
	 * @return 就是传入的subNode
	 */
	private WordNode addSubNode(final WordNode subNode){
		if(subNodes==null)
			subNodes = new LinkedList<WordNode>();
		subNodes.add(subNode);
		return subNode;
	}
	
	/**
	 * 有就直接返回该子节点， 没有就创建添加并返回该子节点
	 * @param value
	 * @return
	 */
	public WordNode addIfNoExist(final int value, final boolean isLast){
		if(subNodes==null){
			return addSubNode(new WordNode(value, isLast));
		}
		for(WordNode subNode : subNodes){
			if(subNode.value==value){
				if(!subNode.isLast && isLast)
					subNode.isLast = true;
				return subNode;
			}
		}
		return addSubNode(new WordNode(value, isLast));
	}
	
	public WordNode querySub(final int value){
		if(subNodes==null){
			return null;
		}
		for(WordNode subNode : subNodes){
			if(subNode.value==value)
				return subNode;
		}
		return null;
	}
	
	public boolean isLast(){
		return isLast;
	}
	
	public void setLast(boolean isLast){
		this.isLast = isLast;
	}
	
	@Override
	public int hashCode() {
		return value;
	}
	
}

```

3. 敏感词操作工具类
```java

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

import javax.annotation.PostConstruct;
import java.util.*;

/**
 * 敏感词操作工具类
 */
@Component
public class SersitiveWordUtil {

    public static final String BASE_SERSITIVE_WORD_URL = "/sw/";

    @Autowired
    private ISersitiveWordService sersitiveWordService;
    @Autowired
    private SersitiveWordCheckService sersitiveWordCheckService;

    private static SersitiveWordUtil util;

    public static FilterSet set = new FilterSet();
    public static Map<Integer, WordNode> nodes = new HashMap<Integer, WordNode>(1024, 1);


    @PostConstruct
    public void init() {
        util = this;
        util.sersitiveWordService = this.sersitiveWordService;
        util.sersitiveWordCheckService = this.sersitiveWordCheckService;
    }

    /**
     * 获取敏感词服务层对象
     * @return
     */
    public static SersitiveWordCheckService getswCheck(){
        return util.sersitiveWordCheckService;
    }

    /**
     * 校验敏感词
     * @param chs
     * @param i
     * @return
     */
    public static int check(char[] chs, int i){
        WordNode node;
        int currc = chs[i];
        int length = chs.length;
        if(!set.contains(currc)){
            return 0;//判断该字符是否存在，不存在直接返回
        }
//			k=i;//警2
        node = nodes.get(currc);//警2
        if(node == null)//其实不会发生，习惯性写上了
            return 0;
        boolean couldMark = false;
        int markNum = -1;
        if(node.isLast()){//是否单字匹配(警)，默认不匹配
            couldMark = true;
            markNum = 0;
        }
        int k=i;
        //默认匹配长的(警察/警察局)，以长的为优化
        //察-3 局-4
        for( ; ++k<length; ){//获取下个字符，从i开始是为了获取i的下个字符
            node = node.querySub(chs[k]);
            if(node==null)//不匹配
                break;
            if(node.isLast()){//匹配，继续往下
                couldMark = true;
                markNum = k-i;//3-2
            }
        }
        if(couldMark){
            return markNum;//返回长度
        }
        return 0;
    }

    /**
     * 初始化敏感词库
     */
    public static void load(){
        List<String> words = util.sersitiveWordService.findAll();//获取敏感词
        addSensitiveWord(words);//添加
    }

    /**
     * 添加新的敏感词
     * @param list
     */
    public static void update(List<String> list){
        addSensitiveWord(list);
    }

    /**
     * 根据敏感词进行删除操作
     * @param list
     */
    public static void remove(List<String> list){
        //没想好怎么搞
        reload();//暂时没有想到怎么去除
    }

    /**
     * 根据ID进行删除操作
     * @param list
     */
    public static void removeById(List<String> list) {
        //如果直接操作，那需要先查询已删除的value
        reload();//暂时没有想到怎么去除
    }

    /**
     * 重新加载敏感词(删除的时候调用)
     */
    private static void reload(){
        set = new FilterSet();
        nodes = new HashMap<Integer, WordNode>(1024, 1);
        load();
    }

    private static void addSensitiveWord(final List<String> words){
        char[] chs;
        int fchar;
        int lastIndex;
        WordNode fnode;
        for(String curr : words){
            chs = curr.toCharArray();
            fchar = chs[0];
            if(!set.contains(fchar)){//没有首字定义
                set.add(fchar);//首字标志位	可重复add,反正判断了，不重复了
                fnode = new WordNode(fchar, chs.length==1);
                nodes.put(fchar, fnode);
            }else{
                fnode = nodes.get(fchar);
                if(!fnode.isLast() && chs.length==1)
                    fnode.setLast(true);
            }
            lastIndex = chs.length-1;
            for(int i=1; i<chs.length; i++){
                fnode = fnode.addIfNoExist(chs[i], i==lastIndex);
            }
        }
    }
}

```

二、在spring boot 里实现
1. 数据库表设计  
```sql
CREATE TABLE `tb_base_sersitive_word` (
  `sw_id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT '主键',
  `sw_value` varchar(100) DEFAULT NULL COMMENT '敏感词',
  `add_time` datetime DEFAULT NULL COMMENT '添加时间',
  `enable_flag` char(2) DEFAULT NULL COMMENT '删除标识',
  `add_user` varchar(32) DEFAULT NULL COMMENT '添加用户',
  PRIMARY KEY (`sw_id`)
) ENGINE=InnoDB AUTO_INCREMENT=8 DEFAULT CHARSET=utf8 COMMENT='敏感词表';

```
2. 生成model、dao、mapper、service  
[自动生成工具](http://git.chm.ac.cn/chm/mpGenerator)及[该工具使用方法]()  
model
```java

import lombok.Data;

import javax.persistence.Column;
import javax.persistence.Id;
import javax.persistence.Table;
import java.io.Serializable;
import java.util.Date;

/**
 * <p>
 * 敏感词表
 * </p>
 *
 * @author chm
 * @since 2019-06-05
 */
@Table(name = "tb_base_sersitive_word")
@Data
public class SersitiveWord implements Serializable {

    private static final long serialVersionUID = 1L;

    /**
     * 主键
     */
    @Id
    @Column(name = "sw_id")
    private Long swId;
    /**
     * 敏感词
     */

    @Column(name = "sw_value")
    private String swValue;
    /**
     * 添加时间
     */

    @Column(name = "add_time")
    private Date addTime;
    /**
     * 删除标识
     */

    @Column(name = "enable_flag")
    private String enableFlag;
    /**
     * 添加用户
     */

    @Column(name = "add_user")
    private String addUser;
    
}
```

dao
```java

import java.util.List;
import java.util.Map;

/**
 * <p>
 * 敏感词表 Mapper 接口
 * </p>
 *
 * @author chm
 * @since 2019-06-05
 */
public interface SersitiveWordMapper extends MyMapper<SersitiveWord> {
    /**
     * 获取全部有效的敏感词
     * @return
     */
    List<String> findAll();

    /**
     * 根据ID删除敏感词
     * @param map
     * @return
     */
    int removeSersitiveWordsByIds(Map<String,Object> map);

    /**
     * 根据value删除敏感词
     * @param map
     * @return
     */
    int removeSersitiveWordsByValues(Map<String,Object> map);
}
```
mapper
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="SersitiveWordMapper">

                <!-- 通用查询映射结果 -->
    <resultMap id="BaseResultMap" type="SersitiveWord">
        <id column="sw_id" property="swId"/>
        <result column="sw_value" property="swValue"/>
        <result column="add_time" property="addTime"/>
        <result column="enable_flag" property="enableFlag"/>
        <result column="add_user" property="addUser"/>
    </resultMap>

                <!-- 通用查询结果列 -->
    <sql id="Base_Column_List">
        sw_id, sw_value, add_time, enable_flag, add_user
    </sql>

    <!-- ids -->
    <update id="removeSersitiveWordsByIds" parameterType="java.util.Map">
        update tb_base_sersitive_word
        set enable_flag = '1'
        where enable_flag = '0'
        <choose>
            <when test="ids != null and ids.size() >0">
                and  sw_id in
                <foreach collection="ids" item="swId" separator="," open="(" close=")">
                    #{swId}
                </foreach>
            </when>
            <otherwise>
                and 1=0
            </otherwise>
        </choose>
    </update>
    <!-- 根据values进行敏感词删除操作 -->
    <update id="removeSersitiveWordsByValues" parameterType="java.util.Map">
        update tb_base_sersitive_word
        set enable_flag = '1'
        where enable_flag = '0'
        <choose>
            <when test="values != null and values.size() >0">
                and  sw_value in
                <foreach collection="values" item="swValue" separator="," open="(" close=")">
                    #{swValue}
                </foreach>
            </when>
            <otherwise>
                and 1=0
            </otherwise>
        </choose>
    </update>
    <!-- 查询所有未删除的敏感词 -->
    <select id="findAll" resultType="java.lang.String">
        SELECT sw_value
        FROM tb_base_sersitive_word
        WHERE enable_flag = '0'
    </select>

</mapper>
```
service
```java

import java.util.List;
import java.util.Map;

/**
 * <p>
 * 敏感词表 服务类
 * </p>
 *
 * @author chm
 * @since 2019-06-05
 */
public interface ISersitiveWordService extends IService<SersitiveWord> {

    /**
     * 查询全部
     * @return
     */
    List<String> findAll();

    /**
     * 批量导入敏感词
     * @param array
     * @return 添加失败抛异常，添加失败:返回已添加过的敏感词列表
     */
    List<String> addSersitiveWords(List<String> array);

    /**
     * 新增
     *
     * @param map
     * @return
     */
    int saveSersitiveWord(Map<String, Object> map) throws Exception;

    /**
     * 编辑
     *
     * @param map
     * @return
     */
    int editSersitiveWord(Map<String, Object> map) throws Exception;

    /**
     * 批量删除敏感词
     * @param sersitiveWords
     * @return
     */
    int removeSersitiveWordsByValue(List<String> sersitiveWords);

    /**
     * 根据ID删除敏感词
     * @param ids
     * @return
     */
    int  removeSersitiveWordsByIds(String ids);
}
```
service-impl
```java

import com.github.pagehelper.PageHelper;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;
import tk.mybatis.mapper.entity.Example;

import java.util.*;

/**
 * <p>
 * 敏感词表 服务实现类
 * </p>
 *
 * @author chm
 * @since 2019-06-05
 */
@Service
@Transactional
public class SersitiveWordServiceImpl extends BaseService<SersitiveWord> implements ISersitiveWordService {

    private Logger log = LoggerFactory.getLogger(this.getClass());
    @Autowired
    private SersitiveWordMapper sersitiveWordMapper;

    @Autowired
    private SersitiveWordCheckService checkService;

    @Override
    public List<String> findAll() {
        return this.sersitiveWordMapper.findAll();
    }

    @Override
    public List<String> addSersitiveWords(List<String> array) {
        Set<String> sets = checkService.queryAll(array);
        List<SersitiveWord> sws = new ArrayList<>();
        List<String> update = new ArrayList<>();
        for (String str: array
             ) {
            if(!sets.contains(str)){
                update.add(str);
                SersitiveWord sw = new SersitiveWord();
                sw.setSwValue(str);
                sw.setAddUser(Tools.getUserId());
                sw.setAddTime(new Date());
                sw.setEnableFlag("0");
                sws.add(sw);
            }
        }
        if(CheckUtils.isNotEmpty(sws)) {
            this.sersitiveWordMapper.insertList(sws);
            SersitiveWordUtil.update(update);//更新敏感词库
        }
        return new ArrayList<>(sets);
    }

    @Override
    public int removeSersitiveWordsByIds(String ids)  {
        if (CheckUtils.isEmpty(ids)) {
            return 0;
        }
        List<String> idArray = Arrays.asList(ids.split(","));
        Map<String,Object> map = new HashMap<>();
        map.put("ids",idArray);
        this.sersitiveWordMapper.removeSersitiveWordsByIds(map);
        SersitiveWordUtil.removeById(idArray);
        return 1;
    }

    @Override
    public int saveSersitiveWord(Map<String, Object> map) throws Exception {
        return 0;
    }

    @Override
    public int editSersitiveWord(Map<String, Object> map) throws Exception {
        return 0;
    }

    @Override
    public int removeSersitiveWordsByValue(List<String> sersitiveWords) {
        if (CheckUtils.isEmpty(sersitiveWords)) {
            return 0;
        }
        Map<String,Object> map = new HashMap<>();
        map.put("values",sersitiveWords);
        this.sersitiveWordMapper.removeSersitiveWordsByValues(map);
        SersitiveWordUtil.remove(sersitiveWords);
        return 1;
    }
}
```
敏感词校验service
```java

import java.util.List;
import java.util.Set;

public interface SersitiveWordCheckService {

    /**
     * 判断对象是否存在敏感词
     * @param object
     * @return
     */
    boolean exist(Object object);//SersitiveWords

    /**
     * 替换对象敏感词并返回替换后的对象
     * @param object
     * @param <T>
     * @return
     */
    <T> T replace(T object, Class<T> t);//SersitiveWords

    /**
     * 替换列表敏感词并返回替换后的列表
     * @param list
     * @param t
     * @param <T>
     * @return
     */
    <T> List<T> replace(List<T> list, Class<T> t);//SersitiveWords

    /**
     * 查询对象包括的敏感词
     * @param o
     * @return
     */
    String query(Object o);//SersitiveWords

    /**
     * 查询对象包括的所有敏感词
     * @param o
     * @return
     */
    Set<String> queryAll(Object o);//SersitiveWords
}

```

敏感词校验-service-impl
```java

import com.alibaba.fastjson.JSONObject;
import org.springframework.stereotype.Service;

import java.util.HashSet;
import java.util.List;
import java.util.Set;

@Service
public class SersitiveWordCheckServiceImpl implements SersitiveWordCheckService {

    @Override
    public boolean exist(Object object) {
        String str = JSONObject.toJSONString(object);
        char[] chs = str.toCharArray();
        int length = chs.length;
        for (int i = 0; i < length; i++) {
            int markNum = SersitiveWordUtil.check(chs, i);
            if (markNum > 0) {
                return true;
            }
        }
        return false;
    }

    @Override
    public <T> T replace(T object, Class<T> t) {
        String str = JSONObject.toJSONString(object);
        char[] chs = str.toCharArray();
        int length = chs.length;
        for (int i = 0; i < length; i++) {
            int markNum = SersitiveWordUtil.check(chs, i);
            if (markNum > 0) {
                for (int k = 0; k <= markNum; k++) {
                    chs[k + i] = '*';
                }
                i = i + markNum;
            }
        }
        String newStr = new String(chs);
        return Tools.str2Bean(newStr, t);
    }

    @Override
    public <T> List<T> replace(List<T> list, Class<T> t) {
        String str = JSONObject.toJSONString(list);
        char[] chs = str.toCharArray();
        int length = chs.length;
        for (int i = 0; i < length; i++) {
            int markNum = SersitiveWordUtil.check(chs, i);
            if (markNum > 0) {
                for (int k = 0; k <= markNum; k++) {
                    chs[k + i] = '*';
                }
                i = i + markNum;
            }
        }
        String newStr = new String(chs);
        return Tools.listClone(newStr, t);
    }

    @Override
    public String query(Object o) {
        String str = JSONObject.toJSONString(o);
        Set<String> query = new HashSet<>();
        char[] chs = str.toCharArray();
        int length = chs.length;
        for (int i = 0; i < length; i++) {
            int markNum = SersitiveWordUtil.check(chs, i);
            if (markNum > 0) {//对数据进行处理
                return Tools.charArr2Str(chs, i, i + markNum);
//                i = i+markNum;
            }
        }
        return null;
    }

    @Override
    public Set<String> queryAll(Object o) {
        String str = JSONObject.toJSONString(o);
        Set<String> query = new HashSet<>();
        char[] chs = str.toCharArray();
        int length = chs.length;
        for (int i = 0; i < length; i++) {
            int markNum = SersitiveWordUtil.check(chs, i);
            if (markNum > 0) {//对数据进行处理
                query.add(Tools.charArr2Str(chs, i, i + markNum));
                i = i + markNum;
            }
        }
        return query;
    }
}
```

controller
```java
import io.swagger.annotations.Api;
import io.swagger.annotations.ApiImplicitParam;
import io.swagger.annotations.ApiImplicitParams;
import io.swagger.annotations.ApiOperation;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.*;

import java.util.*;

/**
 * 敏感词-controller
 */
@Controller
@RequestMapping(SersitiveWordUtil.BASE_SERSITIVE_WORD_URL)
@Api(tags = {"SersitiveWord"}, description = "敏感字过滤相关Api")
public class SersitiveWordController extends BaseController {
    @Autowired
    private SersitiveWordCheckService service;

    @Autowired
    private ISersitiveWordService sersitiveWordService;
    /**
     * 判断参数是否存在敏感词
     * @param map
     * @return
     */

    @ApiOperation(value = "对象是否存在敏感词", notes = "对象是否存在敏感词")
    @GetMapping("exist")
    @ResponseBody
    public ResponseBo exist(Map<String,Object> map){
        if(CheckUtils.isEmpty(map)){
            map = getMap();
        }
        if(!service.exist(map)){
            return ResponseBo.ok("没有查询到敏感词");
        }else{
            return ResponseBo.error("查询到敏感词");
        }
    }

    /**
     * 判断参数是否存在敏感词，并返回第一个敏感词
     * @param map
     * @return
     */
    @ApiOperation(value = "对象存在哪个敏感词", notes = "对象存在哪个敏感词")
    @GetMapping("query")
    @ResponseBody
    public ResponseBo query(Map<String,Object> map){
        if(CheckUtils.isEmpty(map)){
            map = getMap();
        }
        String query = service.query(map);
        if(CheckUtils.isEmpty(query)){
            return ResponseBo.ok("没有查询到敏感词");
        }else{
            ResponseBo result = ResponseBo.error("查询到敏感词,具体查看data内容");
            result.put("data",query);
            return result;
        }
    }
    /**
     * 判断参数是否存在敏感词，并返回全部敏感词
     * @param map
     * @return
     */
    @ApiOperation(value = "对象存在哪些敏感词", notes = "对象存在哪些敏感词")
    @GetMapping("queryAll")
    @ResponseBody
    public ResponseBo queryAll(Map<String,Object> map){
        if(CheckUtils.isEmpty(map)){
            map = getMap();
        }
        Set<String> queryAll = service.queryAll(map);
        if(CheckUtils.isEmpty(queryAll)){
            return ResponseBo.ok("没有查询到敏感词");
        }else{
            ResponseBo result = ResponseBo.error("查询到敏感词,具体查看data内容");
            result.put("data",queryAll);
            return result;
        }
    }

    /**
     * 添加敏感词
     * @return
     */
    @ApiOperation(value = "添加敏感词", notes = "添加敏感词")
    @ApiImplicitParams({
            @ApiImplicitParam(name = "concent", value = "敏感词内容(根据空格和回车进行拆分)", required = true, dataType = "string")
    })
    @PostMapping("add")
    @ResponseBody
    public ResponseBo add(@RequestParam String concent){
        ResponseBo result= null;
        //接收文字
        concent = concent.replaceAll("\\s+",",,");
        try {
            String[] arrays = concent.split(",,");
            List<String> list = sersitiveWordService.addSersitiveWords(Arrays.asList(arrays));
            if (CheckUtils.isEmpty(list)) {
                result = ResponseBo.ok("添加成功");
            }else{
                result = ResponseBo.ok("添加成功，data内的敏感词已存在");
                result.put("data",list);
            }
        } catch (Exception e) {
            e.printStackTrace();
            result = ResponseBo.error("添加失败");
            result.put("data",e.toString());
        }
        return result;
    }
    /**
     * 导入敏感词
     * @return
     */
    @PostMapping("imports")
    @ResponseBody
    public ResponseBo imports(){
        return null;
    }
    /**
     * 根据id删除敏感词
     * @return
     */
    @ApiOperation(value = "根据ID删除敏感词", notes = "根据ID删除敏感词")
    @ApiImplicitParams({
            @ApiImplicitParam(name = "ids", value = "敏感词ID(多个敏感词ID通过\",\"分隔)", required = true, dataType = "string")
    })
    @DeleteMapping("delete/id")
    @ResponseBody
    public ResponseBo deleteById(@RequestParam String ids){
        ResponseBo result= null;
        try {
            sersitiveWordService.removeSersitiveWordsByIds(ids);
            result = ResponseBo.ok("删除成功");
        } catch (Exception e) {
            result = ResponseBo.error("删除成功");
            result.put("data",e.toString());
        }
        return result;
    }
    /**
     * 根据value删除敏感词
     * @return
     */
    @ApiOperation(value = "根据values删除敏感词", notes = "根据values删除敏感词")
    @ApiImplicitParams({
            @ApiImplicitParam(name = "values", value = "敏感词内容(多个敏感词通过\",\"分隔)", required = true, dataType = "string")
    })
    @DeleteMapping("delete/value")
    @ResponseBody
    public ResponseBo deleteByvalue(@RequestParam String values){
        ResponseBo result= null;
        try {
            sersitiveWordService.removeSersitiveWordsByValue(Arrays.asList(values.split(",")));
            result = ResponseBo.ok("删除成功");
        } catch (Exception e) {
            result = ResponseBo.error("删除成功");
            result.put("data",e.toString());
        }
        return result;
    }
}

```

拦截器
```java

import com.alibaba.fastjson.JSON;
import org.apache.shiro.web.util.WebUtils;
import org.springframework.stereotype.Component;

import javax.servlet.*;
import javax.servlet.http.HttpServletRequest;
import java.io.IOException;
import java.io.PrintWriter;
import java.util.Map;

@Component
public class SersitiveWordFilter implements Filter {

    @Override
    public void init(FilterConfig filterConfig) throws ServletException {

    }

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        HttpServletRequest request = WebUtils.toHttp(servletRequest);
        Map properties = request.getParameterMap();
        String data = SersitiveWordUtil.getswCheck().query(properties);
        String uri = request.getRequestURI();
        if(uri.indexOf(SersitiveWordUtil.BASE_SERSITIVE_WORD_URL)>=0||
                uri.indexOf("/login")>=0||CheckUtils.isEmpty(data)){
            filterChain.doFilter(servletRequest, servletResponse);
        }else{
            servletResponse.setCharacterEncoding("UTF-8");
            servletResponse.setContentType("application/json; charset=utf-8");
            PrintWriter out = servletResponse.getWriter();
            ResponseBo res = ResponseBo.error();
            res.put("msg", "存在敏感词,具体敏感词请查看data内容");
            res.put("data",data);
            out.append(JSON.toJSONString(res));
            return;
        }
    }

    @Override
    public void destroy() {

    }
}
```
拦截器初始化类
```java

import org.springframework.boot.web.servlet.FilterRegistrationBean;
import org.springframework.context.annotation.Bean;
import org.springframework.stereotype.Component;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

@Component
public class ApplicationConfig {
    /**
     * 内容拦截
     * @return
     */
    @Bean
    public FilterRegistrationBean sersitiveWordFilterBean() {
        String[] url = {"/api/**/*"};
        FilterRegistrationBean registrationBean = new FilterRegistrationBean();
        registrationBean.setFilter(new SersitiveWordFilter());
        registrationBean.setUrlPatterns(Arrays.asList(url));
        registrationBean.setName("sersitiveWord");
        registrationBean.setOrder(4);
        return registrationBean;
    }
}
```
补充类
返回值
```java

import java.util.HashMap;

public class ResponseBo extends HashMap<String, Object> {

	private static final long serialVersionUID = -8713837118340960775L;

	// 成功
	private static final Integer SUCCESS = 200;
	// 警告
	private static final Integer WARN = 1;
	// 异常 失败
	private static final Integer FAIL = 500;

	public ResponseBo() {
		put("code", SUCCESS);
		put("msg", "操作成功");
	}

	public static ResponseBo error(Object msg) {
		ResponseBo responseBo = new ResponseBo();
		responseBo.put("code", FAIL);
		responseBo.put("msg", msg);
		return responseBo;
	}

	public static ResponseBo warn(Object msg) {
		ResponseBo responseBo = new ResponseBo();
		responseBo.put("code", WARN);
		responseBo.put("msg", msg);
		return responseBo;
	}

	public static ResponseBo ok(Object msg) {
		ResponseBo responseBo = new ResponseBo();
		responseBo.put("code", SUCCESS);
		responseBo.put("msg", msg);
		return responseBo;
	}

	public static ResponseBo put(Object obj) {
		ResponseBo responseBo = new ResponseBo();
		responseBo.put("code", SUCCESS);
		responseBo.put("data", obj);
		return responseBo;
	}



	public static ResponseBo ok() {
		return new ResponseBo();
	}

	public static ResponseBo error() {
		return ResponseBo.error("");
	}

	@Override
	public ResponseBo put(String key, Object value) {
		super.put(key, value);
		return this;
	}
}
```
数据校验类
```java

import java.util.Collection;
import java.util.Map;

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
BaseController
```java

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.web.context.request.RequestContextHolder;
import org.springframework.web.context.request.ServletRequestAttributes;

import javax.servlet.http.HttpServletRequest;
import java.util.*;

public class BaseController {

    public Logger log = LoggerFactory.getLogger(this.getClass());


    /**
     * 得到request对象
     *
     * @return
     */
    public HttpServletRequest getRequest() {
        HttpServletRequest request = ((ServletRequestAttributes) RequestContextHolder.getRequestAttributes())
                .getRequest();
        return request;
    }

    public Map<String,Object> getMap(){
        Map properties = this.getRequest().getParameterMap();
        Map<String,Object> returnMap = new HashMap();
        Iterator entries = properties.entrySet().iterator();
        Map.Entry entry;
        String name = "";
        String value = "";
        while (entries.hasNext()) {
            entry = (Map.Entry) entries.next();
            name = (String) entry.getKey();
            Object valueObj = entry.getValue();
            if(null == valueObj){
                value = "";
            }else if(valueObj instanceof String[]){
                String[] values = (String[])valueObj;
                for(int i=0;i<values.length;i++){
                    value = values[i] + ",";
                }
                value = value.substring(0, value.length()-1);
            }else{
                value = valueObj.toString();
            }
            returnMap.put(name, value);
        }
        return returnMap;

    }
    /**
     * 获取用户登录IP
     *getSubject
     * @return
     * @date 2017年4月10日
     */
    public String getLoginIP() {
        HttpServletRequest request  = this.getRequest();
        String ip = request.getHeader("x-forwarded-for");
        if (ip != null && ip.length() != 0 && !"unknown".equalsIgnoreCase(ip)) {
            // 多次反向代理后会有多个ip值，第一个ip才是真实ip
            if( ip.indexOf(",")!=-1 ){
                ip = ip.split(",")[0];
            }
        }
        if (ip == null || ip.length() == 0 || "unknown".equalsIgnoreCase(ip)) {
            ip = request.getHeader("Proxy-Client-IP");
        }
        if (ip == null || ip.length() == 0 || "unknown".equalsIgnoreCase(ip)) {
            ip = request.getHeader("WL-Proxy-Client-IP");
        }
        if (ip == null || ip.length() == 0 || "unknown".equalsIgnoreCase(ip)) {
            ip = request.getHeader("HTTP_CLIENT_IP");
        }
        if (ip == null || ip.length() == 0 || "unknown".equalsIgnoreCase(ip)) {
            ip = request.getHeader("HTTP_X_FORWARDED_FOR");
        }
        if (ip == null || ip.length() == 0 || "unknown".equalsIgnoreCase(ip)) {
            ip = request.getHeader("X-Real-IP");
        }
        if (ip == null || ip.length() == 0 || "unknown".equalsIgnoreCase(ip)) {
            ip = request.getRemoteAddr();
        }
//        System.out.println("获取客户端ip: " + ip);
        return ip;
    }

}
```

Tools
```java

import com.alibaba.druid.util.StringUtils;
import com.alibaba.fastjson.JSON;
import com.alibaba.fastjson.JSONArray;
import com.alibaba.fastjson.JSONObject;
import com.alibaba.fastjson.serializer.SerializerFeature;
import java.beans.BeanInfo;
import java.beans.IntrospectionException;
import java.beans.Introspector;
import java.beans.PropertyDescriptor;
import java.lang.reflect.Field;
import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.*;

public class Tools {

    /**
     * 将实体对象转换为json对象
     *
     * @param value
     * @return
     */
    public static <V> JSONObject entity2Json(V value) {
        if (value == null) {
            return null;
        }
        JSONObject object = (JSONObject) JSONObject.toJSON(value);
        return object;
    }



    /**
     * 字符串转换为字符串数组
     *
     * @param str        字符串
     * @param splitRegex 分隔符
     * @return
     */
    public static String[] str2StrArray(String str, String splitRegex) {
        if (StringUtils.isEmpty(str)) {
            return null;
        }
        return str.split(splitRegex);
    }


    /**
     * 生成32位uuid
     *
     * @return
     */
    public static String getUUID32() {
        String uuid = UUID.randomUUID().toString().replace("-", "").toLowerCase();
        return uuid;
    }

    public static <T> T str2Bean(String str, Class clazz) {
        if (StringUtils.isEmpty(str)) {
            return null;
        }
        T t = (T) JSONObject.parseObject(str, clazz);
        return t;
    }


    public static double getDouble(Double num) {
        return num == null ? 0.0 : num;
    }

    /**
     * 将字符串转换为数字并相加
     *
     * @param strs
     * @return
     */
    public static int str2IntAdd(String... strs) {
        int a = 0;
        for (String str : strs) {
            //非纯数字不执行加操作
            if (!StringUtils.isEmpty(str) && str.matches("[0-9]*")) {
                a += Integer.valueOf(str);
            }
        }
        return a;
    }

    /**
     * 去除后面的0
     *
     * @param str
     * @return
     */
    public static String removeLastZero(String str) {
        int n = str.length() - 1;
        for (int i = n; i >= 0; i--) {
            if (str.charAt(i) != '0') {
                return str.substring(0, i + 1);
            }
        }
        return "";
    }

    /**
     * 将map转换为bean对象
     *
     * @param map
     * @param clazz
     * @param <T>
     * @return
     */
    public static <T> T map2Bean(Map map, Class<T> clazz) {
        return JSONObject.parseObject(JSON.toJSONString(map, SerializerFeature.WriteMapNullValue), clazz);
    }

    /**
     * 将对象转换为json
     *
     * @param t
     * @param <T>
     * @return
     */
    public static <T> JSONObject bean2Json(T t) {
        return (JSONObject) JSONObject.parse(JSON.toJSONString(t, SerializerFeature.WriteMapNullValue));
    }
    /**
     * 将list对象转换为json
     *
     * @param t
     * @param <T>
     * @return
     */
    public static <T> JSONArray bean2JsonList(T t) {
        return (JSONArray) JSONArray.parse(JSON.toJSONString(t));
    }

    /**
     * N个double数据相加
     *
     * @param dou
     * @return
     */
    public static double doubleAdd(Double... dou) {
        Double d = 0.0;
        for (Double db : dou) {
            d += db == null ? 0.0 : db;
        }
        return d;
    }

    /**
     * n个double数据相乘
     *
     * @param dou
     * @return
     */
    public static double doubleMultiply(Double... dou) {
        Double d = 1.0;
        for (Double db : dou) {
            if (db == null || db == 0.0) {
                return 0.0;
            }
            d = d * db;
        }
        return d;
    }

    /**
     * n个double数据相除
     *
     * @param dou
     * @return
     */
    public static double doubleDivide(Double... dou) {
        Double d = dou[0];
        for (int i = 1; i < dou.length; i++) {
            if (dou[i] == null || dou[i] == 0.0) {
                return 0.0;
            }
            d = d / dou[i];
        }
        return d;
    }

    /**
     * 将一个对象的列表转换为另一个对象的列表
     *
     * @param k
     * @param clazz
     * @param <T>
     * @param <K>
     * @return
     */
    public static <T, K> List<T> listClone(List<K> k, Class<T> clazz) {
        List<T> list = JSONArray.parseArray((JSONArray.toJSON(k)).toString(), clazz);
        return list;
    }

    /**
     * 将json格式的字符串转换为指定对象的列表
     *
     * @param str
     * @param clazz
     * @param <T>
     * @return
     */
    public static <T> List<T> listClone(String str, Class<T> clazz) {
        if(CheckUtils.isEmpty(str)){
            return new ArrayList<>();
        }
        List<T> list = JSONArray.parseArray(str, clazz);
        return list;
    }

    /**
     * 通过map键值对将对象列表转换为指定格式的key:value列表
     *
     * @param ts
     * @param map
     * @param <T>
     * @return
     */
    public static <T> List<JSONObject> list2Json(List<T> ts, Map<String, String> map) {
        String tStr = JSON.toJSONString(ts, SerializerFeature.WriteMapNullValue);
        for (Map.Entry<String, String> entry : map.entrySet()) {
            tStr = tStr.replaceAll(entry.getKey(), entry.getValue());
        }
        return listClone(tStr, JSONObject.class);
    }
    /**
     * 通过map键值对将对象列表转换为指定格式的key:value列表
     *
     * @param ts
     * @param <T>
     * @return
     */
    public static <T> List<Map> list2Map(List<T> ts) {
        String tStr = JSON.toJSONString(ts, SerializerFeature.WriteMapNullValue);
        return listClone(tStr, Map.class);
    }

    /**
     * 将一个对象克隆到另外一个对象中
     *
     * @param k
     * @param clazz
     * @param <T>
     * @param <K>
     * @return
     */
    public static <T, K> T objClone(K k, Class<T> clazz) {
        T t = JSONObject.parseObject(JSON.toJSONString(k, SerializerFeature.WriteMapNullValue), clazz);
        return t;
    }

    //    public static void main(String[] args) {
    public static boolean validatioparametersNotNull(Map<String, Object> map, List<String> params) {

        for (String one :
                params) {
            Object o = map.get(one);
            if (StringUtils.isEmpty((String) o)) {
                return false;
            }
        }
        return true;
    }

    public static String doubleAdd(String kwacCateringCount, String kwacCateringCount1) {
        if (StringUtils.isEmpty(kwacCateringCount)) {
            kwacCateringCount = "0";
        }
        if (StringUtils.isEmpty(kwacCateringCount1)) {
            kwacCateringCount1 = "0";
        }
        Double d1 = Double.parseDouble(kwacCateringCount);
        Double d2 = Double.parseDouble(kwacCateringCount1);

        return (d1 + d2) + "";

    }

    /**
     * 判断该对象是否: 返回ture表示所有属性为null  返回false表示不是所有属性都是null
     */
    public static boolean isAllFieldNull(Object obj) {
        Class stuCla = (Class) obj.getClass();// 得到类对象
        Field[] fs = stuCla.getDeclaredFields();//得到属性集合
        boolean flag = true;
        for (Field f : fs) {//遍历属性
            f.setAccessible(true); // 设置属性是可以访问的(私有的也可以)
            Object val = null;// 得到此属性的值
            try {
                val = f.get(obj);
            } catch (IllegalAccessException e) {
                throw new RuntimeException(e.getMessage());
            }
            if (val != null) {//只要有1个属性不为空,那么就不是所有的属性值都为空
                flag = false;
                break;
            }
        }
        return flag;
    }

    public static SimpleDateFormat FORMAT_DATE = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");

    public static String dateFormat(Date date, String format) {
        if (date == null) {
            return "";
        }

            SimpleDateFormat formatter = StringUtils.isEmpty(format) ? FORMAT_DATE : new SimpleDateFormat(format);
            return formatter.format(date);

    }

    /**
     * 将指定字符串转换为时间，格式错误或字符串为空，返回当前时间
     *
     * @param date
     * @param format
     * @return
     */
    public static Date strFormat(String date, String format) {
        if (StringUtils.isEmpty(date)) {
            return new Date();
        }
        SimpleDateFormat sdf = new SimpleDateFormat(StringUtils.isEmpty(format) ? "yyyy-MMM-dd HH:mm:ss" : format);
        try {
            return sdf.parse(date);
        } catch (ParseException e) {
            e.printStackTrace();
            return new Date();
        }

    }

    public static Date getNextMonth() {
        Calendar theCa = Calendar.getInstance();
        theCa.setTime(new Date());
        //theCa.add(theCa.DATE, 30);
        theCa.add(theCa.MONTH, 1);
        return theCa.getTime();
    }

    /**
     * 获取下个月当前日期（如：2018-01-12-->2018-02-12）
     *
     * @param date
     * @return
     */
    public static Date getNextMonth(Date date) {
        Calendar theCa = Calendar.getInstance();
        theCa.setTime(date);
        theCa.add(theCa.MONTH, 1);
        return theCa.getTime();
    }

    /**
     * 判断是否非空但不抛异常
     *
     * @param strs
     * @return
     */
    public static boolean isNotEmptyBatch(Object... strs) {
        for (Object str : strs) {
            if (str == null || "".equals(str))
                return false;
        }
        return true;
    }

    /**
     * 将bean的属性转换为Map键值对
     *
     * @param obj
     * @return
     * @throws IntrospectionException
     * @throws InvocationTargetException
     * @throws IllegalAccessException
     */
    public static Map<String, Object> transBean2Map(Object obj) {

        if (obj == null) {
            return null;
        }
        Map<String, Object> map = null;
        try {
            map = new HashMap<String, Object>();
            BeanInfo beanInfo = Introspector.getBeanInfo(obj.getClass());
            PropertyDescriptor[] propertyDescriptors = beanInfo.getPropertyDescriptors();
            for (PropertyDescriptor property : propertyDescriptors) {
                String key = property.getName();
                // 过滤class属性
                if (!key.equals("class")) {
                    // 得到property对应的getter方法
                    Method getter = property.getReadMethod();
                    Object value = getter.invoke(obj);

                    map.put(key, value);
                }
            }
        } catch (IntrospectionException e) {
            e.printStackTrace();
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        } catch (InvocationTargetException e) {
            e.printStackTrace();
        }
        return map;
    }

    /**
     * 将bean的属性转换为Map键值对,并且可以通过keys指定转换属性的名称
     *
     * @param obj
     * @return
     * @throws IntrospectionException
     * @throws InvocationTargetException
     * @throws IllegalAccessException
     */
    public static Map<String, Object> transBean2Map(Object obj, String[] keys) {

        if (obj == null || keys == null || keys.length == 0) {
            return null;
        }
        List<String> list = Arrays.asList(keys);
        Map<String, Object> map = new HashMap<String, Object>();
        BeanInfo beanInfo = null;
        try {
            beanInfo = Introspector.getBeanInfo(obj.getClass());

            PropertyDescriptor[] propertyDescriptors = beanInfo.getPropertyDescriptors();
            for (PropertyDescriptor property : propertyDescriptors) {
                String key = property.getName();

                // 过滤class属性 和 serialVersionUID属性
                if (!key.equals("class") && !key.equals("serialVersionUID") && list.contains(key)) {
                    // 得到property对应的getter方法
                    Method getter = property.getReadMethod();
                    Object value = value = getter.invoke(obj);
                    map.put(key, value);
                }

            }
        } catch (IntrospectionException e) {
            e.printStackTrace();
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        } catch (InvocationTargetException e) {
            e.printStackTrace();
        }
        return map;
    }

    /**
     * map 模糊取值
     *
     * @param key
     * @param map
     * @return
     */
    public static List<Object> likeString(String key, Map<String, Object> map) {
        List<Object> list = new ArrayList<Object>();
        Iterator it = map.entrySet().iterator();
        while (it.hasNext()) {
            Map.Entry<String, Object> entry = (Map.Entry<String, Object>) it.next();
            if (entry.getKey().indexOf(key) != -1) {
                list.add(entry.getValue());
            }
        }
        return list;
    }

    /**
     * 将实体类转化为map，并去除空值
     *
     * @param bean
     * @return
     * @throws IntrospectionException
     * @throws IllegalAccessException
     * @throws InvocationTargetException
     */
    public static Map convertBean(Object bean) {
        Map<String ,Object> map = bean2Json(bean);
        for (Map.Entry<String, Object> entry : map.entrySet()) {
            System.out.println(entry);
            System.out.println(entry.getKey());
            System.out.println(entry.getValue());
            if(CheckUtils.isOneEmpty(entry,entry.getKey(),entry.getValue())){
                map.remove(entry.getKey());
            }
        }
        return map;
    }

    public static String strList2Str(List<String> list) {
        StringBuilder sb = new StringBuilder();
        for (String str : list
        ) {
            sb.append(str).append(",");
        }
        return sb.toString();
    }

    /**
     * 将公里数转换为度数
     * @param l
     * @return
     */
    public static Double km2Degree(Double l){
        //公式:l(弧长)=degree（圆心角）× π（圆周率）× r（半径）/180
        //转换后的公式：degree（圆心角）=l(弧长) × 180/(π（圆周率）× r（半径）)
        Double earthRadius = 6371.393;//地球半径:km
        Double degree = (180/earthRadius/Math.PI)*l;
        return degree;
    }

    public static final Double EARTH_RADIUS =  6371.393;
    /**
     * 将度数转换为公里数
     * @param degree
     * @return
     */
    public static Double degree2Km(Double degree){
        //公式:l(弧长)=degree（圆心角）× π（圆周率）× r（半径）/180
        Double l = EARTH_RADIUS/180*Math.PI*degree;//将180放在前面，降低数值
        return l;
    }

    /**
     *
     * @param list
     * @param key
     * @param format null表示使用默认格式yyyy-MM-dd HH:mm:ss
     */
    public static void formatDate(List<Map> list ,String key, String format){
        for (Map<String,Object> map:list
        ) {
            if(map.containsKey(key)){
                if(map.get(key) instanceof Long) {
                    map.put(key, Tools.dateFormat(new Date((Long)map.get(key)), format));
                }else if(map.get(key) instanceof Date){

                    map.put(key, Tools.dateFormat((Date) map.get(key), format));
                }
            }
        }
    }

    public static Double obj2Double (Object str){
        if(CheckUtils.isEmpty(str)){
            return null;
        }
        try{
            return Double.valueOf((String)str);
        }catch (Exception e){
            return null;
        }
    }
    public static Double str2Double (String str){
        if(CheckUtils.isEmpty(str)){
            return null;
        }
        try{
            return Double.valueOf(str);
        }catch (Exception e){
            return null;
        }
    }

    /**
     * 判断是否符合HH:mm格式，并返回时间
     * @param str
     * @return
     */
    public static Date isHHmm(String str){
        if(CheckUtils.isEmpty(str)){
            return null;
        }else if(str.matches("^([0-1][0-9]|[2][0-3]):([0-5][0-9])$")){
            return Tools.strFormat(str,"HH:mm");
        }
        return null;
    }

    /**
     * 判断是否是完整url
     * @param str
     * @return
     */
    public static boolean isUrl(String str){
        if(CheckUtils.isEmpty(str)){
            return false;
        }
        return str.matches("^(http|https):\\/\\/([\\w.]+\\/?)\\S*$");
    }


    /**
     * 将char数组里指定位置的数据转换为字符串
     * <p>['1','2','3'],0,2-->123</p>
     * @param cs char数组
     * @param startSub 开始下标
     * @param endSub 结束下标（结束下标也包括）
     * @return
     */
    public static String charArr2Str(char[] cs,int startSub,int endSub){
        StringBuffer sb = new StringBuffer();
        for (int k = startSub; k <= endSub; k++) {
            sb.append(cs[k]);
        }
        return sb.toString();
    }
}

```