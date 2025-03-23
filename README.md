import json
import datetime

# File to store fitness data
data_file = 'fitness_data.json'

# Load data or initialize if file not found
def load_data():
    try:
        with open(data_file, 'r') as file:
            return json.load(file)
    except FileNotFoundError:
        return {}

# Save data to file
def save_data(data):
    with open(data_file, 'w') as file:
        json.dump(data, file, indent=4)

# Add workout entry
def add_workout_entry(user, workout_type, duration, calories):
    data = load_data()
    if user not in data:
        data[user] = []

    entry = {
        "date": str(datetime.date.today()),
        "workout_type": workout_type,
        "duration_minutes": duration,
        "calories_burned": calories
    }

    data[user].append(entry)
    save_data(data)
    print("Workout entry added successfully!")

# View all entries for a user
def view_entries(user):
    data = load_data()
    if user in data:
        for entry in data[user]:
            print(entry)
    else:
        print("No records found for user.")

# Calculate total calories burned by a user
def total_calories(user):
    data = load_data()
    if user in data:
        total = sum(entry['calories_burned'] for entry in data[user])
        print(f"Total calories burned by {user}: {total}")
    else:
        print("No records found.")

# Main menu
def main():
    while True:
        print("\n--- Personal Fitness Tracker ---")
        print("1. Add Workout Entry")
        print("2. View Workout Entries")
        print("3. View Total Calories Burned")
        print("4. Exit")

        choice = input("Enter your choice (1-4): ")

        if choice == '1':
            user = input("Enter your name: ")
            workout_type = input("Enter workout type (e.g., Running, Yoga): ")
            duration = int(input("Enter duration in minutes: "))
            calories = int(input("Enter calories burned: "))
            add_workout_entry(user, workout_type, duration, calories)

        elif choice == '2':
            user = input("Enter your name to view entries: ")
            view_entries(user)

        elif choice == '3':
            user = input("Enter your name: ")
            total_calories(user)

        elif choice == '4':
            print("Exiting Fitness Tracker. Stay healthy!")
            break

        else:
            print("Invalid choice. Please try again.")

if __name__ == "__main__":
    main()
