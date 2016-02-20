---
layout: post
title: Vue.js -  First Impressions
---
## Background

I love new tech, especially javascript tech. I feel there is always something new with that language.

As a aspiring web dev I try my best to keep up to the latest trends, and thus I've appreciated the elegance and superiority *Angular.js* provides. I tried ReactJS, but realized I really hated embedding HTML in JS code.

I frequent */r/webdev* and */r/javascript* alot, so coming across ***Vue.js*** was pretty much inevitable. Usually, its place is stuck in between the warzone of Angular advocates and React rebels (hehe) (kinda like Nintendo's position among its competitors in the gaming industry). Those who use it seem to appreciate it alot, but most of the time it's overlooked by veteran developers who have already chosen their primary tech.

> Examples of those who used and appreciated Vue.js
> * [Using Vue.js for non SPA's](https://medium.com/@weblee/using-vue-js-for-non-spa-s-c2bd93f69d32#.d5o5sk9o5)
> * [Vue Js - Why another library yet we have sooo many of them](https://medium.com/steel-code/vue-js-a35e9167cefb#.7cqizyo34)

Though before I dive into Vue, I should talk about my experience with Angular.js.

## So...what's the angle on that Angular

Even though my experience with Angular.js is quite limited, I really love the framework. In a nutshell, you're given a convenient tool that lets you modify the blueprint of a webpage however you want...**using your own schematics and semantics**.

There are however some things I dislike about Angular.

1. **It's learning curve is steep.** This is probably one of the most common complaints from any Angular user. You probably don't know, but my first programming language was C. I **really hated** the effort required to familiarize myself with the language. The impression Angular first gave me really mirrors one I got learning C, and, while I really like and appreciate it, I just couldn't bare myself to master it.

2. **Too bulky**. Angular is huge. Not just in terms of file size, but also its complexity and architecture. Many of its features I often find unnecessary, though I cannot take only part of the library and use that.

## Enter Vue.js

The best way to describe Vue.js is just saying it's a *smaller Angular*. It is a small MVVM library that focuses on data-binding and components.

Let's take a look at the basics of Vue.js

```html
<div id="vuecontainer">
  <div>
    <label> Vue label </label>
    <input type="text" v-model="vuemodel">
    <hr>
    <h1> I've typed {{vuemodel}}</h1>
  </div>

  <script>
  var myApp = new Vue({
    el: '#vuecontainer',
    data: 'Default text'
  })

  </script>
</div>
```

We can also use directives...
```html
<div id="vuecontainer">
  <div>
    <mydirective></mydirective>
  </div>

  <script>
  Vue.component('mydirective',{
    template: '<span><h1>{{text}}</h1></span>',
    data: function(){
      return {
        text: 'My default text'
      }
    }
  })

  var myApp = new Vue({
  })

  </script>
</div>
```

Very standard data-binding...except that Vue.js provides this in merely 24.60kb compressed. It was also very easy to learn (took me roughly 10 minutes to grasp on its basic)

Standard use case for this library will be
* Small but quick prototypes
* Non SOAP but templating heavy
