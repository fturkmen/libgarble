# MacOSX Port - Update Notes
This code is just a simple MacOSX port of libgarble and some additional info has been integrated to the README:

+ Compile Configuration has been done as follows for MacOSX support:
```
./configure LDFLAGS="-L/usr/local/opt/openssl/lib" CPPFLAGS="-I/usr/local/opt/openssl/include" --with-msgpack=yes
```
+ Since *malloc.h* is not supported by MacOSX (is deprecated) any more, the following has been added to *garble.c*
```
#ifdef __APPLE__
#include <stdlib.h>
#else
#include <malloc.h>
#endif
```
+ Resolved type conflict over *uint64_t* and *size_t* within the declaration (garble.h) and definition (garble.c) of the method *garble_create_input_labels*. I changed the declaration, i.e. made it the same as definition...
```
garble_create_input_labels(block *labels, uint64_t n, block *delta,
                           bool privacyfree);
```
+ Again for *malloc.h*, the file *scd.h* has been changed as follows:
```
#ifdef __linux__
#include <malloc.h>
#endif
```


# libgarble
Garbling library based on [JustGarble](http://cseweb.ucsd.edu/groups/justgarble/).

This code is still in alpha and the API is not yet fully stable (as in, future commits may change it).  However, I'd love to hear feedback from anyone who uses it, to help improve the library.

## Installation instructions

Run the following:
```
autoreconf -i
./configure
make
sudo make install
```
This installs two libraries: `libgarble` for actually garbling and evaluating a circuit, and `libgarblec` for writing circuits.

To test, run the following:
```
cd test
make aes
./aes
```

This will garble and evaluate an AES circuit, and present timings in cycles/gate.
