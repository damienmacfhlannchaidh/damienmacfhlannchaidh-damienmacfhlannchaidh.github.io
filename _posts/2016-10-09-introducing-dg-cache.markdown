---
layout: post
title: 'Programming Note: Introducing CoreDataObjectCache'
categories: Programming
excerpt: 'An objective-c based URL cache backed by Core Data.'
---

**An objective-c based URL cache backed by Core Data**

This is a network aware pass-thru cache, meaning that you make a request for a URL-based resource to the component. If the cache has the resource it will deliver it from within its store, otherwise if the cache does not have the resource, then it will fetch it for you, insert into its store and return the resource over to you. Subsequent requests for that resource will be delivered from the cache.

It is designed to cache resources for a relatively long time (between application restarts) and is optimized for that use case. It is not as fast as a pure in-memory implementation.

You don’t have to interact with Core Data directly in any way.

**Features**
* High-performance core-data backed cache, utilizing the memory efficiencies of `NSManagedObjects`.
* Configurable cache-size.
* Implemented as a singleton, so very simple to use throughout your codebase.
* Asynchronous, block-based API.
* Small and simple code-base: One class file, one matching header and a Core Data model file.
* Monitor the cache performance, cache size, hits, misses, etc.
* Honors `Expiry` HTTP headers.
* Full OCUnit test suite that can be included in your projects Test target.
* Architected to only use Apple frameworks and API’s. Does not use any third-party libraries.

**Installation**

You can grab the component and installation instructions from [GitHub](https://github.com/damienmacfhlannchaidh/CoreDataObjectCache).

**Using the cache**

Use the cache like this:

{% highlight objc %}
  //init the cache (singleton)
  CoreDataObjectCache *cache = [CoreDataObjectCache cache];
  
  //load a resource
  [cache objectWithURL:[NSURL URLWithString:@"http://www.damienmacfhlannchaidh.com/blogimages/weather1.png"] success:^(NSData *object, NSURLResponse *response, ObjectLoadSource source) {
      //Notes:
      //data = is the contents of your URL resource.
      //response =  contains the http response from your network request, if the cache had to retrieve the resource from the network.
      //response is nil if it was delivered from the cache
      //source indicates if the resource was delivered from the network or the cache
  } failure:^(NSError *error) {
      //an error occured
  }];
{% endhighlight %}

You can reset the cache at any time:

{% highlight objc %}
  CoreDataObjectCache *cache = [CoreDataObjectCache cache];
  [cache resetObjectCache];
{% endhighlight %}

Or you can grab specific stats by reading the `cacheHits`, `cacheMisses` and `totalHits` properties. A cache-miss indicates that the cache had to grab the resource from the network.

You can remove a specific resource from the cache:

{% highlight objc %}
CoreDataObjectCache *cache = [CoreDataObjectCache cache];
  
  //remove a resource from the cache
  [cache removeObjectWithURL:[NSURL URLWithString:@"http://www.damienmacfhlannchaidh.com/blogimages/weather1.png"] success:^(NSData *object, NSURLResponse *response, ObjectLoadSource source) {
      //resource removed
  } failure:^(NSError *error) {
      //an error occured
  }];
{% endhighlight %}

Load a `UIImage` from the cache:

{% highlight objc %}
  CoreDataObjectCache *cache = [CoreDataObjectCache cache];
  
  //load a resource
  [cache objectWithURL:[NSURL URLWithString:@"http://www.damienmacfhlannchaidh.com/blogimages/weather1.png"] success:^(NSData *object, NSURLResponse *response, ObjectLoadSource source) {
       UIImage *image = [UIImage imageWithData:object];
  } failure:^(NSError *error) {
      //an error occured
  }];
{% endhighlight %}

**Feedback**

Please open an GitHub Issue to provide feedback, bugs, enhancements, and suggestions.

**License**

CoreDataObjectCache is available under the MIT license.