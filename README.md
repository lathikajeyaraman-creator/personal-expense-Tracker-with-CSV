# personal-expense-Tracker-with-CSV

import csv
import os
from datetime import date

FILE_NAME = "expenses.csv"


# Create CSV file if it does not exist
def create_file():
    if not os.path.exists(FILE_NAME):
        with open(FILE_NAME, "w", newline="") as file:
            writer = csv.writer(file)
            writer.writerow(["Date", "Category", "Description", "Amount"])


# Add a new expense
def add_expense():
    expense_date = input("Enter date (YYYY-MM-DD) or press Enter for today: ")

    if expense_date == "":
        expense_date = str(date.today())

    try:
        date.fromisoformat(expense_date)
    except ValueError:
        print("Invalid date!")
        return

    category = input("Enter category: ")
    description = input("Enter description: ")

    try:
        amount = float(input("Enter amount: "))
        if amount <= 0:
            print("Amount must be greater than zero!")
            return
    except ValueError:
        print("Please enter a valid amount!")
        return

    with open(FILE_NAME, "a", newline="") as file:
        writer = csv.writer(file)
        writer.writerow([expense_date, category, description, amount])

    print("Expense added successfully!")


# View all expenses
def view_expenses():
    with open(FILE_NAME, "r") as file:
        reader = csv.reader(file)
        rows = list(reader)

    if len(rows) <= 1:
        print("No expenses recorded yet.")
        return

    print("\n----- EXPENSE DETAILS -----")

    for row in rows[1:]:
        print(row)


# Calculate total expenses
def total_expenses():
    total = 0

    with open(FILE_NAME, "r") as file:
        reader = csv.DictReader(file)

        for row in reader:
            total += float(row["Amount"])

    print("Total Expenses: Rs.", total)


# Main menu
def main():
    create_file()

    while True:
        print("\n===== PERSONAL EXPENSE TRACKER =====")
        print("1. Add Expense")
        print("2. View Expenses")
        print("3. Total Expenses")
        print("4. Exit")

        choice = input("Enter your choice: ")

        if choice == "1":
            add_expense()

        elif choice == "2":
            view_expenses()

        elif choice == "3":
            total_expenses()

        elif choice == "4":
            print("Thank you for using Expense Tracker!")
            break

        else:
            print("Invalid choice! Try again.")


if __name__ == "__main__":
    main()
