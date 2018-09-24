---
title: 'Functions and Control Flow'
description: 'Initiation to C++ functions. There is no C++ console as in R, so the function is the most basic building block we''ll use. '
---

## C++ functions belong to C++ files

```yaml
type: VideoExercise
key: 6f251aa30b
lang: r
xp: 50
skills: 1
video_link: //player.vimeo.com/video/154783078
video_hls: //videos.datacamp.com/transcoded/000_placeholders/v1/hls-temp.master.m3u8
```

`@projector_key`
a4b68f779b0d9fe902a9e96f4ba1ef2f

---

## What happens when you compile this c++ file

```yaml
type: MultipleChoiceExercise
key: 6cf7e51442
lang: r
xp: 50
skills: 1
```

Read the following C++ code chunk.

```cpp
#include <Rcpp.h>
using namespace Rcpp ; 

double twice(double x){
    return x + x
}
```

This has been saved in a file given by the variable `twice_file`. What will happen if you compile this file with `sourceCpp()`?

`@possible_answers`
- The file compiles.
- The function, `twice()`, is exported to R.
- A compiler error is thrown.

`@hint`
Call `sourceCpp(twice_file)` to find out!

`@pre_exercise_code`
```{r}
library(Rcpp)
twice_file <- file.path(tempdir(), "twice.cpp")
writeLines(
 "#include <Rcpp.h>
using namespace Rcpp ; 

double twice(double x){
    return x + x
}",
  twice_file
)
```

`@sct`
```{r}
msg1 <- "Unfortunately not. Can you see a problem with the file?"
msg2 <- "No. An export was not declared with `[[Rcpp::export]]`."
msg3 <- "Cool compilation contemplation! The missing semicolon after the return values causes compilation to fail."
ex() %>% check_mc(
  3, feedback_msgs = c(msg1, msg2, msg3)
)
```

---

## Boiler plate

```yaml
type: RCppExercise
key: f1d43acd13
lang: r
xp: 100
skills: 1
```

As you saw in the previous exercise, the code below does not compile. 


```{cpp}
#include <Rcpp.h>
using namespace Rcpp ; 

double twice(double x){
    return x + x
}
```

Let's modify it.

`@instructions`
- Fix the syntax error on the function's return line.
- Add an `[[Rcpp::export]]` decorator so that the function is exported to R.

`@hint`
- In C++ statements end with semi colons. 
- Add the line `// [[Rcpp::export]]` before the function definition.

`@pre_exercise_code`
```{r}
library(Rcpp)
```

`@sample_code`
```{cpp}
#include <Rcpp.h>
using namespace Rcpp ; 

// Export the function to R

double twice(double x) {
    // Fix the syntax error
    return x + x
}
```

`@solution`
```{cpp}
#include <Rcpp.h>
using namespace Rcpp ; 

// Export the function to R
// [[Rcpp::export]]
double twice(double x) {
    // Fix the syntax error
    return x + x ;
}
```

`@sct`
```{r}

library(testwhat.ext)
ex() %>% check_cpp() %>% {
    check_code(., "return\\s+x\\s*\\+\\s*x\\s*;", missing_msg = "No semicolon")
    check_cpp_function_exported(., "double", "twice")
}

success_msg("First-class fixing! You need to add the `[[Rcpp::export]]` decorator to any function that you would like to be able to use in R.")

```

---

## Writing functions in C++

```yaml
type: VideoExercise
key: 0a6485c96f
lang: r
xp: 50
skills: 1
video_link: //player.vimeo.com/video/154783078
video_hls: //videos.datacamp.com/transcoded/000_placeholders/v1/hls-temp.master.m3u8
```

`@projector_key`
add19cbafc1b542ea1071a1a3e75f538

---

## First function - again

```yaml
type: RCppExercise
key: 5ceda676b8
lang: r
xp: 100
skills: 1
```

Recall the `the_answer()` function from Chapter 1, that always returns `42`. Let's rewrite it, this time in a C++ file.

The file's components are: 


1. An "#include" directive to get access to `Rcpp` functionality.
1. A `using namespace` declaration, for convenient typing.
1. The function definition.
1. An Rcpp R comment block, to call the function once the C++ code is compiled.

