#include <iostream>
#include <fstream>
#include <vector>
#include <string>

using namespace std;

struct Task {
    string title;
    bool completed;
};

vector<Task> tasks;

// Load tasks from file
void loadTasks() {
    ifstream file("tasks.txt");
    if (!file) return;

    Task task;
    string status;
    while (getline(file, task.title) && getline(file, status)) {
        task.completed = (status == "1");
        tasks.push_back(task);
    }
    file.close();
}

// Save tasks to file
void saveTasks() {
    ofstream file("tasks.txt");
    for (const auto& task : tasks) {
        file << task.title << endl;
        file << (task.completed ? "1" : "0") << endl;
    }
    file.close();
}

// Add a new task
void addTask() {
    Task task;
    cout << "Enter task title: ";
    cin.ignore();
    getline(cin, task.title);
    task.completed = false;
    tasks.push_back(task);
    cout << "Task added.\n";
}

// Mark a task as completed
void markCompleted() {
    int index;
    cout << "Enter task number to mark completed: ";
    cin >> index;
    if (index > 0 && index <= tasks.size()) {
        tasks[index - 1].completed = true;
        cout << "Task marked as completed.\n";
    } else {
        cout << "Invalid task number.\n";
    }
}

// View all tasks
void viewTasks() {
    cout << "\nTo-Do List:\n";
    for (size_t i = 0; i < tasks.size(); ++i) {
        cout << i + 1 << ". [" << (tasks[i].completed ? "✔" : " ") << "] " << tasks[i].title << endl;
    }
}

int main() {
    loadTasks();
    int choice;
    do {
        cout << "\n==== To-Do List Menu ====\n";
        cout << "1. View Tasks\n";
        cout << "2. Add Task\n";
        cout << "3. Mark Task as Completed\n";
        cout << "4. Exit\n";
        cout << "Choose an option: ";
        cin >> choice;

        switch (choice) {
            case 1: viewTasks(); break;
            case 2: addTask(); break;
            case 3: markCompleted(); break;
            case 4: saveTasks(); cout << "Tasks saved. Exiting...\n"; break;
            default: cout << "Invalid option. Try again.\n";
        }
    } while (choice != 4);

    return 0;
}
