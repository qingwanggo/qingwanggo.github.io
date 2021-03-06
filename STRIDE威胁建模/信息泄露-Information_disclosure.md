[..](./../STRIDE威胁建模/index.md)
- [信息泄露(Information disclosure)](#信息泄露information-disclosure)
  - [处理过程信息泄露](#处理过程信息泄露)
    - [保护信息关注](#保护信息关注)
    - [从处理过程的输出中寻找敏感信息](#从处理过程的输出中寻找敏感信息)
    - [利用输入校验的漏洞，绕过权限检查，获取敏感信息](#利用输入校验的漏洞绕过权限检查获取敏感信息)
      - [校验内容](#校验内容)
      - [校验处理策略](#校验处理策略)
      - [具体场景](#具体场景)
    - [访问内存，获取内存中的敏感信息](#访问内存获取内存中的敏感信息)
    - [获取代码中的敏感信息（反编译、脚本、配置文件）](#获取代码中的敏感信息反编译脚本配置文件)
  - [数据存储信息泄露](#数据存储信息泄露)
    - [保护信息关注](#保护信息关注-1)
    - [绕过访问控制读取敏感数据](#绕过访问控制读取敏感数据)
      - [处理过程权限提升](#处理过程权限提升)
      - [关键文件权限控制不当](#关键文件权限控制不当)
    - [未加密或弱加密存储](#未加密或弱加密存储)
    - [直接访问存储介质](#直接访问存储介质)
  - [数据流信息泄露](#数据流信息泄露)
    - [保护信息关注](#保护信息关注-2)
      - [网络嗅探](#网络嗅探)
        - [加密保护缺陷](#加密保护缺陷)
    - [中间人攻击](#中间人攻击)

# 信息泄露(Information disclosure)

> 机密性（加密、ACL等）

## 处理过程信息泄露

### 保护信息关注

- 产品有没有定义明确的敏感数据列表？这些数据有没有定义明确的敏感级别？

- 隐私数据的收集范围是否最小化?

- 系统是否获得数据收集和使用的许可？

- 产品资料中是否明确了收集个人数据的范围、目的及处理方式？

### 从处理过程的输出中寻找敏感信息

- 处理过程的输出中是否对敏感信息做了过滤操作？

- 命令行的历史记录中是否保存了用户口令？

- 用户界面的的错误信息中是否含有敏感数据？

- 数据库的错误消息是否直接或间接的暴露给了终端用户？

- 程序崩溃后是否会产生Core文件？Core文件中是否会包含内存中的敏感信息？

### 利用输入校验的漏洞，绕过权限检查，获取敏感信息

#### 校验内容

- 是否所有输入信息在未验证前都默认为恶意数据？是否考虑格式、长度、大小、类型和范围的校验？

#### 校验处理策略

- 输入校验是在客户端做的吗？服务端会重新做校验吗？

- 输入校验使用白名单还是黑名单？

- 是否支持集中式输入校验？

- 如果需要将用户输入的内容输入呈现到浏览器，在输出到客户端浏览器之前是否经过编码？

#### 具体场景

- 需要传递给数据库的外部输入是否做了输入校验，以防止数据库访问过程中出现SQL注入？

- 是否使用了XML查询技术？如果是，是否进行了Xpath表达式的输入验证？

- 如果无法避免接受非信任的输入作为文件名，是否校验了文件名称和路径？

- 系统是否使用了LDAP技术？如果是，是否进行了LDAP查询的输入验证？

### 访问内存，获取内存中的敏感信息

- 是否在内存中长期明文存储了敏感信息？

- 是否存在直接访问内存的途径？

### 获取代码中的敏感信息（反编译、脚本、配置文件）

- 代码中有没有硬编码密钥？

- 数据库连接字符串是否加密？

## 数据存储信息泄露

### 保护信息关注

- 产品有没有定义明确的敏感数据列表？这些数据有没有定义明确的敏感级别？

- 系统收集到的个人数据时怎么存储的？

### 绕过访问控制读取敏感数据

#### 处理过程权限提升

#### 关键文件权限控制不当

- 系统中关键的文件、目录权限是怎么设置的？敏感数据文件的权限时怎么设置的？

- 是否存在多种不同的路径可以访问到敏感数据？

- 文件的名称和路径是否来自非信任的输入？

- 是否需要屏蔽搜索引擎爬虫抓取系统数据？

- 在校验数据存储的访问权限时，是否依赖于用户输入的文件路径、URL或者其他字符类输入？

- 系统中是否存在这样的应用场景：用户可以上传文件，并且其他用户可以使用http来下载这些文件？

### 未加密或弱加密存储

- 敏感数据在存储的过程中有没有做保护？

- 有没有使用自定义的加密算法？

- 密钥长度是否符合公司规范要求？

- 加密使用的密钥是怎么生成和管理的？

- 加密算法的选择是否符合规范要求？

- 密钥有没有更新周期？默认是多长？是否强制？

- 代码中是否存在硬编码密钥、口令？

- Web应用的Cookie中是否含有敏感信息？做了哪些措施保护Cookie中的敏感信息？

- 随机数是怎样生成的？

### 直接访问存储介质

- 敏感数据使用后，有没有从存储介质上彻底删除？

- 如果攻击者能够接触数据存储，能否直接从介质上读取敏感数据？

## 数据流信息泄露

### 保护信息关注

- 产品有没有定义明确的敏感数据列表？这些数据有没有定义明确的敏感级别？

- 系统收集到的个人数据，是否需要转移出运营商网络？

- 是否会在不同的业务系统之间转移个人数据？

- 系统收集到的个人数据，是否存在转移出运营商所在国家的场景？

- 如果使用了数据库的本地账号，在传输过程中是怎么保护口令的？

#### 网络嗅探

##### 加密保护缺陷

- 敏感信息在传输的过程中有没有做保护？

- 密钥有没有更新周期？默认是多长？是否强制？

- 密钥长度是否符合公司规范要求？

- 有没有使用自定义的加密算法？

- 加密算法的选择是否符合规范要求？

- 不同网元之间采用的密钥是否相同？

- 随机数是怎样生成的？

### 中间人攻击

- 是否考虑了防护中间人攻击？