Download Link: https://assignmentchef.com/product/solved-cpsc2310-assignment-2-file-pointer-fun
<br>
This assignment is designed to provide practice with the following:

<ul>

 <li><strong>fread</strong></li>

 <li><strong>file I/O</strong></li>

 <li><strong>fseek</strong></li>

 <li><strong>file I/O macros</strong></li>

 <li><strong>general C programming concepts</strong></li>

</ul>

<strong>Overview:</strong>

<strong>Read the entire document to ensure you are aware of all requirements. I will not accept the excuse you did not know you were supposed to do something. </strong>

This is an individual assignment, you are not allowed to receive help from anyone other than, myself, or the 2310 lab TA’s. There are several times in this document I suggest you perform an internet search. This is perfectly fine. Any other information from the internet or any other source must be documented in the form of comments or in your readme file. A violation of this or working with another student could result in a violation of academic integrity charge.




In chapter 1 we discussed the fact that text in our programs are basically represented using ascii values (0-255 which is 1byte)  which is further converted to binary.  We saw a table that showed a hello world program in ascii.







I thought it would be interesting to write a program that will read a file, such as helloWorld.c, and write to output the ascii, and binary representation of the file I will discuss the format of your output a little later in the document.




There are many ways to complete this assignment, but to force you to use several new functions/concepts in “C”, I am going to be very specific on much of the instructions. I am aware that some of what I am going to require you to do could be done easier, and maybe even more efficiently. However, as stated above it is my intention to introduce you to several concepts that some of you may not be familiar with. I strongly suggest you read the entire document and make sure you understand completely what you are required to do. <strong>Points will be deducted for not following directions.</strong>




I know most of you know how to read and write information using scanf/printf or fscanf/fprintf.  For this assignment you are going to use fread to read the appropriate data from a file. You may or may not have already been introduced to the fread function.




The fread function is used to read binary data. This does not mean the data is 1’s and 0’s. It is just an encoding that may not be human readable. It may be an encoding that is outside of the ascii encoding.  There are many text encodings but we will not get into that. The fread function can read an entire chunk of data at one time. There is also an fwrite function that we will not use in this assignment, but it also can write, to a file, a larger chunk of data at one time. For an example of fread and fwrite see the code I gave you in Lab 6.




The syntax for fread is:




<strong>size_t fread(void* ptr, size_t sz, size_t count, FILE* fp);</strong>

Where:

<strong>ptr</strong> is the beginning address to the memory that will hold the data being read from the file

<strong>sz</strong> is the size in bytes , of each element to be read

<strong>count</strong> is the number of elements, each one with a size of sz bytes

<strong>fp</strong> is the file pointer that points to the file being read




The return value of fread is the total number of elements successfully read. If the value returned is different than count either a reading error occurred or the end-of-file was reached while reading.




There are many websites that give information about fread. Feel free to google fread and fwrite examples.




<strong>Files:</strong>

You are required to provide three files:

driver.c – This file is where main will be written.




functions.h – This file will provide the function prototypes. You must also provide a header guard in this file. All of the #include files should be included here. You should include the functions.h file in driver.c, as well as, functions.c.




functions.c  – This is where you will implement the functions.




<strong>Functions:</strong>




Below I will discuss the functions you will implement for this assignment. Failure to implement any of these functions as described will result in a substantial point deduction.




<strong>void checkArg(int);</strong> – This function ensures the number of command line arguments is appropriate. If this fails you must print to <strong>stderr</strong> a message indicating there were not enough command line arguments and <strong>exit</strong> the program. <strong>FYI: stderr prints to the terminal.</strong>




An example of the command line arguments would be: the executable, input file name, output file name. (./a.out input.txt output.txt) I will test your program with various input files.




<strong>void checkArg(int)</strong> – This function determines whether the file opened successfully. If the file does not open successfully you must print to <strong>stderr</strong> a statement that prints the following:

fopen() failed in file (a file name is printed here) at line #(the line number is printed here), of function (the name of the function is printed here). You are probably wondering what is the relevance of printing the file name, line number, and function name. In this case, none actually. This is just one of those times I want you to learn a specific concept.




So, how do you do this. C provides something called macros.

“A macro is a fragment of code which has been given a name. Whenever the name is used, it is replaced by the contents of the macro. There are two kinds of macros. They differ mostly in what they look like when they are used. Object-like macros resemble data objects when used, function-like macros resemble function calls. ” <a href="https://gcc.gnu.org/onlinedocs/cpp/Macros.html">https://gcc.gnu.org/onlinedocs/cpp/Macros.html</a> Macro’s are handled by the pre-processing step of the compilation system. The C programming language has provided macros that will print the current file name, the current line number, and the current function name. You should google something along the lines of “Macros in C”. There are examples that will demonstrate how to do this.