`@instructions`
- Include the `Rcpp.h` header file.
- Declare that you use the Rcpp namespace.
- Make the function return `42`.
- In the Rcpp R comment block, call the function.

`@hint`
- To include `Rcpp.h`, write it without quotes inside the angle brackets.
- The syntax to declare the `std` namespace is `using namespace std;`. Adapt this for `Rcpp`.
- Remember the semicolons at the end of each line!
- Call `the_answer()` without any arguments.

`@pre_exercise_code`
```{r}
library(Rcpp)
```

`@sample_code`
```{cpp}
// Include the Rcpp.h header
#include <___>

// Use the Rcpp namespace
using ___ ___;

// [[Rcpp::export]]
int the_answer() {
    // Return 42
    return ___;
}

/*** R
# Call the_answer() to check you get the right result
___
*/
```

`@solution`
```{cpp}
// Include the Rcpp.h header
#include <Rcpp.h>

// Use the Rcpp namespace
using namespace Rcpp ;

// [[Rcpp::export]]
int the_answer() {
    // Return 42
    return 42;
}

/*** R
# Call the_answer() to check you get the right result
the_answer()
*/
```

`@sct`
```{r}

library(testwhat.ext)
ex() %>% {
  check_cpp(.) %>% {
    check_code(., "#include\\s+<Rcpp.h>", missing_msg = "No header inclusion")
    check_code(., "using\\s+namespace\\s+Rcpp\\s*;", missing_msg = "Not correctly using namespace")
    check_code(., "return\\s+42\\s*;", missing_msg = "Not returning 42 correctly")
  }
  check_embedded_r(.) %>% check_function("answer") %>% check_result()
}


success_msg("Wonderfully written! This code pattern of include Rcpp, use the namespace, define a function, then run it, is very common in Rcpp development.")

```

---

## Exported and unexported functions

```yaml
type: RCppExercise
key: 6ae975ec02
lang: r
xp: 100
skills: 1
```

Functions that are decorated with the `[[Rcpp::export]]` special comment are made available to the R console. However, you typically don't want to export every function to R. Instead, some functions can be kept internal to C++. These are often lower-level calculation functions or utility functions.

Recall the function that calculates distance from a point in the 2d space to the origin: 

```
double dist( double x, double y) {
  return sqrt( x*x + y*y ) ;
}
```

Here, you'll update the code so that the `dist` function (that is exported) calls an unexported `square` function that just squares a `double`.

`@instructions`
- Complete the definition of `square()` so that it accepts and returns a `double`, ...
- that is equal to `x` times `x`.
- Update the definition of `dist()` to use `square()`. That is, to return the square of `x` plus the square of `y`.

`@hint`
- The signature line for `square()` should be `double square(double x) {`.
- The return value for `square()` is `x * x`.
- The updated return value of `dist()` is `square(x)` plus `square(y)`, all square-rooted with `sqrt()`.

`@pre_exercise_code`
```{r}
library(Rcpp)
```

`@sample_code`
```{r}
#include <Rcpp.h>
using namespace Rcpp; 

// Make square() accept and return a double
___ square(___ x) {
  // Return x times x
  return ___ ;
}
// [[Rcpp::export]]
double dist(double x, double y) {
  // Change this to use square()
  return sqrt(___ + ___);
}
```

`@solution`
```{r}
#include <Rcpp.h>
using namespace Rcpp; 

// Make square() return a double
double square(double x) {
  // Return x times x
  return x * x;
}


// [[Rcpp::export]]
double dist(double x, double y) {
  // Change this to use square()
  return sqrt(square(x) + square(y));
}
```

`@sct`
```{r}
success_msg("Radical refactoring! Code written as many small functions is usually easier to maintain and debug, compared to having one gigantic function.")
```

---

## R code in C++ files

```yaml
type: RCppExercise
key: 6a41ea884e
lang: r
xp: 100
skills: 1
```

When you compile your C++ file with `sourceCpp()` (or the "Source" button in RStudio), `Rcpp` compiles your code and makes the exported 
functions available as R functions. 

`sourceCpp()` also treats comments between `/*** R` and `*/` as R code to be executed once the code is compiled. 

```{cpp}
/*** R
# Run the exported R code here
*/
```

This is particularly useful when you are developing your code because you can very quickly test the effects of changes to your code. 

