# 2 Drupal Services
## Drupal Web Services
### Outline
- Module Options
  - Why Restful
- Install & configure Restful
- Create Endpoints
  - Single node endpoint
  - Node collection endpoint
    - Filtering content
  - Referenced content (node author)
  - Handling Media
  - Queued Content
- Hypermedia API Design in practice
  - Apiary for mocking & Docs.
  - Mocking in Drupal directly.
- Drupal Overview

### Different Module Options
- Services
  - Oldest module
  - Good architecture for setting up servers
  - Handles different servers becides REST
  - Horrible default Entity endpoints
  - Custom endpoint creation is not intuitive and hard to work with


- RESTws
  - Not a bad option overall
  - Custom endpoint creation is OK
  - Less robust server implementation
  - Doesn't do versioning


- RESTful
  - The module we are using for this class
  - Very good API for creating endpoints
  - Very well written.
  - No default endpoints
  - Designed from the ground up to create beautiful APIs

## Intro to Restful
> This module allows Drupal to be operated via RESTful HTTP requests, using best practices for security, performance, and usability.

- All endpoints are explicitly created. The module does nothing by default.
  - This allows for clean output and keeps _Drupalisms_ from leaking out into your API
- Versioning is core to how endpoints are built
- Endpoints are built around bundles, not entity types, making it easy to expose certain endpoints, but not others.
- Configurable output formats allowing for output in JSON or XML
- Object oriented architecture that makes it easy to created endpoints and reuse code.
- Very good test coverage in the module.

## Structure of our content
The content we'll be working with is populated with posts from the four kitchens blog. Below is a list of the fields that we'll be working with for easier reference. The login to your Database is admin:admin.

Field Label     | Machine Name                    | Field Type     | Comments
--------------- | ------------------------------- | -------------- | --------
Lead image      | field_lead_image                | Image          |
Body            | body                            | Long Text      |
Inline Images   | field_inline_image              | Image          |
Files           | field_blog_files                | File           |
Blog Categories | field_blog_categories_term_tree | Term reference |
Blog Series     | field_blog_series_term_tree     | Term reference |

