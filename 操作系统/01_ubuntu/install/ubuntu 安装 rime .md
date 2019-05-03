## 一、安装   
1. ibus-rime安装  
1.1. 安装ibus输入法框架  
`sudo apt-get install ibus ibus-clutter ibus-gtk ibus-gtk3 ibus-qt4`   
1.2. 安装rime  
`sudo apt-get install ibus-rime`  
1.3. ibus设置输入法  
`ibus-setup`  

 
2. fcitx-rime安装  
2.1. 安装fcitx-rime  
`sudo apt-get install fcitx-rime`  
2.2. 设置fcitx输入方案  
`fcitx-config-gtk3`  
## 二、安装指定的输入方案（五笔）  
1. 复制安卓端/windows端词库到`/usr/share/rime-data`目录下    
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190426224125399.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI1NTk4NDUz,size_16,color_FFFFFF,t_70)  
2. 到`~/.config/ibus(fcitx)/rime`目录下修改`default.yaml`文件  
（ctrl+h可以显示隐藏文件）  
将指定的输入方案设置到最前面，其它的输入方案可删除可不删除  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190426224015560.png)  
## 三、设置操作方案
1. `;``'`键选择第二第三选项  
打开指定词库的*_schema.yaml文件
```$xslt
speller:
  auto_select: true
  delimiter: " ;'"
  max_code_length: 4
key_binder:
  import_preset: default
  bindings:
    - {accept: semicolon, send: 2, when: has_menu}
    - {accept: apostrophe, send: 3, when: has_menu}
    - {accept: Return,  send: Escape, when: composing }

```
2. 设置z键重复输入(linux端不可以，安卓及windows可以)
```$xslt
engine:
  translators:
    - punct_translator
    - reverse_lookup_translator
    - history_translator@history
    - table_translator
history:
  input: z
  size: 3
  initial_quality: 10000
```