You will use these special comments throughout the rest of this course.

`@instructions`
- Start an Rcpp R comment block with `/*** R`.
- Call the `dist` function to calculate the distance from the origin to the `(3,4)` point.
- Close the Rcpp R comment block with `*/`.

`@hint`
- The block begins with `/*** R` and ends with `*/`, each on its own line.
- Call `dist(3, 4)` to calculate the distance.

`@pre_exercise_code`
```{r}
library(Rcpp)
```

`@sample_code`
```{cpp}
#include <Rcpp.h>
using namespace Rcpp; 

double square(double x) {
  return x * x ;
}

// [[Rcpp::export]]
double dist(double x, double y) {
  return sqrt(square(x) + square(y));
}

// Start the Rcpp R comment block
___
# Call dist() to the point (3, 4)
___
# Close the Rcpp R comment block
___
```

`@solution`
```{cpp}
#include <Rcpp.h>
using namespace Rcpp; 

double square(double x) {
  return x * x ;
}

// [[Rcpp::export]]
double disty(double x, double y) {
  return sqrt(square(x) + square(y));
}

// Start the Rcpp R comment block
/*** R
# Call dist() to the point (3, 4)
disty(3, 4)
# Close the Rcpp R comment block
*/
```

`@sct`
```{r}
success_msg("Awesome R-in-C++-file action! These comment blocks are also handy when you want to share your code as a single file, for example as a [GitHub gist](https://help.github.com/articles/about-gists).")
```

---

## if and if/else

```yaml
type: RCppExercise
key: fef3d40537
lang: r
xp: 100
skills: 1
```

Just like in R, you can use the `if` and `else` keywords for branching. The syntax is the same as in R.

```{cpp}
if(condition) {
  // Code to run if the condition is TRUE
} else {
  // Code to run otherwise
}
```

Here you'll use `if` and `else` to complete the definition of an `absolute()` function to calculate the absolute value of a floating point number. (This mimics the C++ function [`fabs()`](http://www.cplusplus.com/reference/cmath/fabs).)

`@instructions`
- Test if x is greater than zero.
- If the condition holds, return `x`.
- Add the keyword for what to do otherwise.
- If the condition doesn't hold, return negative `x`.

`@hint`
- Call `if()` with condition `x > 0`.
- Remember the semicolon when returning `x`.
- The keyword for the "otherwise" case is `else`.
- The second return value is `-x`.

`@pre_exercise_code`
```{r}
library(Rcpp)
```

`@sample_code`
```{r}
#include <Rcpp.h>
using namespace Rcpp ;

// [[Rcpp::export]]
double absolute(double x) {
  // Test for x greater than zero
  ___(___) {
    // Return x
    ___ 
  // Otherwise
  } ___ {
    // Return negative x
    ___
  }
}

/*** R  
absolute(pi)
absolute(-3)
*/
```

`@solution`
```{r}
#include <Rcpp.h>
using namespace Rcpp ;

// [[Rcpp::export]]
double absolute(double x) {
  // Test for x greater than zero
  if(x > 0) {
    // Return x
    return x;
  // Otherwise
  } else {
    // Return negative x
    return -x;
  }
}

/*** R  
absolute(pi)
absolute(-3)
*/
```

`@sct`
```{r}
success_msg('Absolutely fabulous! Branching your code using if-else is sometimes called "flow control".')
```

---

## For loops

```yaml
type: VideoExercise
key: d521511b2d
lang: r
xp: 50
skills: 1
video_link: //player.vimeo.com/video/154783078
video_hls: //videos.datacamp.com/transcoded/000_placeholders/v1/hls-temp.master.m3u8
```

`@projector_key`
2b5067806e451827d2278708f17763d4

---

## Calculating square roots with a for loop

```yaml
type: RCppExercise
key: 99391205b5
lang: r
xp: 100
skills: 1
```

Just as in R, C++ for loops run the same code a specified number of times, only changing an index value on each iteration. The syntax for a `for` loop is slightly more complex than R though.

```{cpp}
for(int i = 0, i < n, i++) {
  // Do something
}
```

- `int i = 0` declares the index to be an integer (the most common case), and sets the value to be `0` on the first iteration.
- `i < n` sets the iteration condition: once `i` reaches the value `n`, this condition fails and the loop will stop running.
- `i++` means increase the value of `i` by `1` on each iteration.

