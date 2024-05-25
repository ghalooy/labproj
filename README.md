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

    return true; // Ensure the method always returns a boolean value
}

 int Clock::number(string rom) {
    if (rom == "I") return 1;
    if (rom == "II") return 2;
    if (rom == "III") return 3;
    if (rom == "IV") return 4;
    if (rom == "V") return 5;
    if (rom == "VI") return 6;
    if (rom == "VII") return 7;
    if (rom == "VIII") return 8;
    if (rom == "IX") return 9;
    if (rom == "X") return 10;
    if (rom == "XI") return 11;
    if (rom == "XII") return 12;
    return -1; // Return an invalid value if the input is not a valid Roman numeral
}
