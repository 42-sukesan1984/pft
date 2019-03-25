# PFT
*aka Printftester2000*

This is a unit test library and tester for ft\_printf. 

# Installation

In the root of your repo, run this command:

```
git clone https://github.com/gavinfielder/pft.git testing && echo "testing/" >> .gitignore
```

# Usage, the short version

Inside testing/:  
`make`  
`./test`  

# Usage, the long version

There are four options:
 - `./test prefix` runs all the enabled tests whose name starts with 'prefix'
 - `./test 42 84` runs (enabled) test number 42 through test 84
 - `./test 42` runs enabled tests from 42 to the end of all the enabled tests
 - `./test` runs all the enabled tests

Here are some common prefixes that might be useful to run: str, sint, uint, hexl, hexu, octl, ptr, char, flt, mix, nocrash, moul.

```
If the prefix stuff doesn't make sense, look at unit\_tests.c and then `./test nospec`. You should easily pass 3 tests, and have an idea of how to use this program. 
```

unit\_tests.c shows you all the tests that are available. Failing a test means that your output and/or return value was not the same as the libc printf. When this happens, there will be a new file, 'test\_results.txt', that holds information about the failed test, what the expected output was, and what your output was. You can then find the test in unit\_tests.c to see what code was run that produced an error.  

```
vim trick: In the command line, type `/test-name` and hit enter. This takes you to a particular test.
```

You can add your own tests to this unit\_tests.c, following the same format. You do not need to do anything except write the function in this file and remake.   

## Enabling and Disabling tests

I have provided scripts that make it easy to enable and disable tests by prefix. Example:

```bash
./disable-test str                      # All the tests that start with 'str' are disabled
./enable-test str_space_                # All the tests that start with 'str_space_' are enabled
./disable-test "" && ./enable-test str  # Disables all tests except tests that start with 'str'
```

```
While you **can** call `./enable-test ""` to enable all tests, I do not recommend it. Some tests are disabled by default because if you have not implemented certain bonuses, your ft\_printf will segfault.   
```

# How it works, for those who want knowledge and power (or maybe just want to use it to do something specific)

When you run make, the first thing that happens is the test index is created. Two copies of unit\_tests.c are created. In the copy unit\_tests\_indexed.c, the test() function is replaced with ft\_printf(). In the copy unit\_tests\_benched.c, the test() function is replaced with printf() and '\_bench' is added to all the function names. Next, in both files, an array of function pointers is created at the end of the file pointing to all the enabled unit tests.   An array will also be created holding the names of all the functions as string literals.  

When you call `./test str_`, main.c will see alpha input and call run\_search\_tests, which does strncmp on each position in the array of function names, and when it finds a function name starting with 'str\_', it calls run\_test() on that test.  

run\_test() runs a particular test. The way the test works is that it redirects stdout to a file, calls the ft\_printf version, and does the same with the printf version. It compares the return value and the content of the files, and if either is different, the test is failed. This is very essentially the same way moulinette tests printf. The diff is logged to file and a red FAIL is printed instead of a pretty green PASS.  

# Troubleshooting, and Contributing

If something goes wrong--slack me. gfielder. I'm serious--I want people to use this, so you telling me about difficulties you have with it can help me make it easier to use.

Same goes with contributing. Feel free to fork the repo and make pull requests. I'm interested in devops, so I'm motivated to get every little experience with managing collaborative repos. 

A big thing that needs to be done is simply just changing test names so that they're grouped better for using the prefix search. I wrote these tests as I was developing my printf and searching by prefix was an afterthought to all of it, so some of them are a little scattered.  

Adding additional tests would be great as well.  

Before making pull requests, please:

```bash
./enable-test "" && ./disable-test argnum && ./disable-test moul_star && ./disable-test moul_a \
&& ./disable-test moul_g && ./disable-test moul_e && ./disable-test moul_wide \
&& ./disable-test moul_F && ./disable-test moul_D && ./disable-test nocrash
```

## Credits

All code, and all tests (so far) were written by me.