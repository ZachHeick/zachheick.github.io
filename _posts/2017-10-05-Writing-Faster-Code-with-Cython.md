---  
layout: post  
title: Writing Faster Code with Cython  
---  

Choosing a programming language when wanting to learn how to code can be overwhelming. There are [hundreds](https://en.wikipedia.org/wiki/List_of_programming_languages) of different languages out there, how do you know which one is best start out with?  

![Programming Languages](https://zachheick.github.io/images/programming_languages.jpg){: .center-image }  

Like tools in a toolbox, certain languages are better suited for certain jobs. Many people (myself included) start with Python. Its straight forward syntax is easy to learn and read, and Python also encourages good programming habits through its focus on white space and indentation. The list goes on about what Python is great at, but like any programming language, it has trade-offs. Specifically, Python is very slow when it comes to large calculations whereas languages like C and C++ are much faster. Cython helps Python overcome this problem. 

**Cython** is a static compiler and superset of the Python language that makes Python code able to be compiled using C/C++ compilers.  

I'll give a quick example on how to use Cython and explain what's going on under the hood.  

## The Rectangle Method  

[The Rectangle Method](http://www.mathcs.emory.edu/~cheung/Courses/170/Syllabus/07/rectangle-method.html) is the simplest method to calculate the approximate area under a definite integral. Calculating the sum of areas of more rectangles leads to a more accurate approximation. Consider the pure Python code below.

#### Pure Python  

```python
%%time

def f(x):
    """
    A mathematical function.
    :param x: input
    :return: function of x
    """
    return x**2-x

def integrate_f(a, b, N):
    """
    Calculates approximate definite integral using the
    rectangle method.
    :param a: Starting point
    :param b: Ending point
    :param N: Number of rectangles
    :return: Approximate area under the curve
    """
    s = 0
    dx = (b-a)/N
    for i in range(N):
        s += f(a+i*dx)
    return s * dx

integrate_f(0, 100, 50000000)
>>> CPU times: user 20.2 s, sys: 197 ms, total: 20.4 s
>>> Wall time: 20.7 s
```  

Running this code in pure Python takes a very long time. Because Python is not statically typed, the code needs to be interpreted before being turned into instructions that the computer can understand. CPython, the default interpreter from Python.org, does exactly this. CPython turns the code that we wrote into bytecode that is saved in a .pyc file. Once our source code is converted to bytecode, a virtual machine reads each instruction in the bytecode and executes whatever operation is indicated. A *virtual* *machine* is software that interprets bytecode.  

#### Pure Python with Cython  

We load Cython by calling `%load_ext cython`. To compile with Cython, all we have to include is `%%cython` in our code.  

```python
%load_ext cython 

%%time
%%cython

integrate_f(0, 100, 50000000)
>>> CPU times: user 12.8 s, sys: 87.8 ms, total: 12.9 s
>>> Wall time: 13 s
```

Simply compiling the same code from above in Cython gives a good speedup. Cython works by producing a standard Python module. However, the behavior differs from standard Python in that the module code, originally written in Python, is translated into C. While the resulting code is fast, it makes many calls into the CPython interpreter and CPython standard libraries to perform actual work. Adding type declarations can reduce the number of these calls, resulting in a huge difference in speed.  

#### Python with Type Declarations  

```python
%%time
%%cython

def f(double x):
    return x**2-x

def integrate_f(double a, double b, int N):
    cdef int i
    cdef double s, dx
    s = 0
    dx = (b-a)/N
    for i in range(N):
        s += f(a+i*dx)
    return s * dx

f = integrate_f(0, 100, 50000000)
>>> CPU times: user 3.66 s, sys: 46.2 ms, total: 3.7 s
>>> Wall time: 3.73 s
``` 

By adding static types to our variables, the calculations will be compiled to pure C code, resulting in a greater speedup than before. Why is this the case? Static typing allows code to be eventually compiled to machine code. The C code in our example is preprocessed by a *preprocessor* into another modified file. Tasks like removing comments and joining continued lines are done by the preprocessor. Then, this preprocessed code is compiled into assembly by the *compiler*. The assembly language is a low level programming language that corresponds to the machine's architecture. The *assembler* then translates the assembly instructions into machine code, which is finally executed by the machine's processor.   

## Conclusion  

Interpreted code that software runs is much slower than the machine's hardware executing code. Cython helps a slow interpreted language like Python overcome its tradeoffs by combining it with the power of C. Cython supports calling C functions and declaring C types on variables and class attributes, resulting in faster code. More info on Cython and the documentaion can be found [here](http://cython.org/).

 

