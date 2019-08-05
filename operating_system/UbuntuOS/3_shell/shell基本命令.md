## 一、文件处理
### 创建文件

序号|命令|说明
:--:|--|--
1|touch filename|创建一个名称为filename的空文件,若文件已存在，则更新文件的创建时间
2|touch -a filename|更新文件的访问时间

### 复制文件

序号|命令|说明
:--:|--|--
1|`cp` `sourcefile` `targetfile/`|将`sourcefile文件/文件夹`复制`到targetfile/文件夹`下，若文件已存在，则强制替换
2|cp -i sourcefile targetfile|复制文件，若文件已存在，则提示是否替换

表格记录已有的命令，还有一个是详细的命令说明