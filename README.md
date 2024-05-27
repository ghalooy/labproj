int Clock::number(const std::string& s) {
    // This method converts a Roman numeral string to its integer value
    return romanToInt(s);
}

int Clock::romanToInt(const std::string& s) {
    auto getValue = [](char ch) -> int {
        switch (ch) {
        case 'I': return 1;
        case 'V': return 5;
        case 'X': return 10;
        case 'L': return 50;
        case 'C': return 100;
        case 'D': return 500;
        case 'M': return 1000;
        default: return -1; // Invalid character
        }
    };

    int total = 0;
    int prev_value = 0;

    for (int i = s.size() - 1; i >= 0; --i) {
        int value = getValue(s[i]);
        if (value == -1) {
            std::cerr << "Invalid Roman numeral character: " << s[i] << std::endl;
            return -1; // Error handling
        }
        if (value < prev_value) {
            total -= value;
        } else {
            total += value;
        }
        prev_value = value;
    }

    return total;
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
bool Clock::checkClock() {
    if (!head) return false;

    node* current = head;
    int count = 12; // Start from 12

    do {
        if (Type) {
            if (stoi(current->data) != count) return false;
        } else {
            if (romanToInt(current->data) != count) return false;
        }
        current = current->next;
        count = (count % 12) + 1; // Increment and wrap around 12
    } while (current != head);

    return true; // Ensure the method always returns a boolean value
}