Here you'll complete the definition of a function for calculating the square-root using the [Babylonian method](https://en.wikipedia.org/wiki/Methods_of_computing_square_roots). (For real-world code, you should just use `sqrt()`, which uses a faster modern algorithm.)

![](https://assets.datacamp.com/production/repositories/820/datasets/29d7493c34f1f40f51621773b4d483f2590ca635/xn.png)

`@instructions`
- Initialize a local `double` x to one.
- Specify a `for` loop.
    - Initialize an integer, `i`, to `0`.
    - Set the iteration condition as `i` less than `n`.
    - Increment `i` by one on each step.

`@hint`
- `x` is a `double`, so you need to include a decimal point.
- The arguments to the `for` loop are `int i = 0`, `i < n`, and `i++`.

`@pre_exercise_code`
```{r}
library(Rcpp)
```

`@sample_code`
```{cpp}
#include <Rcpp.h>
using namespace Rcpp;

// [[Rcpp::export]]
double sqrt_approx(double value, int n) {
    // Initialize x to be one
    double x = ___;
    // Specify the for loop
    ___(int i = ___; ___; ___) {
        x = (x + value / x) / 2.0;
    }
    return x;
}

/*** R
sqrt_approx(2, 10)
*/
```

`@solution`
```{cpp}
#include <Rcpp.h>
using namespace Rcpp;

// [[Rcpp::export]]
double sqrt_approx(double value, int n) {
    // Initialize x to be one
    double x = 1.0;
    // Specify the for loop
    for(int i = 0; i < n; i++) {
        x = (x + value / x) / 2.0;
    }
    return x;
}

/*** R
sqrt_approx(2, 10)
*/
```

`@sct`
```{r}
success_msg("Formidable for-looping! The `++` operator is a useful shortcut for increasing the value of a variable by one.")
```

---

## Breaking out of a for loop

```yaml
type: RCppExercise
key: 6b7687344d
lang: r
xp: 100
skills: 1
```

Sometimes you may wish to exit a loop before you've completed all the iterations. For example, your code may succeed in its task before all possibilities have been tried. The `break` keyword can be used to break out of a loop early. 

The code pattern looks like this:

```{cpp}
for(int i = 0; i < n; i++) {
  // Do something

  // Test for exiting early
  if(breakout_condition) break;
  
  // Otherwise carry on as usual
}
```

Here you'll rework the previous example so that the loop stops when the value does not change too much (based on a threshold). The function has been modified so that it returns a `List`, enabling you to see the approximation at each iteration.

`@instructions`
Add a break condition that triggers if the solution is good enough.

`@hint`
- Call `if()`, with condition `is_good_enough`.
- If that condition is true, call `break`.

`@pre_exercise_code`
```{r}
library(Rcpp)
```

`@sample_code`
```{r}
#include <Rcpp.h>
using namespace Rcpp;

// [[Rcpp::export]]
List sqrt_approx(double value, int n, double threshold) {
    double x = 1.0;
    double previous = x;
    bool is_good_enough = false;
    int i;
    for(i = 0; i < n; i++) {
        previous = x;
        x = (x + value / x) / 2.0;
        is_good_enough = fabs(previous - x) < threshold
        
        // If the solution is good enough, then "break"
        ___(___) ___;
    }
    return List::create(_["i"] = i , _["x"] = x);
}

/*** R
sqrt_approx(2, 1000, 0.1)
*/
```

`@solution`
```{r}
#include <Rcpp.h>
using namespace Rcpp;

// [[Rcpp::export]]
List sqrt_approx(double value, int n, double threshold) {
    double x = 1.0;
    double previous = x;
    bool is_good_enough = false;
    int i;
    for(i = 0; i < n; i++) {
        previous = x;
        x = (x + value / x) / 2.0;
        is_good_enough = fabs(previous - x) < threshold;
        
        // If the solution is good enough, then "break"
        if(is_good_enough) break;
    }
    return List::create(_["i"] = i , _["x"] = x);
}

/*** R
sqrt_approx(2, 1000, 0.1)
*/
```

`@sct`
```{r}
success_msg("Breathtaking breaking! Using `break` meant that the loop did not have to execute all 1000 iterations of the for loop.")
```

---

## While loops

```yaml
type: VideoExercise
key: 4a286ec63a
lang: r
xp: 50
skills: 1
video_link: //player.vimeo.com/video/154783078
video_hls: //videos.datacamp.com/transcoded/000_placeholders/v1/hls-temp.master.m3u8
```

`@projector_key`
98301ac86c045abd60e4f420917459c7

---

## Calculating square roots with a while loop

```yaml
type: RCppExercise
key: c3957497a6
lang: r
xp: 100
skills: 1
```

While loops keep running iterations until a condition is no longer met. The sytnax for a C++ while loop is the same is in R.

```cpp
while(condition) {
  // Do something
}
```

`@instructions`
Specify the while loop, so it keeps iterating while the value `is_good_enough` is false.

`@hint`
`!` is the C++ not operator (same as in R).

`@pre_exercise_code`
```{r}
library(Rcpp)
```

`@sample_code`
```{cpp}
#include <Rcpp.h>
using namespace Rcpp;

// [[Rcpp::export]]
double sqrt_approx(double value, double threshold) {
    double x = 1.0;
    double previous = x;
    bool is_good_enough = false;
    
    // Specify the while loop
    ___(___) {
        previous = x;
        x = (x + value / x) / 2.0;
        is_good_enough = fabs(x - previous) < threshold;
    }
    
    return x ;
}

/*** R
sqrt_approx(2, 0.00001)
*/
```

`@solution`
```{cpp}
#include <Rcpp.h>
using namespace Rcpp;

// [[Rcpp::export]]
double sqrt_approx(double value, double threshold) {
    double x = 1.0;
    double previous = x;
    bool is_good_enough = false;
    
    // Specify the while loop
    while(!is_good_enough) {
        previous = x;
        x = (x + value / x) / 2.0;
        is_good_enough = fabs(x - previous) < threshold;
    }
    
    return x ;
}

/*** R
sqrt_approx(2, 0.00001)
*/
```

`@sct`
```{r}
success_msg("Wonderful while looping! Notice that using a while loop let you avoid having to specify the number of iterations beforehand.")
```

---

## Do it again: do-while loops

```yaml
type: RCppExercise
key: 434e2cfe50
lang: r
xp: 100
skills: 1
```

<!--`do`/`while` loops shifts the order between the condition and the code. It is 
sometimes more natural to express the condition at the end of the current 
iteration rather than at the beginning.-->

The `repeat` loop in R is called a **do while** loop in C++. Compared to a `while` loop, it moves the condition to the end of the loop, so that the loop contents are guaranteed to be run at least once. The syntax is as follows.

```cpp
do {
  // Do something
} while(condition);
```

Notice the semicolon after the while condition.

`@instructions`
- Initiate the loop with a `do` statement.
- Specify the `while` condition, so the loop keeps iterating until it `is_good_enough`.

`@hint`
- The `while` condition clause is *not* `is_good_enough`.
- Append a semicolon to the while condition.

`@pre_exercise_code`
```{r}
library(Rcpp)
```

`@sample_code`
```{r}
#include <Rcpp.h>
using namespace Rcpp;

// [[Rcpp::export]]
double sqrt_approx(double value, double threshold) {
    double x = 1.0;
    double previous = x;
    bool is_good_enough = false;
    
    // Initiate do while loop
    ___ {
        previous = x;
        x = (x + value / x) / 2.0;
        is_good_enough = fabs(x - previous) < threshold;
    // Specify while condition
    } ___
    
    return x;
}

/*** R
sqrt_approx(2, 0.00001)
*/
```

`@solution`
```{r}
#include <Rcpp.h>
using namespace Rcpp;

// [[Rcpp::export]]
double sqrt_approx(double value, double threshold) {
    double x = 1.0;
    double previous = x;
    bool is_good_enough = false;
    
    // Initiate do while loop
    do {
        previous = x;
        x = (x + value / x) / 2.0;
        is_good_enough = fabs(x - previous) < threshold;
    // Specify while condition
    } while(!is_good_enough);
    
    return x;
}

/*** R
sqrt_approx(2, 0.00001)
*/
```

`@sct`
```{r}
success_msg("What a doozy! That concludes the tour of the three loop types.")
```
