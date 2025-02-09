# Python-Programming-
import mysql.connector

# Establish database connection
mydb = mysql.connector.connect(host='localhost', user='root', password='12345')
mycursor = mydb.cursor()

# Create Database
mycursor.execute("CREATE DATABASE IF NOT EXISTS ATM_MACHINE")
mycursor.execute("USE ATM_MACHINE")

# Create Table
mn = """
CREATE TABLE IF NOT EXISTS RECORDS (
    ACCOUNT_NO INT PRIMARY KEY,
    PASSWORD INT NOT NULL,
    NAME VARCHAR(100),
    CR_AMT INT DEFAULT 0,
    WITHDRAWL INT DEFAULT 0,
    BALANCE INT DEFAULT 1000
)
"""
mycursor.execute(mn)

conn = mysql.connector.connect(host='localhost', user='root', password='12345', database='ATM_MACHINE')
c1 = conn.cursor()

while True:
    print("==============================================================")
    print("               WELCOME TO OUR ATM  ")
    print("==============================================================")
    print("1. To Create Account")
    print("2. To Login")
    print("3. Exit")
    print("==============================================================")
    
    op = input("Enter your choice : ")
    print("==============================================================")
    
    if op == "1":
        while True:
            m = int(input("Enter a 4-digit number as account number: "))
            cb = "SELECT * FROM RECORDS WHERE ACCOUNT_NO = %s"
            c1.execute(cb, (m,))
            d = c1.fetchall()
            
            if d:
                print("Account number already exists.")
                y = input("Do you want to continue? (y/n): ")
                if y.lower() != 'y':
                    print("Thank you. Visit again!")
                    print("Please close this file before exiting.")
                    exit()
            else:
                name = input("Enter your name: ")
                passw = int(input("Enter your password: "))
                ab = "INSERT INTO RECORDS (ACCOUNT_NO, PASSWORD, NAME) VALUES (%s, %s, %s)"
                c1.execute(ab, (m, passw, name))
                conn.commit()
                print("Account successfully created. Minimum balance is ₹1000.")
            
            y = input("Do you want to continue? (y/n): ")
            if y.lower() != 'y':
                break
        continue
    
    elif op == "2":
        while True:
            acct = int(input("Enter your account number: "))
            cb = "SELECT ACCOUNT_NO, PASSWORD FROM RECORDS WHERE ACCOUNT_NO = %s"
            c1.execute(cb, (acct,))
            d = c1.fetchone()
            
            if d:
                pas = int(input("Enter your password: "))
                if pas == d[1]:
                    while True:
                        print("Successfully Logged In")
                        print("1. Deposit Money")
                        print("2. Withdraw Money")
                        print("3. Transfer Money")
                        print("4. Check Balance")
                        print("5. Exit")
                        
                        r = input("Enter your choice: ")
                        
                        if r == "1":
                            amt = int(input("Enter amount to deposit: "))
                            sr = "UPDATE RECORDS SET CR_AMT = CR_AMT + %s, BALANCE = BALANCE + %s WHERE ACCOUNT_NO = %s"
                            c1.execute(sr, (amt, amt, acct))
                            conn.commit()
                            print("Successfully deposited.")
                        
                        elif r == "2":
                            amt = int(input("Enter amount to withdraw: "))
                            ah = "SELECT BALANCE FROM RECORDS WHERE ACCOUNT_NO = %s"
                            c1.execute(ah, (acct,))
                            bal = c1.fetchone()[0]
                            
                            if amt > bal:
                                print("Insufficient balance.")
                            elif amt > 20000:
                                print("Daily withdrawal limit exceeded. Maximum withdrawal is ₹20,000.")
                            else:
                                sr = "UPDATE RECORDS SET WITHDRAWL = WITHDRAWL + %s, BALANCE = BALANCE - %s WHERE ACCOUNT_NO = %s"
                                c1.execute(sr, (amt, amt, acct))
                                conn.commit()
                                print("Withdrawal successful.")
                        
                        elif r == "3":
                            act = int(input("Enter the account number to transfer to: "))
                            if act == acct:
                                print("You cannot transfer money to your own account.")
                            else:
                                cb = "SELECT * FROM RECORDS WHERE ACCOUNT_NO = %s"
                                c1.execute(cb, (act,))
                                d = c1.fetchone()
                                
                                if d:
                                    amt = int(input("Enter amount to transfer: "))
                                    ah = "SELECT BALANCE FROM RECORDS WHERE ACCOUNT_NO = %s"
                                    c1.execute(ah, (acct,))
                                    bal = c1.fetchone()[0]
                                    
                                    if amt > bal:
                                        print("Insufficient balance.")
                                    else:
                                        av = "UPDATE RECORDS SET BALANCE = BALANCE - %s WHERE ACCOUNT_NO = %s"
                                        cv = "UPDATE RECORDS SET BALANCE = BALANCE + %s WHERE ACCOUNT_NO = %s"
                                        c1.execute(av, (amt, acct))
                                        c1.execute(cv, (amt, act))
                                        conn.commit()
                                        print("Transfer successful.")
                                else:
                                    print("Account does not exist.")
                        
                        elif r == "4":
                            ma = "SELECT BALANCE FROM RECORDS WHERE ACCOUNT_NO = %s"
                            c1.execute(ma, (acct,))
                            bal = c1.fetchone()[0]
                            print("Balance: ₹", bal)
                        
                        elif r == "5":
                            print("Thank you. Visit again!")
                            print("Please close this file before exiting.")
                            exit()
                        else:
                            print("Invalid selection. Please try again.")
                        
                        y = input("Do you want to continue? (y/n): ")
                        if y.lower() != 'y':
                            break
                else:
                    print("Wrong password.")
            else:
                print("Account does not exist.")
    
    elif op == "3":
        print("Exiting. Please close this file before exiting.")
        c1.close()
        conn.close()
        print("Thank you. Visit again!")
        exit()
    else:
        print("Invalid selection. Please try again.")
