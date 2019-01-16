
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

	// გამოიძახება როცა Pagekit გააკეთებს მოდულის ინიციალიზაციას
    'main' => function (Application $app) {
        echo "ის ცოცხალია";
    }
];

?>
```

რომ შევამოწმოთ, როგორ მუშაობს, გაფართოება ჩართული უნდა იყოს ბექენდში. როცა ის აქტიურია (მწვანე ), წარწერა (It's alive) დაიბეჭდება ეკრანის მაღლითა ნაწილში.

ეს მინიმალური მაგალითი გვიჩვენებს, თუ როგორი პატარა შეიძლება იყოს მთლიანად ფუნქციონირებადი მოდული. მას აქვს დაშვება `Application` ეგზაპლიართან. ამ ობიექტის დახმარებით შესაძლებელია მივიღოთ დაშვებანებისმიერ სერვისთან, გამოვიძახოთ მოვლენა ან მოვუსმინოთ სხვა მოდულების მიერ გამოძახებულ მოლენას.

## ნაბიჯი 2: მარშუტიზაცია და კონტროლერი

<p class="uk-article-lead">მას მერე, რაც გაფართოების საბაზო სტრუქტურა დაწყობილია, ჩვეულებრივი ამოცანა ხდება ჩვენი საკუთარი კონტროლერების რეგისტრაცია და ადმინ პანელში მენიუს პუნქტების დამატება, ამისათვის განვიხილავთ რამდენიმე დამატებით თვისებას, რომელიც შესაძლებელია დაემატოს მოდულის განსაზღვრებას  `index.php` ფაილში.</p>

<a href="https://www.youtube.com/watch?v=Hi7WGVSI3aw&index=2&list=PL2rU5GxE-MQ7aYIcxTmDh4-BTHRM-9lto" class="uk-button">ნახეთ ამ ნაბიჯის ვიდეო</a>

### კონტროლერის დამატება

Pagekit-ის კონტროლერი - ეს არის PHP-ის მარტივი კლასი. ნებისმიერი, სწორ სახელიანი ( ასეთი სახის `რამეAction ()`), მონტირდება Pagekit-ის მარშუტიზაციით (ანუ `/ todo / რამე`)

სწორი სახელისთვი, რა თქმა უნდა ვიყენებთ ლათინურ ასოებს (ა. ფ.)

შევქმნათ ფაილი `packages / pagekit / todo / src / Controller / TodoController.php` შემდეგი კოდით შიგ.
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
       return "ჰეი.";
    }

}
```

