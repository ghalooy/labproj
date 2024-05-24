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
    bool Type;
    string meridiem;
    string getTimeNumerical();
    string getTimeRoman();
    
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
#include "Clock.hpp"
#include <iostream>
#include <string>
using namespace std;

Clock::Clock(){
    head=nullptr;
    minH=nullptr;
    hourH=nullptr;
}

void Clock::createClock(bool type){
    Type=type;
    string numeric[]={"12","1", "2", "3", "4", "5", "6", "7", "8", "9", "10", "11"};
    string roman[]={"XII","I", "II", "III", "IV", "V", "VI", "VII", "VIII", "IX", "X", "XI"};
    
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
    meridiem="AM"; //initialize to AM
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
/*
string Clock::getTime(){
    string time;
    if(Type){
        int min=stoi(minH->data);
        switch (min) {
            case 1:
                min=5;
                break;
            case 2:
                min=10;
                break;
            case 3:
                min=15;
                break;
            case 4:
                min=20;
                break;
            case 5:
                min=25;
                break;
            case 6:
                min=30;
                break;
            case 7:
                min=35;
                break;
            case 8:
                min=40;
                break;
            case 9:
                min=45;
                break;
            case 10:
                min=50;
                break;
            case 11:
                min=55;
                break;
            default:
                min=0;
                break;
        }
        time = hourH->data + " : " + (min < 10 ? "0" : "") + to_string(min) + " " + meridiem; // return time in format 00:00
        
    }
    else{
            string min=minH->data;
            string hour=hourH->data;
            
            if (min == "I")    min="05";
            if (min == "II")   min="10";
            if (min == "III")  min="15";
            if (min == "IV")   min="20";
            if (min == "V")    min="25";
            if (min == "VI")   min="30";
            if (min == "VII")  min="35";
            if (min == "VIII") min="40";
            if (min == "IX")   min="45";
            if (min == "X")    min="50";
            if (min == "XI")   min="55";
            if (min == "XII")  min="00";
            
            if (hour == "I")    hour="01";
            if (hour == "II")   hour="02";
            if (hour == "III")  hour="03";
            if (hour == "IV")   hour="04";
            if (hour == "V")    hour="05";
            if (hour == "VI")   hour="06";
            if (hour == "VII")  hour="07";
            if (hour == "VIII") hour="08";
            if (hour == "IX")   hour="09";
            if (hour == "X")    hour="10";
            if (hour == "XI")   hour="11";
            if (hour == "XII")  hour="12";
            
        time= hour + " : " + min + " " + meridiem;
    }
    return time ;
}
*/
string Clock::getTime(){
    
    if(Type)
        return getTimeNumerical();
    else
        return getTimeRoman();
}
string Clock::getTimeNumerical(){
    string time;
    string min = minH->data;
    string hour = hourH->data;

    if (min == "1")    min = "05";
    if (min == "2")    min = "10";
    if (min == "3")    min = "15";
    if (min == "4")    min = "20";
    if (min == "5")    min = "25";
    if (min == "6")    min = "30";
    if (min == "7")    min = "35";
    if (min == "8")    min = "40";
    if (min == "9")    min = "45";
    if (min == "10")   min = "50";
    if (min == "11")   min = "55";
    if (min == "12")   min = "00"; // Represent zero minutes as "00"

        time = hour + " : " + min + " " + meridiem; // return time in format 00:00
    return time;
}
/*
string Clock::getTimeRoman(){
    string time;
    string min=minH->data;
    string hour=hourH->data;
    
    if (min == "I")    min="V";
    if (min == "II")   min="X";
    if (min == "III")  min="XV";
    if (min == "IV")   min="XX";
    if (min == "V")    min="XXV";
    if (min == "VI")   min="XXX";
    if (min == "VII")  min="XXXV";
    if (min == "VIII") min="XL";
    if (min == "IX")   min="XLV";
    if (min == "X")    min="L";
    if (min == "XI")   min="LV";
    if (min == "XII")  min="N";//there's no representation for zero in numerical!!
   
    
    time= hour + " : " + min + " " + meridiem;
    return time;
}
*/
string Clock::getTimeRoman(){
    string time;
    string min=minH->data;
    string hour=hourH->data;
    
    if (min == "I")    min="05";
    if (min == "II")   min="10";
    if (min == "III")  min="15";
    if (min == "IV")   min="20";
    if (min == "V")    min="25";
    if (min == "VI")   min="30";
    if (min == "VII")  min="35";
    if (min == "VIII") min="40";
    if (min == "IX")   min="45";
    if (min == "X")    min="50";
    if (min == "XI")   min="55";
    if (min == "XII")  min="00";
    
    if (hour == "I")    hour="01";
    if (hour == "II")   hour="02";
    if (hour == "III")  hour="03";
    if (hour == "IV")   hour="04";
    if (hour == "V")    hour="05";
    if (hour == "VI")   hour="06";
    if (hour == "VII")  hour="07";
    if (hour == "VIII") hour="08";
    if (hour == "IX")   hour="09";
    if (hour == "X")    hour="10";
    if (hour == "XI")   hour="11";
    if (hour == "XII")  hour="12";
   
    
    time= hour + " : " + min + " " + meridiem;
    return time;
}
void Clock::setTime(string hour, string min) {
    // Ensure valid hour input
    if (hour.size() != 2 || min.size() != 2) {
        cout << "Invalid input. Please provide a valid hour and minute." << endl;
        return;
    }

    // Check if the provided hour and minute are within range
    int hourInt = (hour[0] - '0') * 10 + (hour[1] - '0');
    int minInt = (min[0] - '0') * 10 + (min[1] - '0');
    if (hourInt < 1 || hourInt > 12 || minInt < 0 || minInt > 59) {
        cout << "Invalid input. Please provide a valid hour (1-12) and minute (0-59)." << endl;
        return;
    }

    // Find the node corresponding to the provided hour
    node* hourNode = head;
    for (int i = 0; i < hourInt; ++i) {
        hourNode = hourNode->next;
    }

    // Find the node corresponding to the provided minute (in 5-minute intervals)
    node* minNode = head;
    int minIndex = minInt / 5;
    for (int i = 0; i < minIndex; ++i) {
        minNode = minNode->next;
    }

    // Update the hour and minute pointers
    hourH = hourNode;
    minH = minNode;
}
void Clock::moveTime(string hour, string min) {
    // Ensure valid hour input
    if (hour.size() != 2 || min.size() != 2) {
        cout << "Invalid input. Please provide a valid hour and minute." << endl;
        return;
    }

    // Check if the provided hour and minute are within range
    int hourInt = (hour[0] - '0') * 10 + (hour[1] - '0');
    int minInt = (min[0] - '0') * 10 + (min[1] - '0');
    if (hourInt < 1 || hourInt > 12 || minInt < 0 || minInt > 59) {
        cout << "Invalid input. Please provide a valid hour (1-12) and minute (0-59)." << endl;
        return;
    }

    // Find the node corresponding to the provided hour
    node* hourNode = head;
    for (int i = 0; i < hourInt; ++i) {
        hourNode = hourNode->next;
    }

    // Find the node corresponding to the provided minute (in 5-minute intervals)
    node* minNode = head;
    int minIndex = minInt / 5;
    for (int i = 0; i < minIndex; ++i) {
        minNode = minNode->next;
    }

    // Update the hour and minute pointers
    hourH = hourNode;
    minH = minNode;
}

//main
#include <iostream>
#include "Clock.hpp"
using namespace std;
int main() {
    Clock myclock;
    int choice;
    bool type;
    string hour, min;
    
    cout<<"Hello user, Choose what type of clock do you want: "<<endl;
    
    do{
        cout<<"You have two choices \n 1) Numerical clock \n 2) Romanian clock \nEnter 1 or 2 to choose :"<<endl;
        cin>>choice;
        
        if(choice==1 || choice ==2 )
            break;
        
    }while(choice!=1 && choice!=2);
    
    if(choice == 1)
        type = true;
    else
        type = false;
        
    myclock.createClock(type);

   
    cout << "Enter the hour (1-12): ";
    cin >> hour;
    cout << "Enter the minute (0-59): ";
    cin >> min;
    cout << endl;
    myclock.setTime(hour, min);

    // Display the time
    cout << "Current time: " << myclock.getTime() << endl;

    cout << endl;
    myclock.moveTime("03", "20");

    // Display the updated time
    cout << "Updated time: " << myclock.getTime() << endl;

   
    return 0;
}
