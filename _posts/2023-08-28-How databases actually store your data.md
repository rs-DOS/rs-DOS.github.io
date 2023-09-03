As a part of my effort to learn more about computers, I have started learning about databases. For a former analyst like me SQL should have been my bread and butter, alas companies still prefer Excel. In current organization, our database system is a little different and to understand that system better, I have decided that I must be able to compare it with industry standard systems to glean their differences. <br>

CMU has a great course for [introduction to databases](https://youtu.be/uikbtpVZS2s?si=ZvRwPnoFAigeuV_r) and the [Database System Concepts](https://www.db-book.com/) book by Silberschatz, Korth and Sudarshan is a very beginner friendly guide. <br>
**TO BE UPDATED**
### Starting with relations
Databases in the current form, that we all know and love so much, are based on mathematical theory known as relational algebra. It is hard for us to imagine a different way to store data other than the relational model. It is like an idea so simple that it's hard to imagine that we didn't come up with it sooner. <br>
The origins of the relational databases lie in a [paper by Edgar F. Codd](https://cs.uwaterloo.ca/~david/cs848s14/codd-relational.pdf). 

### How DBMS stores data 
In the course they are primarily talking about disk-based databases where the data is stored on magnetic disks and pulled into memory whenever a computation needs to be performed. The third lecture is primarily about how a DBMS represents the database in files on disk. This is persistent storage. <br>
As we saw in the blogpost about performance engineering, how the data is actually represented and stored on the disk has an impact on the performance of the database. Usually, the amount data that a database has to organize and store is significantly higher than what the main memory can store. A CPU can only access data that is in main memory and as the DB size is bigger than main memory, the data must be fetched from non-volatile storage like magnetic disks and SSDs into the main memory and updated back to the persistent storage if there is any change in it. <br>

The disk will have a database file which can be further broken down into pages or blocks. In the memory we will have a buffer manager which manages the pages being brought in from the disk.
<br>
One interesting thing that I didn't understand is that the buffer pool will still only have a pointer to the page that is being called by the execution engine. I thought that the buffer pool would hold the entire page in memory to make the modifications to it but I guess that's not how it works. There's a section in the lecture where the professor discuses why you don't want to use the operating system to memory map the DBMS that went right over my head. 
