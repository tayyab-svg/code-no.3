#include <iostream>
#include <fstream>
#include <vector>
#include <string>
#include <iomanip>

using namespace std;

class Student {
private:
    int id;
    string name;
    int age;
    string grade;

public:
    // Constructors
    Student() : id(0), age(0), name(""), grade("") {}
    Student(int id, const string& name, int age, const string& grade)
        : id(id), name(name), age(age), grade(grade) {}

    // Getter and Setter Methods
    int getId() const { return id; }
    string getName() const { return name; }
    int getAge() const { return age; }
    string getGrade() const { return grade; }

    void setId(int id) { this->id = id; }
    void setName(const string& name) { this->name = name; }
    void setAge(int age) { this->age = age; }
    void setGrade(const string& grade) { this->grade = grade; }

    // Display student details
    void display() const {
        cout << left << setw(10) << id
             << setw(20) << name
             << setw(10) << age
             << setw(10) << grade << endl;
    }

    // Serialize to file
    void saveToFile(ofstream& file) const {
        file << id << "," << name << "," << age << "," << grade << endl;
    }

    // Deserialize from file
    static Student loadFromLine(const string& line) {
        int id, age;
        string name, grade;
        size_t pos1 = line.find(",");
        size_t pos2 = line.find(",", pos1 + 1);
        size_t pos3 = line.find(",", pos2 + 1);

        id = stoi(line.substr(0, pos1));
        name = line.substr(pos1 + 1, pos2 - pos1 - 1);
        age = stoi(line.substr(pos2 + 1, pos3 - pos2 - 1));
        grade = line.substr(pos3 + 1);

        return Student(id, name, age, grade);
    }
};

// Function to load students from a file
vector<Student> loadStudents(const string& filename) {
    vector<Student> students;
    ifstream file(filename);

    if (file.is_open()) {
        string line;
        while (getline(file, line)) {
            students.push_back(Student::loadFromLine(line));
        }
        file.close();
    }
    return students;
}

// Function to save students to a file
void saveStudents(const string& filename, const vector<Student>& students) {
    ofstream file(filename);

    if (file.is_open()) {
        for (const auto& student : students) {
            student.saveToFile(file);
        }
        file.close();
    }
}

// Main menu
void displayMenu() {
    cout << "\n--- Student Management System ---\n";
    cout << "1. Add Student\n";
    cout << "2. View Students\n";
    cout << "3. Update Student\n";
    cout << "4. Delete Student\n";
    cout << "5. Exit\n";
    cout << "Enter your choice: ";
}

// Add a new student
void addStudent(vector<Student>& students) {
    int id, age;
    string name, grade;

    cout << "Enter Student ID: ";
    cin >> id;
    cin.ignore();
    cout << "Enter Student Name: ";
    getline(cin, name);
    cout << "Enter Student Age: ";
    cin >> age;
    cin.ignore();
    cout << "Enter Student Grade: ";
    getline(cin, grade);

    students.push_back(Student(id, name, age, grade));
    cout << "Student added successfully.\n";
}

// View all students
void viewStudents(const vector<Student>& students) {
    if (students.empty()) {
        cout << "No students found.\n";
        return;
    }

    cout << left << setw(10) << "ID"
         << setw(20) << "Name"
         << setw(10) << "Age"
         << setw(10) << "Grade" << endl;
    cout << string(50, '-') << endl;

    for (const auto& student : students) {
        student.display();
    }
}

// Update a student's details
void updateStudent(vector<Student>& students) {
    int id;
    cout << "Enter the Student ID to update: ";
    cin >> id;

    for (auto& student : students) {
        if (student.getId() == id) {
            int age;
            string name, grade;

            cout << "Enter New Name: ";
            cin.ignore();
            getline(cin, name);
            cout << "Enter New Age: ";
            cin >> age;
            cin.ignore();
            cout << "Enter New Grade: ";
            getline(cin, grade);

            student.setName(name);
            student.setAge(age);
            student.setGrade(grade);

            cout << "Student updated successfully.\n";
            return;
        }
    }
    cout << "Student not found.\n";
}

// Delete a student
void deleteStudent(vector<Student>& students) {
    int id;
    cout << "Enter the Student ID to delete: ";
    cin >> id;

    for (auto it = students.begin(); it != students.end(); ++it) {
        if (it->getId() == id) {
            students.erase(it);
            cout << "Student deleted successfully.\n";
            return;
        }
    }
    cout << "Student not found.\n";
}

// Main function
int main() {
    vector<Student> students = loadStudents("students.txt");
    int choice;

    do {
        displayMenu();
        cin >> choice;

        switch (choice) {
        case 1:
            addStudent(students);
            break;
        case 2:
            viewStudents(students);
            break;
        case 3:
            updateStudent(students);
            break;
        case 4:
            deleteStudent(students);
            break;
        case 5:
            saveStudents("students.txt", students);
            cout << "Goodbye!\n";
            break;
        default:
            cout << "Invalid choice. Try again.\n";
        }
    } while (choice != 5);

    return 0;
}
