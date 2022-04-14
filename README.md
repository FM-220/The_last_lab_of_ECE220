////
//// Created by chaari
////

#define max_accounts_number 100
#include <iostream>
#include <string.h>
using namespace std;

#define MAX_NAME_LEN 40

class BankAccount {
protected:
    char name[MAX_NAME_LEN];
    int  account_number;
    double balance;
public:
    BankAccount ();
    BankAccount (char *);
    BankAccount (char *, int, double);
    void printSummary();
    double withdraw(double);
    void deposit(double);
    double getBalance();
    void setName(char *);
    void setAccount_number(int);
    void setbalance (double);
};


class saveBankAccount : public BankAccount{ //inheritance of BankAccount
protected:
    double interestRate;
    int noWithDraws;
    int MAXnoWithDraws;

public:
    saveBankAccount (char *, int, double, double, int);
    void printSummary();
    double withdraw(double);
    void callInterest();
    void resetWithdraws ();
    void transfer(saveBankAccount &acc,double);

};

int main() {
    char name_of_Tom[] = "Tom";
    char name_of_eric[] = "eric";
    int tom_account_number = 234567;
    int eric_account_number = 23454;
    int tom_balance = 2000;
    int eric_balance = 5000;
    double tom_interestRate = 0.05;
    double eric_interestRate = 0.04;
    int MaxnoWithDraws = 1;
    saveBankAccount tomAcc(name_of_Tom,tom_account_number,tom_balance,tom_interestRate,MaxnoWithDraws);
    //register a bank account for our customer Tom
    tomAcc.printSummary();
    tomAcc.deposit(1000);//Tom deposits 1000
    tomAcc.printSummary();
    tomAcc.withdraw(500);//Tom withdraws 500
    tomAcc.printSummary();
    tomAcc.withdraw(500);//Tom withdraws 500
    tomAcc.printSummary();
    tomAcc.getBalance();//Tom wants to see his balance.
    tomAcc.printSummary();
    tomAcc.resetWithdraws();//reset the number of withdraws to make it possible for more withdraws
    tomAcc.printSummary();
    tomAcc.withdraw(500);//Tom withdraws 500
    tomAcc.printSummary();
    tomAcc.callInterest();//Tom wants to see his balance with interests.
    tomAcc.printSummary();
    saveBankAccount ericAcc(name_of_eric,eric_account_number,eric_balance,eric_interestRate,MaxnoWithDraws);
    //register a bank account for our customer Eric
    ericAcc.printSummary();
    ericAcc.transfer(tomAcc,1000);//Tom transfer 1000 to Eric.
    ericAcc.printSummary();
    tomAcc.printSummary();
// add code here
    return 0;
}

//**********************************************
// class BankAccount: methods
//**********************************************
BankAccount :: BankAccount () {
    name[0] = 0;
    account_number = 0;
    balance = 0.0;
}

BankAccount :: BankAccount (char *n) {
    strcpy(name, n);
    account_number = 0;
    balance = 0.0;
}

BankAccount :: BankAccount (char *n, int a_no, double bal) {
    strcpy(name, n);
    account_number = a_no;
    balance = bal;
}

void BankAccount :: printSummary() {
    cout << "---------------------" << endl;
    cout << "Name: " << name << endl;
    cout << "Account Number: " << account_number << endl;
    cout << "Balance: " << balance << endl;
    cout << "---------------------" << endl;
}

double BankAccount :: withdraw(double toWD) {
    if (balance-toWD > 0) {
        balance -= toWD;
    }
    else {
        cout << "!!!Sorry, not enough money!!!" << endl;
        return 0;
    }
    return toWD;
}

void BankAccount :: deposit(double toDep) {
    balance += toDep;
}

double BankAccount :: getBalance() {
    return balance;
}


void   BankAccount ::   setName(char *n) {
    strcpy(name, n);
}

void BankAccount ::setAccount_number(int a_no) {
    account_number = a_no;
}

void BankAccount ::setbalance(double bal) {
    balance = bal;
}

//**********************************************
// class saveBankAccount: methods
//**********************************************

//add your code here
saveBankAccount::saveBankAccount(char* user_name, int user_account_number, double user_balance, double user_interestRate, int user_MaxnoWithDraws)
{
    strcpy(name,user_name);
    account_number = user_account_number;
    balance = user_balance;
    interestRate = user_interestRate;
    MAXnoWithDraws = user_MaxnoWithDraws;
    noWithDraws = 0;
}//what we do here is simply assign all the values from input to the saveBankAccount variable

void saveBankAccount::printSummary() {
    cout << "The name of the user is " << name <<endl;
    cout << "The account number of the user is " << account_number<<endl;
    cout << "The balance of the user is " << balance << endl;
    cout << "The interest rate is " << interestRate << endl;
    cout << "The number of withdraws is " << noWithDraws << endl;
    cout << "The maximum number of withdraw is " << MAXnoWithDraws << endl;
    cout<<"\n"<<endl;
}//we show all the details of the user

void saveBankAccount::callInterest() {
    double interest = interestRate * balance;
    balance += interest;
    cout << "The new balance is " << balance << endl;
    cout << "\n" <<endl;
}// the balance is the interest + original balance, so here we multiply the interest rate and then find the account balance

void saveBankAccount::resetWithdraws() {
    noWithDraws = 0;//simply set the number of withdraws to 0 to make the user be able to  withdraw money
}

void saveBankAccount::transfer(saveBankAccount &acc, double amount) {
    double withdraw_amount;//transfer is to withdraw money from one account and then deposit it to another account
    withdraw_amount = withdraw(amount);//in this step, the money is withdrawn from one account by the withdraw function
    if (withdraw_amount == amount){//in this step we deposit the money to another account
        acc.balance += amount;
    }
}

double saveBankAccount::withdraw(double amount) {
    if (noWithDraws == MAXnoWithDraws){
        cout<<"The number of withdraws has already reach the maximum\n"<<endl;
//at this moment, the user can't withdraw one more time, because they have use up the chances
    }else if ((balance - amount) < 0){//if all the money has been withdrawn, the user can't withdraw.
        cout<<"Sorry, You don't have enough money"<<endl;
    }else{
        noWithDraws++;//count for the number of withdraws
        balance -=  amount; // after withdrawing, the balance will decrease the money been withdrawn
        cout<<"The new balance is "<< balance <<endl;
        cout<<"\n"<<endl;
        return amount;
    }
}
