# 实现一个符合Promise/A+规范的Promise库
## 必要条件
- 一个方法：then
- 两个值：value、reason
- 三个状态： fulfilled、padding、rejected

## 实现思路
以ajax请求为例：
 1. 创建promise实例: 传入executor(resolve, reject) 立即执行函数 两个回调
 ```javascript
 let extcutor = (resolve, reject) => {
     try{
         // todo
         resolve()
     }catch(e) {
         reject(e)
     }
     
 }
 new Promise(executor)
 ```
 2. 构造函数会进行一些列初始化，创建相应变量保存数据，并执行相应操作，具体请看步骤3-6
 3. 创建：value reason 两个参数，分别保存成功的值 失败原因
 3. 创建：status 三个状态：pending/fulfilled/rejected
 1. 创建：resolveCallbacks rejectCallbacks 存放成功或失败的回调
 1. 执行；executor,发送ajax请求
 7. 执行：then 接收onfulfilled，并push到resolveCallbacks; 接收onrejected,并push到rejectCallback
 1. 监听http状态：
    *  成功：执行resolve 1.将status值变为fulfilled; 2.创建一个微任务，遍历resovelCallbacks并执行所有callback
    *  失败：执行reject 1. 将status值变为rejected; 2. 创建一个微任务， 遍历rejectCallbacks并执行所有callback
 1. 执行完主线程所有宏任务，然后清空微任务，此时执行onfulfilled或onrejected

## 测试
- 全局安装测试工具 sudo npm instal promises-aplus-tests -g
- 执行命令：promsies-aplus-tests 文件名
- 需要暴露给测试工具一个对象用于测试

## 测试结果
![](https://public-img.51easymaster.com/image/2fddedee84d10c668db4.png)

