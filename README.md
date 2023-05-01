Download Link: https://assignmentchef.com/product/solved-cis-505-software-systems-project-1
<br>






<ol>

 <li><strong>Introduction </strong></li>

</ol>

In this exercise, you will conduct some simple performance experiments and explore the POSIX standard user level thread library. To submit your work, put your source and text files in a directory and use the turnin command on the speclab cluster. All programs should be written in C or C++ and compiled on the speclab machines especially the third part of the project. Follow the submission guidelines for turnin given at the end of this document.




<strong><em>Important</em></strong><em>: </em>Take the time to write and code your answers clearly and lucidly, whether the language you are using is English or C.

<strong><em> </em></strong>

<strong><em>VERY IMPORTANT</em></strong><em>: </em>Submit <em>only </em>your source code and supporting text. Do <em>not </em>include compiled or large output files (e.g, write.out or fprint.out) in your submission. Those files can be very large and submitting them can have a disruptive effect on our shared computing resources.







<ol start="2">

 <li><strong>Part 1 (15% credit):</strong></li>

</ol>




One of the features of the Unix “standard I/O” library is the use of input and output buffers” that aim to reduce the number of times the (expensive) read() and write() system calls are invoked. Functions like fprintf() and fputc() don’t call write() each time they’re used. Instead, they add their output to a buffer and call write() only when the buffer becomes full. (Write() also gets called when the file is closed and when the program explicitly requests that buffers be flushed). By avoiding excessive use of system calls, programs that use the standard I/O library often have much better performance than they would had they used read() and write() directly. For example, consider the following two programs: The first uses the system call interface to write 50,000 characters (one at a time):




/* syscall_writer.c ‐ write 50,000 characters with write*/

#include &lt;stdio.h&gt;

#include &lt;sys/types.h&gt;

#include &lt;sys/stat.h&gt;

#include &lt;fcntl.h&gt;

#define OUTPUTNAME “write.out”

main()

{ long i; int fd;

if ((fd=open(OUTPUTNAME,O_WRONLY|O_CREAT,0644)) &lt; 0){ fprintf(stderr,”Can’t open %s. Bye.
”,OUTPUTNAME); exit(1);

}

for (i=0; i&lt;50000; i++) { /* write 50,000 Ys with write */ if (write(fd,”Y”,1) &lt; 1) { fprintf(stderr,”Can’t write. Bye
”); exit(1);

}

} close(fd); exit(0);

}




The second uses the standard I/O library to write eight times as many characters.




/* stdlibrary_writer.c ‐ write 400,000 characters with fprintf */

#include &lt;stdio.h&gt;

#define OUTPUTNAME “fprint.out”

main()

{ long i; FILE *fp;

if ((fp=fopen(OUTPUTNAME,”w”)) == NULL) { fprintf(stderr,”Can’t open %s. Bye.
”,OUTPUTNAME); exit(1);

}

for (i=0; i&lt;400000; i++) { /* write 400,000 Xs with fprintf */ if (fprintf(fp,”X”) &lt; 1) { fprintf(stderr,”Can’t write. Bye
”); exit(1);

}

} fclose(fp); exit(0);

}




On most systems the second program runs faster than the first, even though it produces eight times more output. Using the /usr/bin/time command on the speclab cluster, compare the performance of these two programs.  Construct an experiment in which you time each program over ten (or more) consecutive invocations.




Give the output data from the time command experiments and write a brief report summarizing your results; be sure to explain the meanings of each of the figures measured by the time command.

<ul>

 <li>Are there variations in the output of the time command across successive invocations of the same program? Why or why not?</li>

 <li>Do your results suggest that the buffering strategy of the standard I/O library is successful? Explain.</li>

</ul>




You do not need to submit the source code for this part.







<ol start="3">

 <li><strong>Part 2 (30% credit):</strong></li>

</ol>




Many versions of Unix, including that running on the speclab cluster, include a lightweight “threads” library package that allows multiple threads to be part of a single process. See the manual pages for pthreads on Eniac for details. Rewrite the two programs in the previous section as simultaneous threads in a single process using this library. Your multithreaded process should start the two threads in parallel and report which one finishes first.




Submit your source code and a brief discussion of its operation, especially addressing whether one would expect to get the same result each time the program is run. Does it matter which thread your program starts first? Does your experience running your program support your expectation?




<strong><em>Note</em></strong><em>: </em>To complete this exercise, you will need to become familiar with the pthreads package, which we did not discuss extensively in class. Part of the purpose of this exercise is to give you experience interpreting and evaluating the interfaces to unfamiliar system services.













<ol start="4">

 <li><strong>Part 3 (30% credit):</strong></li>

</ol>

In this section, rewrite the program in the previous section as simultaneous processes instead of threads. Use the fork() and execv() instructions to perform the two different tasks in the different processes instead of threads. Like in section 2, your multi-process application should report which process finishes first.




Submit your source code and a brief discussion of its operation, especially addressing whether one would expect to get the same result each time the program is run. Does it matter which process your program starts first? Does your experience running your program support your expectation?




<strong><em>Important Note: </em></strong>Be very careful when using fork() to start a new application. If used incorrectly, your program can fork infinite processes and cause the system to crash. It is very important that you use the speclab cluster for this section particularly. Students found by CETS using Eniac will receive a 10% penalty.




<ol start="5">

 <li><strong>Part 4 (25% credit)</strong></li>

</ol>




At some point this semester, you may find that C/C++ as very tricky languages that can be hard to debug. If you have used C/C++ before, this is probably not new.

Memory management, for example, needs to be carefully taken care of.

Reading/writing past the end of an array, using uninitialized variables and memory leaks are common errors that can lead to crashes or unpredictable behaviors. To make your program more robust and ease the pain of debugging, we suggest you to use Valgrind and GDB, both are powerful tools for programmers.




To expose you to these tools, you are asked to debug some C++ programs using GDB, and run Valgrind to make sure that it is free of certain errors.  You have been provided two cpp files: part4_gdb.cpp and part4_valgrind.cpp.




<ul>

 <li><strong><u>cpp</u></strong></li>

</ul>




This code has some bugs in it. Although you can easily spot the bugs, we want you to run GDB on it to uncover them. The goal of this exercise is to give you an idea about how GDB can be useful in debugging code. This tool will come in handy in future projects and you will certainly be using it in your final project.




Speclab has GDB installed on it. Run GDB on the compiled code. Try to find out where the error(s) happens.<strong> Take a screenshot of the GDB backtrace that suggests the location of the bug(s). You need to attach these screenshots to your README file, or check them in as image files in your submission directory (and tell us where to find them in your README). </strong>




<strong>After spotting the bug(s), you need to fix the code. In the Excel sheet provided, mention the line number where you found the bug, what is the bug and how did you fix it. If you do not use Excel, you can manually draw a similar table and submit the PDF version. </strong>

<strong> </strong>

<ul>

 <li><strong><u>cpp</u></strong></li>

</ul>

You can compile this code and run it and guess what? It will run smoothly without throwing any errors. But something sinister is happening in this code! Can you find out what is it? Hint: Use Valgrind!




Speclab has Valgrind installed on it. Run Valgrind on the compiled code. Try to find out what is wrong with this code! <strong>Take a screenshot of the Valgrind  Summary. You need to attach this screenshot to your README file, or submit the image files as described above.</strong>




<strong>After spotting the bug(s), you need to fix the code. In the Excel sheet provided, describe the bug and how did you fix it.</strong>




<strong><em>Important Note</em></strong><strong>: </strong><strong><em>You need to fix all the bugs in both the codes AND file your answers in the Excel sheet provided to get full credit in this section</em></strong>





