<img src="header_graphic.png?raw=true" alt="PHP7 Mapnik" title="Generated by PHP7 Mapnik" width="640" height="160">

Introduction
------------

PHP7 Mapnik brings geospatial rendering to PHP via the powerful [Mapnik](http://mapnik.org/) XML API.
It is ideal for building dynamic tile servers or livening up websites with custom static maps.

[![Build Status](https://travis-ci.org/garrettrayj/php7-mapnik.svg?branch=master)](https://travis-ci.org/garrettrayj/php7-mapnik)

Requirements
------------

* PHP 7.0.x
* Mapnik 3.0.x

Installation
------------

    # git clone ... && cd php7-mapnik
    phpize
    ./configure --with-mapnik
    make test
    echo "extension=`pwd`/modules/mapnik.so" | sudo tee -a /etc/php.ini

Usage
-----

    <?php

    // Register datasource plugins
    $pluginConfigOutput = [];
    exec('mapnik-config --input-plugins', $pluginConfigOutput);
    \Mapnik\DatasourceCache::registerDatasources($pluginConfigOutput[0]);

    // Create map
    $map = new \Mapnik\Map(640, 480);

    // Register fonts
    $fontConfigOutput = [];
    exec('mapnik-config --fonts', $fontConfigOutput);
    $map->registerFonts($fontConfigOutput[0]);

    // Load Mapnik XML
    $map->loadXmlFile('my_awesome_map.xml', false, $basePath);

    // Situate the map within the viewport
    $map->zoomAll();

    // Create image
    $image = new \Mapnik\Image(640, 480);

    // Render
    $renderer = new \Mapnik\AggRenderer($map, $image);
    $renderer->apply();

    // Save PNG image
    $image->saveToFile('my_awesome_map.png');

Developer Notes
---------------

* Do NOT compile Boost with C++11. On Mac at least, you'll have a bad time with missing symbols.

License
-------

The MIT License (MIT)

Copyright &copy; 2016 Garrett Johnson

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated
documentation files (the "Software"), to deal in the Software without restriction, including without limitation the
rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit
persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE
WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR
COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR
OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.