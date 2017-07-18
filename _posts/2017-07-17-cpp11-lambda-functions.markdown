---
layout: post
title: "C++11 Lambda Functions"
date: 2017-07-17 22:20
comments: true
categories: CPP11 programming
---

Lambda functions have been a long awaited feature for C++ developers appearing on the scene in the C++11 standard.

A Lambda function, also known as an anonymous function (a function without a name) is a convenient way to define a functions without  binding them to an identifier. If a function is only used once, an anonymous function can be defined inline avoiding the verbosity of defining a named function.

Lambda functions generally lead to shorter, concise, and easier to maintain code, which makes them an extremely popular language feature.

## The Basic Syntax
The basic lambda syntax looks like the following:

```
[captures] (parameters) -> return-type { body; }
```

* *captures* are elements outside your function which you want to pass by reference or value for use in your lambda function.  By default, lambda functions won’t have access to variables outside their scope unless they are captured

* *parameters* are your function parameters

* *return-type* defines an optional return type for your function. Most of the time the compiler can infer the return type so its not needed.

* *body* is your lambda functions body consisting of C++ statements.

## Hello Lambda
Here is a “Hello World” example using the basic lambda syntax: 

``` c++
#include <iostream>
using namespace std;

int main() {
  auto func = [] () { cout << "Hello World!" << endl; }
  func()
}
```

Note, we are using `auto`  here which allows for return type deduction for lambda functions. The compiler will figure out the return type automatically as it can see nothing is returned here.

You can quickly test drive the above function if you have compatible C++ compiler installed. However, you will need to compile with C++11 support using the `-std=c++11` flag:

`g++ -std=c++11 hello_world.cpp -o hello_world`

## Working with Generic Functions
C++ includes generic functions such as `std::for_each` and `std::transform`. Usually one would pass a function reference to these methods, however, with lambda functions we can define functions inline rather than defining a named function outside somewhere.

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
Captures allow access to variables outside your lambda function. These are defined in between the square brackets `[]` of your lambda function. There are two ways to capture a variable:  copying them by value, or by reference.

### Passing by value

Using `[variable]`  we can capture a variable by value. This will copy the value, and make it immutable (cannot be changed) inside the lambda function. For example:

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

If you’re passing by reference then the capture is also mutable. You can update the value inside the function and it will update everywhere.

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

If you are tired of explicitly defining every capture the compiler will do the hard work for you with the following syntactic sugar:

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

I’m still learning C++11 and I’ll be sure to post more blog entries in the future.
