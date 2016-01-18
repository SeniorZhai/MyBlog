title: AsyncStorage的使用
date: 2016-01-17 21:50:50
categories: ReactNa
tags: [缓存,持久化]
---
AsyncStorage是一个Key-Value存储系统，对于App是全局的。
<!--more-->
- static getItem(key:string,callback?:?(error:?error,result:?string) => void)
  + 读取key字段，将结果作为参数，传递给callback
- static setItem(key:string,value:string,callback?:?(error:?Error)=>void)
  + 将key字段的值设置为value,结果调用callback
- static removeItem(key:string,callback?:?(error:?Error)=>void)
  + 删除一个字段
- static mergeItem(key:string,value:string,callback?:?(error:?Error)=>)
  + 假设已有的值和新的值都是字符串的JSON，将两个值合并
- static clear(callback?:?(error:?Error)=>void)
  + 删除所有数据
- static getAllKeys(callback?:?(error:?Error,keys:?Array<string>)=>void)
  + 获取所有key
- static flushGetRequests()
  + 清除所有进行中的查询操作
- static multiGet(keys:Array<string>,callback?:?(errors: ?Array<Error>, result: ?Array<Array<string>>) => void)
  + 获取keys所包含的所有字段的值，返回key-value数组
- static multiSet(keysValuePairs:Array<Array<string>>,callback?:?(errors:?Array<Error>)=>void)
  + 设置keys-values，multiSet([['k1':'val1'],['k2':'val2']],cb)
- static multiRemove(keys:Array<string>,callback?:?(error:?Array<Error>)=>void)
  + 删除所有keys的数据
- static multiMerge(keyValuePairs: Array<Array<string>>, callback?: ?(errors: ?Array<Error>) => void)
  + 将多个输入的值和已有的值合并，要求都是字符串化的JSON
