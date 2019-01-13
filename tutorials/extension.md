
# როგორ დავაპროგრამოთ Pagekit-ის გაფართოება


<p class="uk-article-lead">ამ სახელმძღვანელოში აღწერილია ყველა ნაბიჯი, რომელიც საჭიროა სრულყოფიფი ToDo გაფართოების შესამუშავებლად და მოსაწყობად Pagekit-ის ადმინ განყოფილებაში. მოცემულია ძირითადი ცნებები გაფართოების, კონტროლერების, მარშუტიზაციის, წარმოდგენების რენდერინგის და Вы узнаете об основных понятиях расширений, контроллерах, маршрутизации, рендеринге представлений и инфраструктуре Vue.js სამუშაო გარემოსი.</p>

## შინაარსი

<ol>
<li><a href="#step-1-extending-pagekit-using-modules">Pagekit-ის გაფართოება მოდულების საშუალებით</a></li>
<li><a href="#step-2-routing-and-controller">მარშუტიზაცია და კონტროლერები</a></li>
<li><a href="#step-3-view-rendering-and-module-config">წარმოდგენების რენტერინგიდა მოდულის კონფიგურაცია</a></li>
<li><a href="#step-4-using-vue-js-in-a-pagekit-extension"> Vue.js-ს გამოყენება Pagekit-ის გაფართოებაში</a></li>
<li><a href="#the-completed-example">დასრულებული მაგალითი</a></li>
</ol>

