import json
import os

# File to store the tasks
FILENAME = "todo_data.json"

# Load existing tasks from the file
def load_tasks():
    if os.path.exists(FILENAME):
        with open(FILENAME, "r") as file:
            return json.load(file)
    return []

# Save tasks to the file
def save_tasks(tasks):
    with open(FILENAME, "w") as file:
        json.dump(tasks, file, indent=4)

# Display all tasks
def view_tasks(tasks):
    if not tasks:
        print("\n✅ No tasks found!")
    else:
        print("\n📋 Your To-Do List:")
        for i, task in enumerate(tasks, 1):
            status = "✅ Done" if task["done"] else "❌ Not Done"
            print(f"{i}. {task['title']} [{status}]")

# Add a new task
def add_task(tasks):
    title = input("📝 Enter task title: ").strip()
    if title:
        tasks.append({"title": title, "done": False})
        save_tasks(tasks)
        print("✅ Task added successfully!")
    else:
        print("⚠️ Task title cannot be empty!")

# Mark task as done
def mark_done(tasks):
    view_tasks(tasks)
    try:
        num = int(input("\n🔢 Enter task number to mark as done: "))
        if 1 <= num <= len(tasks):
            tasks[num - 1]["done"] = True
            save_tasks(tasks)
            print("✅ Task marked as done!")
        else:
            print("❌ Invalid task number.")
    except ValueError:
        print("⚠️ Enter a valid number.")

# Delete a task
def delete_task(tasks):
    view_tasks(tasks)
    try:
        num = int(input("\n🗑️ Enter task number to delete: "))
        if 1 <= num <= len(tasks):
            deleted = tasks.pop(num - 1)
            save_tasks(tasks)
            print(f"✅ Deleted task: {deleted['title']}")
        else:
            print("❌ Invalid task number.")
    except ValueError:
        print("⚠️ Enter a valid number.")

# Main menu
def main():
    tasks = load_tasks()
    
    while True:
        print("\n==== TO-DO LIST MENU ====")
        print("1. View Tasks")
        print("2. Add Task")
        print("3. Mark Task as Done")
        print("4. Delete Task")
        print("5. Exit")

        choice = input("👉 Choose an option (1-5): ")

        if choice == "1":
            view_tasks(tasks)
        elif choice == "2":
            add_task(tasks)
        elif choice == "3":
            mark_done(tasks)
        elif choice == "4":
            delete_task(tasks)
        elif choice == "5":
            print("👋 Exiting To-Do List. Bye!")
            break
        else:
            print("❌ Invalid choice. Try again.")

if __name__ == "__main__":
    main()
