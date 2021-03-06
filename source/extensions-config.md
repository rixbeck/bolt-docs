# Developing Extensions

## Create and publish a Bolt extension or theme

Extensions and themes that are published on the marketplace must follow a few simple rules to allow them to hook into a Bolt installation. Information about the package needs to be provided in JSON format in the root of a project. 

To be hosted on the Bolt marketplace your project will need to be stored in a VCS repository and publicly readable. If you want to install your own extensions from somewhere other than the official Bolt marketplace then see the advanced documentation page.

### The JSON file

You will need a file called `composer.json` in the root of your project. This tells Bolt all the information it requires to display and install your extension or theme. To demonstrate the format we are going to create a dummy extension called Bolt Widgets. Here's how we create our `composer.json` file.

<pre class="brush: js">
{
    "name": "bolt/widgets",
    "description": "Bolt widgets is an awesome extension that features widgets on your website",
    "type": "bolt-extension",
    "keywords": ["bolt", "widgets", "awesome"],
    "require": {
        "bolt/bolt": ">=2.0,<3.0"
    },
    "authors": [
        {
            "name": "Bolt",
            "email": "info@bolt.cm",
            "homepage": "http://bolt.cm"
        }
    ],
    "autoload": {
        "files": [
            "init.php"
        ],
        "psr-4": {
            "Myextension\\": "src"
        }
    },
}
</pre>


All of the above information is required in order for your extension to be valid to publish on the Marketplace.

Here's a step-by-step run through of what to put in each of the settings.

#### Name
The name needs to be unique, no two packages on the Marketplace can share a name. We suggest that you use a username or company name as the first part and then a descriptive name as the second for example: `myco/loremipsum`, `myco/funtheme`

#### Description
The description tells potential users of your extension or theme what is provided. Make this as accurate and informative as you can.

#### Type
This identifies what type of package this is. For now the choices are `bolt-extension` or `bolt-theme`, it is important that you choose the correct type since extensions and themes are handled differently by Bolt.

#### Keywords
These help users to discover your extension on the Marketplace site and are also used for grouping packages.

#### Require
This is an important setting since it specifies what versions of Bolt your extension is compatible with, preventing users with an older or newer version from being able to install a broken extension. If you are unsure as to which versions to support we would recommend that you support the current major version, so for example if the current version of Bolt is 2.x then use: `"bolt/bolt": ">=2.0,<3.0"` this means that any version from 2.0 but not version 3.0 is supported.

Other possibilities would be: `"bolt/bolt": ">=2.3,<3.0"` or if you cannot support a range and only one specific version `"bolt/bolt": "2.3"`. We wouldn't recommend this since it will require you to manually update your extension with every new Bolt version, but there may be occasions when it is necessary.

#### Authors
This gives users of your extension some information about the author. The email and homepage can either be just your personal details or if you want to provide more indepth documentation or a support address they can point to a specific extension site.

#### Autoload
This configuration does two things, firstly you need to provide a file that initialises your extension, it's important that this file is kept as simple as possible, when it is run it will be provided with a single variable `$app` which is an instance of the running Bolt application. Here is the recommended file.

<pre class="brush: php">
use Myextension\Extension;

$app['extensions']->register(new Extension());
</pre>

Once the extension is registered, Bolt will take care of running the various hooks that you can define within your Extension class.

The second option allows you to define a directory to autoload your classes from, we'd recommend you use the same setting as in the example file. `"psr-4": {"Myextension\\": "src"}` this means that all classes you store inside the `src` directory will be autoloaded correctly.

Note that Bolt will only support PSR-4 autoload namespaces. For examples see here: <a href="http://www.php-fig.org/psr/psr-4/">http://www.php-fig.org/psr/psr-4/</a>


### Submitting your extension
Once you have the above file setup, make sure it is pushed up to your hosted repository then visit <a href="http://extensions.bolt.cm">extensions.bolt.cm</a> to register your extension or theme on the Bolt Marketplace.
