---
title: Flutter基础之Dart语言入门:Future异步使用
date: 2019-12-14 11:00:00
tags: 
    - flutter
category:
    - flutter
---

Dart类库有非常多的返回Future 或者 Stream 对象的函数，这些函数被称为异步函数，它们只会被设置好一些好事操作之后返回，如IO操作。

async 和 await 关键词支持异步编程


## Future 
用于处理异步操作，异步处理成功了就执行成功的操作，异步处理失败就捕获错误或者停止后续操作，一个Future只会对应一个结果，要么成功，要么失败。

Future的所有API的返回值仍然是一个**Future对象**，所以可以很方便的进行链式调用。


#### Future.then
模拟延时操作
`then`中接收异步结果并打印结果

```dart
Future.delayed(new Duration(seconds: 2),(){
   return "hi world!";
}).then((data){
   print(data);
});
```

#### Future.catchError
如果异步任务发生错误，可以在`catchError`中捕获错误，

```dart
Future.delayed(new Duration(seconds: 2),(){
   //return "hi world!";
   throw AssertionError("Error");  
}).then((data){
   //执行成功会走到这里  
   print("success");
}).catchError((e){
   //执行失败会走到这里  
   print(e);
});
```

then 接收结果，catchError铺货异常，但并非只有catchError回调才能铺货错误，then方法还有一个可选参数onError，也可以铺货异常


#### Future.whenComplete
无论异步任务执行成功或失败都需要做一些事时，
1. 可以分别在 `then` 或 `catchError `中关闭以下对话框
2. 可以使用Future的whenComplete回调

```dart
Future.delayed(new Duration(seconds: 2),(){
   //return "hi world!";
   throw AssertionError("Error");
}).then((data){
   //执行成功会走到这里 
   print(data);
}).catchError((e){
   //执行失败会走到这里   
   print(e);
}).whenComplete((){
   //无论成功或失败都会走到这里
});
```

#### Future.wait
如果需要等待多个异步任务都执行结束后做某些操作，可以使用Future.wait,它接受一个Future数组参数，
* 只有数组中所有的Future都执行成功后，才会触发then的成功回调，
* 只要有一个Future执行失败，就会触发错误回调

```dart
Future.wait([
  // 2秒后返回结果  
  Future.delayed(new Duration(seconds: 2), () {
    return "hello";
  }),
  // 4秒后返回结果  
  Future.delayed(new Duration(seconds: 4), () {
    return " world";
  })
]).then((results){
  print(results[0]+results[1]);
}).catchError((e){
  print(e);
});
```

## 使用async/await消除callback hell

```dart
task() async {
   try{
    String id = await login("alice","******");
    String userInfo = await getUserInfo(id);
    await saveUserInfo(userInfo);
    //执行接下来的操作   
   } catch(e){
    //错误处理   
    print(e);   
   }  
}
```
`async`用来表示函数时异步，定义的函数会返回一个Future对象
`await`后面是一个Future，表示等待该异步任务完成，异步完成后才会往下走，await必须出现在async函数内部
async/await只是一个语法糖，编译器或解释器最终会将其转化为一个Promise(Future)的调用链。


## Stream

Stream也是用于接收异步事件数据，和Future不同的是，它可以接收多个异步操作的结果（成功或失败），也就是说，在执行异步任务时，可以通过多次触发成功或失败事件来传递结果数据或错误异常，Stream常用于会多次读取数据的异步任务场景，如网络内容下载，文档读写等

```dart
Stream.fromFutures([
  // 1秒后返回结果
  Future.delayed(new Duration(seconds: 1), () {
    return "hello 1";
  }),
  // 抛出一个异常
  Future.delayed(new Duration(seconds: 2),(){
    throw AssertionError("Error");
  }),
  // 3秒后返回结果
  Future.delayed(new Duration(seconds: 3), () {
    return "hello 3";
  })
]).listen((data){
   print(data);
}, onError: (e){
   print(e.message);
},onDone: (){

});
```

上面的代码依次输出

```dart
I/flutter (17666): hello 1
I/flutter (17666): Error
I/flutter (17666): hello 3
```

## 网络请求 Future应用

异步最应用在网络请求，Flutter同样需要异步请求获取数据，`dio`是Flutter常用的网络请求插件，可以到pub.dev搜索查看。
同样项目中引入改插件，直接在`pubspec.yaml`文件中添加依赖
![](/image/Future-Dio-asynchronous/Future-Dio-asynchronous-png1.png)
在使用的地方引入：
![](/image/Future-Dio-asynchronous/Future-Dio-asynchronous-png2.png)

下面是项目中封装的请求通用类BaseRepository

```dart
/// 网络请求
class BaseRepository {
  Future<Map> get(String url, {Map<String, dynamic> queryParams}) async {
    final nativeD = await Bridge.getNetComParams();
    var dio = new Dio();
    final nativeParams = ValueUtil.toMap(nativeD);
    dio.options.baseUrl = nativeParams['baseUrl'];
    dio.options.headers['sbtype'] = nativeParams['sbtype'];
    dio.options.headers['sbID'] = nativeParams['sbID'];
    dio.options.headers['version'] = nativeParams['version'];
    dio.options.headers['token'] = nativeParams['token'];
    dio.options.responseType = ResponseType.json;
    if (!queryParams.containsKey('domain_id')) {
      queryParams['domain_id'] = nativeParams['domain_id'];
    }
    log('get',
        url: nativeParams['baseUrl'] + url,
        queryParams: queryParams,
        header: nativeParams);
    try {
      Response response = await dio.get(url, queryParameters: queryParams);
      debugPrint('请求数据返回:\n$response');
      return response.data;
    } catch (error) {
      rethrow;
    }
  }

  Future<Map> post(String url, {Map params}) async {
    final nativeParams = await Bridge.getNetComParams();
    var dio = new Dio();
    dio.options.baseUrl = nativeParams['baseUrl'];
    dio.options.headers['sbtype'] = nativeParams['sbtype'];
    dio.options.headers['sbID'] = nativeParams['sbID'];
    dio.options.headers['version'] = nativeParams['version'];
    dio.options.headers['token'] = nativeParams['token'];
    dio.options.responseType = ResponseType.json;
    log('post',
        url: nativeParams['baseUrl'] + url,
        queryParams: params,
        header: nativeParams);
    if (!params.containsKey('domain_id')) {
      params['domain_id'] = nativeParams['domain_id'];
    }
    try {
      Response response = await dio.post(url, data: params);
      debugPrint('请求数据返回:\n$response');
      return response.data;
    } catch (error) {
      rethrow;
    }
  }

  void log(String method,
      {String url, Map<String, dynamic> queryParams, Map header}) {
    debugPrint(
        '\nurl: $url  method: $method\nheader: $header \nparams: $queryParams\n');
  }
}
```
dio相关设置

```dart
1. 创建dio对象: var dio = new Dio(); 
2. 设置baseUrl: dio.options.baseUrl = 'baseUrl';
3. 设置请求头: 
    dio.options.headers['sbtype'] = nativeParams['sbtype'];
    dio.options.headers['sbID'] = nativeParams['sbID'];
    dio.options.headers['version'] = nativeParams['version'];
    dio.options.headers['token'] = nativeParams['token'];
4. 请求方式: dio.options.responseType = ResponseType.json;
5. 设置contentType: dio.options.contentType =
        ContentType.parse("application/x-www-form-urlencoded");
6. 发送请求: 
    Response response = await dio.post(url, data: params);
    debugPrint('请求数据返回:\n$response');
    return response.data;
```


