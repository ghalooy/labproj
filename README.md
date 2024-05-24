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
