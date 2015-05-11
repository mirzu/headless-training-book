# 1. Introduction to Headless Drupal

## Outline

* When to go Headless
  * Speed of Development
  * Separation of teams/resources
  * Same data, lots of different consumers
  * Architecture Patterns
* Architecture Patterns for Decoupled sites.

## When to go Headless

### Speed of development

- Faster if you have a REST API in place.
- Not always initially faster. A headless approach to developing a Drupal site can lead to gains later in a project.

### Separation of teams/resources

- Dedicated teams for building 'Drupal' resources, and dedicated teams for building rendering/presentation resources.
- Lack of technical debt on teams. Focusing on a single set of tasks.

### Same data, lots of different consumers

- Building one backend that will be consumed with many different systems.
    - Mention TSWJF?
- Separation of concerns
- When the the Drupal presentation layer is getting in your way.

## Architecture Patterns for Decoupled sites.

### Architecture parts

- Routing
  - Clean URLs
- Caching
- Templating
- Error Handling
- Interactivity
- Authentication
  - Permissions
- API Versioning strategy & implementation
- API Documentation strategy & implementation

### Patterns

- Cache & Theme
  - Light weight server is a proxy or passthrough to the backend API
  - Provides flexability in how the content is rendered
  - Provides a caching layer that makes the architecture independent of API server performance
  - Routing and caching handled in lightweight application like [node.js](http://nodejs.org/),  [Silex](http://silex.sensiolabs.org/) or [ruby](https://www.ruby-lang.org/en/).
  - Initial page load is faster than client side templating.
  - Example: [The Tonight Show Starring Jimmy Fallon](http://www.nbc.com/the-tonight-show)
- Client Side templating
  - Built like a Web App
  - [Angular](https://angularjs.org/) or [React](https://facebook.github.io/react/) used to render JSON
   directly from an API.
  - Great for one page App experiences.
  - Highly dependent on API performance.
  - Initial page load will always be slower than server side rendering.
- Static Site Generator
  - [jekyll](http://jekyllrb.com/)
  - [baked.js](http://prismicio.github.io/baked.js/)
  - [roots.js](http://roots.cx/)
  - perfect for sites that do not need frequent updates.
  - Can be combined with a clientside MVC to make highly interactive sites.
  - Has security and performance advantages
  - Render it and forget it: Static sites don't need security upgrades.
- Drupal to Drupal
  - Separate creation and rendering but use everyone's favorite CMS for both API and rendering layer.
  - Provides the separation of concerns advantages while keeping everything on the same technology.
- Isomorphic Frameworks
  - Rendered page first, followed by JSON updates to client side MVC.
  - [Angular-server](https://github.com/saymedia/angularjs-server)
  - [Flux](http://fluxible.io/)
  - [React Isomorphic](http://bensmithett.github.io/going-isomorphic-with-react/)
  - The holy grail, best of both worlds.
  - Currently there isn't a framework that supports this type of development.

### One way to think about it is distributed MVC.

The **Model** represents the data, and does nothing else. The model does NOT depend on the controller or the view. The **View** displays the model data, and sends user actions to the controller. The **Controller** provides model data to the view, and interprets user actions such as button clicks. The controller depends on the view and the model.

Drupal is your model. The services you create expose your data. Potentially some preprocessing can happen on that data before it's exposed. The controller handles the interaction between the view, what your user sees, and the exposed data model from Drupal.

1. The user request a page.
2. The controller receives a Request for a page. Any modifications to he request happen here. The response of that request is additionally processed here.
3. The controller then returns the data to the view for rendering.

Your express application is broken up into three main compenents,  your routes, controllers and views. Routes let your users interact with your application and pass data to your controllers. The controllers receive data from the routes, make requests to your data model and render the response.

Technically the controller itself isn't doing the rendering, but calling a 'render' function which should accept a template, and the data from the model.

In Express (similar to other frameworks), you'd do something like this,

    res.render('index', {response : drupal_json});

## Real world Examples

* [The Tonight Show Starring Jimmy Fallon](http://www.nbc.com/the-tonight-show) (Node.js + Backbone.js): / [DrupalCon case study](https://austin2014.drupal.org/session/migrating-worlds-largest-website-drupal-weathercom) /via @vordude
* [Weather.com](http://www.weather.com/) (Angular): / DrupalCon case study /via @DamienMcKenna
* [Radio France Internationale](http://www.rfi.fr/) (Symfony 2): /via @slybud

For a bigger list of examples see [this list](https://groups.drupal.org/node/432938)

## Headless Talks at Drupalcon LA

[Here's a quick blog post about the Headless talks this week](http://fourword.fourkitchens.com/article/drupalcon-la-headless-roundup)
