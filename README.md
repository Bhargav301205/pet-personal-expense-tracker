# pet-personal-expense-tracker
import csv
from datetime import datetime

# Class to represent an expense
class ExpenseEntry:
    def __init__(self, amount, category, description):
        self.amount = amount
        self.category = category
        self.description = description
        self.date = datetime.now().strftime("%Y-%m-%d")
        self.time = datetime.now().strftime("%H:%M:%S")

    def to_list(self):
        return [self.date, self.time, self.amount, self.category, self.description]

# Function to add a new expense
def add_expense():
    try:
        amount = float(input("Enter amount spent (₹): "))
        if amount < 0:
            raise ValueError("Amount cannot be negative.")
        category = input("Enter category (e.g. Food, Travel, Bills): ").capitalize()
        description = input("Enter a short description: ")
        entry = ExpenseEntry(amount, category, description)
        expenses.append(entry)
        print("\n✅ Expense added successfully!")
    except ValueError as e:
        print(f"❌ Error: {e}")

# Function to show category-wise summary
def show_summary():
    if not expenses:
        print("No expenses to summarize.")
        return
    total = 0
    category_summary = {}
    for entry in expenses:
        total += entry.amount
        if entry.category in category_summary:
            category_summary[entry.category] += entry.amount
        else:
            category_summary[entry.category] = entry.amount
    print("\n📊 Expense Summary:")
    print(f"Total Spent: ₹{total:.2f}")
    for cat, amt in category_summary.items():
        print(f"- {cat}: ₹{amt:.2f}")
    max_cat = max(category_summary, key=category_summary.get)
    print(f"\n💡 You spent the most on: {max_cat}!\n")

# Function to save all data to CSV
def save_to_csv():
    if not expenses:
        print("No expenses to save.")
        return
    filename = "expense_log.csv"
    try:
        with open(filename, mode='a', newline='') as file:
            writer = csv.writer(file)
            for entry in expenses:
                writer.writerow(entry.to_list())
        print(f"✅ All entries saved to {filename}\n")
        expenses.clear()
    except Exception as e:
        print(f"❌ Failed to save file: {e}")

# Main program loop
expenses = []
print("🧾 Welcome to Personal Expense Tracker (PET)!")
while True:
    print("\nChoose an option:")
    print("1. Add New Expense")
    print("2. Show Summary")
    print("3. Save & Exit")
    print("4. Exit Without Saving")
    choice = input("Enter your choice (1-4): ")
    if choice == '1':
        add_expense()
    elif choice == '2':
        show_summary()
    elif choice == '3':
        save_to_csv()
        break
    elif choice == '4':
        print("👋 Exiting without saving. Bye!")
        break
    else:
        print("❌ Invalid choice. Try again.")

