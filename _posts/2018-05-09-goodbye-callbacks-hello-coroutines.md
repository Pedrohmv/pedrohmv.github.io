---
title: Goodbye callbacks. Hello Coroutines.
tags: [Android, Kotlin, Coroutines]
style: 
color: Simplifying asynchronous by expressing code sequentially.
description: On the realities of opportunity, success, reputation, and relationships in tech.
---

# Goodbye callbacks. Hello Coroutines.

Seeing an ANR(Application Not Responding) Dialog means that the UI thread has been blocked for too long. Heavy processing tasks should be done in background threads, there's some differents ways to handle this on Android.

The most common need for ansynchronous programming in modern applications is performing network calls. Using Retrofit to make theses calls lead to the callback hell. Callbacks can be replaced by `lambda expresions`, however, since Callback<T> interface has two methods lambdas can't be used. Kotlin Coroutines solves this gracefully. Coroutines simplifies asynchronous by expressing the code sequentially. This post will not explain the main concepts of coroutines.

## Code
To use Kotlin Coroutines + Retrofit we should add this to `build.gradle`:
``` groovy
kotlin {
    experimental {
        coroutines 'enable'
    }
}
```
And the dependencies:
``` groovy
dependencies {
    ...
    implementation 'com.squareup.retrofit2:retrofit:2.4.0'
    implementation 'com.jakewharton.retrofit:retrofit2-kotlin-coroutines-experimental-adapter:1.0.0'  
	implementation 'org.jetbrains.kotlinx:kotlinx-coroutines-core:0.24.0'  
	implementation 'org.jetbrains.kotlinx:kotlinx-coroutines-android:0.24.0'
}
```

A standard Retrofit call would be like:
``` kotlin
api.apiMethod().enqueue(object : Callback<ResponseObject>{
	override fun onResponse(call: Call<ResponseObject>, response: Response<ResponseObject>){
		if (response.isSucessful) {
		// handle sucessful(200 <= code < 300 ) response (update UI)
		} else {
		// handle unsucessful response
		}
	}

	override fun onFailure(call: Call<ResponseObject>, t: Throwable){
		// handle error (show network error or retry call)
	}
})
```

Coroutines version:
``` kotlin
launch{
	try{
		val call = api.apiMethod()
		val responseObject = call.await()
		// handle sucessful(200 <= code < 300 ) response (update UI)
	} catch (httpException: HttpException) {
		// handle unsucessful response
	} catch (t: Throwable) {
		// handle error (show network error or retry call)
	}
}
```

With sequential approach the code is much more easy to read and understand, and the heavy work is still being done in background. Coroutines have a pretty low learning curve compared to RxJava and in kotlin 1.3 they will no longer be experimental. Say goodbye to callbacks and hello to Coroutines.