ინგლისურ ენაზე ხელმისაწვდომია აგრეთვე ვიეოების სერია [Youtube playlist with all screencasts](https://www.youtube.com/playlist?list=PL2rU5GxE-MQ7aYIcxTmDh4-BTHRM-9lto).

**შენიშვნა** Github-ზე არის [დასრულებული მაგალითი](https://github.com/pagekit/example-todo).

## ნაბიჯი 1: Pagekit-ის გაფართოება მოლულების გამოყნენებით


<p class="uk-article-lead">როგორც პროგრამისტმა, ადვილად შეიძლება იმ შესაძლებლობების გაფართოება, რასაც  Pagekit გვთავაზობს დასაწყისში. იმისდამიუხედავად, რა გვინდა, შევქმნათ საკუთარი თემა ან დამატებითი ფუნქციონალობის გაფართოება, ორივე აგებული იქნება ერთი და იგივე მიდგომით. ამ პირველ ეტაპზე წარმოვიდგენთ რა არის *პაკეტი(package)* და რა არის *მოდული(Module)* - ორივე დაფუძნებულია Pagekit-ის კონცეფციაზე.</p>

<a href="https://www.youtube.com/watch?v=m6ntYDOCG4s&index=1&list=PL2rU5GxE-MQ7aYIcxTmDh4-BTHRM-9lto" class="uk-button">იხ( ამ ნაბიჯის ვიდეო</a>

### ტერმინოლოგია: პაკეტი და მოდული
თემა და გაფართოება გარეგნულად შესაძლოა მოგვეჩვენოს განსხვავებულად, მაგრამ ორივეს ჩვენ ვუწოდებთ *პაკეტს*. პაკეტი - ეს არის პაპკა, რომელშიც გაერთიანებულია რამდენიმე ფაილი და შეიცავს მეტა-მონაცემებს ფაილში სახელით `composer.json`.

იმისათვის, რომ რამე დავარეგისტრიროთ Pagekit-ის სისტემაში, აგრეთვე საჭიროა ჩავრთოთ ფაილი სახელით - `index.php`. ის შეიცავს იმის განსაზღვრებას, რასაც ჩვენ ვუწოდებთ *მოდულს*. მოდული წარმოადგენს ყველაზე უფრო მნიშვნელოვან სტრუქტურას Pagekit-ის კოდში. მოდულები თავის მხრივ წარმოადგენენ ავტონომიურ მოდულებს, რომელებიც აერთიანებენ ფუნქციონალურ შესაძლებლობებს და ერთიანდებიან ერთიან სისტემაში. მოდულები გამოიყენება, როგორც Pagekit-ის ბირთვში, ასევე მესამე მხარის მიერ შემუშავებულ გაფართოებაში.

### სად უნდა განვათავსოთ ჩვენი საკუთარი პაკეტის ფაილები

ყოველი პაკეტი ეკუთვნის ავტორის მიერ განსაზღვრულ სახელს, რაც უფრო გასაგები გახდება, თუ შევხედავთ ახლად დაყენებული Pagekit-ის ფაილების წყობას.

საბაზო პაკეტები არიან Базовые пакеты являются частью пространства имен `pagekit` სახელების სივრცის ნაწილი და განთავსებული არიან კვეკატალოგში სახელად ` / packages`. ყველა მხარის პაკეტები (ჩვენი) ფლობენ საკუთარ სახელთა სივრცეს და შესაბამისად განთავსებული არიან ცალკე ქვეკადალოგში.

```
/packages
    /pagekit
        /blog
        /theme-one
    /your-vendor
        /your-theme
        /your-extension
```

ნებისმიერ პაკეტში არის მინიმუმ 2 ფაილი : `composer.json` და `index.php`.

### 1. composer.json: პაკეტის მეტა-მონაცემები

`composer.json` ფაილი შეიცავს პაკეტის მეტა-მონაცემებს. ეს მონაცემები აისახება Pagekit-ის ბექენდში და გამოიყენება Pagekit marketplace-ში, თუ ეს პაკეტი აიტვირთება იქ.

`packages/pagekit/todo/composer.json`

```
{
    "name": "pagekit/todo",
    "version": "0.1",
    "type": "pagekit-extension",
    "title": "ToDo management"
}
```

### 2. index.php: მოდულის განსაზღვრა

მოდულები კრავენ ფუნქციონალს Pagekit-ის შიგნით და საშუალებას გვაძლევენ ჩავერთოთ Pagekit სისტემაში. კოდის თვალსაზრისით, მოდულის განსაღვრება - ეს არის PHP მასივი, რომლისათვის იწერება განსაზღვრული თვისებებიдля. `Index.php`-მ ყოველთვის უნდა `დააბრუნოს` ნამდვილი მასივი.

```
<?php

use Pagekit\Application;

// packages/pagekit/todo/index.php

return [

    'name' => 'todo',

    'type' => 'extension',

	// called when Pagekit initializes the module
    'main' => function (Application $app) {
        echo "It's alive";
    }
];

?>
```

რომ შევამოწმოთ, როგორ მუშაობს, გაფართოება ჩართული უნდა იყოს ბექენდში. როცა ის აქტიურია (მწვანე ), წარწერა (It's alive) დაიბეჭდება ეკრანის მაღლითა ნაწილში.

ეს მინიმალური მაგალითი გვიჩვენებს, თუ როგორი პატარა შეიძლება იყოს მთლიანად ფუნქციონირებადი მოდული. მას აქვს დაშვება `Application` ეგზაპლიართან. ამ ობიექტის დახმარებით შესაძლებელია მივიღოთ დაშვებანებისმიერ სერვისთან, გამოვიძახოთ მოვლენა ან მოვუსმინოთ სხვა მოდულების მიერ გამოძახებულ მოლენას.

## ნაბიჯი 2: მარშუტიზაცია და კონტროლერი

<p class="uk-article-lead">With the basic structure of an extension set up, a common task is to register your own controllers and add menu items to the admin area. For that, we will look at some additional properties that you can add to the module definition in your `index.php`.</p>

<a href="https://www.youtube.com/watch?v=Hi7WGVSI3aw&index=2&list=PL2rU5GxE-MQ7aYIcxTmDh4-BTHRM-9lto" class="uk-button">Watch this step as a video</a>

### Adding a controller

A controller in Pagekit is just a plain PHP class. Every public method named properly (i.e. `someAction()`) will be mounted by Pagekit's routing (i.e. `/todo/some`).

Create a file `packages/pagekit/todo/src/Controller/TodoController.php` with the following code.

```
<?php

namespace Pagekit\Todo\Controller;

/**
 * @Access(admin=true)
 */
class TodoController
{

    public function indexAction()
    {
       return "Yay.";
    }

}
```

To limit the access to the admin area and mount this controller to an admin url, we use the annotation `@Access(admin=true)`. Annotations are keywords which are placed in the comment block above a class or method. Learn [more about annotations](http://pagekit.com/docs/developer/routing#annotations).

To use this controller, we need to autoload our namespace and mount the controller to a route. Add the following properties to the configuration array in your `index.php`.

```
	// ...

	// array of namespaces to autoload from given folders
	'autoload' => [
        'Pagekit\\Example\\' => 'src'
    ],


	// array of routes
    'routes' => [

    	// identifier to reference the route from your code
        '@todo' => [

            // which path this extension should be mounted to
            'path' => '/todo',

            // which controller to mount
            'controller' => 'Pagekit\\Todo\\Controller\\TodoController'
        ]
    ],

    // ...
```

You are now able to access the new controller at the url `<pagekit_path>/admin/todo`. Note how this URL is generated from four parts:

1. `<pagekit_path>` URLs always start from your url
1. `@Access(admin=true)` resulted in an admin url `/admin`
2. The controller is mounted as `/todo`
3. The `indexAction` is the default route at that url.

If you cannot access your route, [clear the cache](http://pagekit.com/docs/user-interface/system#cache). Pagekit caches routes for better performance.

### Debug toolbar

To see all registered routes during development, it is useful to enable the *Debug Toolbar* in Pagekit's system settings. The toolbar appears at the bottom of your screen and shows all registered routes, amongst other useful information.

Remember to turn this off for a live website.

### Adding a menu item

To add menu items use the `menu` property in your module definition. Add the following to the `index.php`.

```
// ...

'menu' => [
    'example' => [
        'label'  => 'ToDo',
        'icon'   => 'app/system/assets/images/placeholder-icon.svg',
        'url'    => '@todo',
    ]
],
// ...
```

Refresh the Pagekit backend and you will see a new menu item which links to the `@todo` route.

## Step 3: View rendering and module config

<p class="uk-article-lead">In the past steps, we have looked at the basics of modules and routing. However, our first controller only returned simple strings. In this step, let us look at actual view rendering.</p>

<a href="https://www.youtube.com/watch?v=EwCAtxcsz18&index=3&list=PL2rU5GxE-MQ7aYIcxTmDh4-BTHRM-9lto" class="uk-button">Watch this step as a video</a>

### Render a view from a controller action

To pass parameters to the view renderer, a controller action returns a PHP array. The reserved key `$view` holds parameters for the view renderer, i.e. the `name` of the view file to render.

```
public function indexAction()
{
    return [
    	'$view' => [
            'title' => 'TODO management',
            'name' => 'todo:views/admin/index.php'
        ],
        'message' => 'Hello how\'s it going?'
    ];
}
```

The rendered view file can be a plain PHP template. Simple parameters (i.e. `message`) are available as PHP variables (i.e. `$message`).

`packages/pagekit/todo/views/admin/index.php`:

```
<h1><?php echo $message; ?></h1>
```


### Resource shorthands

Instead of referencing the full path to a file, we can use a shorthand syntax. `packages/pagekit/todo/views/admin/index.php` turns into `todo:views/admin/index.php`. This is faster to type and nicer to read.

You can register shorthands with a target path in your `index.php`. In this example we want to point `todo:` to the current path of the `index.php`.

```
'resources' => [
    'todo:' => ''
],
```

### Module config

A module can come with a configuration array to hold any kind of settings. We can use this as a simple storage for our TODO items.

```
'config' => [
    'entries' => [
        ['message' => 'Buy milk.', 'done' => false], // ...
    ]
],
```

From the controller, we can access this configuration as a property of the module instance.

`src/Controller/TodoController.php`:

```
use Pagekit\Application as App;

// ...

	$module = App::module('todo');
	$config = $module->config;

// ...
```

We can store changes to the module config in the database. The changes from the default module config are merged with the stored changes so that we always have a valid configuration available in the controller.

```
// modifying the module config
App::config('todo')->set('entries', $entries);
```

## Step 4: Using Vue.js in a Pagekit extension

<p class="uk-article-lead">When building your own screens for the admin area, you can use any library you are used to. But as Pagekit comes with Vue.js included, it makes sense to look into it and see if it's the right choice for you as well. In this step, we will be introducing the basic concepts of working with Vue.js inside Pagekit.</p>

<a href="https://www.youtube.com/watch?v=3gPGyhTroSA&list=PL2rU5GxE-MQ7aYIcxTmDh4-BTHRM-9lto&index=4" class="uk-button">Watch this step as a video</a>

### 1. Passing data to JavaScript

In the past posts we have seen how data can be accessed in PHP controllers. To use this data in JavaScript, use the `$data` keyword to pass PHP arrays to the view renderer. Pagekit will automatically convert this to a JSON representation and render it in the head section of the generated HTML.


```
// packages/pagekit/todo/src/Controller/TodoController.php

// ...

public function indexAction()
{
    $module = App::module('todo');

    return [
        '$view' => [
            'name' => 'todo:views/admin/index.php'
        ],
        '$data' => $module->config
    ];
}
```

This will render the following in your `<head>` section.

```
<script>var $data = {"entries": [{"message":"Buy milk","done":false},{"message":"Drink coffee","done":true}] };</script>
```


### 2. Combine view and model

The ViewModel is a `Vue` instance that syncs the data from your model with the interface of your view. This is called *reactivity* and is one of the key features of Vue. Used correctly, this helps to keep your JavaScript components small and readable.

You can attach the `Vue` instance to a DOM element (`el: '#todo'`). The model will be whatever you pass to the `data` parameter. To use data from Pagekit, take the global `window.$data` object that your view has rendered.

Any `methods` you create can be called from your template. We will create the template markup in the next step.

```

// packages/pagekit/todo/js/todo.js

$(function(){

    var vm = new Vue({

        el: '#todo',

        data: {
            entries: window.$data.entries,
        },

        methods: {

            add: function(e) {
                // ...
            },

            toggle: function(entry) {
                entry.done = !entry.done;
            },

            remove: function(entry) {
                this.entries.$remove(entry);
            },

            save: function() {
                // ...
            }

        }

    });

});
```

### 2. Markup for Vue

From PHP, we still render the view file `views/admin/index.php`. In here, we include the required JavaScript files. To make Vue available in your script, add a dependency to `vue` as a third parameter.

```
<?php $view->script('todo', 'todo:js/todo.js', 'vue') ?>
```

In your HTML, include the DOM element that the Vue instance is looking for (`<div id="todo"></div>`). Inside the element, you can use Vue directives. Directives are certain keywords that tell Vue what to do with the element.

```
<p v-if="entry.done">This will be displayed if the item has been done.</p>
```

With `@click` you can bind to the click event and call methods of your view model.

To output values from your model, you can use curly braces (`{{ entry.message }}`). Pagekit provides a `trans` filter which will replace the string with a translated alternative if there is one present for the selected locale.

```
{{ 'Save' | trans }}
```

This is how a simple view might look like.

```
<!-- packages/pagekit/todo/views/admin/index.php -->

<?php $view->script('todo', 'todo:js/todo.js', 'vue') ?>

<div id="todo">

    <button @click="save()">{{ 'Save' | trans }}</button>

    <ul>
        <li v-for="entry in entries">
            {{ entry.message }}

            <button @click="toggle(entry)">{{ entry.done ? 'Undo' : 'Do' }}</button>
        </li>
    </ul>

</div>
```

### Learn more about Vue

Vue.js is a powerful library that makes it easy to build reactive web interfaces. To learn more, checkout the [official guide](http://vuejs.org/guide/). We can also highly recommend the Laracasts [videos on Vue.js](https://laracasts.com/series/learning-vue-step-by-step). Both are great places for an overview of what Vue can do, either in written form or as videos.

<a id="complete"></a>
## The completed example

In the completed example, we have implemented the actual saving to the backend and cleaned up the source code. It combines all previous steps where we described how to create a simple Todo extension. The result is [available on Github](https://github.com/pagekit/example-todo) for you to fiddle around with.
