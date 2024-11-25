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
        size_t pos2 = line.find(",",
