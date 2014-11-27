title: android-async-http
date: 2014-11-27 10:36:00
categories: Android
tags: [async,异步,网络请求]
---
[android-async-http](https://github.com/loopj/android-async-http)
封装了 http 请求，直接支持 json，gzip 压缩，相当省事
<!--more-->
```java
AsyncHttpClient client = new AsyncHttpClient();
PersitentCookieStore cookieStore = new PersistentCookieCookieStore(context);
client.setCookieStore(cookieStore);
client.post(url,params,new JsonHttpResponseHandler()){
	@Override
    public void onSuccess(int statusCode, Header[] headers, JSONObject response){
    }

    @Override
    public void onFailure(int statusCode, Header[] headers, Throwable throwable, JSONObject errorResponse) {            
    }

    @Override
    public void onFinish() {
    }
}
```