## RequireJs-PreloaderJs-HandleBars

# Configuration

> main require js

```HTML
<section id="s_wizard"></section>
<script data-main="./javascripts/main" src="./node_modules/requirejs/require.js"></script>
```

> main require js

```JS
require.config({
  paths : {
    jquery:"../node_modules/jquery/dist/jquery",
    hbs: '../node_modules/require-handlebars-plugin/hbs',
    preloadjs: '../node_modules/preload-js/index'
  },
  shim: {
        preloadjs: {
            exports:"createjs.LoadQueue"
        }
    },
  hbs : {
    helperDirectory : "templates/helpers/",
    handlebarsPath: "handlebars"
  }
});
require([
  "jquery",
  "js/components/mainController"
],function($) {
});
```

#Use examples

> js/mainController.js

```JS
define([
  'jquery',
  'templates/views',
  'preloader/preloader
], function($,views) {
  $("#s_wizard").html(views.carouselWizard({data}));
});
```
> js/template/views.js

```JS
define([
  'hbs!templates/views/carouselWizard',
], function(carouselWizard) {
  var views = {
    carouselWizard :carouselWizard
  };
return views
});
```
> templates/views/carouselWizard

```HTML
{{#each data}}
  <div data-carousel-value='{{dataCarouselValue}}' data-carousel-number='{{inc @index}}'>
    <div class='circle-default'>
      <div class='circle-image' style='background-image:url("{{src}}")'></div>
    </div>
  </div>
{{/each}}
```
> templates/views/carouselWizard

```JS
define(['handlebars'], function ( Handlebars ) {
  Handlebars.registerHelper("inc", function(value, options)
  {
      return parseInt(value) + 1;
  });
});
```

> js/preloader/

```JS
define([
  'jquery',
  'preloadjs'
], function($) {
  var preloader = {
    images : [],
    runLoader: function() {
      $("#s_preloader").fadeIn(300);
    },
    stopLoader: function() {
      $("#s_preloader").fadeOut(300);
    },
    loadImages : function(data,ownFunction){
      if(jQuery.type(data) == "array") {
        var that = this;
        that.runLoader();
        var preload = new createjs.LoadQueue();
        $.each(data,function(index,value){
          that.images.push(value.src);
        });
        preload.loadManifest(that.images,false);
        preload.on("complete",function(){
          setTimeout(that.stopLoader, 1000);
          that.images = [];
          ownFunction();
        });
        preload.load();
      } else {
        ownFunction();
      }
    }
  }
  return preloader;
});
```


