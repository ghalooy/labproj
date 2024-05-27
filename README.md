bool Clock::checkClock() {
    if (!head) return false;

    node* current = head;
    int count = 12; // Start from 12

    do {
        if (Type) {
            if (stoi(current->data) != count) return false;
        } else {
            if (number(current->data) != count) return false;
        }
        current = current->next;
        count = (count % 12) + 1; // Increment and wrap around 12
    } while (current != head);

    return true; // Ensure the method always returns a boolean value
}

void Clock::correctClock() {
    node* current = head;
    int count = 12; // Start from 12

    do {
        current->data = Type ? to_string(count) : to_string(romanToInt(to_string(count)));
        current = current->next;
        count = (count % 12) + 1; // Increment and wrap around 12
    } while (current != head);
}
