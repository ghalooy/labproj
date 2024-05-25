 bool Clock::checkClock() {
    if (!head) return false;
    node *current = head;
    int count = 1;
    do {
        if (Type) {
            if (stoi(current->data) != count) return false;
        } else {
            if (number(current->data) != count) return false;
        }
        current = current->next;
        count = (count % 12) + 1;
    } while (current != head);

void Clock::correctClock() {
    if (!head) return;
    node *current = head;
    string numeric[] = {"12", "1", "2", "3", "4", "5", "6", "7", "8", "9", "10", "11"};
    string roman[] = {"XII", "I", "II", "III", "IV", "V", "VI", "VII", "VIII", "IX", "X", "XI"};
    string *val = Type ? numeric : roman;
    int index = 0;
    do {
        current->data = val[index++];
        current = current->next;
    } while (current != head);
}
