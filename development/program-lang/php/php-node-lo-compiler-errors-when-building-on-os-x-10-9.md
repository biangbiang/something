PHP – node.lo compiler errors when building on OS X 10.9
========================================================

Some older versions of PHP build fine on OS X 10.7 but fall over while compiling node.c on OS X 10.9

If you have a working but slightly geriatric PHP version built on a previous OS X version you might encounter some compiler errors if you need to build it again after upgrading to 10.9

    php-5.3.6/ext/dom/node.c:1903:21: error: 
          incomplete definition of type 'struct _xmlBuf'
                            ret = buf->buffer->use;
                                  ~~~~~~~~~~~^
    php-5.3.6/ext/dom/node.c:1905:40: error: 
          incomplete definition of type 'struct _xmlBuf'
                          RETVAL_STRINGL((char *) buf->buffer->con...
                                                  ~~~~~~~~~~~^
     
    22 warnings and 2 errors generated.
    make: *** [ext/dom/node.lo] Error 1

Dig a little through the warnings spewed out by this part of the build and there’s a good clue to the cause:-

    /Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/
    Developer/SDKs/MacOSX10.9.sdk/usr/include/libxml2/
    libxml/tree.h:104:16: note:
          forward declaration of 'struct _xmlBuf'
    typedef struct _xmlBuf xmlBuf;
                   ^

The version of libxml2 bundled with the 10.9 developer tools has moved on a little bit too far for some elderly PHP builds (such as the 5.3.6 one above) to cope with.

Advancing to the latest minor version of your target PHP version (e.g. 5.3.28 here) might be worth the investment, but if you’re attached to your existing version it’s fortunately not too hard to get past this error by downloading and building into a local directory an older version of libxml2, such as 2.7.8, from ftp://xmlsoft.org/libxml2/:-

    tar xvfz libxml2-2.7.8.tar.gz
    cd libxml2-2.7.8
    mkdir dist
    ./configure --prefix /home/devsumo/libxml2-2.7.8/dist
    make
    make install

And then point your PHP build at it:-

    cd php-5.3.6
    rm config.cache
    make clean
    ./configure --prefix /home/devsumo/php-5.3.6/dist
          --with-libxml-dir=/home/devsumo/libxml2-2.7.8/dist
    make
    make install

At least on the build tested here it was necessary to remove config.cache for the configure script to pick up the change of location. This, at least for me, gets make test results matching those from OS X 10.7.
