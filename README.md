# expense-import json
from datetime import datetime

FILE_NAME = "expenses.json"


# Load data from JSON file
def load_expenses():
    try:
        with open(FILE_NAME, "r") as f:
            return json.load(f)
    except:
        return []


# Save data to JSON file
def save_expenses(expenses):
    with open(FILE_NAME, "w") as f:
        json.dump(expenses, f, indent=4)


# Add expense
def add_expense(expenses):
    title = input("Enter expense title: ")
    amount = float(input("Enter amount: "))
    category = input("Enter category (Food/Travel/Shopping/Bills): ")
    date = input("Enter date (YYYY-MM-DD) or press Enter for today: ")

    if date == "":
        date = str(datetime.today().date())

    expense = {
        "title": title,
        "amount": amount,
        "category": category,
        "date": date
    }

    expenses.append(expense)
    save_expenses(expenses)
    print("✅ Expense added successfully!\n")


# View all expenses
def view_expenses(expenses):
    if not expenses:
        print("No expenses found.\n")
        return

    print("\n--- ALL EXPENSES ---")
    for i, exp in enumerate(expenses, start=1):
        print(f"{i}. {exp['date']} | {exp['title']} | {exp['category']} | ₹{exp['amount']}")
    print()


# Filter by category
def filter_by_category(expenses):
    category = input("Enter category to filter: ")
    filtered = [e for e in expenses if e["category"].lower() == category.lower()]

    if not filtered:
        print("No expenses found in this category.\n")
        return

    print(f"\n--- {category.upper()} EXPENSES ---")
    for exp in filtered:
        print(f"{exp['date']} | {exp['title']} | ₹{exp['amount']}")
    print()


# Monthly summary
def monthly_summary(expenses):
    month = input("Enter month (YYYY-MM): ")

    total = 0
    print(f"\n--- SUMMARY FOR {month} ---")

    for exp in expenses:
        if exp["date"].startswith(month):
            total += exp["amount"]
            print(f"{exp['date']} | {exp['title']} | ₹{exp['amount']}")

    print(f"\n Total Spent: ₹{total}\n")


# Main menu
def main():
    expenses = load_expenses()

    while True:
        print("====== EXPENSE TRACKER ======")
        print("1. Add Expense")
        print("2. View Expenses")
        print("3. Filter by Category")
        print("4. Monthly Summary")
        print("5. Exit")

        choice = input("Enter your choice: ")

        if choice == "1":
            add_expense(expenses)
        elif choice == "2":
            view_expenses(expenses)
        elif choice == "3":
            filter_by_category(expenses)
        elif choice == "4":
            monthly_summary(expenses)
        elif choice == "5":
            print("Goodbye ")
            break
        else:
            print("Invalid choice! Try again.\n")


if __name__ == "__main__":
    main()
