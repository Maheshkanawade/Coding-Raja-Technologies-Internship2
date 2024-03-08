# Coding-Raja-Technologies-Internship2
import json

class BudgetTracker:
    def __init__(self):
        self.transactions = []
        self.categories = set()

    def add_transaction(self, amount, category, transaction_type):
        self.transactions.append({
            'amount': amount,
            'category': category,
            'type': transaction_type
        })
        self.categories.add(category)

    def calculate_budget(self):
        total_income = sum(transaction['amount'] for transaction in self.transactions if transaction['type'] == 'income')
        total_expenses = sum(transaction['amount'] for transaction in self.transactions if transaction['type'] == 'expense')
        remaining_budget = total_income - total_expenses
        return remaining_budget

    def analyze_expenses(self):
        expense_by_category = {}
        for category in self.categories:
            expense_by_category[category] = sum(transaction['amount'] for transaction in self.transactions
                                                if transaction['type'] == 'expense' and transaction['category'] == category)
        return expense_by_category

    def save_transactions(self, filename):
        with open(filename, 'w') as file:
            json.dump(self.transactions, file)

    def load_transactions(self, filename):
        with open(filename, 'r') as file:
            self.transactions = json.load(file)

def main():
    budget_tracker = BudgetTracker()

    # Load transactions from file if available
    try:
        budget_tracker.load_transactions('transactions.json')
    except FileNotFoundError:
        pass

    while True:
        print("\n1. Add Income")
        print("2. Add Expense")
        print("3. Calculate Budget")
        print("4. Analyze Expenses")
        print("5. Save and Exit")

        choice = input("\nEnter your choice: ")

        if choice == '1':
            amount = float(input("Enter income amount: "))
            category = input("Enter income category: ")
            budget_tracker.add_transaction(amount, category, 'income')
        elif choice == '2':
            amount = float(input("Enter expense amount: "))
            category = input("Enter expense category: ")
            budget_tracker.add_transaction(amount, category, 'expense')
        elif choice == '3':
            remaining_budget = budget_tracker.calculate_budget()
            print(f"Remaining budget: {remaining_budget}")
        elif choice == '4':
            expense_by_category = budget_tracker.analyze_expenses()
            print("Expense Analysis:")
            for category, amount in expense_by_category.items():
                print(f"{category}: {amount}")
        elif choice == '5':
            budget_tracker.save_transactions('transactions.json')
            print("Transactions saved. Exiting...")
            break
        else:
            print("Invalid choice. Please try again.")

if __name__ == "__main__":
    main()

