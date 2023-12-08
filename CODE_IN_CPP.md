# To_Do_List-Task-Manager-
#CODE IN CPP


#include <iostream>
#include <vector>
#include <ctime>
#include <iomanip>

using namespace std;

class Task {
public:
    string description;
    tm dueDate;
    bool completed;

    Task(const string& desc, const tm& date) : description(desc), dueDate(date), completed(false) {}
};

class ToDoList {
private:
    vector<Task> tasks;

public:
    void addTask(const string& desc, const tm& date) {
        Task newTask(desc, date);
        tasks.push_back(newTask);
        cout << "Task added successfully.\n";
    }

    void displayTasks() const {
        if (tasks.empty()) {
            cout << "No tasks in the to-do list.\n";
        } else {
            cout << "To-Do List:\n";
            for (size_t i = 0; i < tasks.size(); ++i) {
                const auto& task = tasks[i];
                cout << i + 1 << ". " << task.description;
                cout << " (Due: " << task.dueDate.tm_year + 1900 << "/" << task.dueDate.tm_mon + 1 << "/"
                      << task.dueDate.tm_mday << ")";
                if (task.completed) {
                    cout << " [Completed]";
                }
                cout << "\n";
            }
        }
    }

    void markTaskAsCompleted(size_t index) {
        if (index < tasks.size()) {
            tasks[index].completed = true;
            cout << "Task marked as completed.\n";
        } else {
            cout << "Invalid task index.\n";
        }
    }

    void removeTask(size_t index) {
        if (index < tasks.size()) {
            tasks.erase(tasks.begin() + index);
            cout << "Task removed successfully.\n";
        } else {
            cout << "Invalid task index.\n";
        }
    }
};

int main() {
    ToDoList toDoList;

    // Example tasks
    tm dueDate1 = {0, 0, 0, 1, 1, 122};  // Due on January 1, 2023
    tm dueDate2 = {0, 0, 0, 15, 3, 123}; // Due on March 15, 2023

    toDoList.addTask("Complete assignment", dueDate1);
    toDoList.addTask("Buy groceries", dueDate2);

    int choice;
    do {
        cout << "\nMenu:\n";
        cout << "1. Add Task\n";
        cout << "2. Display Tasks\n";
        cout << "3. Mark Task as Completed\n";
        cout << "4. Remove Task\n";
        cout << "0. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1: {
                string desc;
                cout << "Enter task description: ";
                cin.ignore(); // Clear input buffer
                getline(cin, desc);

                cout << "Enter due date (YYYY MM DD): ";
                int year, month, day;
                cin >> year >> month >> day;

                tm dueDate = {};
                dueDate.tm_year = year - 1900; // Adjust for the year (tm_year is years since 1900)
                dueDate.tm_mon = month - 1;    // Adjust for the month (tm_mon is 0-based)
                dueDate.tm_mday = day;

                toDoList.addTask(desc, dueDate);
                break;
            }
            case 2:
                toDoList.displayTasks();
                break;
            case 3: {
                size_t index;
                cout << "Enter the index of the task to mark as completed: ";
                cin >> index;
                toDoList.markTaskAsCompleted(index - 1);
                break;
            }
            case 4: {
                size_t index;
                cout << "Enter the index of the task to remove: ";
                cin >> index;
                toDoList.removeTask(index - 1);
                break;
            }
            case 0:
                cout << "Exiting the program.\n";
                break;
            default:
                cout << "Invalid choice. Try again.\n";
        }
    } while (choice != 0);

    return 0;
} 
 
