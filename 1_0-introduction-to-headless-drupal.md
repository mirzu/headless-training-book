# 1 Introduction to Headless Drupal
## Outline
- When to go Headless
    - Speed of Development
    - Separation of teams/resources
    - Same data, lots of different consumers
    - Architecture Patterns
- Architecture Patterns for Decoupled sites.

## When to go Headless
### Speed of development
- Faster if you have a REST API in place.
- Not always initially faster. A headless approach to developing a Drupal site can lead to gains later in a project.

### Separation of teams/resources
- Dedicated teams for building 'Drupal' resources, and dedicated teams for building rendering/presentation resources.
- Lack of technical debt on teams. Focusing on a single set of tasks.

### Same data, lots of different consumers
- Building one backend that will be consumed with many different systems.
    - [TWiT.tv](http://fourword.fourkitchens.com/article/twittv-launches-content-api-and-headless-drupal-site)
    - [Tonight Show with Jimmy Fallon](http://fourword.fourkitchens.com/article/and-emmy-goes)
        - [DrupalCon Austin Session on TSwJF Architecture](https://www.youtube.com/watch?v=oxGfkTgxp6M)
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
- Client Side templating
- Static Site Generator
- Drupal to Drupal
- Isomorphic Frameworks

## You don't have to take down your existing Drupal site!
You can add an API to your existing Drupal site and use it as a superset of the existing functionality.
For a deeper look into this topic, maybe check out my talk this weekend, [Nearly Headless Drupal](http://nyccamp.org/session/nearly-headless-drupal)!

## Real world Examples
- [The Tonight Show Starring Jimmy Fallon](http://www.nbc.com/the-tonight-show) (Node.js + Backbone.js): / [DrupalCon case study](https://austin2014.drupal.org/session/migrating-worlds-largest-website-drupal-weathercom) /via @vordude
- [Weather.com](http://www.weather.com/) (Angular): / DrupalCon case study /via @DamienMcKenna
- [Radio France Internationale](http://www.rfi.fr/) (Symfony 2): /via @slybud

For a bigger list of examples see [this list](https://groups.drupal.org/node/432938)

## Headless Talks at Drupalcon LA
[A roundup of some great Headless talks at Drupalcon LA](http://fourword.fourkitchens.com/article/drupalcon-la-headless-roundup)

## Headless Talks at NYCCamp
[Here's a blog post about some of the Four Kitchens' Headless talks at NYCCamp](http://fourword.fourkitchens.com/article/four-kitchens-nyc-camp-2015)