<strong>int getFileCount(FILE*);</strong> – Because you will use fread when reading the information from the file you need to know the number of characters in the file. This is the purpose of this function. Basically, this function will count the number of characters including spaces in the file. To do this you will need to read each character and increment a counter. There is one additional step that I will discuss in a moment.




<strong>void retFP(FILE* fp);</strong> – To explain this function, I need to make sure you understand what happens when you use a file pointer to read the content of a file. Remember, a file pointer is simply a pointer.  When you call fopen this prepares the file pointer and points it to the beginning of the file being opened. As you read each character, the pointer is incremented to point to the next character. Now let’s consider the function getFileCount again. Remember you read each character in the file using the file pointer. Therefore, after you have read each character, where do you think the file pointer is pointing? If you answered at the end of the file you would be correct. Therefore, we need to have a way move the file pointer back to the beginning, and, <strong>no,</strong> the answer is <strong>not</strong> to call fopen again. C provides a function called fseek. This function allows you to move the file pointer to various places in the file. Search for fseek and you should find the information you need to reset the file pointer back to the beginning of the file. In your research you should have discovered that fseek has a return value. Using this value you must check if fseek was successful. If it was not, then you must print a message to <strong>stderr</strong> that says “fseek failed” and exit the program. If the fseek was successful you must print to <strong>stderr</strong> the position the pointer is currently pointing. This can be done using another function called ftell.  Ftell takes one parameter which is the file pointer. Using ftell, print the position of the pointer before calling fseek, then again after you have determined fseek was successful. Now, where do you think the funtionc retFP is going to be called? Hint: At the end of getFileCount. I also want you to call this function in another function, I will discuss that when I get to it.




<strong>void readFile(char*, FILE*, int);</strong> – This function will be used to read all of the characters in the file and store them in an array of characters. You must use fread to read all of the information in the input file. Each variable passed to readFile will be used in the fread function call. The char* is the array that you must store the information when you read the file, the FILE* is the file pointer that is pointing to the beginning of the file to be read, lastly, int is the variable that holds the number of characters in the file that will be read. Again, in your research of fread, you should have noticed fread has a return value. Using this information, you must check if fread was successful. If it was not successful you must print “fread failed” and exit the program.  If fread was successful you must call retFP to return the file pointer to the beginning of the file it is pointing to.




<strong>void ASCII(char*, FILE*, int);</strong> – This function will be called to print, to a file, the ascii value of each character in the file. The char* is the array that holds each character in the file. The FILE pointer is the pointer to the output file, and the int is the count of characters in the file. The format for the output is shown below. Your output format must match the given output’s format.




<strong>void Binary(char*, FILE*, int);</strong> – This function will be called to print, to the output file, the binary representation of each character in the file. The format of the output must match the format shown below. You have already written a function that could be adapted to this program. However, you are <strong>not</strong> <strong>allowed to use % and / in this function. You must use bit wise operators such as, &amp;, |, &gt;&gt;, &lt;&lt;, ~, ^.</strong>




The ascii and binary output must both be printed to the same output file, ascii first then binary, as shown below.




<strong>void print(void (*fp)(char*, FILE*, int), char*, FILE*, int);</strong> – This function will be used to call the ascii and binary functions. Notice the parameter is a function pointer. This is a simple example of a callback function.










<strong>driver.c</strong>

You will implement the driver.c file. Below are the steps you must take in the driver.




<strong>Steps for the driver:</strong>

Check the number of arguments on the command line.

Create and open the input and output file pointers, call the function to check the success of the opening of the file pointers.

Get the count of characters in the file.

Dynamically allocate the memory for the array of characters that will hold the characters of the file.

Read the file, storing each character, including spaces, in the array.

Create the function pointer that will be used to print the ascii and binary representations of the file.

Call the print function once for the ascii then for the binary.

Close the file

Free the dynamically allocated memory.




<strong>Extra Credit (5 points)</strong>

This is going to be an easy 5 points of EC.

In addition to printing the file information in ASCII and BINARY print the file in HEX. You are <strong>not</strong> allowed to use the C provided hex specifier for printf. Hint: You may want to adapt the print hexidecimal function from the Mimir assignment to this one.  However, remember characters are represented by 8 bits or 1 byte. If you choose to do the extra credit you must also make the print function work with the function pointer in this assignment. You should print 9 bytes, with a space between the bytes, per line and each line must be numbered.




<strong>Formatting, Compiling, and Hand-in Requirements:</strong>

Tar your file <strong>PA2.tar.gz</strong> and submit the file through <strong>hand-in to the folder PA2.  </strong>If you are doing the EC you must submit a tarred file called <strong>PA2EC.tar.gz</strong> through<strong> hand-in to the folder PA2EC. <u>PLEASE </u></strong>