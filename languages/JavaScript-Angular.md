# JavaScript / Angular

## Links

### JavaScript

* [MDN JavaScript Reference](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference)
* [TypeScript Documentation](https://www.typescriptlang.org/docs/)
* [Node.js Documentation](https://nodejs.org/en/docs/)
* [npm Documentation](https://docs.npmjs.com/)
* [ESLint Documentation](https://eslint.org/docs/latest/)
* [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript)
* [Image Compressor](https://github.com/xkeshi/image-compressor)
* [JavaScript - Creating an Ajax module with Promises](http://blog.da2k.com.br/2015/03/06/javascript-criando-um-modulo-ajax-com-promises/)
* [The Web is getting its bytecode: WebAssembly](http://arstechnica.com/information-technology/2015/06/the-web-is-getting-its-bytecode-webassembly/)
* [JSFiddle - Test HTML, Angular, CSS, JS online](http://jsfiddle.net/)

### Angular

* [Angular Official Documentation](https://angular.dev/overview)

### AngularJS

* [AngularJS Style Guide (John Papa)](https://github.com/johnpapa/angular-styleguide)
* [John Papa Style Guide - a1 README](https://github.com/johnpapa/angular-styleguide/blob/master/a1/README.md)
* [Using Track-By With ngRepeat in AngularJS 1.2](http://www.bennadel.com/blog/2556-using-track-by-with-ngrepeat-in-angularjs-1-2.htm)
* [11 Tips to Improve AngularJS Performance](http://www.alexkras.com/11-tips-to-improve-angularjs-performance/)
* [AngularJS Tutorial - TutorialsPoint](http://www.tutorialspoint.com/angularjs/index.htm)
* [Angular Directives by Scalyr](https://github.com/scalyr/angular)
* [NG Conf 2015](https://www.youtube.com/watch?list=PLOETEcp3DkCoNnlhE-7fovYvqwVPrRiY7&v=QHulaj5ZxbI)
* [Rodrigo Branas - YouTube](https://www.youtube.com/user/rodrigobranas)
* [$resource service docs](https://docs.angularjs.org/api/ngResource/service/$resource)
* [Restangular](https://github.com/mgonto/restangular)
* [AngularJS Batarang Chrome DevTools Extension](https://chrome.google.com/webstore/detail/angularjs-batarang/ighdmehidhipcmcojjgiloacoafjmpfk)
* [angular-xeditable plugin](http://vitalets.github.io/angular-xeditable/)

### Frontend Tooling

* [Bootstrap](https://getbootstrap.com/)
* [Bootstrap Getting Started](https://getbootstrap.com/docs/4.0/getting-started/introduction/)
* [Material Design by Google](http://www.google.com.br/design/)
* [Angular Material](https://material.angularjs.org/)
* [Bower - Client-side dependency management](http://bower.io/)
* [Gulp - Build system](http://gulpjs.com/)
* [gulp-rev](https://github.com/sindresorhus/gulp-rev)
* [Yeoman - Code generator](http://yeoman.io/)
* [Browsersync with Gulp](http://www.browsersync.io/docs/gulp/)
* [json-server - Mock REST API](https://github.com/typicode/json-server)
* [requirejs - Lazy Load](http://requirejs.org/)
* [ddslick - Dropdown plugin](http://designwithpc.com/plugins/ddslick)

### Gulp Minification Packages

* [gulp-cssmin](https://www.npmjs.com/package/gulp-cssmin)
* [gulp-htmlmin](https://www.npmjs.com/package/gulp-htmlmin)
* [gulp-imagemin](https://www.npmjs.com/package/gulp-imagemin)
* [gulp-uglify](https://www.npmjs.com/package/gulp-uglify)
* [gulp-svgmin](https://www.npmjs.com/package/gulp-svgmin)
* [gulp-concat](https://www.npmjs.com/package/gulp-concat)
* [gulp-watch](https://www.npmjs.com/package/gulp-watch)

## JavaScript Snippets

### Check for Duplicate Elements in Array

```js
!test.every((elem, i, array) => array.lastIndexOf(elem) === i);
```

## CSS Snippets

### FlexBox Column Layout

```css
:not(.no-flex) {
    display: flex;
    flex-direction: column;
}
```

## AngularJS Architecture Reference

### Jedi Project (AngularJS Framework)

* [Jedi Project](http://jediproject.github.io/)
* [ng-jedi-demo](https://github.com/jediproject/ng-jedi-demo)
* [generator-jedi](https://github.com/jediproject/generator-jedi)

Run with: `npm run start`

Key modules:
- `ng-jedi-activities` - Background activity management
- `ng-jedi-dialogs` - Alert and custom modals
- `ng-jedi-table` - Paginated grid (in-memory or API)
- `ng-jedi-security` - Token-based auth/authz
- `ng-jedi-factory` - Simplifies creating AngularJS components
- `ng-jedi-loading` - Progress bar intercepting HTTP requests
- `ng-jedi-i18n` - Internationalization
- `ng-jedi-layout` - Ready-to-use UI components

## HTTP Cache Headers Reference

* [Increasing Application Performance with HTTP Cache Headers](https://devcenter.heroku.com/articles/increasing-application-performance-with-http-cache-headers)

Types:
- `Cache-control` / `Expires` - Direct cache
- `Last-Modified` (time-based) / `ETag` - Conditional requests
