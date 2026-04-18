# Automated-Household-Inventory-Intelligent-Consumption-Analytics-System-HI-CAS-
'''The Automated Household Inventory &; Intelligent Consumption Analytics System (HI-CAS) uses IoT and AI to track items, analyze usage patterns, predict needs, reduce waste, and automate household inventory management efficiently.'''


class Item:
    def __init__(self, item_id, name, quantity, threshold, price):
        self.item_id = item_id
        self.name = name
        self.quantity = quantity
        self.threshold = threshold
        self.price = price
        self.total_used = 0
        self.days_tracked = 0


inventory = {}


def add_item():
    item_id = int(input("Enter Item ID: "))
    name = input("Enter Item Name: ")
    quantity = int(input("Enter Quantity: "))
    threshold = int(input("Enter Threshold: "))
    price = float(input("Enter Price: "))

    inventory[item_id] = Item(item_id, name, quantity, threshold, price)
    print("Item added successfully")


def view_items():
    if not inventory:
        print("No items found")
        return

    for item in inventory.values():
        print("\nID:", item.item_id)
        print("Name:", item.name)
        print("Quantity:", item.quantity)
        print("Threshold:", item.threshold)
        print("Price:", item.price)


def consume_item():
    item_id = int(input("Enter Item ID: "))

    if item_id not in inventory:
        print("Item not found")
        return

    qty = int(input("Enter quantity used: "))
    item = inventory[item_id]

    if qty > item.quantity:
        print("Not enough stock")
        return

    item.quantity -= qty
    item.total_used += qty
    item.days_tracked += 1

    print("Consumption recorded")

    if item.quantity <= item.threshold:
        print("⚠ Low Stock Alert!")


def analytics():
    item_id = int(input("Enter Item ID: "))

    if item_id not in inventory:
        print("Item not found")
        return

    item = inventory[item_id]

    if item.days_tracked == 0:
        print("No usage data available")
        return

    avg = item.total_used / item.days_tracked
    future_days = int(input("Enter future days to predict: "))
    predicted = avg * future_days

    print("Average Daily Usage:", round(avg, 2))
    print("Predicted Requirement:", round(predicted, 2))


def main():
    while True:
        print("\n--- HI-CAS MENU ---")
        print("1. Add Item")
        print("2. View Items")
        print("3. Record Consumption")
        print("4. Analytics")
        print("5. Exit")

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
            print("Exiting...")
            break
        else:
            print("Invalid choice")


if __name__ == "__main__":
    main()
