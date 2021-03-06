Dialogs for Jaxon
=================

Modals, alerts and confirmation dialogs for Jaxon with various javascript libraries.


Features
--------

This package provides modal, alert and confirmation dialogs to Jaxon applications with various javascript libraries.
16 libraries are currently supported.

The javascript library to use for each function is chosen by configuration, and the package takes care of loading the library files into the page and generating the javascript code.

Installation
------------

Add the following lines in the `composer.json` file, and run the `composer update` command.

```json
"require": {
    "jaxon-php/jaxon-core": "~2.0",
    "jaxon-php/jaxon-dialogs": "~1.0"
}
```

Configuration
-------------

This package defines 3 config options in the `default` section to set the default library to be used resp.
for modal, alert and confirmation dialogs.
The `libraries` option allows to load additional libraries into the page.
The `confirm` section defines options for the confirm dialog. 
The `lib.uri` option defines the URI where to download the libraries files from. 

Specific options can also be set for each library.

```php
    'dialogs' => array(
        'default' => array(
            'modal' => 'bootstrap',  // Default library for modal dialogs
            'alert' => 'noty',       // Default library for alerts
            'confirm' => 'noty',     // Default library for confirmation
        ),
        'libraries' => array('pgwjs', 'toastr'), // Additional libraries
        // Confirm options
        'confirm' => array(
            'title' => 'Question',   // The confirm dialog
            'yes' => 'Oh Yes',       // The text of the Yes button
            'no' => 'No way',        // The text of the No button
        ),
        // Options for the Toastr library
        'toastr' => array(
            'options' => array(
                'closeButton' => true,
                'positionClass' => 'toast-top-center'
            ),
        ),
        'lib' => array(
            'uri' => 'https://cdn.jaxon-php.org/libs',
        ),
    ),
```

Usage
-----

### Modal dialogs

This plugin provides functions to show and hide modal dialogs, with a title, a content and zero or more buttons.

```php
/**
 * Show a modal dialog.
 */
public function show($title, $content, array $buttons, array $options = array());

/**
 * Hide the modal dialog.
 */
public function hide();
```

The parameters of the `show()` methods are described as follow:

