title: GsonRequest
date: 2014-08-03 13:45:25
categories: Android
tags: [Android]
---
利用Gson和Volley实现URL读取JSON数据转model类
```java
GsonRequest<T> extends Request<T> {
	private final GSON gson = new Gson();
	private final Class<T> clazz;
	private final Map<String,String> headers;
	private final Listener<T> listener;

	public GsonRequest(String url,Class<T> clazz,Map<String,String> headers,Listener<T> listener,ErrorListener errorListener){
		super(Method.GET,url,errorListener);
		this.clazz = clazz;
		this.headers  headers;
		this.listener = listener;
	}	

	@Override
	public Map<String,String> getHeaders() throws AuthFailureError {
		return headers != null ? headers : super.getHeaders();
	}

	@Override
	protected void deliverResponse(T response) {
		listener.onResponse(response);
	}

	@Override
	protected Response<T> parseNetworkResponse(NetWorkResponse response) {
		try {
			String json = new String(response.data,HttpHeaderParser.parseCharset(response.headers));
			return Response.success(gson.fromJson(json,clazz),HttpHeaderParser.parseCacheHeaders(response));
		} catch (UnsupportedEncodingException e) {
            return Response.error(new ParseError(e));
        } catch (JsonSyntaxException e) {
            return Response.error(new ParseError(e));
        }
	}
}
```
- 使用
1. 创建`executeRequest`方法
```java
void executeRequest(Request request) {
        RequestManager.addRequest(request, this);
}
```
2. 调用请求
```java
executeRequest(new GsonRequest<Model>(url,Model.class,null,listener,errorListner);
```
3. 在listener中处理数据
```
new Listner<Moderl>(){
	@Override
	public void onResponse(final Model requestData) {
		// 
	}
}
```