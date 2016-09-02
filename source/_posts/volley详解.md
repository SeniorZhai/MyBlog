title: volley详解
date: 2014-10-14 10:20:43
categories: Android
tags: [网络,Volley]
---
<!--more-->
## 基本请求
```java
StringRequest stringRequest = new StringRequest("http://www.baidu.com",new Response.Listener<String>(){
	@Override
	public void onResponse(String response) {
		//
	}, new Response.ErrorListener() {
		@Override
		public void onErrorResopnse(VolleyError error) {
			//
		}
	}
});
// 添加请求道请求队列中
mQueue.add(stringRequest);
```
三个参数分别是目标服务器的URL地址，服务器响应成功的回调，服务器响应失败的回调
## Post
```java
StringRequest stringRequest = new StringRequest(Method.POST,url,listener,errorListenrer) {
	@Override
	protected Map<String,String> getParams() throws AuthFailureError {
		Map<String,String> map = newHash<String,String>();
		map.put("params1","value1");
		map.put("params2","value2");
		return map;
	}
}
```
## JsonRequest
JsonRequest是一个抽象类，有两个直接子类，JsonObjectRequest和JsonArrayRequest，分别用于处理JSON数据和JSON数组
```java
JsonObjectRequest jsonObjectRequest = new JsonObjectRequest("http://m.weather.com.cn/data/101010100.html",null, new Response.Listener<JSONObject>() {  
            @Override  
            public void onResponse(JSONObject response) {  
                Log.d("TAG", response.toString());  
            }  
        }, new Response.ErrorListener() {  
            @Override  
            public void onErrorResponse(VolleyError error) {  
                Log.e("TAG", error.getMessage(), error);  
            }  
        });
```
## ImageRequest
```java
ImageRequest imageRequest = new ImageRequest(
	"https://avatars3.githubusercontent.com/u/5416585?v=2&s=460",
	new Response.Listener<Bitmap>() {
		@Override  
        public void onResponse(Bitmap response) {  
            imageView.setImageBitmap(response);  
       	}
    },0,0,Config.RGB_565,new Response.ErrorListener() {  
        @Override  
        public void onErrorResponse(VolleyError error) {  
       	   imageView.setImageResource(R.drawable.default_image);  
        }  
	});
```
第三第四个参数指定允许图片最大的宽度和高度，如果指定的网络图片的宽度或高度大于这里的最大值，则会对图片进行压缩，指定为0即表示不管图片多大都不压缩。
## ImageLoader
