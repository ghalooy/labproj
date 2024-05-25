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
    int number(string rom);//converts from roman to number
    string getTimeNumerical();
    string getTimeRoman();
    void NumClockwise(int hour,int min);
    void NumCounterClockwise(int hour,int min);
    void RomClockwise(int hour,int min);
    void RomCounterClockwise(int hour,int min);
    
public:
    
    Clock();
    void createClock(bool type);//if type==true then make the clock in numbers ,else in Roman
    bool checkClock();//to check clock sequence
    void correctClock();//if checkClock is false we correct the clock sequence
    string getTime();//returns time (if in numbers then display the minutes with 0,5,10,15..)
    bool setTime(int hour,int min,string mer);//set time to hour and min
    void moveTime(int hour,int min,bool clockwise);//move time n hours and n minutes
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
    
    minH=head;  // initialize
    hourH=head; // initialize
    meridiem="AM"; //initialize
}
Clock::~Clock(){
    if (!head) return;  // If is empty, nothing to delete

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
string Clock::getTime(){
    if(Type) //check if the clock is in Numerical or Roman
        return getTimeNumerical();
    else
        return getTimeRoman();
}
string Clock::getTimeNumerical() {
    string time;

    // Convert the minute node data to integer and multiply by 5
    int minInt = stoi(minH->data) * 5;

    // Special case: if minute data is "12", it should represent "00" minutes
    if (minH->data == "12") {
        minInt = 0;
    }

    // Format the minute and hour as a two-digit string
    string min = (minInt < 10) ? "0" + to_string(minInt) : to_string(minInt);
    string hour = (hourH->data.length() < 2) ? "0" + hourH->data : hourH->data;
    
    // Construct the time string in the format "00:00 AM/PM"
    time = hour + " : " + min + " " + meridiem;

    return time;
}
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

bool Clock::setTime(int hour, int min, string mer) {
    if (head == nullptr) { // Check if the clock is initialized
        cout << "Clock is not initialized." << endl;
        return false;
    }

    // Check if the provided hour is within range (1-12)
    if (hour < 1 || hour > 12) {
        cout << "Invalid input. Please provide a valid hour (1-12)." << endl;
        return false;
    }

    // Check if the provided minute is within range (multiples of 5, 0-55)
    if (min % 5 != 0 || min < 0 || min > 55) {
        cout << "Invalid input. Please provide a valid minute (multiples of 5, 0-55)." << endl;
        return false;
    }

    // Check if the meridiem is valid
    if (mer != "AM" && mer != "PM") {
        cout << "Invalid input. Please provide a valid meridiem (AM or PM)." << endl;
        return false;
    }

    // Find the node corresponding to the provided hour
    node* hourNode = head;
    for (int i = 0; i < hour; i++) {
        hourNode = hourNode->next;
    }

    // Find the node corresponding to the provided minute
    node* minNode = head;
    int minIndex = min / 5; // Convert to the minutes value on clock
    for (int i = 0; i < minIndex; i++) {
        minNode = minNode->next;
    }

    // Update the hour and minute pointers
    hourH = hourNode;
    minH = minNode;
    meridiem = mer;
    return true;
}
void Clock::moveTime(int hour, int min,bool clockwise){
    if(clockwise)
    {
        if(Type)
            NumClockwise(hour,min);
        else
            RomClockwise(hour, min);
    }
    else{
        if(Type)
            NumCounterClockwise(hour,min);
        else
            RomCounterClockwise(hour, min);
    }
}
void Clock::NumClockwise(int hour, int min){
    if(min != 0){
        int remMinToCompleteHour = 60 - (stoi(minH->data) * 5); // how many minutes are remaining to complete an hour
        
        if (remMinToCompleteHour <= min) // check if the minutes to move is greter than or equal to remMinToCompleteHour
            hourH = hourH->next; // move one hour
        
        if(hourH->data=="12")
            meridiem= (meridiem == "AM") ? "PM" : "AM"; // if we hit 12 o'clock change AM or PM
        
        min=min/5; // convert the minutes to loop times
        
        // move the minutes hand
        for (int i = 0; i < min; i++) {
            minH = minH->next;
        }
    }
    //if the hours to move is zero there is nothing to do to the hours hand
    if(hour != 0){
        
        if (hourH->data!="12") {
            
            int remHourtoMidnight = 12 - stoi(hourH->data);
            
            if (remHourtoMidnight < hour)
                meridiem= (meridiem == "AM") ? "PM" : "AM";
        }
        
        for (int i = 0; i < hour; i++) {
            hourH = hourH->next;
        }
        
    }
}
void Clock::NumCounterClockwise(int hour, int min){
    if (min != 0) {
                int remMinToCompleteHour = stoi(minH->data) * 5;

                if (remMinToCompleteHour >= min)
                    hourH = hourH->prev;

                if (hourH->data == "12")
                    meridiem = (meridiem == "AM") ? "PM" : "AM";

                min = min / 5;

                for (int i = 0; i < min; i++) {
                    minH = minH->prev;
                }
    }
    if (hour != 0) {

                if (hourH->data != "12") {

                    int remHourtoMidnight = stoi(hourH->data);

                    if (remHourtoMidnight < hour)
                        meridiem = (meridiem == "AM") ? "PM" : "AM";
                }
                else
                {
                    if(hour==12)
                        meridiem = (meridiem == "AM") ? "PM" : "AM";
                }

                for (int i = 0; i < hour; i++) {
                    hourH = hourH->prev;
                }
    }

}
int Clock::number(string rom){
    {
        int numeric[]={12,1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11};
        string roman[]={"XII","I", "II", "III", "IV", "V", "VI", "VII", "VIII", "IX", "X", "XI"};
        int num=-1;
        for(int i=0;i<12;i++){
            if (rom==roman[i]) {
                num = numeric[i];
            }
        }
        return num;
    }
}
void Clock::RomClockwise(int hour, int min) {
    if (min != 0) {
        int remMinToCompleteHour = 60 - (number(minH->data) * 5); // how many minutes are remaining to complete an hour

        if (remMinToCompleteHour <= min) // check if the minutes to move is greater than or equal to remMinToCompleteHour
            hourH = hourH->next; // move one hour

        if (hourH->data == "XII")
            meridiem = (meridiem == "AM") ? "PM" : "AM"; // if we hit 12 o'clock change AM or PM

        min = min / 5; // convert the minutes to loop times

        // move the minutes hand
        for (int i = 0; i < min; i++) {
            minH = minH->next;
        }
    }
    //if the hours to move is zero there is nothing to do to the hours hand
    if (hour != 0) {
        if (hourH->data != "XII") {
            int remHourtoMidnight = 12 - number(hourH->data);

            if (remHourtoMidnight < hour)
                meridiem = (meridiem == "AM") ? "PM" : "AM";
        }

        for (int i = 0; i < hour; i++) {
            hourH = hourH->next;
        }
    }
}
void Clock::RomCounterClockwise(int hour, int min) {
    if (min != 0) {
        int remMinToCompleteHour = number(minH->data) * 5;

        if (remMinToCompleteHour >= min)
            hourH = hourH->prev;

        if (hourH->data == "XII")
            meridiem = (meridiem == "AM") ? "PM" : "AM";

        min = min / 5;

        for (int i = 0; i < min; i++) {
            minH = minH->prev;
        }
    }

    if (hour != 0) {
        if (hourH->data != "XII") {
            int remHourtoMidnight = number(hourH->data);

            if (remHourtoMidnight < hour)
                meridiem = (meridiem == "AM") ? "PM" : "AM";
        }
        else {
            if (hour == 12)
                meridiem = (meridiem == "AM") ? "PM" : "AM";
        }

        for (int i = 0; i < hour; i++) {
            hourH = hourH->prev;
        }
    }

}

#include <iostream>
#include <string>
#include "Clock.hpp"
using namespace std;
int main() {
    
    Clock myclock;
    
    int choice; //to let the user choose the type of clock (Numerical or Roman)
    bool type; // parameters to createClock
    int hour,min;
    string meridiem; //parameters for setTime
   
    
    cout<<"Hello user, Choose what type of clock do you want: "<<endl;
    
    do{
        cout<<"You have two choices \n 1) Numerical clock \n 2) Romanian clock \nEnter 1 or 2 to choose :"<<endl;
        cin>>choice;
        
    }while(choice!=1 && choice!=2);
    
    if(choice == 1)
        type = true;
    else
        type = false;
        
    myclock.createClock(type);
    
    cout<<"Initial clock time: "<<myclock.getTime()<<endl;
    
    cout<<"..................................................................................."<<endl;
    
    cout<<"To set the time\n1)Enter the hour (1-12)\n2)Enter the minutes (multiples of 5 from (0-55))\n3)Enter the meridiem (AM or PM)"<<endl;
    
    cin>>hour;
    cin>>min;
    cin>>meridiem;
    
    //keep calling the function until the parameters are right
    while (!myclock.setTime(hour, min, meridiem))
    {
        cin>>hour;
        cin>>min;
        cin>>meridiem;
    }
    
    cout<<"..................................................................................."<<endl;
    
    cout<<"Time after setting is "<<myclock.getTime()<<endl;
    
    cout<<"..................................................................................."<<endl;
    
    cout<<"To move the time:"<<endl;
    do{
        cout<<"Choose the direction of movement\n1)Clockwise\n2)Counter Clockwise\nEnter 1 or 2 to choose :"<<endl;
        cin>>choice;
    }while(choice!=1 && choice!=2);
    
    if(choice == 1)
        type = true;
    else
        type = false;
    
    cout<<"Enter the amount of hours you want to move "<<endl;
    cin>>hour;
    
    cout<<"Enter the amount of minutes you want to move (multiples of 5 from (0-55)) "<<endl;
    cin>>min;
    
    while(!(min>=0 && min<=55 && min%5==0)){
        cout<<"Invalid,Minutes should be multiples of 5 from (0-55) "<<endl;
        cin>>min;
    }
   
    myclock.moveTime(hour,min,type);
    
    cout<<"Time after moving is "<<myclock.getTime()<<endl;
   
    return 0;
}






