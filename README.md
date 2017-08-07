# Base Docker images for Swift
These images are currently the smallest possible to run a swift app on.  To run the a compiled app on this image it
must be compiled with the flags `-Xswiftc -static-stdlib` to statically link Swift Core to the image.  In addition to 
this the shared object such as libFoundation.so must be copied from a compatible ubuntu installation.  In the ibmcom/kitura-ubuntu 
image these libraries reside at the path `/usr/lib/swift/linux/*.so`.

To compile your application you can use the following snippet which can be added to a Makefile.

```automake
BUILD_COMMAND=swift build -c release 
STATIC_BUILD_COMMAND=${BUILD_COMMAND} -Xswiftc -static-stdlib

build:
	${BUILD_COMMAND}

build_release:
	${RELEASE_BUILD_COMMAND}

build_linux_small:
	docker run -it --rm -v $(shell pwd):/src -w /src ibmcom/kitura-ubuntu ${STATIC_BUILD_COMMAND} -Xlinker -L/usr/lib/swift/linux
	docker run -it --rm -v $(shell pwd):/src -w /src ibmcom/kitura-ubuntu /bin/bash -c "cp /usr/lib/swift/linux/*.so /src/linux"
```
