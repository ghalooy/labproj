//header Clock.hpp
#ifndef Clock_hpp
#define Clock_hpp

#include <stdio.h>
#pragma once

#include <iostream>
#include <string>
using namespace std;


struct node {
    
    string data;
    node *prev;
    node *next;
    
    node(const string d,node *p=nullptr,node *n=nullptr ){
        data=d;
        prev=p;
        next=n;
    }
    
};


class Clock {
   
    node *head;
    node *minH;
    node *hourH;

public:
    
    Clock();
    void createClock(bool type);//if type==true then make the clock in numbers ,else in Roman
    bool checkClock();//to check clock sequence
    void correctClock();//if checkClock is false we correct the clock sequence
    string getTime();//returns time (if in numbers then display the minutes with 0,5,10,15..)
    void setTime(string hour,string min);//set time to hour and min
    void moveTime(string hour,string min);//move time n hours and n minutes
    ~Clock();//delete clock

};

#endif /* Clock_hpp */
//Clock.cpp
#include "Clock.hpp"
#include <iostream>
using namespace std;

Clock::Clock(){
    head=nullptr;
    minH=nullptr;
    hourH=nullptr;
}

void Clock::createClock(bool type){
    
    string numeric[]={"1", "2", "3", "4", "5", "6", "7", "8", "9", "10", "11", "12"};
    string roman[]={"I", "II", "III", "IV", "V", "VI", "VII", "VIII", "IX", "X", "XI", "XII"};
    
    string *val = type ? numeric : roman;
    
    for(int i = 0 ; i<12 ; i++){
        
        node *newNode=new node(val[i]); //create new node for each value
        
        if(!head) //if the clock is empty
        {
            
            head = newNode;
            head->prev = head;
            head->next = head;
            
        }//end if
        else //if the clock is not empty
        {
            node *tail = head->prev; // pointer for the tail pointing at the last node
            tail->next = newNode; // link the last node with the new node
            newNode->prev = tail; // link prev of new node with the tail
            newNode->next = head; // link next of new node with the head
            head->prev = newNode; // link the prev of the head with the new node
            
        }//end else
        
    }//end for
    
    minH=head;  // initialize to zero
    hourH=head; // initialize to zero
}
Clock::~Clock(){
    if (!head) return;  // If the list is empty, nothing to delete

    node* current = head;
    do {
        node* nextNode = current->next;
        delete current;  // Delete the current node
        current = nextNode;  // Move to the next node
    } while (current != head);

    head = nullptr;  // Ensure head is null after deletion
    hourH = nullptr;
    minH = nullptr;
}
//main
#include <iostream>
#include "Clock.hpp"
using namespace std;
int main() {
    Clock myclock;
    int choice;
    bool type;
    
    cout<<"Hello user, Choose what type of clock do you want: "<<endl;
    
    do{
        cout<<"You have two choices \n 1) Numerical clock \n 2) Roman clock \nEnter 1 or 2 to choose :"<<endl;
        cin>>choice;
        
        if(choice==1 || choice ==2 )
            break;
        
    }while(choice!=1 && choice!=2);
    
    if(choice == 1)
        type = true;
    else
        type = false;
        
    myclock.createClock(type);
   
    return 0;
}
