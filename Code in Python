import sys

class Friend:
    """
    Represents a friend participating in expenses.
    """
    def __init__(self, name):
        self.name = name
        self.balance = 0.0

    def __str__(self):
        status = "owes" if self.balance < 0 else "is owed"
        return f"{self.name}: {abs(self.balance):.2f} {status}"

class Expense:
    """
    Represents an expense paid by a friend and shared among participants.
    """
    def __init__(self, payer, amount, description, participants):
        self.payer = payer
        self.amount = amount
        self.description = description
        self.participants = participants

    def split_expense(self):
        split_amount = self.amount / len(self.participants)
        for friend in self.participants:
            if friend != self.payer:
                friend.balance -= split_amount
                self.payer.balance += split_amount

    def __str__(self):
        participants_names = ", ".join(friend.name for friend in self.participants)
        return f"{self.description}: {self.amount:.2f} paid by {self.payer.name}, shared by [{participants_names}]"

class ExpenseSplitter:
    """
    Manages friends, expenses, and calculations of balances and settlements.
    """
    def __init__(self):
        self.friends = []
        self.expenses = []

    def add_friend(self, name):
        if self.find_friend(name):
            print(f"Friend '{name}' already exists. Cannot add duplicate.")
            return False
        self.friends.append(Friend(name))
        print(f"Friend '{name}' added successfully.")
        return True

    def find_friend(self, name):
        for friend in self.friends:
            if friend.name.lower() == name.lower():
                return friend
        return None

    def add_expense(self, payer_name, amount, description, participant_names):
        payer = self.find_friend(payer_name)
        participants = []
        for name in participant_names:
            friend = self.find_friend(name)
            if not friend:
                print(f"Error: Participant '{name}' not found in friends list.")
                return False
            participants.append(friend)

        if not payer:
            print(f"Error: Payer '{payer_name}' not found in friends list.")
            return False
        if amount <= 0:
            print("Error: Amount must be positive.")
            return False
        if payer not in participants:
            print("Error: Payer must be included among the participants.")
            return False

        expense = Expense(payer, amount, description, participants)
        expense.split_expense()
        self.expenses.append(expense)
        print(f"Expense '{description}' of {amount:.2f} added successfully.")
        return True

    def show_friends(self):
        if not self.friends:
            print("No friends added yet.")
            return
        print("\nFriends in the group:")
        for idx, friend in enumerate(self.friends, start=1):
            print(f"  {idx}. {friend.name}")

    def show_expenses(self):
        if not self.expenses:
            print("No expenses added yet.")
            return
        print("\nExpenses:")
        for idx, expense in enumerate(self.expenses, start=1):
            print(f"  {idx}. {expense}")

    def show_balances(self):
        if not self.friends:
            print("No friends to show balances for.")
            return
        print("\nCurrent Balances:")
        print("-" * 40)
        print(f"{'Friend':<20} {'Status':<10} {'Amount':>8}")
        print("-" * 40)
        for friend in self.friends:
            status = "owes" if friend.balance < 0 else "is owed"
            print(f"{friend.name:<20} {status:<10} ${abs(friend.balance):>8.2f}")
        print("-" * 40)

    def settle_debts(self):
        print("\nSettling Debts:")
        creditors = [f for f in self.friends if f.balance > 0]
        debtors = [f for f in self.friends if f.balance < 0]

        if not creditors or not debtors:
            print("No debts to settle. Everyone is even!")
            return

        creditors = sorted(creditors, key=lambda f: f.balance, reverse=True)
        debtors = sorted(debtors, key=lambda f: f.balance)

        transactions = []

        i, j = 0, 0
        while i < len(debtors) and j < len(creditors):
            debtor = debtors[i]
            creditor = creditors[j]
            debt_amount = -debtor.balance
            credit_amount = creditor.balance
            settle_amount = min(debt_amount, credit_amount)

            transactions.append(
                (debtor.name, creditor.name, settle_amount)
            )

            debtor.balance += settle_amount
            creditor.balance -= settle_amount

            # Move to next debtor or creditor if settled
            if abs(debtor.balance) < 0.01:
                i += 1
            if abs(creditor.balance) < 0.01:
                j += 1

        if transactions:
            print(f"{'Debtor':<15} {'Creditor':<15} {'Amount':>10}")
            print("-" * 45)
            for debtor, creditor, amount in transactions:
                print(f"{debtor:<15} {creditor:<15} ${amount:>9.2f}")
            print("-" * 45)
            print("Debts settled. Balances updated.")
        else:
            print("No transactions needed for settling debts.")

def clear_screen():
    """
    Clears the terminal screen for better readability.
    """
    import os
    os.system('cls' if os.name == 'nt' else 'clear')

def input_positive_float(prompt):
    """
    Prompts the user for a positive float until valid input is received.
    """
    while True:
        val = input(prompt).strip()
        try:
            fval = float(val)
            if fval > 0:
                return fval
            else:
                print("Please enter a positive number.")
        except ValueError:
            print("Invalid input. Please enter a numeric value.")

def input_non_empty_string(prompt):
    """
    Prompts for a non-empty string.
    """
    while True:
        val = input(prompt).strip()
        if val:
            return val
        else:
            print("Input cannot be empty. Please try again.")

def input_participant_names(prompt, splitter):
    """
    Prompts user to enter comma-separated participant names, validates them.
    """
    while True:
        names_input = input(prompt).strip()
        if not names_input:
            print("Participant names cannot be empty.")
            continue
        names = [name.strip() for name in names_input.split(',') if name.strip()]
        invalid_names = [name for name in names if not splitter.find_friend(name)]
        if invalid_names:
            print(f"These participants are not found: {', '.join(invalid_names)}")
            print("Please enter valid participant names separated by commas.")
            continue
        return names

def main():
    splitter = ExpenseSplitter()

    clear_screen()
    print("=" * 50)
    print("        Welcome to Expense Splitter 3000")
    print("  Split bills easily and keep track of debts")
    print("=" * 50)

    while True:
        print("\nMain Menu:")
        print("  1. Add Friend")
        print("  2. List Friends")
        print("  3. Add Expense")
        print("  4. List Expenses")
        print("  5. Show Balances")
        print("  6. Settle Debts")
        print("  7. Exit")

        choice = input("Select an option (1-7): ").strip()

        if choice == '1':
            name = input_non_empty_string("Enter friend's name to add: ")
            splitter.add_friend(name)

        elif choice == '2':
            splitter.show_friends()

        elif choice == '3':
            if not splitter.friends:
                print("Add friends before adding expenses.")
                continue
            payer_name = input_non_empty_string("Enter payer's name: ")
            if not splitter.find_friend(payer_name):
                print(f"Payer '{payer_name}' does not exist in friends list.")
                continue
            amount = input_positive_float("Enter amount paid: $")
            description = input_non_empty_string("Enter description of expense: ")
            print("Enter the participants who shared this expense (comma separated):")
            participant_names = input_participant_names("Participants: ", splitter)
            # Ensure payer is among participants
            if payer_name not in [n.lower() for n in participant_names]:
                print("Payer must be one of the participants. Adding payer to participants.")
                participant_names.append(payer_name)
            splitter.add_expense(payer_name, amount, description, participant_names)

        elif choice == '4':
            splitter.show_expenses()

        elif choice == '5':
            splitter.show_balances()

        elif choice == '6':
            splitter.settle_debts()

        elif choice == '7':
            print("Thank you for using Expense Splitter 3000. Goodbye!")
            break

        else:
            print("Invalid selection. Please choose a valid menu option.")

if __name__ == "__main__":
    main()