- `$title`: is a one line text to be printed at the top of the dialog.
- `$content`: the HTML content of the dialog.
- `$buttons`: a list of buttons to be printed in the dialog. Each button is an array with the following entries:
  - `title`: the text to be printed in the button.
  - `class`: the CSS class or classes to be applied on the button.
  - `click`: the javascript code to be executed on a click on this button. It can be defined using the [Request Factory](https://www.jaxon-php.org/docs/requests/factory.html), or it can be set to `close` to close the dialog.
- `$options`: an array of config options that are specific to the javascript library in use.

Example.

```php
    public function showDialog()
    {
        // The dialog buttons
        $buttons = array(
            array(
                'title' => 'Close',
                'class' => 'btn',
                'click' => 'close'
            )
        );
        // The HTML content of the dialog
        $content = "This modal dialog depends on application settings!!";
        // The dialog specific options
        $options = array('width' => 500);
        // Show the dialog
        $this->response->dialog->show("Modal Dialog", $content, $buttons, $options);

        return $this->response;
    }
```

### Alerts or notifications

This plugin provides functions to show 4 different types of alerts or notification messages.

```php
/**
 * Print a success message.
 */
public function success($message, $title = null);

/**
 * Print an information message.
 */
public function info($message, $title = null);

/**
 * Print a warning message.
 */
public function warning($message, $title = null);

/**
 * Print an error message.
 */
public function error($message, $title = null);
```

Example.

```php
    public function save($formValues)
    {
        if(!$this->validator->valid($formValues))
        {
            $this->response->dialog->error("Invalid input", "Error");
            return $this->response;
        }
        $this->repository->save($formValues);
        $this->response->dialog->success("Data saved!", "Success");
        return $this->response;
    }
```

### Confirmation question

The `confirm()` function adds a confirmation question to a Jaxon request, which will then be called only if the user answers yes to the given question.

```php
/**
 * Add a confirmation question to the request
 */
public function confirm($question, ...);
```

The first parameter, which is mandatory, is the question to ask.

The next parameters are optional; they allow the insertion of content from the web page in the confirmation question, using Jaxon or jQuery selectors and positional placeholders.
They are specially useful when pieces of information from the web page need to be inserted in translated strings.

In the example below, the user has to choose a color, and the selected color is inserted in the confirmation question.

Example with Jaxon selector.

```php
<select class="form-control" id="colorselect" name="colorselect" onchange="<?php
    echo rq()->call('HelloWorld.setColor', rq()->select('colorselect'))
        ->confirm('Set color to {1}?', rq()->select('colorselect')) ?>; return false;">
    <option value="black" selected="selected">Black</option>
    <option value="red">Red</option>
    <option value="green">Green</option>
    <option value="blue">Blue</option>
</select>
```

Example with jQuery selector.

```php
<select class="form-control" id="colorselect" name="colorselect" onchange="<?php
    echo rq()->call('HelloWorld.setColor', jq('#colorselect')->val())
        ->confirm('Set color to {1}?', jq('#colorselect')->val()) ?>; return false;">
    <option value="black" selected="selected">Black</option>
    <option value="red">Red</option>
    <option value="green">Green</option>
    <option value="blue">Blue</option>
</select>
```

Supported libraries
-------------------

This package currently supports 16 javascript libraries, each implementing one or more interfaces.

Bootstrap Dialog: https://nakupanda.github.io/bootstrap3-dialog

- Dialog id: bootstrap
- Implements: Modal, Alert, Confirm
- Options:


Bootbox: http://bootboxjs.com

- Dialog id: bootbox
- Implements: Modal, Alert, Confirm
- Options:

jAlert: http://flwebsites.biz/jAlert/

- Dialog id: jalert
- Implements: Alert, Confirm
- Options:

PgwJS: http://pgwjs.com/pgwmodal/

- Dialog id: pgwjs
- Implements: Modal
- Options:

Toastr: https://codeseven.github.io/toastr/

- Dialog id: toastr
- Implements: Alert
- Options:

Tingle: http://robinparisi.github.io/tingle/

- Dialog id: tingle
- Implements: Modal
- Options:

Simply Toast: https://github.com/ericprieto/simply-toast

- Dialog id: simply
- Implements: Alert
- Options:

Noty: http://ned.im/noty/

- Dialog id: noty
- Implements: Alert, Confirm
- Options:

Notify: https://notifyjs.com

- Dialog id: notify
- Implements: Alert
- Options:

Lobibox: http://lobianijs.com/site/lobibox

- Dialog id: lobibox
- Implements: Modal, Alert, Confirm
- Options:

Overhang: http://paulkr.github.io/overhang.js/ (requires jQuery and jQuery UI)

- Dialog id: overhang
- Implements: Alert, Confirm
- Options:

PNotify: http://sciactive.com/pnotify/ (requires jQuery and jQuery UI)

- Dialog id: pnotify
- Implements: Alert, Confirm
- Options:

Sweet Alert: http://t4t5.github.io/sweetalert/

- Dialog id: sweetalert
- Implements: Alert, Confirm
- Options:

JQuery-Confirm: https://craftpip.github.io/jquery-confirm/

- Dialog id: jconfirm
- Implements: Modal, Alert, Confirm
- Options:

IziToast: http://izitoast.marcelodolce.com

- Dialog id: izi.toast
- Implements: Alert, Confirm
- Options:

YmzBox: https://github.com/returnphp/ymzbox

- Dialog id: ymzbox
- Implements: Alert, Confirm
- Options:


Adding a new library
--------------------

In order to add a new javascript library to the Dialogs plugin, a new class needs to be defined and registered.

The class must inherit from `Jaxon\Dialogs\Libraries\Library`, and implement a few functions and interfaces.

### Functions

These optional functions can be defined in the class.

```php
    /**
     * Get the javascript header code and file includes
     *
     * @return string
     */
    public function getJs(){}

    /**
     * Get the CSS header code and file includes
     *
     * @return string
     */
    public function getCss(){}

    /**
     * Get the javascript code to be printed into the page
     *
     * @return string
     */
    public function getScript(){}
```

The `getJs()` and `getCss()` methods return the HTML header code for loading javascript and CSS files of the library.
The `getScript()` method returns the javascript code to be executed after the page is loaded to initialize the library.

### Interfaces

Depending on the javascript library features, the class must implement one or more of the following three interfaces.

```php
namespace Jaxon\Dialogs\Interfaces;

Interface Modal
{
    /**
     * Show a modal dialog.
     */
    public function show($title, $content, array $buttons, array $options = array());

    /**
     * Hide the modal dialog.
     */
    public function hide();
}
```

```php
namespace Jaxon\Request\Interfaces;

Interface Alert
{
    /**
     * Print a success message.
     */
    public function success($message, $title = null);

    /**
     * Print an information message.
     */
    public function info($message, $title = null);

    /**
     * Print a warning message.
     */
    public function warning($message, $title = null);

    /**
     * Print an error message.
     */
    public function error($message, $title = null);
}
```

```php
namespace Jaxon\Request\Interfaces;

interface Confirm
{
    /**
     * Return a script which makes a call only if the user answers yes to the given question
     */
    public function confirm($question, $yesScript, $noScript);
}
```

### Config options

The `getOption($sName)` method provided by the `Jaxon\Dialogs\Libraries\Library` class returns the value of a config option of the library.
The parameter `$sName` is the name of the option without the `dialogs.<library_name>` prefix.

In the example below, a call to `$this->getOption('options.position')` will return the value `center`.

### Registration

After it is defined, the library class needs to be configured and registered before it can be used in the application.

First, declare the class in the Dialogs plugin configuration.

```php
    'dialogs' => array(
        'default' => array(
            'modal' => 'myplugin',   // Default library for modal dialogs
            'alert' => 'myplugin',   // Default library for alerts
            'confirm' => 'myplugin', // Default library for confirmation
        ),
        'classes' => array(
            'myplugin' => \Path\To\My\Plugin::class,
        ),
        'myplugin' => array(         // Plugin config options
            'options' => array(
               'position' => 'center',
            ),
        ),
    ),
```

If you are not using Armada or a framework integration package, make sure to register the classes, after the configuration options are set.

```php
$jaxon->plugin('dialog')->registerClasses();
```

Contribute
----------

- Issue Tracker: github.com/jaxon-php/jaxon-dialogs/issues
- Source Code: github.com/jaxon-php/jaxon-dialogs

License
-------

The package is licensed under the BSD license.
