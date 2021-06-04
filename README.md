## Introduction  <!-- omit in toc -->
This is the list of modern CPP tricks often used in Coding Interviews and Competitive Programming.  
If you like Rachit's work, you can follow at - 
- Discord - https://bit.ly/discord-rachit
- Programming YouTube Channel - https://bit.ly/rachityoutube

## Contents: <!-- omit in toc -->
- [JavaScript like Destructuring using Structured Binding in C++](#javascript-like-destructuring-using-structured-binding-in-c)
- [Powerful Logging and Debugging](#powerful-logging-and-debugging)
  - [How debug macros work?](#how-debug-macros-work)
  - [The Problem with this macro - its not scalable](#the-problem-with-this-macro---its-not-scalable)
  - [Solution using a powerful macro](#solution-using-a-powerful-macro)
  - [Generic Reader and Writer for multiple variables and containers](#generic-reader-and-writer-for-multiple-variables-and-containers)

## JavaScript like Destructuring using Structured Binding in C++
```cpp
pair<int, int> cur = {1, 2};
auto [x, y] = cur;
// x is now 1, y is now 2
// no need of cur.first and cur.second

array<int, 3> arr = {1, 0, -1};
auto [a, b, c] = arr;
// a is now 1, b is now 0, c is now -1
```

## Powerful Logging and Debugging
### How debug macros work?
Straight to the point, I have often used the `debug` macro which stringifies the variable names and their values.

```cpp
#define deb(x) cout << #x << " " << x 
int ten = 10;
deb(ten); // prints "ten = 10"
```

This is often useful in debugging.

### The Problem with this macro - its not scalable
However, when you have multiple variables to log, you end up with more `deb2` and `deb3` macros.

```cpp
#define deb(x) cout << #x << " " << x 
#define deb2(x) cout << #x << " " << x << " "  << #y << " " << y 
#define deb3(x, y, z) cout << #x << " " << x << " "  << #y << " " << y << " "  << #z << " " << z 
```

This is not scalable.

### Solution using a powerful macro
Here is the solution using variadic macros and fold expressions,

```cpp
#define deb(...) logger(#__VA_ARGS__, __VA_ARGS__)
template<typename ...Args>
void logger(string vars, Args&&... values) {
    cout << vars << " = ";
    string delim = "";
    (..., (cout << delim << values, delim = ", "));
}

int xx = 3, yy = 10, xxyy = 103;
deb(xx); // prints "xx = 3"
deb(xx, yy, xxyy); // prints "xx, yy, xxyy = 3, 10, 103"
```

## Generic Reader and Writer for multiple variables and containers
```cpp
template <typename... T>
void read(T &...args) {
    ((cin >> args), ...);
}

template <typename... T>
void write(string delimiter, T &&...args) {
    ((cout << args << delimiter), ...);
}

template <typename T>
void readContainer(T &t) {
    for (auto &e : t) {
        read(e);
    }
}

template <typename T>
void writeContainer(T &t, string delimiter = " ") {
    for (const auto &e : t) {
        write(delimiter, e);
    }
    write("\n");
}
```
### Usage
```cpp
// Question: read three space seprated integers and print them in different lines.
	int x, y, z;
	read(x, y, z);
	write("\n", x, y, z);
	
// even works with variable data types :)
	int n;
	string s;
	read(n, s);
	write(" ", s, "has length", n, "\n");
	
// Question: read an array of `N` integers and print it to the output console.
	int N;
	read(N);
	vector<int> arr(N);
	readContainer(arr);
	writeContainer(arr); // output: arr[0] arr[1] arr[2] ... arr[N - 1]
	writeContainer(arr, "\n);
	/**
	* output:
	* arr[0]
	* arr[1]
	* arr[2]
	* ...
	* ...
	* ...
	* arr[N - 1]
	*/
```
