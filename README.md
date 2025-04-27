# cpp-gradebook-system
#include <iostream>
#include <string>
using namespace std;

const int MAX_STUDENTS = 100;

void inputStudents(string names[], int grades[], int &count);
void sortGrades(string names[], int grades[], int count);
int searchStudent(string names[], int grades[], int count, string searchName);
void displayStats(int grades[], int count);

int main() {
    string names[MAX_STUDENTS];
    int grades[MAX_STUDENTS];
    int count;

    inputStudents(names, grades, count);

    sortGrades(names, grades, count);

    cout << "\nSorted Grades:\n";
    for (int i = 0; i < count; i++) {
        cout << names[i] << ": " << grades[i] << endl;
    }

    string searchName;
    cout << "\nEnter student name to search: ";
    cin >> ws; // clear newline from previous input
    getline(cin, searchName);

    int index = searchStudent(names, grades, count, searchName);
    if (index != -1) {
        cout << names[index] << "'s grade: " << grades[index] << endl;
    } else {
        cout << "Student not found.\n";
    }

    displayStats(grades, count);
    return 0;
}

void inputStudents(string names[], int grades[], int &count) {
    cout << "Enter the number of students: ";
    cin >> count;

    if (count > MAX_STUDENTS) {
        cout << "Error: Maximum number of students is " << MAX_STUDENTS << ".\n";
        count = MAX_STUDENTS; // Clamp to max
    }

    for (int i = 0; i < count; i++) {
        cout << "\nEnter student name: ";
        cin >> ws; // skip whitespace/newlines
        getline(cin, names[i]);

        while (true) {
            cout << "Enter " << names[i] << "'s grade (0-100): ";
            cin >> grades[i];

            if (cin.fail() || grades[i] < 0 || grades[i] > 100) {
                cout << "Invalid grade. Please enter a value between 0 and 100.\n";
                cin.clear(); // clear error flag
                cin.ignore(1000, '\n'); // discard invalid input
            } else {
                break;
            }
        }
    }
}

void sortGrades(string names[], int grades[], int count) {
    for (int i = 0; i < count - 1; i++) {
        for (int j = i + 1; j < count; j++) {
            if (grades[i] > grades[j]) {
                swap(grades[i], grades[j]);
                swap(names[i], names[j]);
            }
        }
    }
}

int searchStudent(string names[], int grades[], int count, string searchName) {
    for (int i = 0; i < count; i++) {
        if (names[i] == searchName) {
            return i;
        }
    }
    return -1;
}

void displayStats(int grades[], int count) {
    if (count == 0) {
        cout << "\nNo grades to analyze.\n";
        return;
    }

    int total = 0, max = grades[0], min = grades[0];

    for (int i = 0; i < count; i++) {
        total += grades[i];
        if (grades[i] > max) max = grades[i];
        if (grades[i] < min) min = grades[i];
    }

    double average = static_cast<double>(total) / count;

    cout << "\nClass Statistics:\n";
    cout << "Average Grade: " << average << endl;
    cout << "Highest Grade: " << max << endl;
    cout << "Lowest Grade: " << min << endl;
}
