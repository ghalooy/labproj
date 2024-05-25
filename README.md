bool Clock::checkClock() {
    node* currentHour = head;
    node* currentMin = head;
    
    for (int i = 0; i < 12; ++i) {
        if (currentHour->next != currentMin->next)
            return false;
        
        currentHour = currentHour->next;
        currentMin = currentMin->next->next;
    }
    
    return true;
}

void Clock::correctClock() {
    node* currentHour = head;
    node* currentMin = head;
    
    for (int i = 0; i < 12; ++i) {
        if (currentHour->next != currentMin->next) {
            currentHour->next = currentMin->next;
            currentMin->next->prev = currentHour;
        }
        
        currentHour = currentHour->next;
        currentMin = currentMin->next->next;
    }
}
void Clock::setTime(int hourInt, int minInt) {
    if (hourInt < 1 || hourInt > 12 || minInt < 0 || minInt > 59) {
        cout << "Invalid input. Please provide a valid hour (1-12) and minute (0-59)." << endl;
        return;
    }

    node* current = head;
    while (stoi(current->data) != hourInt) {
        current = current->next;
        if (current == head) {
            cout << "Invalid hour." << endl;
            return;
        }
    }
    hourH = current;

    current = head;
    while (stoi(current->data) != minInt) {
        current = current->next;
        if (current == head) {
            cout << "Invalid minute." << endl;
            return;
        }
    }
    minH = current;
}
void Clock::moveTime(int hourInt, int minInt) {
    int currentHour = stoi(hourH->data);
    int currentMin = stoi(minH->data);
    int currentMinutes = currentHour * 60 + currentMin; // Current time in minutes

    int totalMinutes = hourInt * 60 + minInt; // Convert hours to minutes and add minutes

    int minutesDiff = (totalMinutes - currentMinutes + 720) % 720; // Ensure positive and wrap around 12 hours
    int hours = minutesDiff / 60;
    int minutes = minutesDiff % 60;

    node* newHourNode = hourH;
    for (int i = 0; i < hours; ++i) {
        newHourNode = newHourNode->next;
    }

    node* newMinNode = minH;
    for (int i = 0; i < minutes / 5; ++i) {
        newMinNode = newMinNode->next;
    }

    hourH = newHourNode;
    minH = newMinNode;
}
