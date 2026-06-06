#include <iostream>
#include <fstream>
#include <string>
using namespace std;

class Student
{
public:
    int rollNo;
    string name;
    int age;
    string department;

    void addStudent()
    {
        cout << "\nEnter Roll Number: ";
        cin >> rollNo;
        cin.ignore();

        cout << "Enter Name: ";
        getline(cin, name);

        cout << "Enter Age: ";
        cin >> age;
        cin.ignore();

        cout << "Enter Department: ";
        getline(cin, department);

        ofstream file("students.txt", ios::app);

        file << rollNo << endl;
        file << name << endl;
        file << age << endl;
        file << department << endl;

        file.close();

        cout << "\nStudent Record Added Successfully!\n";
    }

    void displayStudents()
    {
        ifstream file("students.txt");

        if (!file)
        {
            cout << "\nNo Records Found!\n";
            return;
        }

        cout << "\n----- Student Records -----\n";

        while (file >> rollNo)
        {
            file.ignore();

            getline(file, name);
            file >> age;
            file.ignore();

            getline(file, department);

            cout << "\nRoll Number : " << rollNo;
            cout << "\nName        : " << name;
            cout << "\nAge         : " << age;
            cout << "\nDepartment  : " << department << endl;
        }

        file.close();
    }

    void searchStudent()
    {
        int searchRoll;
        bool found = false;

        cout << "\nEnter Roll Number to Search: ";
        cin >> searchRoll;

        ifstream file("students.txt");

        while (file >> rollNo)
        {
            file.ignore();

            getline(file, name);
            file >> age;
            file.ignore();

            getline(file, department);

            if (rollNo == searchRoll)
            {
                cout << "\nStudent Found!\n";
                cout << "\nRoll Number : " << rollNo;
                cout << "\nName        : " << name;
                cout << "\nAge         : " << age;
                cout << "\nDepartment  : " << department << endl;

                found = true;
                break;
            }
        }

        file.close();

        if (!found)
        {
            cout << "\nStudent Not Found!\n";
        }
    }

    void deleteStudent()
    {
        int deleteRoll;
        bool found = false;

        cout << "\nEnter Roll Number to Delete: ";
        cin >> deleteRoll;

        ifstream file("students.txt");
        ofstream temp("temp.txt");

        while (file >> rollNo)
        {
            file.ignore();

            getline(file, name);
            file >> age;
            file.ignore();

            getline(file, department);

            if (rollNo != deleteRoll)
            {
                temp << rollNo << endl;
                temp << name << endl;
                temp << age << endl;
                temp << department << endl;
            }
            else
            {
                found = true;
            }
        }

        file.close();
        temp.close();

        remove("students.txt");
        rename("temp.txt", "students.txt");

        if (found)
        {
            cout << "\nStudent Record Deleted Successfully!\n";
        }
        else
        {
            cout << "\nStudent Not Found!\n";
        }
    }
};

int main()
{
    Student s;
    int choice;

    do
    {
        cout << "\n========== Student Management System ==========\n";
        cout << "1. Add Student\n";
        cout << "2. Display Students\n";
        cout << "3. Search Student\n";
        cout << "4. Delete Student\n";
        cout << "5. Exit\n";

        cout << "\nEnter Your Choice: ";
        cin >> choice;

        switch (choice)
        {
        case 1:
            s.addStudent();
            break;

        case 2:
            s.displayStudents();
            break;

        case 3:
            s.searchStudent();
            break;

        case 4:
            s.deleteStudent();
            break;

        case 5:
            cout << "\nExiting Program...\n";
            break;

        default:
            cout << "\nInvalid Choice! Try Again.\n";
        }

    } while (choice != 5);

    return 0;
}
