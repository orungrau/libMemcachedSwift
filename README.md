# libMemcachedSwift
A small Swift package exposing libMemcached to Swift.

## Installation

Install the libMemcached library on your computer. If you're using macOS, install [Homebrew](http://brew.sh/) then run the command `brew install libmemcached`. If you're using Linux, run `apt-get memcached libmemcached-dev` as root.

Modify your Package.swift file to include the following dependency:

    .package(url: "https://github.com/orungrau/libMemcachedSwift.git", from: "0.1.0")

## Use

```Swift
#if os(Linux)
import libMemcachedLinux
#else
import Darwin
import libMemcachedMac
#endif

var key: String = "key"
var value: String = "value"
var flags: UInt32 = 0;
var length: size_t = 0;
var rc: memcached_return?

var memc: UnsafeMutablePointer<memcached_st> = memcached_create(nil)

// Setup memcached server
memcached_server_add(memc, "localhost", 11211);

// Set value
rc = memcached_set(memc, key, strlen(key), value, strlen(value)+1, 0, flags);
if (rc != MEMCACHED_SUCCESS) {
    // handle an error
}

// Get value
var returnValue = memcached_get (memc, key, strlen(key), &length, &flags, &rc!);
if (rc == MEMCACHED_SUCCESS) {
    print(String(cString: returnValue!)) // Print string value
} else {
    // handle an error
}

memcached_free(memc);
```
