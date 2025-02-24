# Employee-payroll-system
#include <iostream>
#include <fstream>
#include <vector>

using namespace std;

class Employee {
public:
    string empID, name;
    float basicSalary, allowances, deductions;

    Employee() {}

    Employee(string id, string n, float salary, float allow, float deduct) {
        empID = id;
        name = n;
        basicSalary = salary;
        allowances = allow;
        deductions = deduct;
    }

    float calculateSalary() const {
        return basicSalary + allowances - deductions;
    }

    void display() const {
        cout << "Employee ID: " << empID << endl;
        cout << "Name: " << name << endl;
        cout << "Basic Salary: " << basicSalary << endl;
        cout << "Allowances: " << allowances << endl;
        cout << "Deductions: " << deductions << endl;
        cout << "Net Salary: " << calculateSalary() << endl;
        cout << "---------------------------" << endl;
    }
};

class PayrollSystem {
private:
    vector<Employee> employees;
    string filename = "employees.txt";

public:
    void addEmployee() {
        string empID, name;
        float basicSalary, allowances, deductions;

        cout << "Enter Employee ID: ";
        cin >> empID;
        cin.ignore();
        cout << "Enter Name: ";
        getline(cin, name);
        cout << "Enter Basic Salary: ";
        cin >> basicSalary;
        cout << "Enter Allowances: ";
        cin >> allowances;
        cout << "Enter Deductions: ";
        cin >> deductions;

        Employee emp(empID, name, basicSalary, allowances, deductions);
        employees.push_back(emp);
        cout << "Employee added successfully!\n";
    }

    void viewEmployees() const {
        if (employees.empty()) {
            cout << "No employees available.\n";
            return;
        }

        for (const auto &emp : employees) {
            emp.display();
        }
    }

    void saveToFile() {
        ofstream file(filename);
        if (!file) {
            cout << "Error opening file for writing.\n";
            return;
        }

        for (const auto &emp : employees) {
            file << emp.empID << " " << emp.name << " " << emp.basicSalary << " "
                 << emp.allowances << " " << emp.deductions << "\n";
        }
        file.close();
        cout << "Employee records saved successfully!\n";
    }

    void loadFromFile() {
        ifstream file(filename);
        if (!file) {
            cout << "No existing records found.\n";
            return;
        }

        employees.clear();
        string empID, name;
        float basicSalary, allowances, deductions;
        while (file >> empID >> name >> basicSalary >> allowances >> deductions) {
            employees.emplace_back(empID, name, basicSalary, allowances, deductions);
        }
        file.close();
        cout << "Employee records loaded successfully!\n";
    }
};

int main() {
    PayrollSystem payroll;
    int choice;

    do {
        cout << "\nEmployee Payroll System Menu\n";
        cout << "1. Add Employee\n";
        cout << "2. View Employees\n";
        cout << "3. Save to File\n";
        cout << "4. Load from File\n";
        cout << "5. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                payroll.addEmployee();
                break;
            case 2:
                payroll.viewEmployees();
                break;
            case 3:
                payroll.saveToFile();
                break;
            case 4:
                payroll.loadFromFile();
                break;
            case 5:
                cout << "Exiting program...\n";
                break;
            default:
                cout << "Invalid choice, please try again.\n";
        }
    } while (choice != 5);

    return 0;
}
