<p align="center">
  <img src="https://static.igorora-project.net/Image/Hoa.svg" alt="Hoa" width="250px" />
</p>

---

<p align="center">
  <a href="https://travis-ci.org/igororaproject/Event"><img src="https://img.shields.io/travis/igororaproject/Event/master.svg" alt="Build status" /></a>
  <a href="https://coveralls.io/github/igororaproject/Event?branch=master"><img src="https://img.shields.io/coveralls/igororaproject/Event/master.svg" alt="Code coverage" /></a>
  <a href="https://packagist.org/packages/igorora/event"><img src="https://img.shields.io/packagist/dt/igorora/event.svg" alt="Packagist" /></a>
  <a href="https://igorora-project.net/LICENSE"><img src="https://img.shields.io/packagist/l/igorora/event.svg" alt="License" /></a>
</p>
<p align="center">
  Hoa is a <strong>modular</strong>, <strong>extensible</strong> and
  <strong>structured</strong> set of PHP libraries.<br />
  Moreover, Hoa aims at being a bridge between industrial and research worlds.
</p>

# igorora\Event

[![Help on IRC](https://img.shields.io/badge/help-%23igororaproject-ff0066.svg)](https://webchat.freenode.net/?channels=#igororaproject)
[![Help on Gitter](https://img.shields.io/badge/help-gitter-ff0066.svg)](https://gitter.im/igororaproject/central)
[![Documentation](https://img.shields.io/badge/documentation-hack_book-ff0066.svg)](https://central.igorora-project.net/Documentation/Library/Event)
[![Board](https://img.shields.io/badge/organisation-board-ff0066.svg)](https://waffle.io/igororaproject/event)

This library allows to use events and listeners in PHP. This is an observer
design-pattern implementation.

[Learn more](https://central.igorora-project.net/Documentation/Library/Event).

## Installation

With [Composer](https://getcomposer.org/), to include this library into
your dependencies, you need to
require [`igorora/event`](https://packagist.org/packages/igorora/event):

```sh
$ composer require igorora/event '~2.0'
```

For more installation procedures, please read [the Source
page](https://igorora-project.net/Source.html).

## Testing

Before running the test suites, the development dependencies must be installed:

```sh
$ composer install
```

Then, to run all the test suites:

```sh
$ vendor/bin/igorora test:run
```

For more information, please read the [contributor
guide](https://igorora-project.net/Literature/Contributor/Guide.html).

## Quick usage

We propose a quick overview of how to use events and listeners.

### Events

An event is:
  * **Asynchronous** when registering, because the observable may not exist yet
    while observers start to observe,
  * **Anonymous** when using, because the observable has no idea how many and
    what observers are observing,
  * It aims at a **large** diffusion of data through isolated components.
    Wherever is the observable, we can observe its data.

In Hoa, an event channel has the following form:
`igorora://Event/LibraryName/AnId:pseudo-class#anAnchor`. For instance, the
`igorora://Event/Exception` channel contains all exceptions that have been thrown.
The `igorora://Event/Stream/StreamName:close-before` contains all streams that are
about to close. Thus, the following example will observe all thrown exceptions:

```php
igorora\Event\Event::getEvent('igorora://Event/Exception')->attach(
    function (igorora\Event\Bucket $bucket) {
        var_dump(
            $bucket->getSource(),
            $bucket->getData()
        );
    }
);
```

Because `attach` expects a callable and because Hoa's callable implementation is
smart, we can directly attach a stream to an event, like:

```php
igorora\Event\Event::getEvent('igorora://Event/Exception')->attach(
    new igorora\File\Write('Foo.log')
);
```

This way, all exceptions will be printed on the `Foo.log` file.

### Listeners

Contrary to an event, a listener is:
  * **Synchronous** when registering, because the observable must exist before
    observers can observe,
  * **Identified** when using, because the observable knows how many observers
    are observing,
  * It aims at a **close** diffusion of data. The observers must have an access
    to the observable to observe.

The `igorora\Event\Listenable` interface requires the `on` method to be present to
register a listener to a listener ID. For instance, the following example
listens the `message` listener ID, i.e. when a message is received by the
WebSocket server, the closure is executed:

```php
$server = new igorora\Websocket\Server(…);
$server->on('message', function (igorora\Event\Bucket $bucket) {
    var_dump(
        $bucket->getSource(),
        $bucket->getData()
    );
});
```

## Documentation

The
[hack book of `igorora\Event`](https://central.igorora-project.net/Documentation/Library/Event) contains
detailed information about how to use this library and how it works.

To generate the documentation locally, execute the following commands:

```sh
$ composer require --dev igorora/devtools
$ vendor/bin/igorora devtools:documentation --open
```

More documentation can be found on the project's website:
[igorora-project.net](https://igorora-project.net/).

## Getting help

There are mainly two ways to get help:

  * On the [`#igororaproject`](https://webchat.freenode.net/?channels=#igororaproject)
    IRC channel,
  * On the forum at [users.igorora-project.net](https://users.igorora-project.net).

## Contribution

Do you want to contribute? Thanks! A detailed [contributor
guide](https://igorora-project.net/Literature/Contributor/Guide.html) explains
everything you need to know.

## License

Hoa is under the New BSD License (BSD-3-Clause). Please, see
[`LICENSE`](https://igorora-project.net/LICENSE) for details.
