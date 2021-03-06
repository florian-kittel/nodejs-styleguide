# Node.js application style guide

A style guide for Node.js REST applications based on best practices in enterprise development. You can use it as whole or in parts to structure your Node.js application. It works perfect together with Angular, React or Vue as backend for frontend, as proxy handler or as microservices for large scale applications. You can host it on a normal webhost, that supports Node.js or in a could container via Docker or other.

**Status:**  `Draft` | `v0.1.0`


## Purpose
If you need a opinionated style guide to structure your Node.js application and enables teams to work together, you're right here.

This guide is based on my development experience in big teams and reading hundreds of article and books about Node.js. 

In my role as solution architect and lead developer, it is hard to align Node.js projects in big enterprises. I never find a guide or book, that works in the given setup. So I started to write down conventions, that helps the developers working together. What I hardly learned for all of my projects: *You need to make constrains, otherwise you will get a mess of code*.

This style guide is inspired by the awesome Angular style guide from John Papa.

## Table of Contents

1. [Application structure](#application-structure)
1. [Single responsibility principle](#single-responsibility-principle)
1. [File naming](#file-naming)
  

## Application structure
Structure you app in the following way to enable people to find quickly features, libraries, routes and configs.

**How to read:** The following code shows the folder and files you will need for your project. If it has a `/` at the end, it's a folder. If it has an `.something` at the end, you can imagine, it's a file. If it is in `( )` it is an optional folder/file. If the name begin with `my-`, it's your individual name for this folder or file and the give one is an example to illustrate it. Autogenerated files and folders (like node_modules, dist, etc.) are not present. If there are `[...]` at the end, that means a next folder or file with same schema. If there are only `...` at the end, that means other files, that are not necessary to name at this point, will follow.

* [Project folder structure](#project-folder-structure)
* [Application folder structure](#application-folder-structure)

**Note:** I provide this for a monorepo approach. Not a hole-company-all-code-in-one-repo, more a complete project mono-repo, that includes all Node.js applications and libraries in one repository, excluding frontend. You can use it for a single application as well.

### Project folder structure
This should be the folder and initial file structure for your project. As you know some of the files in root folder should be initial generate by its depending cli commands (described in section XY). Your applications will be stored in the `applications` folder. You will have an optional `database` folder for your local database, migrations and seeds. A `libraries` folder, where you store code, that is used by more than one application to stay `DRY`. A `scripts` folder, where you store scripts that are need for infrastructure, builds, deployments or setups for your repository.

```
applications/
    my-awesome-app/
    my-microservice-one/
    my-microservice-two/
    my-gateway-proxy/
    [...]
(databases)/
    (my-awesome-app)/
        migrations/
        seeds/
        knexfile.js
    (my-microservice-one)/
        migrations/
        seeds/
        knexfile.js
    [...]
libraries/
    my-authentication-functions/
    my-user-functions/
    [...]
scripts/
    (postinstall.script.js)
    [...]
.editorconfig
(.env)
.eslintrc.json
.gitignore
(.jsbeautifyrc)
package.json
(process.json)
(webpack.config.js)
```

### Application folder structure
As you have seen before, there a `applications` folder, where you will put in your applications. You can have multiple applications in your project. The following descripts how to structure one at the time.

There are two methods with which you can structure your application. First is folder-by-type, second is folder-by-feature. If you are familiar with both, you will see at the first look where to find what in the application.

#### Folder by type

**Do** use this if you have a smaller application or a smaller microservice with a handful of features/files.

**Do** use this if you have a gateway or proxy application.

**Why?** You don't need the overhead on a tiny application setup.

**Do** use this for up to three features/controllers.

**Do** create a controllers folder, where your store your controller.

**Do** create a models folder, if a controller or two needs a model.

**Do** create a routes folder, where you store your routing configuration by feature.

**Avoid** combine it with the following folder-by-feature structure. 

**Why?** You can't identify your structure anymore and it getting harder to maintain the project.

**Avoid** using this structure, when your create more than three features.

**Why?** Take a look at the folder-by-feature structure, this makes more sense on my features in an applications.

```
/**
 * Folders-by-type structure
 * Avoid when you have more then three features/controllers
 * in your application
 */
applications/
    my-awesome-app/
        controllers/
            my-feature-one.controller.js
            my-feature-two.controller.js
            [...]
        (models)/
            (my-feature-one.model.js)
            [...]
        routes/
            my-feature-one.routes.js
            my-feature-two.routes.js
            [...]
        test/
          mocha.opts
          my-feature-one.spec.js
          my-feature-two.spec.js
          [...]
        (db.js)
        index.js
        readme.md
    [...]
...
```

#### Folder by feature
This should be the default choice for an application or microservice with more than three features or when you like it better than the folder-by-feature approach.

**Do** use it for larger applications or microservices.

**Do** use it for applications that will possibly grow.

**Do** use it for applications with more than three features/controllers.

**Do** create a separate folder for every feature.

**Do** store your controller, model and routes in the feature folder.

**Do** create an model folder, when you have models, that are used by more then one feature.

**Avoid** combine it with the folder-by-type structure, beside the models folder.

**Why?** You will lost the structure and the overview about your application.


```
/**
 * Folders-by-feature structure (recommended)
 * Use when you have more then three features/controllers
 * in your application
 */
applications/
    my-awesome-app/
        my-feature-one/
            my-feature-one.controller.js
            (my-feature-one.model.js)
            my-feature-one.routes.js
        my-feature-two/
            my-feature-two.controller.js
            (my-feature-two.model.js)
            my-feature-two.routes.js
        (models)/
            my-model-used-in-several-controllers.model.js
        test/
          mocha.opts
          my-feature-one.spec.js
          my-feature-two.spec.js
        (db.js)
        index.js
        readme.md
    [...]
...
```


**[Back to top](#table-of-contents)**



## Single responsibility principle

Use the [SRP](https://en.wikipedia.org/wiki/Single_responsibility_principle) to organize your code base. 

**Do** use a separate file per type.

**Why?** helps finding a piece of code easier and reduces the lines of code in the file

**Do** try to limit your file to 400 lines of code.

**Avoid** one big feature file. 

**Do** Divide your feature in logical parts like `controllers`, `routes` and `models`.

**Why?** makes it easier to read, maintain, and avoid collisions.

**Do** shift code that is used twice in a library or separate model.

**Why?** will reduce your file size and it getting easier to maintain.

**Do** use a descriptive name for your feature folder and reuse this name for your nested files (in folder-by-feature structure).


**[Back to top](#table-of-contents)**



## File naming

File naming can be essential by dealing with a large code base. It is important to name your files so, that a third person are easy to guess the context of the file.

**Do** use the pattern feature.type.js, where the type describes the function of this file like `controller`, `model`, `spec`, `routes`, etc.

**Why?** this will helps identify the context of your file before you open it.

**Why?** it will help you find the file quicker you searching for this feature.

**Do** use full word for types.

**Why?** helps to stay consistent and prevents interpretation errors.

**Why?** make it easier to find in an IDE by the search functionality.

**Do** use a descriptive word for your feature in your filename like `authentication.controller.js` or `shopping-cart.controller.js`

**Why?** should help to identify the context of the feature quicker and help to find the desired code.

**Do** use kebab case for your features in filenames like `shopping-cart`, `rule-the-world` or `reset-matrix`

**Why** helps to stay consistent in your filenames and make it more readable.

**Do** use the same name as the name of the folder for your feature (in folder-by-feature structure).

```
/**
 * File naming examples
 * Makes it easier to identify and find files
 */
applications/
    my-awesome-app/
        my-feature-one/
            my-feature-one.controller.js
            my-feature-one.model.js
            my-feature-one.routes.js
        test/
          my-feature-one.spec.js
```

**[Back to top](#table-of-contents)**
