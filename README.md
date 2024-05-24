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
    for (int i = 1; i < hourInt; ++i) {
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
