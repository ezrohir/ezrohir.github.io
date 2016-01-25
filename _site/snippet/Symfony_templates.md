### Twig
Twig defines thress types of special synatax:

+ '{{ ... }}' says something
+ {% ... %} does something
+ {# ... #} comment something

### Twig使用注意
+ 如果使用 {% extends %},必须是template的第一个tag.
+ 设计上,base templates 的{% block %}越多越好.
+ 如果在很多templates中发现相同的content,是时候将这部分内容移到parent template 的{% block %}.
+ 如果需要获得parent template的block,使用{{ parent() }}.

### Referencing Templates in a Bundle
Symfony uses a `bundle:directory:filename` string syntax for templates that live
inside a bundle.

### Template Suffix
`blog/index.html.twig` `blog/index.html.php` `blog/index.css.twig` .
By default, any Symfony template can be written in either Twig or PHP, and the last part of the extension (e.g. .twig or .php) specifies which of these two engines should be used.


### Including other Templates
```
{# app/Resources/views/article/article_details.html.twig #}
<h2>{{ article.title }}</h2>
<h3 class="byline">by {{ article.authorName }}</h3>
<p>
    {{ article.body }}
</p>

{# app/Resources/views/article/list.html.twig #}
{% extends 'layout.html.twig' %}
{% block body %}
    <h1>Recent Articles<ht>
    {% for article in articles %}
        {{ inclue('article/article_details.html.twig',{'article':article})}}
    {% end %}
{% endblock %}
```

### Embedding Controllers
```
// src/AppBundle/Controller/ArticleController.php
namespace AppBundle\Controller;
class ArticleController extends Cotrollere
{
    public function recentArticlesAction($max = 3)
    {
        $articles = ...;
        return $this->render(
            'article/recent_list.html.twig',
            array('articles'=>$articles)
        );
    }
}


{# app/Resources/views/article/recent_list.html.twig #}
{% for article in articles %}
    <a href="/article/{{ article.slug }}">
        {{ article.title }}
    </a>
{% endfor %}

{# app/Resources/views/base.html.twig #}
{# ... #}
<div id="sidebar">
    {{ render(controller(
        'AppBundle:Article:recentArticles',
        { 'max':3 }
    ))}}
</div>
```

### Linking to Pages
```
# app/config/routing.yml
article_show:
    path:  /article/{slug}
    defaults:  { _controller: AppBundle:Article:show }

{# app/Resources/views/article/recent_list.html.twig #}
{% for article in articles %}
    <a hrf="{{ path('article_show',{'slug':article.slug}) }}"
        {{ article.title }}
    </a>
{% endfor %}
```

### Linking to Assets
Symfony provides a more dynamic option via the `asset` Twig function:
```
<img src="{{ asset('image/logo.png')}}" alt="Symfony" />
<link href="{{ asset('css/blog.css') }}" rel="stylesheet" />
```

### Including Stylesheets and JavaScripts in Twig
```
{# app/Resources/views/base.html.twig #}
<html>
    <head>
        {# #}
        {% block stylesheets %}
            <link href="{{ asset('css/main.css')}}" rel="stylesheet" />
        {% endblock %}
    </head>
    <body>
        {# #}
        {% block javascripts %}
            <script scr="{{ asset('js/main.js') }}"></script>
        {% endblock %}
    </body>
</html>

{# app/Resources/views/contact/contact.html.twig #}
{% entends 'base.html.twig' %}
{% block stylesheets %}
    {{ parent() }}
    <link href="{{ asset('css/contact.css') }}" ref="stylesheet" />
{% endblock %}
```


### Globall Template Variables
During each request,Symfony will set a global template variable app in both Twig
and PHP template engines by default.
`app security` `app.user` `app.request` `app.session` `app.environment` `app.debug`


### Debugging
use `dump()` function if you need to quickly find the value of a variable passed.
```
// src/AppBundle/Controller/ArticleController.php
// ...
class ArticleController extends Controller
{
    public function recentListAction()
    {
        $articles = ...;
        dump($articles);
    }
}

{# app/Resources/views/article/recent_list.html.twig #}
{{ dump(articles) }}
{% for article in articles %}
    <a href="/article/{{ article.slug }}">
        {{ article.title }}
    </a>
{% endfor %}
```

### Syntax Checking
You can check for syntax errors in Twig templates using the lint:twig console command:
```
# You can check by filename
php app/console lint:twig app/Resources/views/article/recent_list.html.twig

# or by directory
php app/console lint:twig app/Resources/views
```
