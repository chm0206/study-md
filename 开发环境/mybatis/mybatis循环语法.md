##1. in查询  
关键词：列表`types(collection)`，foreach的key值：`type(item)`
传入的列表集合  List<String> type
```xml
<if test="types!=null and types.size()>0">
    AND `type`IN
    <foreach collection="types" open="(" close=")" separator="," item="type">
        #{type, jdbcType=SMALLINT}
    </foreach>
</if>
```
传入的是数组集合：String[] type  
```xml
<if test="types!=null and types.length > 0">
    AND `type`IN
    <foreach collection="types" open="(" close=")" separator="," item="type">
        #{type, jdbcType=SMALLINT}
    </foreach>
</if>
```
