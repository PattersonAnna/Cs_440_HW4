Introduction
In the previous assignment, we learned how to store data in external memories using page structure. In this assignment, you'll learn to implement indexing, specifically hash indexing, which provides a desirable trade-off between the time and I/O complexity of search and update/insertion/deletion operations over a file on external storage.

What you must do
Linear Hash Indexing (8 points)
Assume that we have a relation Employee(id, name, bio, manager-id). The values of id and manager-id are integers each with a fixed size of 8 bytes. The values of name and bio are character strings and take at most 200 and 500 bytes, respectively. Note that as opposed to the values of id and manager-id, the sizes of the values of name (bio) are not fixed and are between 1 to 200 (500) bytes. The size of each page is 4096 bytes (4KB). The size of each record is less than the size of a page. You can reuse and adopt your solutions for Assignment 3 to implement the hash index structure described in this assignment. Using the provided skeleton code with this assignment, write a C++ program that creates a linear hash index file for relation Employee using attribute id. Your program must also enable users to search the created index by providing the id of a record.
The Input File: The input relation is stored in a CSV file, i.e., each tuple is in a separate line, and the fields of each record are separated by commas. Your program must assume that the input CSV file is in the current working directory, i.e., the one from which your program is running, and its name is Employee.csv. We have included an input CSV file with this assignment as a sample test case for your program. Your program must correctly create hash indexes and search for id(s) using them for other CSV files with the same fields as the sample file.

Index File Creation: Your program must first read the input Employee relation and build a linear hash index for the relation using attribute id. Your program must store the linear hash index in a file called EmployeeIndex.dat (binary file like the previous assignment) in the current working directory. Your index file must not be a text / csv file. You must use one of the methods explained in our lectures on storage management for storing variable-length records and the method described for storing pages of variable-length records to store records and pages in the new data file. They are also explained in Sections 9.7.2 and 9.6.2 of the Cow Book, respectively. If your submitted program does not use these formats and page data structures to store data in the data file, it does not get any points.

Page Structure: Records on each page must be written from top to bottom. Each page has an overflow page index (overflowPointerIndex in the skeleton code), which points to the address of the overflow page linked to by the page. The overflow page index is written at the end. If the page does not have any overflow page, you must write -1 in the overflow page index. The slot directory is written to the end of the page before the overflow page index in reverse order. This arrangement allows you to follow the i-th record's offset and length in the i-th entry of the slot directory from the bottom of the page.


Hash function: You must use the hash function h = id mod 212  . Hence, m (the maximum possible number of bits considered for addressing a page) is 12.
 

Insertion to the index: Your program must follow the algorithm of insertion in a linear hash index discussed in the class and textbook to insert records in the index. But, you must modify the algorithm so it does not need a bucket array in the main memory for the insertion and searching of records. This is done by separating the part of the index that contains the main index pages (pages in the first level of indexing) and the portion that stores overflow pages. Your program must reserve the first 5,000 pages in the index file for the main index pages. It must use the remaining space in the index file for overflow pages. This approach enables your program to find pages on the index without keeping a bucket array in the main memory. Other methods of allocating space for overflow pages that do not maintain a bucket array in the main memory are also acceptable. Your program must grow the index (increment the value of n) if the average number of records per page exceeds 70% of the page's capacity.
 

 Searching the Data File: After creating the hash-indexed file, your program must accept an Employee id in its command line and search the file for all records with the given id. Your program must follow the algorithm for the search of a linear hash index discussed in the class and textbook. But,  as explained before, your program must not use any bucket array to search the index. After finding a page in the index, your program must use the slot directory information to navigate to each record inside the page and find the record with the desired id. 
Approaches that do not utilize linear hash indexing and page data structure but rather read pages in the index sequentially will not get any points.

The user of your program should be able to search for records of multiple ids, one id at a time.

 Main Memory Limitation: During the file creation and search, your program must keep up to one page in main memory at any time. The submitted solutions that use more main memory will not get any points. The local variables you use in your programs are excluded from this restriction.
Each student has an account on hadoop-master.engr.oregonstate.edu server, which is a Linux machine. You should ensure your program can be compiled and run on this machine. You can use the following bash command to connect to it:

  > ssh your_onid_username@hadoop-master.engr.oregonstate.edu

 Then it asks for your ONID password and probably one another question. To access this server, you must be on campus or connect to the Oregon State VPN.

You can use the following commands to compile and run C++ code:

  > g++ -std=c++11 main.cpp -o main.out
  > main.out id1 id2

Necessary files
Input file: Employee.csvDownload Employee.csv

Following files contain the skeleton code to generate the required EmployeeIndex.dat file. You may make changes to these files adhering to the assignment requirements.

main.cppDownload main.cpp

classes.hDownload classes.h

What to turn in
The assignment is to be turned in before Midnight (by 11:59pm) on February 11. You may turn in the source code of your program through Canvas. Each group may submit only one file that contains the full name, OSU email, and ONID of every member of the group.

Grading criteria
The programs that implement the right algorithm, return the correct answers, and do not use more than the allowed buffers will get the perfect score. The ones that use more buffer than allowed will not get any points.  The ones that implement the right algorithm but return partially correct answers will get partial scores.
