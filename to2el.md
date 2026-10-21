# Open Source Tools and Frameworks — Practicals 2 to 11

## Practical 2

```python
# Practical 2 - Branching Programs

first = float(input("Enter first number: "))
second = float(input("Enter second number: "))

if first > second:
    print("Largest number:", first)
elif second > first:
    print("Largest number:", second)
else:
    print("Both numbers are equal")
```

## Practical 3

```python
# Practical 3 - Looping Programs

text = input("Enter a string: ")
count = 0

for character in text:
    count += 1

print("Length of string:", count)
```

## Practical 4

```python
# Practical 4 - Python Functions

def add(first, second):
    return first + second


def subtract(first, second):
    return first - second


def multiply(first, second):
    return first * second


def divide(first, second):
    if second == 0:
        return "Cannot divide by zero"
    return first / second


first = float(input("Enter first number: "))
second = float(input("Enter second number: "))
operator = input("Enter operator (+, -, *, /): ")

if operator == "+":
    print("Result:", add(first, second))
elif operator == "-":
    print("Result:", subtract(first, second))
elif operator == "*":
    print("Result:", multiply(first, second))
elif operator == "/":
    print("Result:", divide(first, second))
else:
    print("Invalid operator")
```

## Practical 5

```python
# Practical 5 - File Handling

class Phone_list:
    def add_number(self, name, number):
        with open("contacts.txt", "a", encoding="utf-8") as file:
            file.write(f"{name},{number}\n")
        print("Contact saved")

    def lookup(self, name):
        try:
            with open("contacts.txt", "r", encoding="utf-8") as file:
                for line in file:
                    saved_name, number = line.strip().split(",", 1)
                    if saved_name.lower() == name.lower():
                        print("Phone number:", number)
                        return
            print("Contact not found")
        except FileNotFoundError:
            print("No contacts saved yet")

    def remove_contact(self, name):
        try:
            with open("contacts.txt", "r", encoding="utf-8") as file:
                contacts = file.readlines()
        except FileNotFoundError:
            print("No contacts saved yet")
            return

        remaining = []
        removed = False

        for line in contacts:
            saved_name = line.strip().split(",", 1)[0]
            if saved_name.lower() == name.lower() and not removed:
                removed = True
            else:
                remaining.append(line)

        with open("contacts.txt", "w", encoding="utf-8") as file:
            file.writelines(remaining)

        print("Contact removed" if removed else "Contact not found")


phone = Phone_list()

while True:
    print("\n1. Add contact")
    print("2. Search contact")
    print("3. Remove contact")
    print("4. Exit")
    choice = input("Enter choice: ")

    if choice == "1":
        name = input("Enter name: ")
        number = input("Enter phone number: ")
        phone.add_number(name, number)
    elif choice == "2":
        name = input("Enter name to search: ")
        phone.lookup(name)
    elif choice == "3":
        name = input("Enter name to remove: ")
        phone.remove_contact(name)
    elif choice == "4":
        break
    else:
        print("Invalid choice")
```

## Practical 6

```python
# Practical 6 - Lists

first_list = [int(value) for value in input("Enter first list values separated by spaces: ").split()]
second_list = [int(value) for value in input("Enter second list values separated by spaces: ").split()]

merged = first_list + second_list
merged.sort()

if merged:
    most_frequent = max(merged, key=merged.count)
    merged = [value for value in merged if value != most_frequent]

print("Final list:", merged)
```

## Practical 7

```python
# Practical 7 - Tuples

tuple_data = (10, 20, 30, 20, 40)

print("Tuple:", tuple_data)
print("First item:", tuple_data[0])
print("Last item:", tuple_data[-1])
print("Slicing:", tuple_data[1:4])
print("Length:", len(tuple_data))
print("Count of 20:", tuple_data.count(20))
print("Index of 30:", tuple_data.index(30))
print("20 is present:", 20 in tuple_data)
print("Concatenation:", tuple_data + (50, 60))
print("Repetition:", tuple_data * 2)
print("Minimum:", min(tuple_data))
print("Maximum:", max(tuple_data))
print("Sum:", sum(tuple_data))

for item in tuple_data:
    print(item)
```

## Practical 8

```python
# Practical 8 - Dictionary

student = {"name": "Ankit", "roll_number": 101}
print("Original dictionary:", student)

student["course"] = "IMCA"
print("After adding:", student)

student["roll_number"] = 102
print("After updating:", student)

print("Name:", student["name"])

student.pop("roll_number", None)
print("After deleting:", student)

for key, value in student.items():
    print(key, ":", value)
```

## Practical 9

```python
# Practical 9 - Exception Handling

import math

try:
    number = float(input("Enter a number: "))
    if number < 0:
        raise Exception("Cannot calculate the square root of a negative number")
    print("Square root:", math.sqrt(number))
except ValueError:
    print("Invalid input. Enter a numeric value")
except Exception as error:
    print("Error:", error)
```

## Practical 10

```python
# Practical 10 - Class and Constructors

class Product:
    def __init__(self, name, product_code, price):
        self.name = name
        self.product_code = product_code
        self.price = price

    def display(self):
        print("Product name:", self.name)
        print("Product code:", self.product_code)
        print("Product price:", self.price)


class TestProduct:
    def test(self):
        product = Product("Laptop", "P101", 50000.0)
        product.display()


if __name__ == "__main__":
    tester = TestProduct()
    tester.test()
```

## Practical 11

```python
# Practical 11 - Class and Objects

class Account:
    def __init__(self, name, initial_balance=0):
        self.name = name
        self.amount = initial_balance

    def deposit(self, amount):
        if amount <= 0:
            raise ValueError("Deposit amount must be positive")
        self.amount += amount

    def withdraw(self, amount):
        if amount <= 0:
            raise ValueError("Withdrawal amount must be positive")
        if amount > self.amount:
            raise ValueError("Insufficient balance")
        self.amount -= amount

    def transfer(self, other_account, amount):
        self.withdraw(amount)
        other_account.deposit(amount)

    def balance(self):
        print(self.name, "balance:", self.amount)


try:
    account1 = Account("Ankit", 5000)
    account2 = Account("Rahul", 2000)

    account1.deposit(1000)
    account1.withdraw(500)
    account1.transfer(account2, 1000)

    account1.balance()
    account2.balance()
except ValueError as error:
    print("Transaction error:", error)
```
