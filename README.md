"""
HI-CAS: Automated Household Inventory & Consumption Analytics System

This program helps track household inventory, monitor usage,
and predict future consumption using simple analytics.
"""

class Item:
    """Represents a household inventory item."""

    def __init__(self, item_id: int, name: str, quantity: int, threshold: int, price: float):
        self.item_id = item_id
        self.name = name
        self.quantity = quantity
        self.threshold = threshold
        self.price = price
        self.total_used = 0
        self.days_tracked = 0

    def __str__(self):
        return (
            f"ID: {self.item_id}\n"
            f"Name: {self.name}\n"
            f"Quantity: {self.quantity}\n"
            f"Threshold: {self.threshold}\n"
            f"Price: ${self.price:.2f}"
        )


# Global inventory storage
inventory = {}


def add_item():
    """Add a new item to inventory."""
    try:
        item_id = int(input("Enter Item ID: "))
        if item_id in inventory:
            print("⚠ Item ID already exists!")
            return

        name = input("Enter Item Name: ")
        quantity = int(input("Enter Quantity: "))
        threshold = int(input("Enter Threshold: "))
        price = float(input("Enter Price: "))

        inventory[item_id] = Item(item_id, name, quantity, threshold, price)
        print("✅ Item added successfully")

    except ValueError:
        print("❌ Invalid input. Please enter correct data types.")


def view_items():
    """Display all inventory items."""
    if not inventory:
        print("📭 No items found")
        return

    print("\n--- INVENTORY LIST ---")
    for item in inventory.values():
        print(item)
        print("-" * 25)


def consume_item():
    """Record item consumption."""
    try:
        item_id = int(input("Enter Item ID: "))

        if item_id not in inventory:
            print("❌ Item not found")
            return

        qty = int(input("Enter quantity used: "))
        item = inventory[item_id]

        if qty > item.quantity:
            print("❌ Not enough stock")
            return

        item.quantity -= qty
        item.total_used += qty
        item.days_tracked += 1

        print("✅ Consumption recorded")

        if item.quantity <= item.threshold:
            print("⚠ LOW STOCK ALERT!")

    except ValueError:
        print("❌ Invalid input")


def analytics():
    """Analyze usage and predict future requirements."""
    try:
        item_id = int(input("Enter Item ID: "))

        if item_id not in inventory:
            print("❌ Item not found")
            return

        item = inventory[item_id]

        if item.days_tracked == 0:
            print("📊 No usage data available")
            return

        avg_usage = item.total_used / item.days_tracked
        future_days = int(input("Enter future days to predict: "))
        predicted = avg_usage * future_days

        print("\n--- ANALYTICS REPORT ---")
        print(f"Average Daily Usage: {avg_usage:.2f}")
        print(f"Predicted Requirement: {predicted:.2f}")

    except ValueError:
        print("❌ Invalid input")


def main():
    """Main menu loop."""
    while True:
        print("\n========== HI-CAS MENU ==========")
        print("1. Add Item")
        print("2. View Items")
        print("3. Record Consumption")
        print("4. Analytics")
        print("5. Exit")
        print("================================")

        choice = input("Enter choice: ")

        if choice == '1':
            add_item()
        elif choice == '2':
            view_items()
        elif choice == '3':
            consume_item()
        elif choice == '4':
            analytics()
        elif choice == '5':
            print("👋 Exiting system...")
            break
        else:
            print("❌ Invalid choice. Try again.")


if __name__ == "__main__":
    main()
