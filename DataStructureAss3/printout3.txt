// DatastructureAss2.cpp : Defines the entry point for the console application.
//*********************************** Program Identification ***********************************
//*                                                                                            *
//*   Program File Name : program1.cpp    Assignment #  : 3               Grade______          *
//*                                                                                            *
//*   Program Author :            ______________________________                               *
//*                                       Brandon Chalker                                      *
//*                                                                                            *
//*   Course #: CSC 40300 11                                     Due Date : January  30, 2019  *
//*                                                                                            *
//**********************************************************************************************
//***********************************  PROGRAM DESCRIPTION  ************************************
//*                                                                                            *
//*   PROCESS: This program is designed to take in input file commands. If the command         *
//*   is 'A' then the program will add a node to the linked list. The node contains a first    *
//*   name, last name address, city, state, and zip code. If the command is 'D', a node is     *
//*   delete with the sumbitted first and last name. If the command is 'C', then the node      * 
//*   with the specific first and last name will have a specific field edited, based off the   *
//*   field value that is submitted. Finally, if the command is 'P', the list will be printed. *
//*   When the command is equal to Q, file will close and the program will end                 *
//*                                                                                            *
//**********************************************************************************************

#include "stdafx.h"
#include <fstream>
#include <ostream>
#include <iostream>
#include <string>
#include <iomanip>

using namespace std;

//********************************STRUCT NODE************************************
struct node 
{
	int arrivalTime; //creates a node for the arrivaltime
	int processingTime; //creates a node for the processingtime 
	char namearray[21]; //creates an array for the name
	node* next;

};

//********************************CLASS NAMELIST************************************
class nameList
{
private:
	listNode * head; 
	listNode * tail;

public:
	nameList()
	{
		/*
		receives: nothing
		process : makes head and tail null
		returns : nothing
		*/
		head = NULL;
		tail = NULL;
	}

	//********************************NAMELIST FUNCTION ADDNODE************************************
	void addNode(string inputn)
	{
		/*
		receives: string
		process : makes a node with the name 
		returns : nothing
		*/
		listNode* nextNode = new listNode;
		for (int i = 0; i < 21; i++) //reads runs till i is 20
		{
			if (i < inputn.length()) //runs whilstever i is less than input length
			{
				nextNode->namearray[i] = inputn[i]; //saves input into name array
			}
			else
			{
				nextNode->namearray[i] = '\0';
			}
		}
		nextNode->next = NULL; //reads in next node

		if (head == NULL)
		{
			head = nextNode;
			tail = nextNode;
		}
		else
		{
			tail->next = nextNode;
			tail = tail->next;
		}
	}

	//********************************NAMELIST FUNCTION GETHEAD************************************
	listNode* getHead()
	{
		/*
		receives: nothing
		process : nothing
		returns : returns head
		*/
		return head;
	}
};

//********************************STRUCT LISTNODE************************************
struct listNode 
{
	char namearray[21]; //array to store name
	listNode* next; //list node for next
};

//********************************CLASS CHECKOUTLINE************************************
class checkOutLine 
{
private:
	int num; // integer labeled num
	node* head; // node for head
	node* tail; // node for tail

public:
	checkOutLine(int x) 
	{
		/*
		receives: integer imput
		process : stores input into num and heads and tails to null
		returns : nothing
		*/
		num  = x; // set input to num
		head = NULL; // sets head to null
		tail = NULL; // sets tail to null
	}

	//********************************CHECKOUTLINE FUNCTION ADDNODE************************************

	void addNode(string nvar, int arrivalint, int processingint) 
	{
		/*
		receives: name arrival integer, processing integer
		process: stores the values in appropriate areas 
		returns: nothing
		*/

		node* nextNode = new node;
		for (int i = 0; i < 21; i++) 
		{
			if (i < nvar.length())  // runs till i is less than nvar
			{
				nextNode->namearray[i] = nvar[i]; // stores the name into array
			}
			else {
				nextNode->namearray[i] = '\0';
			}
		}
		nextNode->arrivalTime = arrivalint; // stores arrival time
		nextNode->processingTime = processingint; //stores processing time
		nextNode->next = NULL; // makes next null

		if (head == NULL)
		{
			head = nextNode; // stores node to head
			tail = nextNode; // stores node to tail 
		}
		else 
		{
			tail->next = nextNode; //stores tail
			tail = tail->next; 
		}
	}