## Install and Configure Restful
The module is already installed. The only dependency is the [Entity API](https://drupal.org/project/entity) module. There is one [patch](https://www.drupal.org/node/2086225#comment-9627407) to the Entity API module required.

Once the module is installed there is no exposed admin page. You need to create your own module that declares a Restful plugin to use the module.

Lets do that now!

## Custom Module was created for you.
We skipped the normal setup of a Drupal module, including creating the .info file and in the module folder we declared where ctools should look for restful plugins. The 'plugins/' folder in this module.

`restful_fourword/restful_fourword.module`

```
<?php
/**
 * Implements hook_ctools_plugin_directory().
 */
 function restful_fourword_ctools_plugin_directory($module, $plugin) {
   if ($module == 'restful') {
     return 'plugins/' . $plugin;
   }
 }
```

## How the Drupal Lessons are setup.
The custom module is already created and you can start working with it. The repo for it has Git tags for each lesson. You can always see what the end result of a lesson is and check if you are on the righ track by using a simple diff command.

### Checkout the first tag.
To get started checkout a branch that is based on the lesson_1_start tag.

`git checkout -b lesson1 lesson_1_start`

You can now make changes to your code. To compare your code to the end of the lesson use the following command:

`git diff lesson_1_end`

As you can see we are going to add a file "plugins/restful/node/blog_posts/1.0/blogPosts__1_0.inc"

Lets get started.

### 1. Declare an endpoint for blog posts.
`restful_fourword/plugins/restful/node/blog_posts/1.0/blogPosts__1_0.inc`

```PHP
<?php
$plugin = array(
  'label' => t('Blog Posts'),
  'resource' => 'articles',
  'name' => 'blogPosts__1_0',
  'entity_type' => 'node',
  'bundle' => 'blog_post',
  'description' => t('Fourword blog posts using view modes'),
  'class' => 'RestfulEntityBaseNode',
  'authentication_types' => TRUE,
  'authentication_optional' => TRUE,
  'view_mode' => array(
    'name' => 'default',
    'field_map' => array(
      'body' => 'body',
    ),
  ),
);
```

The resource key determines the root URL of the resource. The name key must match the filename of the plugin: in this case, the name is `blogPosts__1_0`, and therefore, the filename is `blogPosts__1_0.inc`.

This endpoint is created using the default view mode, and exposes any fields that are listed in the `field_map` array. _Note:_ Fields must be visible in the view mode for them to show up.

Lets talk about the different parts of the array.

- `label` - The label of the plugin.
- `resource` - Name of the _resource_, must match the filename of the plugin.
- `name` - Machine name of the plugin.
- `entity_type` - Type of Entity this resource will expose.
- `bundle` - Name of the bundle this resource will expose.
- `description` - Longer description of the plugin.
- `class` - Class file that will declare the plugin.
- `authentication_types` - What type of authentication this plugin will use.
- `authentication_optional` - Determines if the authentication is required.
- `minor_version` - Minor version of the API that this plugin will be exposed under.
- `view_mode` - Array with info about the view mode to use for this endpoint.

### 1.1 All the stuff you can do now!
RESTful comes with a bunch of out of the box functionality for any resource that you create. Including:
- See a paginated list of blog posts, and use hyper links to go to different pages: `api/articles`
- Get a specific blog post: `api/articles/29,143`
- Limit the fields returned using the _fields=_ query parameter `api/articles/29?fields=body`
- Filter the API using the _filter[lable]=_ query parameter.
- More info on consuming the API is in (this file)[https://github.com/RESTful-Drupal/restful/blob/7.x-1.x/docs/api_url.md].

### 1.2 Error Handling
If an error occurs when operating the REST endpoint via URL, A valid JSON object with `code`, `message` and `description` would be returned.

The RESTful module adheres to the [Problem Details for HTTP APIs](http://tools.ietf.org/html/draft-nottingham-http-problem-06) draft to improve DX when dealing with HTTP API errors.

For example, trying to sort a list by an invalid key

```shell
curl http://finished.drupal.headless.4kclass.com:8080/api/articles?sort=wrong_key
```

Will result with an HTTP code 400, and the following JSON:

```javascript
{
  'type' => 'http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.4.1',
  'title' => 'The sort wrong_key is not allowed for this path.',
  'status' => 400,
  'detail' => 'Bad Request.',
}
```

### 1.3 Add categories.
Add a new key to the `view_modes` key that is `field_blog_categories_term_tree` with a value of `categories`. Go to the interface and add the blog categories to the default view mode.

This isn't output isn't great. The HTML surrounding the categories is going to make life hard for our API consumers. We could install (Display Suite)[[https://www.drupal.org/project/ds](https://www.drupal.org/project/ds)], but even with those tools we are going to hit a wall sometime or another. Lets try creating a more custom endpoint instead.

### 2.0 Versioning your API.

`git add .`

`git commit -am "end of lesson1"`

`git checkout -b lesson2 lesson_2_start`

Lets create a new version of our API. Once your API is published to the world it's important to be diligent about versioning. There is nothing worse than having your site broken by an API that changed. OK, having your site broken on a _Friday_ by a broken API is worse.

Luckily RESTful is built from the ground up to facilitate easy versioning.

- Copy the 1.0 folder to a 1.1 folder.
- Rename the `blogPosts__1_0.inc` file to `blogPosts__1_1.inc`
- Add a `minor_version` key with a value of 1 to the plugin definition.
- Clear the cache `drush cc all`
- Visit the new version `/api/v1.1/articles`

#### Accessing different API versions in RESTful.
You can access different versions of the API using two different methods.

1. Using the URL. Simply replace the `v1.1` with `v1.0`.
2. Using a header.

```bash
curl http://finished.drupal.headless.4kclass.com:8080/api/articles \
-H "X-API-Version: v1.0"
```

The URL is useful for humans, but the header is useful for building API clients.

### 3.0 Create a custom endpoint.
Using the view modes is fine, but the real power of Restful comes from creating custom endpoints. We'll make one now. You can save your work in this branch and create a new branch from the lesson 2 starting point.

`git add .`

`git commit -am "end of lesson2"`

`git checkout -b lesson3 lesson_3_start`

#### 3.1 Start by creating a new API endpoint version.

- Copy the 1.1 folder to a 1.2 folder.
- Rename the `blogPosts__1_1.inc` file to `blogPosts__1_2.inc`
- Add a `minor_version` key with a value of 2 to the plugin definition.
- Add the following class to

`blogPosts__1_2.inc`

```PHP
/**
 * @file
 * Contains RestfulFourwordBlogPostsResource__1_2.
 */

class RestfulFourwordBlogPostsResource__1_2 extends RestfulEntityBaseNode {
  public function publicFieldsInfo() {
    $public_fields = parent::publicFieldsInfo();

    $public_fields['body'] = array(
      'property' => 'body',
      'sub_property' => 'value',
    );

    return $public_fields;
  }
}
```

In the plugin definition, the `class` value to `RestfulFourwordBlogPostsResource__1_2`.

```php
  'class' => 'RestfulFourwordBlogPostsResource__1_2',
```

Finally, remove the `view_mode` key; it's no longer needed with the class! The final result:

```
$plugin = array(
  'label' => t('Blog Posts'),
  'resource' => 'articles',
  'name' => 'blogPosts__1_2',
  'entity_type' => 'node',
  'bundle' => 'article',
  'description' => t('Fourword blog posts using view modes'),
  'class' => 'RestfulFourwordBlogPostsResource__1_2',
  'authentication_types' => TRUE,
  'authentication_optional' => TRUE,
  'minor_version' => 2,
);
```
This is all the code you need to create your new endpoint.

- Clear the cache `drush cc all`
- Visit the endpoint, which will be available at `/api/v1.2/articles`

It does the following:

- This extends the class `RestfulEntityBaseNode`. This is a convience class that makes working with entities easier. If you wanted to create an endpoint that exposed something else you would use a different base class.
- Creates a public function `publicFieldsInfo()`.
- In that function creates a variable called `$public_fields` populated with the parent's `publicFieldsInfo()`
- Adds a new field called body and sets the body to the value of the nodes body field.

### 4.0 Add categories to the endpoint.
The same technique can be used to add any text or numerical fields as properties to your API, but what if you want to add links to other resources? For example lets say we created and endpoint that is for categories, and wanted to link to all the categories that each blog post has?

Hey look at that someone created a categories endpoint for us: `/api/articles`. Open up the folder and take quick look, we didn't need to do anything besides declare the class.

#### Add the categories property.
Add the following after the body property. This will create a link to the blog categories which is a link to the `articles` endpoint.

```
$public_fields['categories'] = array(
  'property' => 'field_blog_categories_term_tree',
  'resource' => array(
    'blog_categories' => 'blog_categories',
  ),
);
```

### 5.0 Adding an image.
#### Add a property for the image.

```
$public_fields['lead_image'] = array(
  'property' => 'field_lead_image',
);
```

That works, but we are leaking lots of info our API consumers don't need, plus there isn't a simple link to the image. Lets fix that with a custom processor.

#### Add a process callback
RESTful gives us an easy way to process any callback.

Add the following method to our class.

```PHP
/**
 * Process callback, Remove Drupal specific items from the image array.
 *
 * @param array $value
 *   The image array.
 *
 * @return array
 *   A cleaned image array.
 */
protected function fourwordImageProcess($value) {

  return array(
    'id' => $value['fid'],
    'self' => file_create_url($value['uri']),
    'filemime' => $value['filemime'],
    'filesize' => $value['filesize'],
    'width' => $value['width'],
    'height' => $value['height'],
    'styles' => $value['image_styles'],
  );
}
```

The process function accepts a Drupal field output array, and should return an array of mapped keys and values.

We use this method on our resource by adding a `process_callbacks` array to the `lead_image` public_field. Add the following code below the `property` key.

```PHP
      'process_callbacks' => array(
        array($this, 'fourwordImageProcess'),
      ),
```

Finally there is a neat little function that will add image styles. Add the following after the `process_callbacks`.

```PHP
'image_styles' => array('thumbnail', 'medium', 'large'),
```

Let's wrap up by putting this latest work into our git repository.

`git add .`

`git commit -am "end of lesson3"`

#### Extra Credit

- _Silver_: Add a resource for the series by copying the categories endpoint and add a new property called series using the `field_blog_series_term_tree` property to link to it like categories.
- _Gold_: Create an endpoint that links to the user API resource who authored the blog post.
- _Platinum_: Create a new endpoint that exposes the sites variables see (this example)[[https://github.com/RESTful-Drupal/restful/blob/7.x-1.x/modules/restful_example/plugins/restful/variable/1.0/](https://github.com/RESTful-Drupal/restful/blob/7.x-1.x/modules/restful_example/plugins/restful/variable/1.0/)]
