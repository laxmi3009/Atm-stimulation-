# Atm-stimulation-

"""
ATM Simulation
A simple console-based ATM system with PIN auth, balance check,
deposit, withdrawal, and transaction history.
"""

import datetime

class ATM:
    def __init__(self, balance=10000.0, pin="1234"):
        self.balance = balance
        self.pin = pin
        self.history = []
        self.currency = "\u20b9"  # Indian Rupee symbol

    def _log(self, action, amount=None):
        timestamp = datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")
        entry = f"[{timestamp}] {action}"
        if amount is not None:
            entry += f": {self.currency}{amount:.2f}"
        self.history.append(entry)

    def authenticate(self):
        for attempt in range(3):
            entered = input("Enter your 4-digit PIN: ").strip()
            if entered == self.pin:
                print("Authentication successful.\n")
                return True
            print(f"Incorrect PIN. {2 - attempt} attempt(s) left.")
        print("Too many failed attempts. Card blocked.")
        return False

    def check_balance(self):
        print(f"Current balance: {self.currency}{self.balance:.2f}")
        self._log("Balance inquiry")

    def deposit(self):
        try:
            amount = float(input(f"Enter deposit amount: {self.currency}"))
            if amount <= 0:
                print("Amount must be positive.")
                return
            self.balance += amount
            self._log("Deposit", amount)
            print(f"Deposited {self.currency}{amount:.2f}. New balance: {self.currency}{self.balance:.2f}")
        except ValueError:
            print("Invalid amount.")

    def withdraw(self):
        try:
            amount = float(input(f"Enter withdrawal amount: {self.currency}"))
            if amount <= 0:
                print("Amount must be positive.")
                return
            if amount > self.balance:
                print("Insufficient funds.")
                return
            if amount % 100 != 0:
                print(f"Please enter an amount in multiples of {self.currency}100.")
                return
            self.balance -= amount
            self._log("Withdrawal", amount)
            print(f"Withdrew {self.currency}{amount:.2f}. New balance: {self.currency}{self.balance:.2f}")
        except ValueError:
            print("Invalid amount.")

    def change_pin(self):
        old = input("Enter current PIN: ").strip()
        if old != self.pin:
            print("Incorrect PIN.")
            return
        new = input("Enter new 4-digit PIN: ").strip()
        if len(new) == 4 and new.isdigit():
            self.pin = new
            self._log("PIN changed")
            print("PIN updated successfully.")
        else:
            print("PIN must be exactly 4 digits.")

    def print_history(self):
        if not self.history:
            print("No transactions yet.")
            return
        print("\n--- Transaction History ---")
        for entry in self.history:
            print(entry)
        print("----------------------------")

    def run(self):
        print("=== Welcome to Python ATM ===\n")
        if not self.authenticate():
            return

        menu = """
1. Check Balance
2. Deposit
3. Withdraw
4. Change PIN
5. Transaction History
6. Exit
"""
        while True:
            print(menu)
            choice = input("Select an option (1-6): ").strip()

            if choice == "1":
                self.check_balance()
            elif choice == "2":
                self.deposit()
            elif choice == "3":
                self.withdraw()
            elif choice == "4":
                self.change_pin()
            elif choice == "5":
                self.print_history()
            elif choice == "6":
                print("Thank you for using Python ATM. Goodbye!")
                break
            else:
                print("Invalid option. Try again.")


if __name__ == "__main__":
    atm = ATM(balance=10000.0, pin="1234")
    atm.run()



