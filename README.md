The directoyr contains C code that are intentinally vulnerable.
This is to test if they can be deteced with static code analyzer.


Result on different tools:

The result is rather disappointing. At the time of writing, Codeql caught about 8/16 of the mistakes, Snyk caught 6/16, and Semgrep caught 2/16.


My observation:

• For very simple things they have about 50% chance of catching them, this is like use-after-free, using "gets" function, etc.

• The fact they both caught possible SQL injection and use of "system()" function based on user input is the only pleasant surprise I found in this test.

• On contrary, there is 50% chance they would miss very obvious things, such as int x = INT_MAX+1

• When things gets even slightly complicated, they are almost hopeless. For example, in memory_leak3.c file, I malloced an array. I also made a conditional branch in the main program, and only frees the array on one of the branch. In memory_leak2.c , I malloced an outer array, and each element in the outer array contains a struct of pointer, pointing to another inner array on heap. I only free the outer array at exit. None of the analyzers caught either memory leaks.