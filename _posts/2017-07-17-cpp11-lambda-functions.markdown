---
layout: post
title: "C++11 Lambda Functions"
date: 2017-07-17 22:20
comments: true
categories: CPP11 programming
---

Lambda functions have been a long awaited feature for C++ developers appearing on the scene in the C++11 standard.

A Lambda function is a convenient way to define anonymous functions (functions without a name) inside a function body which can then be invoked, returned, and passed around to other functions.

Lambda functions generally lead to cleaner, clear, maintainable code which makes them popular amongst programmers.

## The Basic Syntax
The basic lambda syntax defines *captures* which are elements outside the lambda function which you can pass by value or reference, *parameters* which are your function parameters, an optional return type, and the *body* of the function which consists of C++ statements.

```
[captures] (parameters) -> return-type { body; }
```

* *captures* are elements outside your function which you want to pass by reference or value for use in your lambda function.  By default, lambda functions won’t have access to the outer scope.

* *parameters* your function parameters, for example `func = [](int n) { ... }`  can be invoked with `func(n)`

* *return-type* which is optional since the compiler can automatically determine the functions return type most of the time.

* *body* your lambda functions body consisting of C++ statements.

## Hello Lambda
Here is a “Hello World” example using the basic lambda syntax. Note, we are using `auto`  here which allows for return type deduction for lambda functions. The compiler will figure out the return type automatically as it can see that nothing is returned from the lambda function:

``` c++
#include <iostream>
using namespace std;

int main() {
  auto func = [] () { cout << "Hello World!" << endl; }
  func()
}
```

You can quickly test drive the above function if you have compatible C++ compiler installed. However, you will need to compile with C++11 support using the `-std=c++11` flag:

`g++ -std=c++11 hello_world.cpp -o hello_world`

## Working with Generic Functions
C++ includes generic functions such as `std::for_each` and `std::transform`. Usually one would pass a function reference to these methods, however, with lambda functions we can define functions inline avoiding the verbosity of named functions everywhere in our code.

Here is an example using [std::transform](http://en.cppreference.com/w/cpp/algorithm/transform)  which iterates over an array of elements given a transformation function:

``` c++
#include <iostream>
#include <algorithm>

using namespace std;

int main() {
    int arr[] = { 1, 2, 3, 4, 5 };
    int size = sizeof(arr) / sizeof(int);

    transform(arr, arr + size, arr, [](int n) {
        return n * 2;
    });

    for(int i = 0 ; i < size ; ++i) {
        cout << arr[i] << " ";
    }
}
// outputs => 2 4 6 8 10
```

## Captures

So far we’ve looked at creating lambda functions without specifying captures `[]`.  Captures allow you to capture variables outside your function by either copying them by value or reference.

### Passing by value

Using `[variable]`  we can capture a variable by value. This will copy the value and make it immutable inside the lambda function. For example:

``` c++
int a = 1;
auto add = [a] () {
  cout << "value is " << a << endl;
};
a = 2;
```

Will output:

``` bash
$ ./a.out
value is 1
```

### Passing by reference

Using `[&variable]` will allow us to pass by reference. This will allow you reference variables outside your lambda function. For example,

``` c++
int a = 1;
auto add = [&a] () {
  cout << "value is " << a << endl;
};
a = 2;
```

Will output:

``` bash
$ ./a.out
value is 2
```

If you’re passing by reference then the capture is also mutable. You can update the value inside the function and it will update in the scope where it was defined. For example,

``` c++
int a = 1;
auto func = [&a] () {
    a = 2;
};
func();
cout << "value is " << a << endl;
```

Will output:

``` bash
$ ./a.out
value is 2
```

### Captures all the things 

If you get tired of explicitly defining every capture the compiler will do the hard work for you with the following syntax sugar:

*Using [=]*

All elements will be passed by value:

``` c++
int a = 1;
int b = 2;

auto func = [=]() {
  return a + b;
}
```

*Using [&]*

All elements are passed by reference:

``` c++
int a = 0;
int b = 0;

auto func = [&]() {
  a = 1;
  b = 2;
}

return a + b;
```

### That’s all folks

I’m still learning C++11 and I’ll be sure to post more blog entries in the future. Let me know on Twitter if I messed something up @rramsden :)