	//********************************CHECKOUTLINE FUNCTION DELETENODE************************************
	string deleteNode() 
	{
		/*
		receives: nothing
		process : deletes front of the queue
		returns : name
		*/
		string name; // variable for name
		if (head == NULL)
		{
			return "";
		}
		else 
		{
			name = string(head->namearray); // stores name
			head = head->next; //stores head
			return name; // returns name
		}
	}


	//********************************CHECKOUTLINE FUNCTION GETHEAD************************************
	node* getHead()
	{
		/*
		receives: nothing
		process : nothing
		returns : head
		*/
		return head; // returns head
	}


	//********************************CHECKOUTLINE FUNCTION CHECKNODETIME************************************
	int checkNodeTime(int currTime) 
	{
		/*receives: time
		//process : sets the integer time to the arrival time of the head node, minus the current time, plus the processing time of the head node 
		returns: the integer time
		*/

		int time = -1; // takes one of the time
		if (head != NULL) 
		{
			time = head->arrivalTime - currTime + head->processingTime; // stores the time
			return time; // returns the time
		}
		return time;
	}

	//********************************CHECKOUTLINE FUNCTION GETTIME************************************
	int getTime() 
	{

		/*
		receives: nothing
		process : checks total processing time
		returns : processing time
		*/


		int time = 0; // stores 0 in time
		node* varhead = head; // set traverse to head
		while (varhead != NULL) // runs whilst traver
		{
			time = time + varhead->processingTime; // sets time
			varhead = varhead->next; //reads in next
		}
		return time; // returns time
	}
};



