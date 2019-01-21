# როგორ დავაპროგრამოთ Pagekit-ის თემა 271123

<p class="uk-article-lead">ამ ინსტრუქციაში აღვწერთ, როგორ დავაპროგრამოთ საკუთარი თემა პროექტ  Hello-ს მაგალითზე. გავარჩევთ თემის სტრუქტურას და  საჭირო ნაბიჯებს, ახალი პოზიციების და ოპციების დასამატებლად.</p>

**ყურადღება** [დასრულებული თემა](https://github.com/pagekit/example-theme) ხელმისაწვდომია Github-ზე.

## დასაწყისი

ამ სახელძღვანელოში იგულისხმება, რომ გვაქვს  Pagekit-ის მუშა ვარიანტი ლოკალური სერვერის გარემოში. თუ არა, უნდა [ჩამოვტვირთოთ] (https://pagekit.com/download) Pagekit-ი და [დავაყენოთ] (../ Getting-Start / Installation.md). შემდეგ შევიდეთ ადმინ პანელში და შევხედოთ ინტეგრირებული მაღაზიის „თემების“ განყოფილებაში.

Pagekit-ის მაღაზიაში არის ჩვენი [Hello Theme] (https://pagekit.com/marketplace/package/pagekit/theme-hello) თემებთან სამუშაო პროგრამა თავისი მაგალითებით და საერთო საფუძვლებით, რომოლიც დაგვეხმარება დავიწოთ მუშაობა.

<figure class="uk-thumbnail">
    <img src="assets/tutorial-theme-hello-plain.png" alt="Hello Theme unstyled">
    <figcaption class="uk-thumbnail-caption">Hello თემა არ წარმოადგენს არანაირ სტილს, მაგრამ გვაძლევს მყარ საწყის წერტილს საკუთარი თემის შესამუშავებლად.</figcaption>
</figure>

პიველ რიგში, Pagekit-ის მაღაზიიდან დავაყენოთ თემა და შევხედოთ ფაილების საერთო სტრუქტურას. После установки თემა *Hello* დაყენების შემდეგ განთავსდება `packages/pagekit/theme-hello` პაპკაში. თუ სამომავლოდ ჩვენ უნდა შევიმუშაოთ საკუთარი თემა, ჩვენი რეკომენდაციია იქნება კიდევ დავაყენოთ უბრალო თემა, მაგალითად [Theme One] (https://pagekit.com/marketplace/package/pagekit/theme-one). ასე ჩვენ გვექნება საშუალება შევადაროთ სტრუქტურული ელემენტები. ამ გაკვეთილისთვის კი ჩვენთვის საკმარისია მხოლოდ თემა Hello.

## სტრუქტურა

განვიხილოთ ის ცენტრალური ფაილები და პაპკები, რომელემბთანაც აუცილებელია შეხება საკუთარი თემის შემუშავების შემთხვევაში.

```txt
/css
  theme.css               // css-ის ჩონჩხი განსაზღვრული კლასებით
/js
  theme.js                // ცარიელი ფაილი ჩვენი სკრიპტებისათვის
/views
  template.php            // თემების გამოსახვის რენდერი; ლოგო, მენიუ, ძირეული კონტენტი, გვერდითა პანელი და სქოლიოს პოზიციები ჩართულია ავტომატურად
composer.json             // პაკეტის დეფინიცია, საიდანაც Pagekit-ი გაარჩევს თამას
image.jpg                 // თემის სკრიშოტი
index.php                 // თემის ცენტრალური კონფიგურაცია
CHANGELOG.md              // შეტანილი ცვლილებების ნუსხა
README.md                 // შეიძცავს საბაზისო ინფორმაციას
```

### გადარქმევა საკუთარ თემად

Hello თემის ფაილების უბრალოდ შეცვლა გამოიწვევს იმას, რომ ნებისმიერი ამ თემის განახლება მაღაზიაში გადააწერს ფაილებს და ჩვენს მიერ შეტანილი ცვლილებები დაიკარგება. ამის გარდა, აბათ სასურველია საკუთარ თემას ერთქვას შესაბამისი საკუთარი სახელი. ამასთან, თუ სურვილი გვაქვს, იმის, რომ ეს თემა ავტვირთოთ საეთო მაღაზიაში, მაშინაც მას უნდა მივცეთ უნიკალური სახელი. ყოველივე ეს მოიცავს სამ მარტივ ნაბიჯს:

1. დავაკოპიროთ ყველა ფაილი `packages/pagekit/theme-hello`-დან to `packages/ჩვენი-სახელი/ჩვენი-თემა` პაპკაში (ჩვენ დაგვჭირდება ამ შესაბამისი პაპკების შექმნა, რა თქმა უნდა ლათინური სიბოლოებით).
2. გავხსნათ `composer.json` და შევთცვალოთ `"name": "pagekit/theme-hello",` ასე `"name": "ჩვენი-სახელი/ჩვენი-თემა"`. ასევე შევცვალოთ `"title": "Hello"` to `"title": "ჩვენი თემის სახელი"`.
3. გავხსნათ `index.php` და შევცვალოთ `'name' => 'theme-hello'` ასე `'name' => 'ჩვენი-თემა'`.

ამ სახელმძღვანელოში, შემდგომ სიმარტივისათვის მაგალითებში დავტოვებთ `theme-hello`-ს სახელით.

### CSS

Hello Theme არ შეიცავს არანაირ სტილებს. ჩართული ფაილი `css/theme.css` CSS კლასების სიას, რომელებიც აისახება которые отображаются базовыми расширениями Pagekit-ის ძირითადი გაფართოებების მიერ დამატებით სტილების გარეშე. ჩვენ შეგვიძლია ჩავამატოთ საკუთარი  CSS  კლასები.

Pagekit-ის ადმინ პანელი და თანმოყოლილი თემები იმთნება [UIkit ინტერფეისული] (http://getuikit.com/) გარემოს დახმარებით. ჩვენც ასევე შეგვიძლია გამოვიყენოთ მისი შესაძლებლობები საკუთარი პროექტის შესასრულებლად. შემდეგ მაგალითებშიც თემას ავაწყობთ UIkit-ის გამოყენებით. თუ თქვენ გამოიყენებთ სხვა ფრეიმოვრკს ან არ გსურთ რაიმე ფრეიმვორკის გამოყენება,  CSS-ის ჩართვის პროცედურა მაინც ანალოგიურია.

არის მრავალი შესაძლო ხერხი, რომლითაც შეიძლება მოვაწყოთ ჩვენი თემის ფალების სტრუქტურა. აქ ჩვენ რეკომენდაციას გავუწევთ ორ მიდგომას. პირველი მათგანი არის „კლისიკური“ ჩართვა უბრალო  CSS ფაილისა. მეორე ვარიანტი უფრო რთულია პირველ ეტაპზე მაგრამ უზრუნველყოფს დიდ მოქნილობას.

#### მარტივი მეთოდი: მარტივი  CSS ფაილების ჩართვა

გადავიდეთ [UIkit ვებსაიტზე](http://getuikit.com/), ჩამოვტვირთოთ უახლესი ვერსია და გავხსნათ. UIkit წამოდგენილია სამი ვარიატთით: Default, Gradient და Almost Flat. Default თემის ჩასართველად უნდა დაკოპირდეს `css/uikit.min.css` ფაილი და ჩაისვას `theme-hello/css/` პაპკაში.

იმაში დასარწმუნებლად, რომ ფაილი ჩაიტვირთა Pagekit-ის მიერ, გავხსნათ შაბლონის ფაილი `theme-hello/views/template.php`.

 `<head>` სექციაში ვხედავთ, რომ / CSS ფაილი უკვე ჩართულია.

```
<?php $view->style('theme', 'theme:css/theme.css') ?>
```

დავამატოთ ახალი ხაზი  UIkit CSS ფაილის დასამატებლად. ეს უნდა დაემატოს ძირეული ფაილის **ზემოთ** , როგორც ქვემოთაა ნაჩვენები.

```
<?php $view->style('custom-uikit', 'theme:css/uikit.min.css') ?>
<?php $view->style('theme', 'theme:css/theme.css') ?>
```

ამის შემდეგ თემა შეიცავს  UIkit-ის CSS-ს. ჩვენი საკუთარი CSS წესების დასამატებლად მარტივად უნდა დავარედაქტირთო `theme-hello/css/theme.css`.

<figure class="uk-thumbnail">
    <img src="assets/tutorial-theme-basic-css.png" alt="Default UIkit styling">
    <figcaption class="uk-thumbnail-caption">AДобавление CSS из UIkit-დან CSS-ის დამატება გვაძლევს ძირეულ სტილებს გამოსახვით დაკაბადონებაში. იმისათვის, რომ ეს გავაკეთოთ სინამდვილეში ლამაზად, ჩვენ დაგვჭირდება რამოდენიმე კლასის დამატება,  UIkit-სტილის დაწყობა და შესაძლოა ჩვენი საკუთარი CSS-ის დამატება</figcaption>
</figure>


#### დამატებითი გზა: Gulp-ის და  LESS-ის გამოყენება

წინა თავში შევხედეთ, თუ რა მარტივია თემაში მარტივი  CSS-ფაილების დამატება. თუ თქვენ გაქვთ ვებ-საიტების კეთების გამოცდილება,მაშინ თქვეთვის სავარაუდოთ ცნობილი უნდა იყოს უფრო მოქნილი საშუალებები კონტენტის სტილიზაციისათვის. ამის კარგი მაგალითია  CSS-ის პრეპროცესორის გამოყენება, ისეთი როგორიცაა  [LESS-ი] (http://lesscss.org/). იგი მოგვცემს საშუელებას გამოვიყენოთ ცვლადები, რომლებიც აადვილებს კითხვას და ჩვენი კოდის მართვას.

UIkit-ის გამოყენებით ეს გვაძლებნ დიდ უპირატესობას: გლობალური ცვლილებების შესატანად საკმარისი იქნება ცვლადის მნიშვნელობის შეცვლა, მაგალითად, შევცვალოთ თემის ძირითადი ფერი.

Для комфортной работы с препроцессором პრეპროცესორ LESS-თან კომფორტული მუშაობისათვის ტერმინალში უნდა დავაყენოთ და მივიღოთ წვდომა რამოდენიმე ინსტრუმენტზე: *npm*, *gulp* და *bower*. თუ ისინი არ გვიყნია, მოვძებნოთ ისინი Google-ში, სადაც მრავლადაა ამ თემაზე სასწავლო მასალა.

ზემოთ ნახსენები ლოკალური სერვერის გარემოსთვის რეკომენდაციას ვაძლევ ლინუქს პლატფორმას, სადაც ყველა ეს ინსტრუმენტი ადაპტირებულია მშობლიურ დონეზე (ა. ფ)

არსებობს ფაილური სისტემის რამოდენიმე შესაძლო მოწყობა. შემდეგ ნაწყვეში ჩვენ გვაქვს შემოთავაზება სტრუქჭტურის, რომელიც ასევე გამოიყენება Pagekit-ის ოფიციალურ თემებში. შესაძლოა, რომ თემის შექმნის პირველ ეტაპზე ეს მოგვეჩვენოს, რომ ეტაპები ბევრია, მაგრამ გამოცდილებამ აჩვენა, რომ ეს მოწყობ საშუალებას იძლევა მივიღოთ კარგად სრუქტუირებული კოდი, UIkit-ის მარტივი მოწყობა და მარტივი მექანიზმი UIkit-ის აქტუალურ მდგომარეობის გამოყენება Bower-ის დახმარებით.

#### ნაბიჯი 1

ჩვენი თემის პაპკაში ვქმნით ფაილებს `package.json`, `bower.json`, `.bowerrc`, `gulpfile.js` და `less/theme.less`. ჩავსვათ ამ ფაილებში ქვემოთ მოყვანილი კონტენტი. თემის სახლი შევცვალოთ შესაბამისი ჩვენი თემის სახელით, თუ ის განსხვავებულია  `theme-hello`-გან.

`package.json`. ეს ფაილი განსაზღვრავს ყველა JavaScript კავშირებს, რომელბიც ყენდება `npm install` ბრძანებით და შეიცავენ რამოდენიმე npm პაკეტს, რომელებიც გამოიყენება, მაგალითად  LESS-ის  CSS-ში კომპილაციისათვის:

```
{
    "name": "theme-hello",
	"devDependencies": {
	     "bower": "*",
	     "gulp": "3.*.*", // გალპის ბოლო ვერსია ჩემთვის დაუძლეველ შეცდომას აგდებ, ამიტომ ვაყენებ იმას, რაც მუშაობს
	     "gulp-less": "*",
	     "gulp-rename": "*"
	 }
}
```


`bower.json` bower-ს ატყობინებს, რომ მან ჩათოტვირთოს უახლესი  UIkit-ის ვერსია. აგვარად, ჩვენ შეგვიძლია გავუშვათ  `bower install`, იმისათვის რომ UIkit-დან მივიღოთ მიმდინარე წყარო LESS ფალები:

```js
{
	"name": "theme-hello",
	"dependencies": {
		"uikit": "*"
	},
	"private": true
}
```

`.bowerrc` შეიცავს საკონფიგურაციო მოწყობას  bower-სთვის. По умолчанию bower ავტომატურად ყველაფერს აყენებს ქვეპაპკაში `/ bower_components` თემის პაპკის შიგნით. ჩვენ ამ პაკის მდებარეობას შევცვლით სხვით:

```js
{
    "directory": "app/assets"
}
```
**შენიშვნა** bower-ის ლოკალურად პროექტის პაპკაში მუშაობისათვის შესაძლოა დაგჭირდეთ ამის გაკეთება sudo chown -R $USER:$(id -gn $USER /home/თქვენი-იუზერი/.config


`gulpfile.js` შეიცავს ყველა ამოცანას, რომლის გაშვებაც  Gulp-ის მეშვეობითაა შესაძლებელი. ჩვენ გვინდა მხოლოდ LESS-ის CSS-ში კომპილაციის ამოცანა. მოხერხებულობისათვის ჩვენ ასევე დავუთატებთ ამოცანა `watch`, რომელიც ფაილებში რაიმე ცვლილებების აღმოჩენის შემთხვევაში შეძლებს გაუშვას  LESS-ის ავტომატური პერეკომპლაცია:

```js
var gulp       = require('gulp'),
    less       = require('gulp-less'),
    rename     = require('gulp-rename');

	gulp.task('default', function () {
	    return gulp.src('less/theme.less', {base: __dirname})
	        .pipe(less({compress: true}))
	        .pipe(rename(function (file) {
	            // კომპილირებული ფაილები უნდა შენახული იქნას  css/ პაპკაში ნაცვლად less/ პაპკისა
	            file.dirname = file.dirname.replace('less', 'css');
	        }))
	        .pipe(gulp.dest(__dirname));
    });

	gulp.task('watch', function () {
	    gulp.watch('less/*.less', ['default']);
	});

```

`less / theme.less` - ეს არის ადგილი, სადაც ჩვენ შევინახავთ ჩვენი თემის სტილებს. გახსოვდეთ, რომ თავიდან საჭიროა UIkit-ის იმპორტი, რათა ისიც დაშკომპილირდეს Gulp-ის მეშვეობით, ის რაც აღვწერეთ მაღლა.

```less
@import "uikit/uikit.less";

// icon ფონტდების გამოყენება სისტემიდან
@icon-font-path: "../../../../app/assets/uikit/fonts";

// ჩვენი თამის სტილები უნდა დაიწეროს აქ...
```


`.gitignore` - ეს ფალი არის ოპციონალური, რომელიც სასარგებლოა, როცა ჩვენს კოდს ვმართავთ Git-ის დახმარებით.  Hello თემაში ამ ფაილის ვერსია უკვე არსებებობს. ჩვენ შეგვიძლია მას დავუმატოთ ახალი ჩანაწერები, რომ ის გამოიყურებოდეს შემდეგნარიად. ჩვენ რა თმა უნდა, არ დავაფიქსირებთ bower-ის ჩამოტვირთულ ფაილებს და დაგენერირებულ CSS-ს. დაგენერირებული CSS საყურადღებოა მხოლოდ მაშინ, როცა ჩვენს თემას ვტირთავთ საიტის სერვერზე ან  Pagekit Marketplace-ზე.

```txt
/app/bundle/*
/app/assets/uikit/*
/node_modules
/css
.DS_Store
.idea
*.zip
```

**შენიშვნა** მოცემული სახელმძღვანელო დაწერილია Uikit 2.x ვერსიისათვის, მე მაქვს მცდელობა ეს ყველაფერი გადავწერო 3.X ვერსიისათვის, ამიტომ შემდეგი ნაბიჯები განსხვავებულია ოფიციალური დოკუმენტაციისაგან

#### ნაბიჯი 2

 ზემოთ მითითებული ფაილების შექმნის შემდეგ После создания указанных выше файлов перейдите в [UIkit Github რეუპოზიტორიდან] (https://github.com/uikit/uikit), უნდა ჩამოვტვირთოთ Zip-ი, გავშალოთ და გადავიდეთ `themes/default` (ან რომელიმე სხვა თემაში, სურვილისამებრ). ყურადღება მივაქციოთ, რომ ამისათვის ჩვენ გვჭირდება  Github ვერსია და არა მხოლოდ  css ვერსია, რომელიც ჩვენ ჩამოვწერეთ უბრალო მოწყობისათვის.

UIkit-ს  Github-იდან ვტვირთავთ bower-ის საშუალებით: bower install. მითითებისამებრ UIkit-ის 3.X ვესია ჩამოიქაჩება და დადგება app/asset/-ში, სადაც LESS ფაილები განთავსებულია uikit/src/less-ში. აქ გვაქვს ორი პაპკა /components და /theme. უკანასკნელი არის სუფთა ფაილების სტრუქტურით.


#### ნაბიჯი 3

შევქმნათ  `/less` პაპკა ჩვენი თემის შიგნით, დავაკოპიროთ და ჩავსვათ `theme` პაპკა. ამასთან გადმოვწეროთ uikit.theme.less ფაილი, გადავარქვათ მას სახელი theme.less

**შენიშვნა** ამ სახელის დარქმევა პირობითია, \*.CSS ფაილი დაკომპილირდება იგივე სახელით და ეს სახელი უნდა შევიდეს შაბლონის ფაილში სტილის სახელად. რადგან შაბლონის მაგალითში გამოყენებულია theme.css, სიმარტივისთვის ვარქმევთ theme.less-ს.


#### Step 4

theme.less-ში სტრიქონი  @import "components/\_import.less";  შევცვალოთ @import "../app/assets/uikit/src/less/components/\_import.less";-ით.

ახლა ჩვენ მზად გვაქვს უკვე მთლიანი სტრუქტურა, ../app/assets/uikit/src/less/components/ არის Uikit3-ის ავტომატური ფაილები, ხოლო theme/-ში განთავსებულია იგივე სტრუქტურის less ფაილები, რომელშიც უნდა შევიდეს ჩვენთვის საჭირო ცვლილებები. ხოლო დამატებითი, ჩვენი საკუთარი სტილები გაკეთდება theme.less-ში

#### ნაბიჯი 5

ამ ნაბიჯზე ყველაფერი გვაქვს კომპილაციისათვის. ტერმინალში გავხსანთ ჩვენი თემის პაპკა (მაგალითისთვის `cd pagekit/packages/theme-hello`) და გავუშვით `npm install`, `bower install` and `gulp`.

**შენიშვნა** აქაც მე გავაკეთე ცოტა განსხვავებულად. პირველი, npm მე დავაინსტალირე გლობალურად. `npm install -g npm`, ასევე `npm install -g gulp` `npm install -g gulp-rename`,
`npm install -g gulp-less` და `npm install -g gulp-header` (ეს უკანასკნელი თვითონ მოითხოვა), შემდეგ `npm install -g bower`. იმისათვის, რომ bower-ს ემუშავა ლოკალურად დამჭირდა ზემოთ ნახსენები ბრძანება  sudo chown -R $USER:$(id -gn $USER, ხოლო დანარჩენი პაკეტებისათვის npm link (npm link gulp და ა. შ.). ამით მიიღწევა ის, რომ npm კომპონენტების უარავ ფაილს არ ვწერ ჩემი პროექტის პაპკაში.

ახლა როდესაც გავიარეთ ეს ნაბიჯები, დავრწმუნდეთ რომ ჩვენი ფაილური სტრუქტურა გამოიყურება შემდეგნარად (პლიუს დამატებით ფალიბი, რომლებიც იყვნენ პაპკაში მანამდე)::

```
app/
    assets/

        uikit/                      // bower install-ის შედეგი
              src/                    // bower install-ის შედეგი
                  components/       // bower install-ის შედეგი
                  theme/            // bower install-ის შედეგი
                  uikit.less        // bower install-ის შედეგი
                  uikit.theme.less // bower install-ის შედეგი
css/
    theme.css      // gulp-ის კომპილაციის შედეგი
less/
    theme/
        ... uikit components
    	theme.less   // გადარქმეული uikit.theme.less  

node_modules/      // npm  install ან npm link-ის შედეგი
.bowerrc
bower.json
gulpfile.js
package.json
.gitignore
... თემის სხვა ფაილები
```

ფაილების ასეთი მოწყობით ჩვენ მივაღწიეთ შემდეგს:

- თემის სტილების და UIkit მოწყობის **განცალკევება**. საკუთარ სტილებს ვამატებთ "less/theme.less"-ში, UIkit-ში ცვლილებები შეგვაქვს "less/theme/"-ში
- **UIkit-ის ადვილი მოწყობა**: UIkit-ის ყოველი კომპონენტის მოწყობა მოთავსებულია თავის საკუთარ  \*.less-ფაილში. მაგალითად, იმისათვის, რომ შევცვალოთ ძირეული შრიფტის ზომა, გავხსნათ  «less/theme/base.less» და შევცვალოთ «@base-body-font-size»-ს მნიშვნელობა, შემდეგ ისევ გავუშვათ «gulp». theme პაპკაში მოთავსებული less ფაილები არის მინიმალური, რომელიც უნდა შევცვალოთ ჩვენი თემის დიზაინის შესაბამისად. ყველა ცვლილების შედეგად გვჭირდება ხელახლა გავუშვათ `gulp`-ი. 
- **UIkit-ის ადვილი განახლება**: გავუშვათ `bower install` და ჩამოვქაჩოთ Uikit-ის უახლესი ვერსია, შემდეგ ისევ `gulp` ჩვენი LESS ფააილების რეკომპილაციისათვის CSS-ში.


## Adding JavaScript

Included in Hello theme, you will find an empty JavaScript file `js/script.js`. Here, you can add your own JavaScript code. In Hello Theme, the file is loaded because it is already included in the `template.php` as follows:

```
<?php $view->script('theme', 'theme:js/theme.js') ?>
```

When including a script, it needs a unique identifier (`theme`) and the path to the script file (`theme:js/theme.js`). As you can see, you can use `theme:` as a short version for the file path to your theme directory. To add more JavaScript files, simply add more lines in the same way. Make sure to assign different identifier to the scripts. If you would call all of them `theme`, only the last one will be included.

```
<?php $view->script('theme', 'theme:js/theme.js') ?>
<?php $view->script('plugins', 'theme:js/plugins.js') ?>
<?php $view->script('slideshow', 'theme:js/slideshow.js') ?>
```

While adding multiple `script()` calls is the simplest way to include JavaScript, the `script()` helper allows for way more powerful ways to include JavaScript files which can also depend on each other. Let us look at that in more depth in the next sections.

### Adding multiple JavaScript files with dependencies

In the earlier examples we have now worked with CSS from UIkit. If you also want to use UIkit's JavaScript components and utilities, it makes sense to add the UIkit JavaScript files. Note that UIkit requires loading jQuery before you can use the UIkit JavaScript components.

Locate the previous line `script('theme', ...)` in the head of `views/template.php` and replace it with the following three lines.

```
<?php $view->script('theme-jquery', 'theme:app/assets/jquery/dist/jquery.min.js') ?>
<?php $view->script('theme-uikit', 'theme:app/assets/uikit/js/uikit.min.js', 'theme-jquery') ?>
<?php $view->script('theme', 'theme:js/theme.js', 'theme-uikit') ?>
```

This example assumes you have used the advanced setup from above, where bower has installed both UIkit and jQuery into the `app/assets` folder. If you are working with a simpler setup instead, just download and copy `jquery.min.js` and `uikit.min.js` to some place in your theme and change the paths in the example accordingly.

Note how we now add a third parameter, which defines _dependencies_ of the script that we are loading. Dependencies are other JavaScript files that have to be loaded earlier. So in the example, Pagekit will definitely make sure to load the three files in the following order: jQuery, UIkit and then our own `theme.js`. In this specific example, this mechanism does not seem very useful, because the scripts will probably be loaded in exactly the order that we put them in the `template.php`, right? That is correct, but imagine these lines being located in different files and sub-templates of your theme. Defining dependencies will make sure that Pagekit always loads the files in a correct order.

As you can already see in the example above, dependencies are referenced using the unique string identifier (e.g. `theme-jquery`). In our example, this identifier is given to the script the first time it is included using the `script()` method. So, as you have seen now, this method takes three parameters: `$view->script($identifier, $path_to_script, $dependencies)`.

To confirm this has worked, open `views/template.php` and add the following lines (`data-uk-*` is the prefix for UIkit's JavaScript components).

```
<!-- ADD id="up" to body -->
<body id="up">

	<!-- LEAVE existing content ... -->
	...

	<!-- ADD to-top-scroller -->
	<div class="uk-text-center">
       <a href="#up" data-uk-smooth-scroll=""><i class="uk-icon-caret-up"></i></a>
    </div>

	<!-- LEAVE rendering of footer section  -->
    <?= $view->render('footer') ?>

</body>
```

When you refresh the browser, you will see a small arrow that you can use to smoothly scroll to the top of the browser window. If the browser does not scroll smoothly, but jumps immediately, please check if you have written everything exactly as in the examples.

### Adding third party scripts, like jQuery

You may ask yourself why we called the included jQuery script `theme-jquery` and not simply `jquery`. In general, it is always useful to prefix your own identifiers, to avoid collisions with other extensions. However, in this specific example, the identifiers `jquery` and `uikit` are already taken, because Pagekit itself includes jQuery and UIkit. This means that you can already load these JavaScript files without including them in your theme. That way, all themes and extensions can share a single version of jQuery (and UIkit, if they use UIkit) to avoid conflicts.

```
<?php $view->script('theme', 'theme:js/theme.js', ['uikit', 'jquery']) ?>
```

As you can see in the example, the third parameter of the `script()` method can also take a list of multiple dependencies. In the earlier example we have only passed in a single string (for example `theme-jquery`). Pass in a string for a single dependency, or a list for multiple depencies — both are possible.

The currently loaded version of jQuery and UIkit depend on the current version of Pagekit. With new releases of Pagekit, the versions of these libraries will continually be updated. While this allows for always having a current version available, a potential downside would be that you need to make sure your code also works for the new versions of these libraries.

## Layout
The central files for your theme's layout are `views/template.php` and `index.php`. The actual rendering happens in the `template.php`.

When you open the `template.php`, you see a very basic setup for you to get started. Let's wrapping a container around our main content and divide the system output and sidebar into a grid.

Around **line 30** the `views/template.php` file renders the sidebar position and the actual content.

```
<!-- Render widget position -->
<?php if ($view->position()->exists('sidebar')) : ?>
    <?= $view->position('sidebar') ?>
<?php endif; ?>

<!-- Render content -->
<?= $view->render('content') ?>
```

Using UIkit's [Block](http://getuikit.com/docs/block.html) and [Utility](http://getuikit.com/docs/utility.html) components, we will create a position block and a container with a fluid width.

It is always a good idea to prefix your own classes, so they will not collide with other CSS you might be using. For example, all UIkit classes are prefixed `uk-`. To distinguish classes or IDs that come from this theme, we will use the prefix `tm-`. Consequently, we add the class and ID `tm-main` to identify the section.

```
<div id="tm-main" class="tm-main uk-block">
    <div class="uk-container uk-container-center">

        <!-- Render widget position -->
        <?php if ($view->position()->exists('sidebar')) : ?>
            <?= $view->position('sidebar') ?>
        <?php endif; ?>

        <!-- Render content -->
        <?= $view->render('content') ?>

    </div>
</div>
```

Now we want the system output and sidebar to actually be side by side. The [Grid](http://getuikit.com/docs/grid.html) component can help us here. For a more semantic layout, we will use `<main>` and `<aside>` elements for the containers.

```
<div id="tm-main" class="tm-main uk-block">
    <div class="uk-container uk-container-center">

        <div class="uk-grid" data-uk-grid-match data-uk-grid-margin>

            <main class="<?= $view->position()->exists('sidebar') ? 'uk-width-medium-3-4' : 'uk-width-1-1'; ?>">

                <!-- Render content -->
                <?= $view->render('content') ?>

            </main>

            <?php if ($view->position()->exists('sidebar')) : ?>
            <aside class="uk-width-medium-1-4">
                <?= $view->position('sidebar') ?>
            </aside>
            <?php endif; ?>

        </div>

    </div>
</div>
```

### Theme elements

To create more complex layouts, you can add your own widget positions, menus and options for both. A regular theme basically consists of widgets, menus and the page content itself.

The page *content* is nothing other than Pagekit's system output. That means that the content of any page you create will be rendered in this area.

*Widgets* are small chunks of content that you can render in different positions of your site, so that they will be displayed in specific locations of your site's markup.

To navigate through any site, you first need to set up a *menu*. For this purpose, Pagekit provides different menu positions that allow users to publish menus in several locations of the theme markup.

<figure class="uk-thumbnail">
    <img src="assets/tutorial-theme-elements.png" alt="Typical theme elements">
    <figcaption class="uk-thumbnail-caption">A typical site layout consists of the main navigation, the page content and several widget positions</figcaption>
</figure>

However, your theme needs to register all positions before. This happens in the `index.php` file through the `menus` and `positions` properties. These contain arrays of the position name and a label, which is displayed in the admin panel. This file is also used to load additional scripts and much more.

## Navbar

One of the first things you will want to render in your theme is the main navigation.

<figure class="uk-thumbnail">
    <img src="assets/tutorial-theme-menu-unstyled.png" alt="Main navigation unstyled">
    <figcaption class="uk-thumbnail-caption">By default, Hello theme renders menu items in a very simple vertical navigation.</figcaption>
</figure>

#### Step 1

Hello theme comes with the predefined *Main* menu position. When adding a new position it needs to be defined by an identifier (i.e. `main`) and a label to be displayed to the user (i.e. *Main*).

```
'menu' => [

    'main' => 'Main',

]
```

<figure class="uk-thumbnail">
    <img src="assets/tutorial-theme-menu-position.png" alt="Menu position in Site Tree">
    <figcaption class="uk-thumbnail-caption">A menu can be published to the defined positions in the Pagekit Site Tree.</figcaption>
</figure>


#### Step 2

With the concept of modularity in mind, Pagekit renders position layouts in separate files. For the navigation, create the `views/menu-navbar.php` file containing the following:

```
<?php if ($root->getDepth() === 0) : ?>
<ul class="uk-navbar-nav">
<?php endif ?>

    <?php foreach ($root->getChildren() as $node) : ?>
    <li class="<?= $node->hasChildren() ? 'uk-parent' : '' ?><?= $node->get('active') ? ' uk-active' : '' ?>" <?= ($root->getDepth() === 0 && $node->hasChildren()) ? 'data-uk-dropdown':'' ?>>
        <a href="<?= $node->getUrl() ?>"><?= $node->title ?></a>

        <?php if ($node->hasChildren()) : ?>

            <?php if ($root->getDepth() === 0) : ?>
            <div class="uk-dropdown uk-dropdown-navbar">
            <?php endif ?>

                <?php if ($root->getDepth() === 0) : ?>
                <ul class="uk-nav uk-nav-navbar">
                <?php elseif ($root->getDepth() === 1) : ?>
                <ul class="uk-nav-sub">
                <?php else : ?>
                <ul>
                <?php endif ?>
                    <?= $view->render('menu-navbar.php', ['root' => $node]) ?>
                </ul>

            <?php if ($root->getDepth() === 0) : ?>
            </div>
            <?php endif ?>

        <?php endif ?>

    </li>
    <?php endforeach ?>

<?php if ($root->getDepth() === 0) : ?>
</ul>
<?php endif ?>
```

#### Step 3

To render the actual navbar in the `template.php` file, create a `<nav>` element and add the `.uk-navbar` class. Load the `menu-navbar.php` file inside the element as follows (you can remove the existing block where `$view->menu('main')` is rendered).

```
<nav class="uk-navbar">

    <?php if ($view->menu()->exists('main')) : ?>
    <div class="uk-navbar-flip">
        <?= $view->menu('main', 'menu-navbar.php') ?>
    </div>
    <?php endif ?>

</nav>
```

The main menu should now automatically be rendered in the new *Navbar* position.

#### Step 4

You will probably also want the logo to appear inside the navbar. So wrap the `<nav>` element around the logo as well and add the `.uk-navbar-brand` class, to apply the appropriate spacing.

```
<nav class="uk-navbar">

    <!-- Render logo or title with site URL -->
    <a class="uk-navbar-brand" href="<?= $view->url()->get() ?>">
        <?php if ($logo = $params['logo']) : ?>
            <img src="<?= $this->escape($logo) ?>" alt="">
        <?php else : ?>
            <?= $params['title'] ?>
        <?php endif ?>
    </a>

    <?php if ($view->menu()->exists('main')) : ?>
    <div class="uk-navbar-flip">
        <?= $view->menu('main', 'menu-navbar.php') ?>
    </div>
    <?php endif ?>

</nav>
```

<figure class="uk-thumbnail">
    <img src="assets/tutorial-theme-navbar.png" alt="Horizontal navbar">
    <figcaption class="uk-thumbnail-caption">With our changes, menu items are now rendered in a horizontal navbar.</figcaption>
</figure>

### Adding theme options

Pagekit uses [Vue.js](https://vuejs.org/) to build its administration interface. Here is a [Video tutorial](https://youtu.be/3gPGyhTroSA?list=PL2rU5GxE-MQ7aYIcxTmDh4-BTHRM-9lto) on Vue.js in Pagekit.

A frequently requested feature is for the navbar to remain fixed at the top of the browser window when scrolling down the site. In the following steps, we are going to add this as an option to our theme.

#### Step 1

First, we need to create the folder `app/components` and in it the file `site-theme.vue`. Settings stored in this file affect the entire website and can be found under *Theme* in the *Settings* tab of the Site Tree. They cannot be applied to a specific page.

<figure class="uk-thumbnail">
    <img src="assets/tutorial-theme-site-tree.png" alt="Site Tree">
    <figcaption class="uk-thumbnail-caption">You can add any kind of Settings screen to the admin area.</figcaption>
</figure>

In the freshly created file `site-theme.vue` we add the option which will be displayed in the Pagekit administration.

```
<template>

    <div class="uk-margin uk-flex uk-flex-space-between uk-flex-wrap" data-uk-margin>
        <div data-uk-margin>
            <h2 class="uk-margin-remove">{{ 'Theme' | trans }}</h2>
        </div>
        <div data-uk-margin>
            <button class="uk-button uk-button-primary" type="submit">{{ 'Save' | trans }}</button>
        </div>
    </div>

    <div class="uk-form uk-form-horizontal">

        <div class="uk-form-row">
            <label for="form-navbar-mode" class="uk-form-label">{{ 'Navbar Mode' | trans }}</label>
            <p class="uk-form-controls-condensed">
                <label><input type="checkbox" v-model="config.navbar_sticky"> {{ 'Sticky Navigation' | trans }}</label>
            </p>
        </div>

    </div>

</template>
```

#### Step 2

Now we still have to make this option available in the Site Tree. To do so, we can create a *Theme* tab in the interface by adding the following script below the option in the `site-theme.vue` file.

```
<script>

    module.exports = {

        section: {
            label: 'Theme',
            icon: 'pk-icon-large-brush',
            priority: 15
        },

        data: function () {
            return _.extend({config: {}}, window.$theme);
        },

        events: {

            save: function() {

                this.$http.post('admin/system/settings/config', {name: this.name, config: this.config}).catch(function (res) {
                    this.$notify(res.data, 'danger');
                });

            }

        }

    };

    window.Site.components['site-theme'] = module.exports;

</script>
```

#### Step 3

To load the script and add the option to the Site Tree, you also need to add the following to the `index.php` file.

```
'events' => [

    'view.system/site/admin/settings' => function ($event, $view) use ($app) {
        $view->script('site-theme', 'theme:app/bundle/site-theme.js', 'site-settings');
        $view->data('$theme', $this);
    }

]
```

#### Step 4

Add the default setting for the navbar mode in the `index.php` file.

```
'config' => [
    'navbar_sticky' => false
],
```

#### Step 5

Vue components need to be compiled into JavaScript using Webpack. To do so, create the file `webpack.config.js` in your theme folder:

```
module.exports = [

    {
        entry: {
            "site-theme": "./app/components/site-theme.vue"
        },
        output: {
            filename: "./app/bundle/[name].js"
        },
        module: {
            loaders: [
                { test: /\.vue$/, loader: "vue" }
            ]
        }
    }

];
```

#### Step 6

After that, run the command `webpack` inside the theme folder and `site-theme.vue` will be compiled into `/bundle/site-theme.js` with the template markup converted to an inline string.

If you haven't worked with it before, you will quickly need to [install webpack globally](http://webpack.github.io/docs/installation.html) and then also run the following in your project directory to have a local webpack version and a Vue compiler available.

```
npm install webpack vue-loader vue-html-loader babel-core babel-loader babel-preset-es2015 babel-plugin-transform-runtime --save-dev
```

Whenever you now apply changes to the Vue component, you need to run this task again. Alternatively, you can run `webpack --watch` or `webpack -w` which will stay active and automatically recompile whenever you change the Vue component. You can quit this command with the shortcut *Ctrl + C* For more information on Vue and Webpack, take a closer look at [this doc](https://pagekit.com/docs/developer/vuejs-and-webpack).

#### Step 7

Lastly, we want to load the necessary JavaScript dependencies in the head of our `views/template.php` file. In our case we are using the [Sticky component](http://getuikit.com/docs/sticky.html) from UIkit. Since it is not included in the framework core, it needs to be loaded explicitely. YOu can do that by adding a dependency to the sticky component when loading the theme's JavaScript.

```
<?php $view->script('theme', 'theme:js/theme.js', ['uikit-sticky']) ?>
```

Now all you need to do is render the option into the actual navbar in `template.php`.

```
<nav class="uk-navbar uk-position-z-index" <?= $params['navbar_sticky'] ? ' data-uk-sticky' : '' ?>>
```

In the admin area, go to *Site &gt; Settings &gt; Theme*  and enable the _Sticky Navigation_ option to see it take effect on your website.

## Widgets

Widget positions allow users to publish widgets in several locations of your theme markup. They appear in the *Widgets* area of the Pagekit admin panel and can be selected by the user when setting up a widget.

#### Step 1

To render a new widget position, you first need to register it with the `index.php` file. For example, if we want a create a new *Top* position, we will define it through the `positions` property.

```
'positions' => [

    'sidebar' => 'Sidebar',
    'top' => 'Top'

],
```

#### Step 2

Now that we have made the new position known to Pagekit, we need also to create a position renderer. We could skip this step and use a default renderer, but then all widgets would simply render below each pther. So to lay out the widgets in a grid, create the file `views/position-grid.php`:

```
<?php foreach ($widgets as $widget) : ?>
<div class="uk-width-medium-1-<?= count($widgets) ?>">

    <div class="uk-panel">

        <h3 class="uk-panel-title"><?= $widget->title ?></h3>

        <?= $widget->get('result') ?>

    </div>

</div>
<?php endforeach ?>
```

#### Step 3

To render the new position in the theme's markup, we need to add it to the `views/template.php` file, after the closing `</nav>` tag:

```
<?php if ($view->position()->exists('top')) : ?>
<div id="top" class="tm-top">
    <div class="uk-container uk-container-center">

        <section class="uk-grid uk-grid-match" data-uk-grid-margin>
            <?= $view->position('top', 'position-grid.php') ?>
        </section>

    </div>
</div>
<?php endif ?>
```

Now can select *Top* for any widget that you want to render in the newly created position.

<figure class="uk-thumbnail">
    <img src="assets/tutorial-theme-select-position.png" alt="Widget position">
    <figcaption class="uk-thumbnail-caption">Select the new position when creating a widget.</figcaption>
</figure>

### Adding position options
The example before added configuration options which applied for the whole site. We can also extend the Site Tree with configuration that only applies for a certain page. Let us add the option to apply a different background color to our new Top position.

#### Step 1

First, we need to create the file `node-theme.vue` inside the folder `app/components`. Here we add the option which will be displayed in the Pagekit administration. Settings that are stored in this file can be applied to each page separately by entering the appropriate item in the site tree and clicking the *Theme* tab.

```
<template>

    <div class="uk-form-horizontal">

        <div class="uk-form-row">
            <label for="form-top-style" class="uk-form-label">Top {{ 'Position' | trans }}</label>
            <div class="uk-form-controls">
                <select id="form-top-style" class="uk-form-width-large" v-model="node.theme.top_style">
                    <option value="uk-block-default">{{ 'Default' | trans }}</option>
                    <option value="uk-block-muted">{{ 'Muted' | trans }}</option>
                </select>
            </div>
        </div>

    </div>

</template>
```

#### Step 2

Now we still have to make this option available in the Site Tree. To do so, we can create a *Theme* tab in the interface by adding the following to the `node-theme.vue` file.

```
<script>

    module.exports = {

        section: {
            label: 'Theme',
            priority: 90
        },

        props: ['node']

    };

    window.Site.components['node-theme'] = module.exports;

</script>
```

#### Step 3

In the chapter about theme options, we inserted the event listener to the `index.php` file in order to load the script and add the option to the Site Tree. Now we need to do the same thing for the site setting. The complete `events` section should then look like this.

```
'events' => [

    'view.system/site/admin/settings' => function ($event, $view) use ($app) {
        $view->script('site-theme', 'theme:app/bundle/site-theme.js', 'site-settings');
        $view->data('$theme', $this);
    },

    'view.system/site/admin/edit' => function ($event, $view) {
        $view->script('node-theme', 'theme:app/bundle/node-theme.js', 'site-edit');
    }

]
```

#### Step 4

The default setting for the widget position also needs to be added in the `index.php`.

```
'node' => [

    'top_style' => 'uk-block-muted'

],
```

#### Step 5

In the chapter about theme options we created the file `webpack.config.js`. Our `node-theme.vue` file also needs to be registered to be compiled into JavaScript.

```
entry: {
    "node-theme": "./app/components/node-theme.vue",
    "site-theme": "./app/components/site-theme.vue"
},
```

#### Step 6

Now you can run the command *webpack* inside the theme folder and `node-theme.vue` will be compiled into `app/bundle/node-theme.js`.

#### Step 7

Lastly, to actually render the chosen setting into the widget position, we need to add the `.uk-block` class and the style parameter to the position itself in the `template.php` file.

```
<div id="top" class="tm-top uk-block <?= $params['top_style'] ?>">
```

#### Step 8

In the site tree, you now see a _Theme_ tab when editing a page. Here you can configure the new option. This configuration only applies for this specific page.

<figure class="uk-thumbnail">
    <img src="assets/tutorial-theme-node.png" alt="Node options added by the theme">
    <figcaption class="uk-thumbnail-caption">Node options added by the theme.</figcaption>
</figure>

### Adding widget options
You can also add specific options to widgets themselves. In this case, we would like to provide a panel style option that can be selected for each widget.

<figure class="uk-thumbnail">
    <img src="assets/tutorial-theme-widget-options.png" alt="Widget options">
    <figcaption class="uk-thumbnail-caption">A theme can add any kind of options to the Widget editor.</figcaption>
</figure>

#### Step 1

First, we need to create a `app/components/widget-theme.vue` file inside the folder `app/components`. This renders the select box from which we will be able to choose the widget's style.

```
<template>

    <div class="uk-form-horizontal">

        <div class="uk-form-row">
            <label for="form-theme-panel" class="uk-form-label">{{ 'Panel Style' | trans }}</label>
            <div class="uk-form-controls">
                <select id="form-theme-panel" class="uk-form-width-large" v-model="widget.theme.panel">
                    <option value="">{{ 'None' | trans }}</option>
                    <option value="uk-panel-box">{{ 'Box' | trans }}</option>
                    <option value="uk-panel-box uk-panel-box-primary">{{ 'Box Primary' | trans }}</option>
                    <option value="uk-panel-box uk-panel-box-secondary">{{ 'Box Secondary' | trans }}</option>
                    <option value="uk-panel-header">{{ 'Header' | trans }}</option>
                </select>
            </div>
        </div>

    </div>

</template>
```

#### Step 2

Now we still have to make this option available in the widget administration. To do so, we can create a *Theme* tab in the interface by adding the following to the `widget-theme.vue` file.

```
<script>

    module.exports = {

        section: {
            label: 'Theme',
            priority: 90
        },

        props: ['widget', 'config']

    };

    window.Widgets.components['theme'] = module.exports;

</script>
```

#### Step 3

To make the option available in the widget administration, we can create a *Theme* tab to the interface by adding the following to the `events` section in the file `index.php`.

```
'view.system/widget/edit' => function ($event, $view) {
    $view->script('widget-theme', 'theme:app/bundle/widget-theme.js', 'widget-edit');
}
```

#### Step 4

The default setting for the widget also needs to be added in the `index.php`.

```
'widget' => [

    'panel' => ''

],
```

#### Step 5

Add the `widget-theme` entry to the `webpack.config.json` file, so it will be compiled to JavaScript (don't forget to run `webpack` in the theme folder afterwards).

```
entry: {
    "node-theme": "./app/components/node-theme.vue",
    "site-theme": "./app/components/site-theme.vue",
    "widget-theme": "./app/components/widget-theme.vue"
},
```

#### Step 6

Now all we need to do is render the chosen setting into the widget in the `position-grid.php` file.

```
<div class="uk-panel <?= $widget->theme['panel'] ?>">
```

#### Step 7

When editing a widget, you will now see a _Theme_ tab on top of the editor, where you can access the _Panel Style_ configuration that we have just added.

## Overwrite system views

A theme can also customize the way how Pagekit renders static pages and blog posts. You simply overwrite system view files to apply your own layout. To do so, create corresponding folders inside your theme to mimic the original structure of the Pagekit system and put the template files there:

File                         | Original view file                       | Description
---------------------------- | ---------------------------------------- | ------------------------
`views/system/site/page.php` | `/app/system/site/views/page.php`        | Default static page view
`views/blog/post.php`        | `/packages/pagekit/blog/views/post.php`  | Blog post single view
`views/blog/posts.php`       | `/packages/pagekit/blog/views/posts.php` | Blog posts list view

To understand which variables are available in these views, look at the markup in the original view file.

## Wrapping up

In this tutorial, you have learned the basic knowledge and tools to create themes for Pagekit. Let us summarize which topics we have covered.

**Note** The [completed theme](https://github.com/pagekit/example-theme) can be found on Github.

- You are now familiar with the **file structure** of a Pagekit theme. The main entry point for configuration and custom code is the theme's `index.php`, the main template file is located at `views/template.php` inside the theme.
- We have presented an approach for a modern **front-end workflow** with tools such as LESS, Gulp and Bower. You can customize this setup to your own preferences.
- To **add JavaScript** to your theme, you can add your own code and include the script files in your template. You can also add third party libraries like UIkit and jQuery that help you to add interaction to your website.
- **Widgets and menus** are managed from the Pagekit admin area and are rendered in special positions of your theme. You have learned how to create these positions and how to change the default widget rendering.
- To make sure your theme is customizable from the admin area, you can add your own **settings screens**. These can be added to the Site Tree, to the Site settings and to the Widget editor.
- To **overwrite system views**, your theme can include custom view files to alter the way static pages and blog posts are rendered.

With these skills you are now in a position to create Pagekit themes, both for client projects and also for the Pagekit marketplace.
