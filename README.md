Recursive-Descent-Parser
========================

The following is a recursive descent parser written in C. The parser utilizes lookahead sets in order to function in a deterministic manner. The lookahead set enables the machine to traverse the inputted string and establish whether or not that string is in the given language

#include <iostream>
#include <cstdlib>
#include <cctype>
 
using namespace std;

void match(char lookahead, char& curr);
void error();
void Expr(char& curr);
void F(char& curr);
void G(char& curr);
void Digit(char& curr);
void Term(char& curr);
void H(char& curr);
void Factor(char& curr);
void AddOp(char& curr);
void MulOp(char& curr);
void Number(char& curr);
void I(char& curr);

int main(void)
{
     char curr;
     cout << "Enter a string for parsing: ";
     cin.get(curr);
     Expr(curr);
     if (curr == '\n')
         cout << "String is in the language." << endl;
     else
         error();
}

void match(char lookahead, char& curr)
{
    if (curr == lookahead)
        cin.get(curr);
    else error();
}
 
void error()
{
     cout << "syntax error" << endl;
     exit(1);
}
 
void F(char& curr)
{
    if (curr == '+' || curr == '-'){
         AddOp(curr);
    }
}
 
void G(char& curr)
{
     if (curr == '+' || curr == '-')
     {
         AddOp(curr);
         Term(curr);
         G(curr);
     }
}
 
void Expr(char& curr)
{
     F(curr);
     Term(curr);
     G(curr);
}
 
void Digit(char& curr)
{
     if(isdigit(curr))
     {
         match(curr, curr);
     }
     else
         error();
}
 
void Term(char& curr)
{
     Factor(curr);
     H(curr);
}
  
void AddOp(char& curr)
{
     if (curr == '+')
     {
         match('+', curr);
     }
     else if (curr == '-')
     {
         match('-', curr);
     }
     else
         error();
}
 
void MulOp(char& curr)
{
     if (curr == '*')
     {
         match('*', curr);
     }
     else if (curr == '/')
     {
         match('/', curr);
     }
     else
         error();
}
 
void Number(char& curr)
{
     Digit(curr);
     I(curr);
}
 
void I(char& curr)
{
     if(isdigit(curr))
     {
         Digit(curr);
         I(curr);
     }
 
 
 
}
 
void Factor(char& curr)
{
     if (isdigit(curr))
     {
         Number(curr);
     }
     else if (curr == '(')
     {
        match('(', curr);
        Expr(curr);
        match(')', curr);
     }
     else
         error();
}
 
void H(char& curr)
{
    if (curr == '*' || curr == '/')
    {
         MulOp(curr);
         Factor(curr);
         H(curr);
    }
}