int main()
{

	//initialize variables used in main
	nameList inList = nameList(); 
	nameList printList = nameList();
	string name; // name string
	string printName; //name to be printed
	int processingTime = 0; // processing time
	int currTime = -99; // currtime
	ifstream inFile;
	checkOutLine checkOut1 = checkOutLine(1);
	checkOutLine checkOut2 = checkOutLine(2);
	checkOutLine checkOut3 = checkOutLine(3);
	ofstream outFile;

	//initialize the file streams
	inFile.open("data3.txt");
	outFile.open("results.txt");

	//reads in currtime
	inFile >> currTime;

	
	while (currTime != -99) //runs till currtime is -99
	{
		//reads in name, processing time 
		getline(inFile, name);
		getline(inFile, name);
		inFile >> processingTime;

		//compares lines
		if (checkOut1.getTime() <= checkOut2.getTime() && checkOut1.getTime()
			<= checkOut3.getTime()) 
		{
			checkOut1.addNode(name, currTime, processingTime);
		}
		else if (checkOut2.getTime() < checkOut1.getTime() && checkOut2.getTime()
			<= checkOut3.getTime()) 
		{
			checkOut2.addNode(name, currTime, processingTime);
		}
		else 
		{
			checkOut3.addNode(name, currTime, processingTime);
		}
		inList.addNode(name);
		inFile >> currTime;
	}
	for (int i = 0; i < 200; i++) //runs till i is 200
	{
		if (checkOut1.getHead() != NULL && checkOut1.checkNodeTime(i) <= 0 && checkOut1.checkNodeTime(i)
			<= checkOut2.checkNodeTime(i) && checkOut1.checkNodeTime(i) <= checkOut3.checkNodeTime(i)) 
			//runs test for checkout1
		{
			printName = checkOut1.deleteNode(); //saves name to be printed 
			printList.addNode(printName); // stores the name into a list to be printed
			if (checkOut2.getHead() != NULL && checkOut2.checkNodeTime(i) 
				<= 0 && checkOut2.checkNodeTime(i) <= checkOut3.checkNodeTime(i))
				//runs test for checkout2
			{
				printName = checkOut2.deleteNode(); //saves name to be printed 
				printList.addNode(printName); // stores bane to be printed
				if (checkOut3.checkNodeTime(i) <= 0 && checkOut3.getHead() != NULL) 
					//runs test for checkout3
				{
					printName = checkOut3.deleteNode(); //saves name to be printed
					printList.addNode(printName); //stores name to be printed
				}
			}
			else if (checkOut3.getHead() != NULL && checkOut3.checkNodeTime(i) 
				<= 0 && checkOut3.checkNodeTime(i) <= checkOut2.checkNodeTime(i))
				//runs test checkout 3
			{
				printName = checkOut3.deleteNode(); //saves name to be printed
				printList.addNode(printName); //stores name to be printed
				if (checkOut2.checkNodeTime(i) <= 0 && checkOut2.getHead() != NULL) 
				{
					printName = checkOut2.deleteNode(); // saves name to be printed
					printList.addNode(printName); // stores name to be printed 
				}
			}
		}
		else if (checkOut2.getHead() != NULL && checkOut2.checkNodeTime(i)
			<= 0 && checkOut2.checkNodeTime(i) < checkOut1.checkNodeTime(i) 
			&& checkOut2.checkNodeTime(i) <= checkOut3.checkNodeTime(i)) 
		{
			printName = checkOut2.deleteNode();// saves name to be printed
			printList.addNode(printName); // stores name to be printed 
			if (checkOut1.getHead() != NULL && checkOut1.checkNodeTime(i)
				<= 0 && checkOut1.checkNodeTime(i) <= checkOut3.checkNodeTime(i)) 
			{
				printName = checkOut1.deleteNode();// saves name to be printed
				printList.addNode(printName); // stores name to be printed 
				if (checkOut3.checkNodeTime(i) <= 0 && checkOut3.getHead() != NULL) 
				{
					printName = checkOut3.deleteNode();// saves name to be printed
					printList.addNode(printName);// stores name to be printed 
				}
			}
			else if (checkOut3.getHead() != NULL && checkOut3.checkNodeTime(i)
				<= 0 && checkOut3.checkNodeTime(i) <= checkOut1.checkNodeTime(i)) 
			{
				printName = checkOut3.deleteNode();// saves name to be printed
				printList.addNode(printName); // stores name to be printed 
				if (checkOut1.checkNodeTime(i) <= 0 && checkOut1.getHead() != NULL) 
				{
					printName = checkOut1.deleteNode();
					printList.addNode(printName);
				}
			}
		}
		else if (checkOut3.getHead() != NULL && checkOut3.checkNodeTime(i) 
			<= 0 && checkOut3.checkNodeTime(i) < checkOut2.checkNodeTime(i) 
			&& checkOut3.checkNodeTime(i) < checkOut1.checkNodeTime(i))
		{
			printName = checkOut3.deleteNode();// saves name to be printed
			printList.addNode(printName); // stores name to be printed 
			if (checkOut1.getHead() != NULL && checkOut1.checkNodeTime(i) 
				<= 0 && checkOut1.checkNodeTime(i) <= checkOut2.checkNodeTime(i)) 
			{
				printName = checkOut1.deleteNode(); // saves name to be printed
				printList.addNode(printName);  // stores name to be printed 
				if (checkOut2.checkNodeTime(i) <= 0 && checkOut2.getHead() != NULL) 
				{
					printName = checkOut2.deleteNode(); // saves name to be printed
					printList.addNode(printName); // stores name to be printed 
				}
			}
			else if (checkOut2.getHead() != NULL && checkOut2.checkNodeTime(i)
				<= 0 && checkOut2.checkNodeTime(i) <= checkOut1.checkNodeTime(i))
			{
				printName = checkOut2.deleteNode();// saves name to be printed
				printList.addNode(printName);  // stores name to be printed 
				if (checkOut1.checkNodeTime(i) <= 0 && checkOut1.getHead() != NULL) 
				{
					printName = checkOut1.deleteNode();// saves name to be printed
					printList.addNode(printName); // stores name to be printed 
				}
			}
		}
	}

	//prints out the names in order of them coming in, and in order of them leaving, side by side
	listNode* varhead = printList.getHead();
	listNode* varhead2 = inList.getHead();
	outFile << "The order of customer arrival is:         The order of customer departure is: \n";
	outFile << "----------------------------------    |   ------------------------------------\n";
	while (varhead != NULL) // runs till varhead is null
	{
		outFile << setw(20) << left << string(varhead2->namearray) << right 
			<< setw(22) << "|   " << left << setw(20) << string(varhead->namearray) << endl;
		//prints out string
		varhead = varhead->next;
		varhead2 = varhead2->next;
	}
	return 0;
}
