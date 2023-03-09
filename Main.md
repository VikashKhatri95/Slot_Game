# Slot_Game
import random
MAX_LINES=3#CONSTANT VALUE
MAX_BET=100
MIN_BET=10

ROWS=3
COLS=3

symbol_count={
    "A":2,
    "B":4,
    "C":6,
    "D":8
}

def get_spin(rows,cols,symbols):
    all_symbols=[]
    for symbol,symbol_count in symbols.items():
        for i in range(symbol_count):
            all_symbols.append(symbol)
    columns=[]
    for col in range(cols):
        column=[]
        current_symbols=all_symbols[:]
        for i in range(rows):
            value=random.choice(current_symbols)
            current_symbols.remove(value)
            column.append(value)
        columns.append(column)
    return columns

def print_slot_machine(columns):
    for row in range(len(columns[0])):
        for i,column in enumerate(columns):
            if i!=len(columns)-1:
                print(column[row],end=" | ")
            else:
                print(column[row],end=" ")
        print("\r")

symbol_value={
    "A":5,
    "B":4,
    "C":3,
    "D":2
}        
def check_winning(columns,lines,bet,values):
    winnings=0
    winning_lines=[]
    for line in range(lines):
        symbol=columns[0][line]
        for column in columns:
            symbol_to_check=column[line]
            if symbol != symbol_to_check:
                break
        else:
            winnings+=values[symbol]*bet
            winning_lines.append(lines+1)
    return winnings,winning_lines
    

def deposit():
    while True:
        amount=input("What would you like to deposite? ")
        if amount.isdigit():
            amount=int(amount)
            if amount>0:
                break
            else:
                print("Amount must be Greater than ZERO!!!")
        else:
            print("Amount should be in form of digits")
    return amount

def get_lines():
    while True:
        lines=input("Enter the number of lines you want to bet on?(1-3) ")
        if lines.isdigit():
            lines=int(lines)
            if MAX_LINES>=lines>=1:
                break
            else:
                print("Enter Valid Number of lines!!!")
        else:
            print("Lines should be in form of digits")
    return lines
    
def get_bet():
    while True:
        bet_amount=input("What amount would you like to Bet on each line? ")
        if bet_amount.isdigit():
            bet_amount=int(bet_amount)
            if MAX_BET>=bet_amount>=MIN_BET:
                break
            else:
                print(f'Amount must be beetween {MAX_BET} and {MIN_BET} !!!')
        else:
            print("Bet must be in digits!!!")
    return bet_amount    
def spin(balance):
    lines=get_lines()
    while True:
        bet=get_bet()
        total_bet=bet*lines
        if total_bet>balance:
            print(f'Insuffucent Balance to Bet!!!. Your current balance is: {balance}')
        else:
            break
    
    print(f'You bet {bet} on {lines} lines')
    print(f'Your total bet is {total_bet} ')
    slots=get_spin(ROWS,COLS,symbol_count)
    print_slot_machine(slots)
    winning,winning_lines=check_winning(slots,lines,bet,symbol_value)
    print(f'Congratulations!!!! You have won {winning}.')
    print(f'You won on lines:',*winning_lines)
    return winning-total_bet
def main():
    balance=deposit()
    while True:
        print(f'Current Balance is: {balance}')
        ans=input("Press enter to play.(q to quit)")
        if ans =='q':
            break
        balance+=spin(balance)
    print(f'You left with Balance: {balance}')
main()
