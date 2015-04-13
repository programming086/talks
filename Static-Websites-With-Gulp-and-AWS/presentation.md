## [fit] Welcome to
## [fit] Copenhagen Frontenders

^
Introduce myself
Thanks for coming (# members)
What a turnout
Venue & Program

---

![](robocat_bg.jpg)

![inline 40%](robocat.png)

^
Introduce Robocat & what we do
Spilhuset

---

## [fit] Static Web Sites
## [fit] With Gulp & AWS

^
Introduce the talk

---

![](apps.png)

^
We work on a lot of apps
and all these apps need websites.
Static web sites are a natural fit

---

![](robocat-site.png)

---


![](bemyeyes-site.png)

---


![](breaking-site.png)

---


![](team.png)

---

![70%](hammer.png)

^
We used to used two awesome tools
What is Hammer?

---

![40%](forge.png)

^
What is Forge?

---

![inline](hammer_interface.tiff)

^
Introduce the features of hammer and how it works

---

## What are the problems?

- No toolchain control
- Limited by Liquid
- Updates are infrequent
- Compiler is integrated

---

![](riot.png)

---

![](goddamnit.gif)

---

# I can make that

---

## Automate Workflows

- Compile Sass / Coffeescript
- Minify Javascript / CSS
- Compress image files

![right 100%](gulp.png)

^
Gulp is a command line tool that automates workflows.
Define tasks that can be executed

---

## Installing Gulp

```bash
$ npm install --save-dev gulp
```

``gulpfile.js``

```javascript
var gulp = require('gulp');

gulp.task('hello-world', function() {
	console.log("Hello World");
})
```

```bash
$ gulp
Hello World
```

---

## Installing Gulp

```bash
$ npm install --save-dev gulp coffee-script
```

``gulpfile.coffescript``

```coffeescript
gulp = require 'gulp'

gulp.task 'hello-world', ->
	console.log "Hello World"
```

```bash
$ gulp
Hello World
```

---

## Compile Sass

```coffeescript
gulp = require 'gulp'
sass = require 'gulp-sass'

gulp.task 'compile-sass', ->
	gulp.src("./sass")
		.pipe(sass())
		.pipe(gulp.dest('./css'))
```

---

## Compile Sass with Sourcemaps

```coffeescript
gulp = require 'gulp'
sass = require 'gulp-sass'
sourcemaps = require 'gulp-sourcemaps'

gulp.task 'compile-sass', ->
	gulp.src("./sass")
		.pipe(sass())
		.pipe(sourcemaps.init())
		.pipe(sourcemaps.write('./maps'))
		.pipe(gulp.dest('./css'))
```

---

## Watching for changes

```coffeescript

gulp.task 'watch', ->
	gulp.watch './css', ['compile-sass']
```

---

## Watching for changes & Reloading

```coffeescript
reload = require 'gulp-livereload'

gulp.task 'watch', ->
	reload.listen '9091'

	gulp.watch './css', ['compile-sass']
```

``index.html``

```html
<script src="//localhost:9091/livereload.js"></script>
```

---

# Demo

---

![](awesome.gif)

---

## Deploying to S3

```bash
$ gem install s3_website
```

---

## Deploying to S3

``s3_website.yml``

```ruby
s3_id: <%= ENV['S3_ACCESS_KEY_ID'] %>
s3_secret: <%= ENV['S3_SECRET_ACCESS_KEY'] %>
s3_bucket: awesome-bucket-name

site: build
max_age:
    "*": 300
gzip: true

cloudfront_distribution_config:
    aliases:
        quantity: 1
        items:
            CNAME: awesomewebsite.tld
```

---

## Deploying to S3

``.env``

```ruby
S3_ACCESS_KEY_ID=YADAYADAYADA
S3_SECRET_ACCESS_KEY=TROLOLOLOLO
```

Watch the magic happen

```bash
$ gulp && s3_website push
```

---

![](startover.png)

# Startover

---

# Templates with Handlebars

![right 100%](handlebars.png)

```handlebars
{{> header}}

<div class="products">
	{{#each apps}}
		<div class="product">
			<h2>{{this.name}}</h2>
			{{img this.icon_url}}
		</div>	
	{{/each}}
</div>

{{> footer}}
```

---

# Templates with Handlebars

![right 100%](handlebars.png)

```coffeescript
opts = {
	helpers: {
		img: (path, retina = true, cls = null) ->
			rp = retinaPath(path)
			str = "<img src=\"#{config.imgpath}/#{path}\""
			if retina
				str += " data-at2x=\"#{config.imgpath}/#{rp}\""
			if typeof cls == 'string'
				str += " class=\"#{cls}\""
			str += ">"

			return str
	}
}

gulp.task 'html', ->
	gulp.src(paths.handlebars)
		.pipe(handlebars(data, opts))
		.pipe(gulp.dest(build_path))

```

---

# Sass with Bourbon

```scss
$themes: "dark" "light";
$buttons: "download", "website", "adventure"
@each $theme in $themes {
	@each $button in $buttons {
		.button.#{$theme}.#{$button} { 
			@include retina-image(
				"button-#{$class}-#{$theme}", 
				24px 12px);
		}
	}
}
```

---

# Demo

---

![](spilhuset2.png)

---

![](robocat_bg.jpg)

## robo.cat/jobs