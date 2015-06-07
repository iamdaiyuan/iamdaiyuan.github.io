---
layout: post
title: Android Volley库的使用
---

{{ page.title }}
================

<p class="meta">2015-05-26 - 北京</p>

做移动端开发，很多时候是离不开网络访问的。这时候，一个得心应手的网络访问库就成了开发者手中必不可少的利器。综合Github上的开源网络库，发现评价较高的有Google出品的[Volley]({{https://android.googlesource.com/platform/frameworks/volley}})，square出品的[OkHttp]({{http://square.github.io/okhttp/}})，还有一些其他优秀的开源库，例如[ion]({{https://github.com/koush/ion}})。

这几个库胜利哥大致浏览了一下，都不错。这次的文章，就从Volley这个库的使用说起。

# 安装Volley库

## 从源代码编译

* 克隆代码： git clone https://android.googlesource.com/platform/frameworks/volley
* 编译jar： android update project -p . ant jar

将编译好的jar引入到工程中就可以了。

## 用Maven或Gradle引入
有人已经将volley的代码托管到了[github]({{https://github.com/mcxiaoke/android-volley}})，编译好了jar，我们可以方便的通过maven或gradle引入到我们的项目中。

### Maven的方式
```html
<dependency>
    <groupId>com.mcxiaoke.volley</groupId>
    <artifactId>library</artifactId>
    <version>{latest-version}</version>
</dependency>
```
当前（2015-05-26），最新版本是1.0.16
<p>
### Gradle的方式
```html
compile 'com.mcxiaoke.volley:library:1.0.+'
// or 
compile 'com.mcxiaoke.volley:library:1.0.+@aar'

```
<p>
# 使用Volley

## 简单的入门

我们用volley来访问一下google。

```java

// 使用volley时，必须要创建一个请求队列RequestQueue
RequestQueue queue = Volley.newRequestQueue(this);
String url ="http://www.google.com";

// 创建一个request
StringRequest stringRequest = new StringRequest(Request.Method.GET, url,
            new Response.Listener<String>() {
    @Override
    public void onResponse(String response) {
        Log.d("tag","Response is: "+ response.substring(0,500));
    }
}, new Response.ErrorListener() {
    @Override
    public void onErrorResponse(VolleyError error) {
        Log.e("That didn't work!");
    }
});

// 将请求添加到队列
queue.add(stringRequest);

```
<p>
	
## 创建单例队列
在我们实际的项目中，一般都会创建一个唯一的RequestQueue实例，方便我们对请求的管理。

```java

public class MyApp extends Application {

public static final String TAG = AppController.class.getSimpleName();

private RequestQueue mRequestQueue;

private static MyApp mInstance;

@Override
public void onCreate() {
    super.onCreate();
    mInstance = this;
}

public static synchronized MyApp getInstance() {
    return mInstance;
}

public RequestQueue getRequestQueue() {
    if (mRequestQueue == null) {
        mRequestQueue = Volley.newRequestQueue(getApplicationContext());
    }

    return mRequestQueue;
}

public <T> void addToRequestQueue(Request<T> req, String tag) {
    // set the default tag if tag is empty
    req.setTag(TextUtils.isEmpty(tag) ? TAG : tag);
    getRequestQueue().add(req);
}

public <T> void addToRequestQueue(Request<T> req) {
    req.setTag(TAG);
    getRequestQueue().add(req);
}

public void cancelPendingRequests(Object tag) {
    if (mRequestQueue != null) {
        mRequestQueue.cancelAll(tag);
    }
}

}

```
<p>
	
## GET

在实际的开发中，我们经常会请求JSON对象，JSON数组，XML，HTML或是文本信息。相应的，Volley为我们提供了不同的类来实现我们的请求。
有StringRequset，JsonArrayRequest，JsonObjectRequest,JsonRequest,ImageRequest。根据需要，使用对应的类就可以了。

下面请看一个请求Json对象的例子：

```java
String url = "http://my-json-feed";

JsonObjectRequest jsObjRequest = new JsonObjectRequest
        (Request.Method.GET, url, null, new Response.Listener<JSONObject>() {

    @Override
    public void onResponse(JSONObject response) {
        Log.d("tag","Response: " + response.toString());
    }
}, new Response.ErrorListener() {

    @Override
    public void onErrorResponse(VolleyError error) {
        // TODO Auto-generated method stub

    }
});

// Access the RequestQueue through your singleton class.
queue.addToRequestQueue(jsObjRequest);

```

Get请求比较简单，没有什么需要特别注意的地方。

## POST

GET请求不同的是，只要在创建请求的时候将请求类型改为POST请求，并且override Request的getParams方法即可，这个方法是用来提供Post数据的。

```java

String url = "http://api.androidhive.info/volley/person_object.json";

    JsonObjectRequest jsonObjReq = new JsonObjectRequest(Method.POST,
            url, null,
            new Response.Listener<JSONObject>() {

                @Override
                public void onResponse(JSONObject response) {
                    Log.d("TAG", response.toString());
                }
            }, new Response.ErrorListener() {

                @Override
                public void onErrorResponse(VolleyError error) {
                    VolleyLog.d("TAG", "Error: " + error.getMessage());
                }
            }) {

        @Override
        protected Map<String, String> getParams() {
            Map<String, String> params = new HashMap<String, String>();
            params.put("name", "myName");
            params.put("email", "myemail@gmail.com");
            return params;
        }
		
		@Override
		public Map<String, String> getHeaders() throws AuthFailureError {
		    HashMap<String, String> headers = new HashMap<String, String>();
			// 注意，如果是提交json数据，
			// 那么，必须设置头信息中得Content-Type为application/json，
			// 否则，提交上去的数据会是空数据。
			// 要特别注意。
		    headers.put("Content-Type", "application/json");
		    return headers;
		}
    };

// Adding request to request queue
queue.addToRequestQueue(jsonObjReq);

```
<p>
## 请求图片

```java
ImageView mImageView;
String url = "http://i.imgur.com/7spzG.png";
mImageView = (ImageView) findViewById(R.id.myImage);
...

// Retrieves an image specified by the URL, displays it in the UI.
ImageRequest request = new ImageRequest(url,
    new Response.Listener<Bitmap>() {
        @Override
        public void onResponse(Bitmap bitmap) {
            mImageView.setImageBitmap(bitmap);
        }
    }, 0, 0, null,
    new Response.ErrorListener() {
        public void onErrorResponse(VolleyError error) {
            mImageView.setImageResource(R.drawable.image_load_error);
        }
    });

// Access the RequestQueue through your singleton class.
queue.addToRequestQueue(request);

```

以上算是Volley的入门吧，在实际项目中，会遇到形形色色的坑，还需要大家多查资料多看源码，才能把这些坑填平。