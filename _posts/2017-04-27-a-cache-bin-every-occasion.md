How to create and use a your own Drupal 8 cache bin.

Caching is useful for data that takes a long time to compute. You can store data permanent or invalidate it based on time, an event or manually.

Besides the basic caching of rendered pages with the <em>Dynamic Page Cache module</em> in Drupal 8 you can also use the caching API to store, manage and invalidate data in your custom module.
<h2>Cache bin</h2>
Data is cached by storing it in a cache bin, for this you can use one of Drupal's build in cache bins but in some cases it may be more interesting to create your own cache bin.

The main advantage is that for every cache bin you can set  a different cache backend, such as APCu, Memcache or Redis for storage. The default backend will be the database.

Lets say we have created a repository that connects to an external REST API to gets its data and we want to cache some of the calls in our very own cache bin.

This cache bin called `foobin` needs to be defined as a service in your modules services yaml file.
{% highlight yml %}cache.foobin:
  class: Drupal\Core\Cache\CacheBackendInterface
  tags:
    - { name: cache.bin }
  factory: cache_factory:get
  arguments: [foobin]
{% endhighlight %}
Now you can use this cache bin to store your data.
{% highlight php %}
\Drupal::cache('foobin')->set($cid, $data);
{% endhighlight %}

<h2>Cache backend</h2>
In a settings.php file you can set the cache backend you want to use for this bin. If you have a different settings.php file for a development or a production environment you can set a cache backend for each one.
{% highlight php %}
$settings['cache']['bins']['foobin'] = 'cache.backend.apcu'
{% endhighlight %}