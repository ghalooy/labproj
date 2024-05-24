void Clock::setTime(string hour, string min) {
    // Determine hour and meridiem
    int hourInt = (hour[0] - '0') * 10 + (hour[1] - '0');
    if (hourInt == 0) {
        hourInt = 12;
        meridiem = "AM";
    } else if (hourInt == 12) {
        meridiem = "PM";
    } else if (hourInt >= 12) {
        hourInt -= 12;
        meridiem = "PM";
    } else {
        meridiem = "AM";
    }

    // Set hourH
    while (hourH->data != hour && hourH->data != "0" + hour) {
        hourH = hourH->next;
    }

    // Set minH
    int minInt = (min[0] - '0') * 10 + (min[1] - '0');
    for (int i = 0; i < minInt / 5; i++) {
        minH = minH->next;
    }
}
void Clock::moveTime(string hour, string min) {
    int hourInt = (hour[0] - '0') * 10 + (hour[1] - '0');
    int minInt = (min[0] - '0') * 10 + (min[1] - '0');

    // Move minutes
    for (int i = 0; i < minInt / 5; i++) {
        minH = minH->next;
    }

    // Move hours
    for (int i = 0; i < hourInt; i++) {
        hourH = hourH->next;
    }

    // Update AM/PM if necessary
    if (hourInt >= 12) {
        if (meridiem == "AM") {
            meridiem = "PM";
        } else {
            meridiem = "AM";
        }
    }
}
