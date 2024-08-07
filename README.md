import sqlite3
import getpass

# Connect to the database
conn = sqlite3.connect('exam.db')
cursor = conn.cursor()

# Create tables if they don't exist
cursor.execute('''
    CREATE TABLE IF NOT EXISTS users
    (username TEXT PRIMARY KEY, password TEXT)
''')
cursor.execute('''
    CREATE TABLE IF NOT EXISTS scores
    (username TEXT, score INTEGER)
''')
conn.commit()
def signup():
    username = input("Enter username: ")
    password = getpass.getpass("Enter password: ")
    cursor.execute("INSERT INTO users (username, password) VALUES (?, ?)", (username, password))
    conn.commit()
    print("Signed up successfully!")


def sign_in():
    username = input("Enter username: ")
    password = getpass.getpass("Enter password: ")
    cursor.execute("SELECT * FROM users WHERE username = ? AND password = ?", (username, password))
    user = cursor.fetchone()
    if user:
        print("Signed in successfully!")
        return username
    else:
        print("Invalid credentials!")
        return None

def run_exam(username):
    score = 0
    # Add your exam questions and logic here
    # For example:
    print("What is the capital of France?")
    answer = input("Enter your answer: ")
    if answer.lower() == "paris":
        score += 1
        cursor.execute("INSERT INTO scores (username, score) VALUES (?, ?)", (username, score))
    conn.commit()
    print(f"Your final score is: {score}")

def main():
    while True:
        print("1. Signup")
        print("2. Sign in")
        print("3. Exit")
        choice = input("Enter your choice: ")
        
        if choice == "1":
            signup()
        elif choice == "2":
            username = sign_in()
            if username:
                run_exam(username)
        elif choice == "3":
            break
        else:
            print("Invalid choice!")

if __name__ == "__main__":
    main()

