When creating a controller In Drupal 8 you typically would extend the `ControllerBase` class.
When you do this you can use the `create()` and `__construct()` pattern to get the services you need in that controller, by overriding the `create()` function and use the provided `$container` object.

<strong>my_module.routing.yml</strong>
{% highlight yml %}
my_module.foo_bar.page:
 path: 'foo-bar'
 defaults:
 _controller: '\Drupal\my_module\Controller\FooController::FooBarPage'
{% endhighlight %}

<strong>FooController.php</strong>
{% highlight php %}
class FooController extends ControllerBase {

 private $fooBar;

 public function __controller(FooBarInterface $fooBar) {
    $this->fooBar = $fooBar;
 }

 public static function create(ContainerInterface $container) {
     return new static(
        $container->get('foobar.service')
     );
 }

 public function fooBarPage() {
 }
}
{% endhighlight %}


While this works and this method is even used in core, the code isn't really pretty and I prefer to use dependency injection by passing the services as arguments defined in a services.yml file.
Although in some case, like when creating a <a href="https://docs.acquia.com/article/lesson-114-dependency-injection-and-plugins">Block plugin</a>, the only way is use this pattern.
<h2>Define as a service</h2>
Fortunately, in the case of controllers, there is an alternative way.
All you need to do is to register your controller as a service and use the service name in the routing instead of the class name.
Using this syntax `serivce.name:methodeName`.

<strong>my_module.services.yml</strong>
{% highlight yml %}
 services:
  foo.controller:
    class: \Drupal\my_module\Controller\FooController
    arguments: ['@foobar.service']
{% endhighlight %}
<strong>my_module.routing.yml</strong>
{% highlight yml %}
my_module.foo_bar.page:
 path: 'foo-bar'
 defaults:
   _controller: foo.controller:fooBarPage
{% endhighlight %}
As a result you can keep your controller even smaller and you can manage all your dependencies in one place.