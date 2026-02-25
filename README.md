 /*
Author: Alexa De La Cruz
Date: 2/16/2026
Purpose: Composition_Lab
*/

//transaction
#ifndef TRANSACTION_H
#define TRANSACTION_H

#include <string>

struct Transaction {
    std::string type;
    double amount;
    std::string timestamp; // In a real app, use <chrono>, but string works for labs
};

#endif

//the new bankaccount.h
#ifndef BANKACCOUNT_H
#define BANKACCOUNT_H

#include <vector>
#include <string>
#include <iostream>
#include "Transaction.h"

class BankAccount {
protected:
    double balance;
    std::vector<Transaction> transactionHistory;

public:
    BankAccount(double initialBalance) : balance(initialBalance) {}

    void deposit(double amount) {
        balance += amount;
        transactionHistory.push_back({"Deposit", amount, "2026-02-24"}); 
    }

    virtual void withdraw(double amount) {
        if (amount <= balance) {
            balance -= amount;
            transactionHistory.push_back({"Withdrawal", amount, "2026-02-24"});
        } else {
            std::cout << "Insufficient funds!" << std::endl;
        }
    }

    void printHistory() {
        std::cout << "\n--- Transaction History ---" << std::endl;
        for (const auto& t : transactionHistory) {
            std::cout << "[" << t.timestamp << "] " << t.type 
                      << ": $" << t.amount << std::endl;
        }
        std::cout << "Current Balance: $" << balance << "\n" << std::endl;
    }
};

#endif


//the main
#include "BankAccount.h"
#include <iostream>

class CheckingAccount : public BankAccount {
    using BankAccount::BankAccount; 
};

class SavingsAccount : public BankAccount {
    using BankAccount::BankAccount;
};

int main() {
    //Checking Account
    CheckingAccount myChecking(500.0);
    myChecking.deposit(150.0);
    myChecking.withdraw(50.0);
    
    std::cout << "Checking Account History:";
    myChecking.printHistory();

    SavingsAccount mySavings(1000.0);
    mySavings.deposit(500.0);
    mySavings.withdraw(200.0);
    
    std::cout << "Savings Account History:";
    mySavings.printHistory();

    return 0;
}