იმისათვის, რომ დავუშვათ მხოლოდ ადმინისტრატორის წვდომა და მივუერთოთ  ეს კონტროლერი ადინისტრატორის URL-მისამართს უნდა გამოვიყენოთ ანოტაცია `@Access (admin = true)`. ანოტაციები - ეს არის გასაღები ტერმინები, რომელებიც თავსდებიან კლასის ან მეთოდის დასაწყისში მოყვანილ გომენტარის ბლოკში. მეტი [ანოტაციების შესახებ] (http://pagekit.com/docs/developer/routing#annotations) წაიკითხეთ ამ ბმულზე.

იმისათვის, რომ შევძლოთ ამ კონტროლერის გამოყენება, საჭიროა ავტომატურად ჩავტვირთოთ ჩვენი სახელების სივრცე და მივუერთოთ კონტროლერი მარშუტს. დავამატოთ შემდეგი თვისება index.php ფაილის კონფიგურაციის მასივში.


```
	// ...

	// სახელობითი სივრცეთა მასივი რომელიც ავტომატურად იტვირთება მოცემული პაპკიდან
	'autoload' => [
        'Pagekit\\Example\\' => 'src'
    ],


	// array of routes
    'routes' => [

    	// მარშუტის ბმულის იდენტიფიკატორი ამ კოდში
        '@todo' => [

            // რომელ გზაზე უდა დადგეს ეს კონკრეტული გაფართოება
            'path' => '/todo',

            // რომელი კონტროლერი უნდა მოებას
            'controller' => 'Pagekit\\Todo\\Controller\\TodoController'
        ]
    ],

    // ...
```

ამის მერე იხსნება წვდომა ახალ კონტროლერზე, რომელიც არის შემდეგ მისამართზე `<pagekit_გზა>/admin/todo`.

შენიშვნა: როგორ იქმნება ეს მისამართი ოთხი ნაწილისაგან:

1. `<pagekit_path>` ყველა URL-ი იწყება საიტის ძირითადი url-დან
2. `@Access(admin=true)` უერთებს ადმინისტრატორის url-ს `/admin`
3. კონტროლერი ებმება როგორც  `/todo`
4. `indexAction` არის ავტომატური მარშუტი ამ  url-თვის.

თუ შეფერხდა მარშუტთან წვდომა, უნდა [გავასუფთაოთ კეში](http://pagekit.com/docs/user-interface/system#cache). უკეთესი წარმადობისათვის Pagekit-ი ახდენს მარუტების კეშირებას.

### Debug ინსტრუმენტების პანელი

იმისათვის, რომ ვნახოთ ყველა მარშუტი პროგრამირების პროცესში, უნდა ჩაირთოს *Debug ინსტრუმენტების პანელი* Pagekit-ის სისტემური პარამეტრებიდან. ინტრუმენტების პანელი იკავებს ეკრანის ქვედა ზოლს და მაში აისეხება ყველა რეგირტრირებული მარშუტი და რიგი სხვა სასარგებლო ინფორმაცია.

არ აგამოგრჩეთ ამ ინსტრუმენტის გამორთვა ცოცხალი ვებსაიტისათვის.

### მენიუს პუნქტის დამატება

მენიუს პუნქტის დასამატებლად მოდულის აღწერაში უნდა გამოიყენოთ თვისება `menu`. დავამატოთ შემდეგი `index.php` ფაილში.

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

გავაახლოთ (დავარეფრეშოთ)  Pagekit-ის ბექენდი და დავინახავთ ახალ მეინუს ბმულს `@todo`.

## ნაბიჯი 3: ხედის (წარმოდგენის view) რენდერინგი და მოდულის კონფიგურაცია

<p class="uk-article-lead">წინა ნაბიჯებით გავიარეთ მოდულების და მარშუტიზაციის საფუძვლები. ამის მიუხედავათ, ჩვენი პირველი კონტროლერმა დაგვიწერა მხოლოდ ერთი მარტივი სტრიქონი. შემდეგ ეტაპზე განვიხილოთ ხედის ფაქტიური რენდერინგი.</p>

<a href="https://www.youtube.com/watch?v=EwCAtxcsz18&index=3&list=PL2rU5GxE-MQ7aYIcxTmDh4-BTHRM-9lto" class="uk-button">ვნახოთ ნაბიჯის ვიდეო</a>

### ხედის რენდერი კონტროლერის მოქმედებით

იმისათვის,რომ წარმოდგენას (ხედს - View) გადაეცეს პარამეტრები, კონტროლერის ექშენ-მეთოდი აბრუნებს PHP მასივს. რეზერვირებული გასაღები  `$view` შეიცავს პარამეტრებს ვიზუალური წარმოდგენის საშუალებისათვის, ანუ გამოსახვის ფაილის `სახელს`.

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

წარმოდგენის რენდერ ფაილი არის მარტივი PHP შაბლონი. მარტივი პარამეტრები (ისეთი, როგორც `message`) ხელმისაწდომია PHP ცვლადების სახით (იგივე `$message`).

`packages/pagekit/todo/views/admin/index.php`:

```
<h1><?php echo $message; ?></h1>
```


### რესურსების შემოკლება

იმის მაგივრად, რომ ვწეროთო ფაილის მთლიანი გზა, შეგვიძლია ვისარგებლოთ შემოკლებული სინტაქსით. `packages/pagekit/todo/views/admin/index.php` გამიოისახება ` todo:views/admin/index.php`. ეს უკეთ იბეჭდება და იკითხება.

შემოკლების რეგისტრაცია მიზნობრივი მისამართით ხდება ფაილში  index.php. ამ მაგალითში `todo:`-თი მივუთითებთ `index.php`-ს მიმდინარე მისამართზე.

```
'resources' => [
    'todo:' => ''
],
```

### მოდულის კონფიგურირება

მოდული შესაძლებელია მოიცეს ნებიმიერი პარამეტრების შესანახ კონფიგურაციის მასივის სახით. ჩვენს შემთხვევაში შეგვიძლია გამოვიყენოთ, როგორც მარტივი საყობი ჩვენი TODO ელემენტებისათვის.

```
'config' => [
    'entries' => [
        ['message' => 'Buy milk.', 'done' => false], // ...
    ]
],
```




კონტროლერიდან ამ კონფიგურაციას შესაძლებელია მივწვდეთ როგორც მოდულის ეგზეპლიარის პარამეტრებს.

`src/Controller/TodoController.php`:

```
use Pagekit\Application as App;

// ...

	$module = App::module('todo');
	$config = $module->config;

// ...
```

Мы можем сохранить изменения в конфигурации модуля в базе данных. Изменения из конфигурации модуля по умолчанию объединяются с сохраненными изменениями, поэтому в контроллере всегда есть действительная конфигурация.

ჩვენ შეგვიძლია შევინახოთ მონაცემთა ბაზაში მოდულის კონფიგურაციაში შეტანილი ცვლილებები. მოდულის კონფიგურაციის ცვლილებები ერთიანდება შენახულ ცვლილებებთან, ამიტომ კონტროლერში ყოველთვის იქნება ნამდვილი კონფიგურაცია.

```
// მოდულის კონფიგურაციის ცვლილება
App::config('todo')->set('entries', $entries);
```

## ნაბიჯი 4: Using Vue.js-ის გამოყენება Pagekit-ის გაფართოებაში

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
