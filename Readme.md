# ScriptAuth

ScriptAuth plugin for 3PROXY.

This plugin allows to validate (proxy) client's username and password using script file.

Username and password can contain only digits and letters.

## Install

Clone or download ``ScriptAuth`` plugin to `/<3proxy source files>/src/plugins` directory.

Edit `Makefile.Linux` file and add `ScriptAuth` to your plugins list, like in example below:

    PLUGINS = StringsPlugin TrafficPlugin PCREPlugin TransparentPlugin ScriptAuth

Compile 3PROXY:

    make -f Makefile.Linux

## Configure

In your `3proxy.cfg` file include `scriptauth.ld.so` library/plugin, like in example bellow:

    plugin "/<path to>/scriptauth.ld.so" start "<script>"

Plugin settings:

* start - run plugin
* script - Path to you script file in order to execute. Ex. ``/etc/3proxy/scripts/auth.php``

After that, in your `3proxy.cfg` file you will be able to use ScriptAuth plugin in order to authenticate users:

    ...
    authcache user,password 60
    auth cache strong script
    allow *
    ...

    proxy -p3128

## Example

An example in PHP (using curl to request your API) you can found in ``scripts/auth.php`` file.

## Issues

Issue: `error: ld returned 1 exit status`...

    ...
    make[2]: Entering directory `/root/3proxy/src/plugins/ScriptAuth'
    gcc -o ../../scriptauth.ld.so -O2 -fno-strict-aliasing -pthread -shared -lpam scriptauth.o
    /usr/bin/ld: cannot find -lpam
    collect2: error: ld returned 1 exit status
    make[2]: *** [scriptauth.ld.so] Error 1
    make[2]: Leaving directory `/root/3proxy/src/plugins/ScriptAuth'
    ...

Solution:

Install `libpam` modules:

    sudo apt-get install libpam0g-dev
