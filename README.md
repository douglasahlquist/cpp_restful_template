
# Warning:
The api might change as the library is in pre-alpha stage. If you find any bugs
please open an issue. 

# Restful
Restful is a simple REST client for C++, built on top of libcurl.


## Integration

You can use your project's existing build system to compile
all the files in `src` directory and link to your project.

To use this all you need to do is add

```cpp
#include <restful.hpp>

// for a more readable name
using Http = commio::Handle;
```

## Examples

```cpp


/*
 * A simple get request
 **/

auto res1 = Http().get("http://www.ahlquist.com/resume.pdf")
            .exec();

/*
 * A get request with basic auth and a custom header
 **/

auto res2 = Http().get("http://www.ahlquist.com/get", "password-for-basic-auth")
            .header({{"Hello", "This is a header"}, {"Second","Another header"}})
            .exec();

/*
 * A post request with basic auth
 **/
auto post1 = Http().post("http://www.ahlquist.com/post", "super-secret-password")
                    .content("text/plain", "Hello world")
                    .exec();

/*
 * A simple put request
 **/

auto put1 = Http().put("http://www.ahlquist.com/put")
            .content("text/plain", "Hello world")
            .exec();

/*
 * A simple delete request
 **/

auto del = Http().del("http://www.ahlquist.com/delete")
           .exec();


```


## Dependencies
* [libcurl](http://curl.haxx.se/libcurl/)
* A compiler with C++11 support (GCC 4.8 and higher, Clang 3.4 and higher)
* cmake (At least version 2.8.11)
* [gtest](https://chromium.googlesource.com/chromium/testing/gtest) - for building and running the tests
* [json](https://github.com/nlohmann/json) - as a helper in testing


**Note:** A vagrant file has been provided that will provision a Ubuntu14.04
box with all the pre-requisites. Just ```vagrant up``` then ```vagrant ssh```.
You will find the code in /vagrant directory inside the vagrant box.

## Steps to build
* cd build
* cmake ..
* make
* make test (optional)

## Contribute
* Fork the project
* Make an addition or bugfix
* Add test cases
* Commit, then make a pull request (topic branches are recommended)
