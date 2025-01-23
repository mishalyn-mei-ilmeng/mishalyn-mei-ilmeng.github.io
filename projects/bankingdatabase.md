---
layout: project
type: project
image: img/bank-counter-crop.jpg
title: "Banker's Dashboard"
date: 2024
published: true
labels:
  - Unix
  - C++
summary: "A banking system I created for ICS 212."
---

<img class="rounded mx-auto d-block" src="../img/bank-counter.jpg">

Banker's Dashboard is a banking system consisting of a user interface and a database. The purpose of the project was to create a banking system in which a "banker" would be able to manage account records for customers. It allowed the user to create, print, find, and delete records. In addition, upon quitting the program would write the data to a file. The saved file can then be used to read and recover the database to avoid resubmitting previous data. 

For this project, I was responsible for the implementation and design of the database and the user interface. It uses a linked list data structure in which accounts are stored by increasing account number. I started first with the user interface and then implemented the methods that allowed the user to interact with the database. In addition to running normally, I also implemented the option to run the program with debugging statements assist me in developing the program by using compiling directives and makefiles. The debug statements showed which the method called and with what parameters. The debug implementation also helped with testing, which I was also responsible for. 

Overall, this project improved my understanding of C++ and how to design a program from scratch. This experience taught me a lot about I/O in C++ and highlighted the differences of I/O in C and C++. It also taught me a lot about dynamic memory and reinforced my understanding about linked lists as a data structure. However, my favorite skill that I learned from this project was the use of makefiles and compiling directives because they made compiling more efficient and simple.

###The user's menu has 5 options for the user to choose from, allowing them to edit the database. It looks like this: 

<hr>

<pre>
-=+#################################################+=-
Welcome to Banker's Dashboard! 

Please type option name to select menu option.
add: Create a new customer record
printall: Print all records
find: Find a customer record
delete: Delete a customer record
quit: Quit program
</pre>



###I also included my implementation of adding a record to the database.
<hr>

<pre>
int llist::addRecord(int accNum, char name[], char address[])
{   
    record *previous;
    record *current;
    int outcome;
    record *temp;
    
    previous = NULL;
    current = start;
    outcome = 1;
    
    /* Debug Flag! */
    int debugmode = 0;
   
    #ifdef DEBUG
        debugmode = 1;
    #endif
    
    if (debugmode == 1)
    {   
        cout << "\nDebug: addRecord was called" << endl; 
        cout << "Debug: addRecord Parameters: accNum = " << accNum << ", name = " << name << ", address = " << address << "\n\n";
    }
    
    /* Find the spot to insert record */ 
    while (current != NULL && outcome == 1)
    {   
        if (accNum == current->accountno)
        {   
            outcome = -1; /* Found a duplicate record */
        }
        else if (accNum < current->accountno)
        {   
            outcome = 0; /* Found correct position */
        }
        else /* Iterate through list until found or at end of list */
        {   
            previous = current;
            current = current->next;
        }
    }
    
    if (start == NULL || (outcome == 1))
    {   
        outcome = 0;
    }
    
    /* If there is somewhere to insert, insert into spot */
    if (outcome == 0)
    {   
        //temp = (struct record *) malloc(sizeof(struct record));
        temp = new record;
        temp->accountno = accNum; 
        strncpy(temp->name, name, sizeof(temp->name) - 1);
        strncpy(temp->address, address, sizeof(temp->address) - 1);
        
        if (previous == NULL) /* Record belongs before start  */
        {   
            temp->next = start;
            start = temp;
        }
        
        else /* Belongs elsewhere */
        {   
            temp->next = current;
            previous->next = temp;
        }
    }
    return outcome;
}
</pre>
