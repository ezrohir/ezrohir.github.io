### What is a Service Container?
> A Service Container is simply a PHP object that manages the instantiation of services.

### Creating/Configuring Services in the Container
A better answer is to let the service container create the Mailer object for you.
```
# app/config/config.yml
services:
    my_mailer:
    class:        Acme\HelloBundle\Mailer
    arguments:    [sendmail]
```
An instance of the Acme\HelloBundle\Mailer object is now available via the service
container.The container is available in any traditional Symfony controller where you
can access the service of the container via the `get()` shortcut method:
```
class HelloController extends Controller
{
    // ...
    public function sendEmailAction()
    {
        $mailer = $this->get('my_mailer');
        $mailer->send('hello@world.mail',...);
    }
}
```
+ When you ask for the my_mailer service from the container.the container constructs
  object and return it,
+ Namely,a service is never constructed until it's needed.
+ As a bonus,the Mailer service is only created once and the same instance is returned
  each time you ask for the  service.

### Service Parameters
