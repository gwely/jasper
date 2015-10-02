# jasper
JS development environment for multiple project ASP.NET MVC solution
### Example project layout
```
- MySolution
- - Assets
- - - JS
- - - - Layout
- - - - - index.js
- - - - - bundle
- - - - - - dev.js
- - - - - - (prod.js)
- - - - Modules
- - MyFirstProject
- - - Controllers
- - - - ClassController.cs
- - - Javascript
- - - - Class
- - - - - Admin
- - - - - - index.js
- - - - - - bundle
- - - - - - - dev.js
- - - - - - - (prod.js)
- - - - Shared
- - - - - Layout
- - - - - - bundle
- - - - - - - dev.js
- - - - - - - (prod.js)
- - - Views
- - - - Class
- - - - - Admin.cshtml
- - - - Shared
- - - - - _Layout.cshtml
- - MySecondProject
....

```

The `(prod.js)` is generated by the `prod` task on your build server. Speaking of which, my build server (Jenkins) 
runs `npm install && npm run-script prod` as a pre-build step.

The "Layout" bundle is site-wide JS that is loaded in the `_Layout.cshtml` file. Said file should contain

```
@if (Request.IsLocal)
{
    <script src="http://localhost:35729/livereload.js"></script>
}
<script src="~/Javascript/Shared/Layout/bundle/dev.js"></script>
```

The `prod` task will update this script `src` (along with all the others in other views) to point to `prod.js`. 
So your source code is always in development mode.

`Assets/JS/Modules` is for global modules used in multiple projects.

### Development

In a command prompt in the project directory, or using 
[Task Runner Explorer](https://visualstudiogallery.msdn.microsoft.com/8e1b4368-4afb-467a-bc13-9650572db708) 
, call `grunt dev`. The task will watch all the `cshtml`, `css` and `dll` files for `livereload`. It will watch all
`js` and `jsx` files in the project and rebuild only that page's JS. For example, if 
`MyFirstProject/Javascript/Class/Admin/index.js` is updated, Grunt will re-bundle only the files in 
`MyFirstProject/Javascript/Class/Admin` and its subfolders. Then perform a `livereload`.
