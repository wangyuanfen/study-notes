## http报文首部字段中与缓存相关的字段

### 通用首部字段
* Cache-Control 控制缓存的行为
  * 缓存请求指令
  
    指令|参数|说明
    --|--|--
    no-cache|无|强制向源服务器再次验证
    no-store|无|不缓存请求或响应的任何内容 
    max-age = [ 秒]|必需|响应的最大Age值 
    max-stale( = [ 秒])|可省略|接收已过期的响应 
    min-fresh = [ 秒]|必需|期望在指定时间内的响应仍有效 
    no-transform|无|代理不可更改媒体类型 
    only-if-cached|无|从缓存获取资源 
    cache-extension|-|新指令标记（token）
    
  * 缓存响应指令
    
    指令|参数|说明
    --|--|--
    public|无|可向任意方提供响应的缓存
    private|可省略|仅向特定用户返回响应
    no-cache|可省略|缓存前必须先确认其有效性
    no-store|无|不缓存请求或响应的任何内容
    no-transform|无|代理不可更改媒体类型
    must-revalidate|无|可缓存但必须再向源服务器进行确认
    proxy-revalidate|无|要求中间缓存服务器对缓存的响应有效性再 进行确认
    max-age = [ 秒]|必需|响应的最大Age值
    s-maxage = [ 秒]|必需|公共缓存服务器响应的最大Age值
    cache-extension|-|新指令标记（token）
  
* Pragma 报文指令

### 请求首部字段
* If-Match 比较实体标记（ETag）
* If-None-Match 比较实体标记（与 If-Match 相反）
* If-Modified-Since 比较资源的更新时间
* If-Unmodified-Since 比较资源的更新时间（与If-Modified-Since相反）

### 响应首部字段
* E-Tag 资源的匹配信息

### 实体首部字段
* Expires 实体主体过期的日期时间
* Last-Modified 资源的最后修改日期时间
