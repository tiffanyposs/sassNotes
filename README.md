##SASS AND SCSS

###What is SASS

Sass and SCSS are pre-processors. SASS steps before css and it is a language on top of CSS

SASS have powerful capabilities:

* Variables
* Nesting
* Mixins
* Automatic Vendor Prefixing (moz, webkit, etc)
* Better Code
* Powerful Frameworks and Libraries



###SASS vs SCSS

####SASS

* "Syntactically Awesome Style Sheets"
* Original Language
* Shorter Syntax
* Uses indentation instead of curly braces


####SCSS

* Sassy CSS
* Newer Syntaxs
* Closer to CSS
* De-facto Standard
* Uses curly braces


###Getting Started

You will need to download

* [Sublime Text 3](https://www.sublimetext.com/3)
* [Package Control](https://packagecontrol.io/installation#st3) - for sublime packages
* You will also need to install ruby so you can install SASS

```
$ gem install sass

```


Open Sublime and go to View > Show Console. There you can run the snippet of code from the Package Control website. That will install the package controller in your sublime

Once you have Package Control installed you can install the follow packages `Command Shift + P` will show you the list of options, if you click on "Package Control - Install Package", a list will populate with the available packages.

We want to install:

* Sass
* Scss
* Emmet - shortcuts for the text editor

###Building - compiling SCSS to CSS

Once you have everything installed, you can build you first css file from a sass file

Create `style.scss` within a folder named `sass` . In that file write a simple scss script such as:

```
  body{
	background-color: #333;
	color: #800;
	p {
		color: #008;
		font-style: bold;
	}
  }

```

You can build this into a css file by running `Command B`, if it's your first time building you might need to select the non compressed one.

Now you will have a `style.css` file and a `style.css.map` file. The `style.css` will read:

```
  body {
    background-color: #333;
    color: #800; }
    body p {
      color: #008;
      font-style: bold; }

/*# sourceMappingURL=style.css.map */
```

###Setting up the File Structure

Create your html files with two folders one called `scss` and one called `css`

`Tools > Build System > New Build System`

This will create an untitled `.sublime-build` file. You can title it whatever you want and save. It will need to be saved in the directory that it defaults to `~/Library/Application\ Support/Sublime\ Text\ 3/Packages/User` root.

The doc's content should be something like this:

```
  {

  "cmd": ["sass", "--update", "$file:${file_path}/../css/${file_base_name}.css", "--stop-on-error", "--no-cache"],

  "osx":
  {

      "path": "/usr/local/bin:$PATH"
  },

  "windows":
  {
      "shell": "true"
  }

  }

```

Basically saying:

* run the sass command then put the build file in the path relative to my css file to be two levels up from the current folder then within a css folder
* Then it tells you some more info for OS X and windows

Once this file is saved, go back to `Tools > Build System` and you should see the base name of your file as an option for Build System. Select that. Now you can go to you scss file in sublime and run `command + b` to run the build. This should save the built css file into the css folder.


But what if we wanted to Sublime to build the css fill every time we save the scss file? There's a plugin for that!

`Command Shift + P` then click Install Package, then search for the plugin "SublimeOnSaveBuild" 

Now every time we save our scss file it will build it for us.


###Variables

In Sass we can make variables to hold styling that might be repeated throughout the project. For example there may be a text color that comes up multiple times, so you can make a variable for that.

The syntax is simple, at the top of your SCSS file you can simply write the variable at the top of the page:

```
  $text-color: #222222;

```

The convention is to make variable names all lower case with dashes (kebab case). You must use a $ sign at the beginning. 

If you want to call that variable you can simply do:

```
  body {
    color: $text-color;
  }

```

Variables can hold any value including lengths, colors, fonts, sizes, etc. Below is an example on how you might structure some variables in a project.

```
  // === COLORS ===
  $text-color: #222222;
  $theme-color: #170a48;
  $secondary-color: #f27731;
  $ternary-color: #ccf962;
  $link-color: $secondary-color;
  $menu-item-color: white;

  // === FONTS ===
  $text-font: Verdana, Arial, sans-serif;
  $headline-font: 'Palatino Linotype', Georgia, serif;

  // === SIZES ===
  $content-width: 960px;
  $header-height: 60px;
  $footer-height: 90px;

```

Notice how 	`$link-color` is set to another variable. Yes you can do that!

####When should you use variables

When you use the same value often is a good use, but not just that. When you use the variables a person can read all of them very quickly and get an idea of the layout, so even if something is only used a couple times or even once, variables that have explicit title that are organized can be useful.


###Partials

A SCSS separate file that y	ou can save your variables in that the builder won't convert into an additional css file. You might also just make a separate file for just colors, or other things like that.

To your SCSS folder, you can save a file called something like `_variables.scss`. The underscore is mandatory in telling your builder not to create a new file our of this.

Our builder plugin doesn't handle this properly though, so we must customize it. Go to  the following link in the top dropdown in sublime: `Sublime Text > Preferences > Package Settings > SublimeOnSaveBuild > Settings - User`. A blank file will open and this is where we can update the package settings with the below object.

```
  {
    "filename_filter": "(/|\\\\|^)(?!_)(\\w+)\\.(css|js|sass|less|scss)$",
    "build_on_save": 1
  }

```

This is basically saying, ignore files that begin with an underscore.

We can connect the partial to the main file by simply adding this line of code to the beginning of our code:

```
  @import "variables";

```

You only need to put the name of the file you are connecting, ignoring the underscore and the extension of the file name. SCSS is smart enough to know.

Another use for partials might be for linking to a normalize.css file, so when you build all of your css will be in one file.


###Mixins

Allow you to create reusable styles. You can utilize existing Mixins available, or make your own and reuse them in projects.

To create a mixing is easy. Below we create a mixin called warning

```
  @mixin warning {
	background-color: orange;
	color: #fff;
  }

```

To apply our mixin we can call it by including it by name. We also added additional styling to this class.

```
  .warning-button {
	@include warning;
	padding: 8px 12px;
  }
```


You can also do nested things like this in sass with anything is hyphenated (font-size, font-weight)

```
  @mixin large-text {
    font:{
    	size: 22px;
    	weight: bold;
    }
  }
  
  h2 {
	@include large-text;
  }

```

You can also use mixins within mixins

```

  @mixin rounded {
	border-radius: 6px;
  }

  @mixin box {
	@include rounded;
	border: 1px solid #333;
  }
  
  #header{
    @include box;
    background-color: red;
  }

```

You can also do something like this, say you want all your links on your website to have this style, you can put an a tag in the mixin, then include it outside of any selector

```
  @mixin fancy-links {
	a {
		font-style: italic;
		text-decoration: none;
	}
  }

  @include fancy-links;

```

You can also transfer all of your mixins into a partial just for mixins.

####Mixins with Arguments

You can create mixins that take arguments just like a function in JavaScript

```
  @mixin rounded($radius) {
	border-radius: $radius;
  }

  div {
    @include rounded(8px);
    width: 100%;
  }

```

See our Mixin above takes an argument $radius, so you can pass the argument when you set it in your code.


You might also have to pass an argument twice

```
  @mixin rounded($radius) {
	border-radius: $radius;
  }

  @mixin box($radius, $border) {
	@include rounded($radius);
	border: $border;
  }
  
  #header {
    @include box(8px, 1px solid #999);
  }

```

It might be hard to remember what and when to pass, so we can set up default values to these items so we don't always have to define them.

```
  @mixin rounded($radius: 6px) {
	border-radius: $radius;
  }

  @mixin box($radius: 6px, $border: 1px solid #000) {
	@include rounded($radius);
	width: 40%
  }

```

Ways to call the box Mixin look like this, notice that if it's the second argument or after and you are not calling the ones before it you must define the param you are passing it to. This is also useful because it's explicit, so you can passing all the arguments like that if it's helpful in your code.

```
  box
  box(8px)
  box(8px, 1px solid #fff)
  box($border: 1px solid #fff)

```

#####Variable Arguments

What if want multiple shadows? We can add 3 dots ("vararg") to the end of the argument so we can pass multiple shadows to be applied.

```
  @mixin box-shadow($shadows...) {
	box-shadow: $shadows;
	-moz-box-shadow: $shadows;
	-webkit-box-shadow: $shadows;
  }
  
  #header{
    @include box-shadow(2px 0px 4px #999, 1px 1px 6px $secondary-color)
  }

```

The above will apply two shaddows.


#####Add Content Directive

Below you can see a content directive. This allows you to pass content to a certain use case. See below we have a Mixin meant for IE 6 using the `* html` hack. Then we pass it the content directive `@content` then when we include the Mixin we can pass it any content, and it will be placed inside the Mixin.

```
  @mixin apply-to-ie-6 {
      * html {
    	@content;
      }
  }
  
  @include apply-to-ie-6 {
	body {
		font-size: 125%;
	}
  }


```

The output would be:

```
  * html body{
    font-size: 125%;
  }

```

[Other Hacks found Here](https://css-tricks.com/snippets/css/browser-specific-hacks/)

###Imports

Some imports will just be interpreted as regular css imports:

```
  @import url();
  @import "http://...";
  @import "filename.css";
  @import "style-screen" screen;


```

There are also SASS imports that allow for additional capabilities. We also saw this when we were including the variables and mixins files.

For example, the below is what we used before, it will go into the same directory as the current folder and look for that file. You can also include regular SCSS files in this without an underscore to import. Keep in mind though if you have a file `variables.css` and `_variables.scss` the processor won't know which one to use, so no overlapping names, even if one does us an underscore.

```
  @import "variables";

```

These can also use pathnames relative to the current folder, in case you want to have sub folders of SASS files

```
  //like this
  @import "partials/variables";
  @import "partials/mixins";
  
  //or like this (separated by commas)
  @import "partials/variables", "partials/mixins";

```

####Example

Here is an interesting example of a google font builder. The `$font` is a variable passed in of the font name. We reassign it to be unquoted (build in sass feature). Then we import the url and we interpolate it into the google font api root. Notice at the end of the url we use `#{$font}` That format of wrapping a variable name in `#{}` will show the pre-processor to interpret inside the variable name.

```
  //creating the Mixin
  @mixin google-font($font) {
    $font: unquote($font)
    @import url(https://fonts.googleapis.com/css?family=#{$font})
  }
  
  //including the Mixin
  @include google-font("Alegreya+Sans");
  @include google-font("Titillium+Web");
  
  //variable
  $text-font: "Alegreya Sans", Arial, sans-serif;
  $headline-font: "Titillium Web", Georgia, serif;
  
  //applying it to styles
  h1{
    font-family: $headline-font;
  }
  
  p{
    font-family: $text-font;
  }

```


###Nested Media Queries

You can add media queries nested inside of selectors.


```

  #main {
  width: $content-width;
  @media only screen and (max-width: 960px){
    width: auto;
    max-width: $content-width;
  }
  margin-left: auto;
  margin-right: auto;

  }

```

will compile to:

```
  #main {
    width: 960px;
    margin-left: auto;
    margin-right: auto;
  }
  
  @media only screen and (max-width: 960px) {
    #main {
      width: auto;
      max-width: 960px; 
    } 
  }

```

####Media Queries with Mixins

Consider the below example using the content derivative with a mixin that contains a media query. Then you can just include large-screens and pass the styling you want.

```
  @mixin large-screens(){
	  @media only screen and (max-width: 960px){
		  @content;
	  }
  }

  body {
    font-family: $text-font;
    color: $text-color;
    @include large-screens {
      font-size: 125%;
    }
  }


```

###Operators

####Math

You can also do math with SASS that allows you calculate things.

SASS has a shell that you can do to try out basic operations. You trigger it by running the below command.

```
  $ sass -i

```

If you were to run `3px + 4px` in the shell it would spit out 7px. You can also multiple and divide.

```
  $size: 3px * 4px // 12px*px
  $size / 6 // 6px

```

You can also do math using different measurements `1in - 4px` for example, but not all are valid. i.e. `2em - 8px`

Dividing is a little different because some of it is valid css `4px/2px` is valid css so it won't convert, so if you really want to divide you would have to do `(4px/2px)`. you can also capture numbers in a variable and then divide, that will preform the math for you.

####Color Arithmetic

You can add hex colors:

```
  #333 + #777 

```

Output > #aaaaaa

You can multiply by color scale

```
  #123456 * 2

```

Output > #2468ac

```
  #222 * #040404

```

Output > #888888



###Functions

CSS only has a few functions built in that browsers can read `rgb()` (red, green, blue), `hsl()` (hue, saturation, lightness), `calc()`.

####Built in SASS functions

* darken(color, percent) 
* lighten(color, percent)
* transparentize(color, 0-to-1)
* opacify(color, 0-to-1)
* unquote(string)


To use pseudo selectors in SASS you can put a `&` in front of them.

Below see some examples:

```
  a {
    color: $link-color
    &:hover{
      color: darken($link-color, 15%)
    }
  }


```

```
  a {
	color: $menu-item-color;
    padding: 6px 8px;
    border-bottom: 1px solid transparentize(#fefefe, 1);
    transition: border-bottom 1s;
    transition-timing-function: ease-in-out;
    &:hover{
      border-bottom: 1px solid opacify(#fefefe, .5);
    }
  }

```


####Custom Functions

Writing a custom function is similar to other programming languages. See below for a simple example of a function that adds the sum of two numbers;

```
  @function sum($left, $right){
	@return $left + $right;
  }
  
  sum(2px, 3px)

```

A more useful example might be the below functions that will take a pixel value, and the context it's in and convert it to ems

```
  @function em($pixels, $context: 16px){
	  @return ($pixels / $context) * 1em;
  }

```

Below is an example of a column functions. It calculates the width of a column in a column layout, then you can use math to calculate what the width should be. If it should span 6 columns you might want to do `6*$col` in this example.

```
  @function col-width($columns: 12, $page-width: 100%, $gap: 1%) {
	  @return ($page-width - $gap*($columns - 1)) / $columns;
  }
  
  $col: col-width($columns: 8);
  
  #content {
    float: left;
    width: 6*$col;
  }

  #sidebar {
    float: right;
    width: 2*$col;
  }

```

###Inheritance

You might sometimes want to have two classes where the second class has all of the same styles as the first, but has some additional styling. Take the below example you might want  `.critical-error` to inherit styles from `.error`, but that would require adding it to critical error or applying both classes to your html styling. 

```
  .error {
    color: red;
  }

  .critical-error{
    bottom: 1px solid red;
    font-weight: bold;
  }


```

We can solve this by using `@extend` which will add the applied styles from another. 

This is an example:

```
  .error {
    color: red;
  }

  .critical-error{
    @extend .error;
    bottom: 1px solid red;
    font-weight: bold;
  }

```

It will be computed into this:

```
  .error, .critical-error {
    color: red; }

  .critical-error {
    bottom: 1px solid red;
    font-weight: bold; }

```

`@extend` can only be used for single element selectors.

You can also do multiple inheritance on multiple levels.

```
.error {
  color: red;
}

.warning-button {
	padding: 8px 12px;
}

.cta-button {
  @extend .warning-button;
  @extend .error;
  @include rounded;
  font-weight: bold;
}

.super-cta-button {
  @extend .cta-button;
  font-size: em(20px);
}

```

####Rules

* Currently you cannot pass any selectors that have a space into a @extend expression
* When using a media query, you can only use the @extend if the elem you are extending is within the media query.

The below would work, but if the `.foo` class was outside of the media query it would not work.


```
@media screen {
  .foo{
    color: red;
  }
  .super-foo {
    @extend .foo;
    font-size: em(20px);
  }
}

```

####Extend only selectors

Extend only selectors are selectors that you use as a base for other styling.

* use a `%` sign to create an extend only selector.
* use `@extend %whatever` to call the extend inside of another selector.
* the build will error if there is an extend on a selector that doesn't exist, so, you can add a `!optional` to the end to work around that. If it exists it will use it, if not it will skip it.

```
  %highlight {
    font-style: italic;
  }

  .sub-title {
    @extend %highlight;
    @extend .foo !optional;
    font-size: em(20px);
  }

```

####@extend vs @mixin

* @extend 
  * generates a lot of selectors but not a lot of repeat code
  * limited on what you can do
* @mixin 
  * generates fewer selector but repeats code
  * bigger CSS file


After some research, after gzipping some code comparing the load of @extend and @mixin, @mixen won even thought the code is larger

[About Gzip](https://css-tricks.com/the-difference-between-minification-and-gzipping/)

###Conditionals

```
  3 < 4  // true
  4 == 5 // false
  "sass" == "sass" // true
  #444 == #333 // false
  #333 == #333 // true
  4 >= 4 // true

```
Below is an example if you want to use if and else if and else statements to define some style properties. This will allow you to export different css files. This is good for color themes etc.

```
$contrast: high;

  body {
    font-family: $text-font;
    font-size: em(18px);
    @if $contrast == high {
      color: #000;
    } @else if $contrast == low{
      color: #999;
    } @else {
      color: $text-color;
    }
  }

```

Here is an example of theming.

```
// === THEMES ===
// Allowed Values: Dark, Light, Default
$theme: Dark;

// === COLORS ===
$text-color: #222222;
$body-background-color: #fff;
$theme-color: #170a48;
$secondary-color: #f27731;
$ternary-color: #ccf962;
$link-color: $secondary-color;
$menu-item-color: white;

@if $theme == Dark {
  $text-color: #fff;
  $body-background-color: #22222a;
  $theme-color: #42424a;
  $secondary-color: #c24721;
  $ternary-color: #698932;
  $link-color: $secondary-color;
  $menu-item-color: white;
} @else if $theme == Light {
  $text-color: #000;
  $body-background-color: #fff;
  $theme-color: #372a27;
  $secondary-color: #d26741;
  $ternary-color: #b9da63;
  $link-color: $secondary-color;
  $menu-item-color: white;
}

```

Ways to use conditionals in SASS

* Theming
* Positions of sections (sidebar on left or right)
* If a sections should be shown or not.

You could also use it to target different brands you are making:

```
  
  //Allowed Values : blue-beans, rogers-rover, pastry-pizza
  $brand: rogers-rover;

  @mixin blue-beans(){
	@if $brand == blue-beans {
		@content;
	}
  }

  @mixin rogers-rover(){
	@if $brand == rogers-rover {
		@content;
	}
  }

  @mixin pastry-pizza(){
	@if $brand == pastry-pizza {
		@content;
	}
  }
  
  #footer {
	height: $footer-height;
	background-color: $ternary-color;
    @include rogers-rover {
      border-top: 5px solid darken($ternary-color, 15%);
    }
  }
```

###Loops

You can write simple loops in SASS

####For loops

You can use the formats:

* `@for $i from 1 through 6` will do through 6
* `@for $i from 1 to 6` will do until 5

This easy example creates a grid layout dynamically

```

@for $i from 1 through 6 {
  .col-#{$i} {
    width: i * 2em;
  }
}

```

####Each Loops

Loop through lists or maps. 

#####With Lists
See below we create a list, then create class names based on the items in the list, and then dynamically generate filenames for the images.

```
$speakers: bob-banker, patty-plume, sandra-smith;
@each $speaker in $speakers {
  .#{$speaker}-profile {
    background-image: url('/img/#{$speaker}.png')
  }
}

```

#####With Maps

You can also create maps in SASS. See below the format for creating maps.

```
$font-sizes: (tiny: 8px, small: 11px, medium: 13px, large: 18px);

```


The standard structure of looping through a map looks like the below

```
  @each $key, $value in $map-name {
    //do stuff
  }

```
The below example:

```
  $font-sizes: (tiny: 8px, small: 11px, medium: 13px, large: 18px);

  @each $name, $size in $font-sizes {
    .#{$name} {
      font-size: $size;
    }
  }

```
Would put out CSS like this:

```
.tiny {
  font-size: 8px; }

.small {
  font-size: 11px; }

.medium {
  font-size: 13px; }

.large {
  font-size: 18px; }
  
```


####While Loops

Take the below example, we will have more control over the increments every loop through the while loop

```
  $j: 2;
  @while $j <= 8 {
    .picture-#{$j} {
      width: $j * 10%;
    }
    $j: $j + 2;
  }

```