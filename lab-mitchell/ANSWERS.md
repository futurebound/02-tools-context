1. When this code is run in Node, e.g. node index.js, what are the two stages of execution for this file called, and which order do they happen in?

The two stages of execution are compilation and then actual execution, or the running of the compiled and optimized code.


2. Write an explanation, using as much space as you need, relating to how the first stage of execution for this file operates.
  * For example, identify the high level steps in a line by line overview and then define what each of those steps are accomplishing.

The first stage (compilation) consists of three main parts: tokening/lexing, parsing, and code-generation. The conversation goes line by line in compilation, checking each declaration and attempting to identify variables and their assignments in nested scope. 

Beginning on line 3, the compiling engine checks with global scope to see if it has a value for the declared var foo, which returns the string bar. It then moves to line 5 where it checks to see if function bar exists, and what is assigned to it. Within the scope of the function bar, var foo is created and assigned a specific local value instead of reassigning the global variable foo, and is given the value of the string 'baz'. On line 8, the engine checks to see if the nested function scope of baz exists and has a value. Since it has the parameter foo, although it exists in both its parent function scope as well as the global scope, a new var foo is essentially created for the nested baz function scope. On line 10 the engine checks to see whether the variable foo exists in the baz function scope, which it does, and then assigns it the value of the string 'bam'. On line 11, there are differences between using strict mode or not. In strict mode, there is no var foo so running the file will result in an error on this line since bam has not been declared. If not running in strict mode, the engine will check to see if bam exists in the baz function scope, will find it doesn't and then check the bar function scope, will not find it their either and will then check the global scope. The engine will then create a var in the global scope for bam, and will then assign it the value of the string 'yay' local to the baz function scope.

On line 13, the engine won't do anything since the code is invoking a function. This line will be revisited in the second phase. This is the same for lines 16 and 19 as well.
Lines 17 & 18 will also be processed in the second phase.


3. Write an explanation, using as much space as you need, relating to how the second stage of execution for this file operates.
  * For example, identify the high level steps in a line by line overview and then define what each of those steps are accomplishing.

In the execution phase, the engine will check with scope to determine what the value of variables are in their respective scopes, since it's already confirmed they exist, and then execute the various functions and variable calls in lines 16-19. On line 13, 16, and 19 those respective functions will be called as needed, 16 -> 13 -> 19, although line 19 will not be able to run since the function invocation is only available within the function scope of bar.


4. During the second stage of execution how many scopes have been registered by the engine?
  * Which segments of the code do they belong to?
  * Please identify any variables/refs and which scope each belongs to?

There are 3 scopes registered by the engine. The global belongs to the entire index.js file, the function scope of bar nested inside the global scope, and the function scope of baz nested within and belonging to the function scope of bar.

There are variables within each of these scopes as well. The globally scope contains a variable foo and reference to function bar. If not in strict mode, the global scope would also contain the variable bam. The scope of function bar contains a locally scoped variable of foo, and a reference to the function baz. The scope of function baz contains a locally scoped variable foo as well.


5. When line 13 invokes the baz function, which foo will be assigned a value of bam? More specifically, bam will be assigned to the foo in ??? scope. Give a brief description in your own words to support your conclusion.

Bam will be assigned to the value of foo in the scope of the function baz. Baz is invoked within the function bar, but it's specific foo value of 'bam' is only usable in the scope of the baz function.


6. Which scope, if any, will the variable bam on line 11 be registered to when the first stage of execution occurs on this file? Provide a brief description in your own words to support your conclusion.

As previously mentioned, our given environment has 'use strict' on the first line so the variable bam will return an error and will not be assigned anywhere. If we were not operating within strict mode, a variable bam would be created in the global scope. 


7. For each line, 16 through 19, what is the return value for each?

16: undefined
17: the string 'bar'
18: the string 'yay'
19: a ReferenceError, as baz is not defined in global scope and only available in the scope of function bar