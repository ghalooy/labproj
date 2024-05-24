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
using namespace std;
int main() {
    Clock myclock;
    int choice;
    bool type;

    cout << "Hello user, Choose what type of clock do you want: " << endl;

    do {
        cout << "You have two choices \n 1) Numerical clock \n 2) Romanian clock \nEnter 1 or 2 to choose :" << endl;
        cin >> choice;

        if (choice == 1 || choice == 2)
            break;

    } while (choice != 1 && choice != 2);

    if (choice == 1)
        type = true;
    else
        type = false;

    myclock.createClock(type);
    string hour, min;
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
