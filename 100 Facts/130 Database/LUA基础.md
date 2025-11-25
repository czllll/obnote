---
tags:
- db
- redis
---
* 变量定义
```lua
local a = 10          -- 定义局部变量
local name = "czl"    -- 字符串变量
```
* 字符串拼接
```lua
local full = "Hello" .. " World"   -- 使用 `..` 拼接字符串
```
* 条件语句
```lua
if a > 5 then
  return "大于5"
elseif a == 5 then
  return "等于5"
else
  return "小于5"
end

```
* 循环语句
```lua
for i = 1, 5 do
  redis.call("set", "key"..i, i)
end
```
* 表（lua的Map/Array）
```lua
local t = {}              -- 创建空表
t["name"] = "czl"         -- 键值对
t[1] = "first"            -- 数组下标从 1 开始
```
* 函数调用（调用redis命令）
```lua
redis.call('set', 'key', 'value')        -- 正常执行命令
redis.call('get', 'key')                 -- 取值
redis.pcall('set', 'key', 'value')       -- 安全调用
```
* 参数传入（Redis脚本专用）
```lua
local key = KEYS[1]
local value = ARGV[1]
redis.call('set', key, value)
```
* 执行
```lua
EVAL "<Lua脚本>" <keys数量> key1 key2 ... arg1 arg2 ...
         ↑            ↑       ↑            ↑
     Lua代码       KEYS个数   KEYS[i]     ARGV[i]

```