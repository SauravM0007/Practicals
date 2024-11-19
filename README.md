Commands to run the file are:-
- flex filename.lex
- gcc lex.yy.c
- ./a.out
  
1. Write a Lexical Analyzer program that identifies any 10 keywords from C language and identifiers following all the naming conventions of the C program.
#include <stdio.h>
#include <string.h>
#define keyword_count 10
char keywords[keyword_count][10] = {
"void", "int", "float", "char", "double", "return", "if", "else", "for", "while"
};
int is_keyword(char* str) {
for (int i = 0; i < keyword_count; i++) {
if (strcmp(str, keywords[i]) == 0) {
return 1;
}
}
return 0;
}
%}
%option noyywrap
%%
[ \t\n]+
￼ 7
CSE-A Saurav Malik 06017702722
"void"|"int"|"float"|"char"|"double"|"return"|"if"|"else"|"for"|"while" {
printf("Keyword: %s\n", yytext);
}
[a-zA-Z_][a-zA-Z0-9_]* {
if (is_keyword(yytext)) {
printf("Keyword: %s\n", yytext);
} else {
printf("Valid Identifier: %s\n", yytext);
}
}
%%
int main() {
printf("Enter a string: ");
yylex();
return 0;
}


2. Write a C program that takes as input string from the text file (let's say input.txt) and identifies and counts the frequency of the keywords appearing in that string
Input File Text:
#include <stdio.h>
int main() {
int a, b;
printf("Enter two numbers: ");
scanf("%d %d", &a, &b);
int sum = a + b;
printf("Sum: %d\n", sum);
return 0;
}
CODE:
#include <iostream>
#include <fstream>
#include <unordered_map>
#include <string>
#include <sstream>
#include <vector>
#include <algorithm>
// List of 32 C keywords
const std::vector<std::string> c_keywords = {
"auto", "break", "case", "char", "const", "continue", "default",
"do", "double",
"else", "enum", "extern", "float", "for", "goto", "if", "int",
"long", "register",
"return", "short", "signed", "sizeof", "static", "struct",
"switch", "typedef",
"union", "unsigned", "void", "volatile", "while"
};
// Function to check if a word is a keyword
bool is_keyword(const std::string& word) {
return std::find(c_keywords.begin(), c_keywords.end(), word) !=
c_keywords.end();
}
int main() {
std::ifstream infile("input.txt");
if (!infile) {
std::cerr << "Error: input.txt file not found!" <<
std::endl;
return 1;
}
std::string line, program_code;
std::unordered_map<std::string, int> keyword_count;
￼ 10
CSE-A Saurav Malik 06017702722
// Read the file and store its contents
while (std::getline(infile, line)) {
program_code += line + "\n";
// Extract words from the line
std::istringstream iss(line);
std::string word;
while (iss >> word) {
// Check if the word is a keyword and update its count
if (is_keyword(word)) {
keyword_count[word]++;
}
}
}
infile.close();
// Output the C program read from the file
std::cout << "Program read from input.txt:\n";
std::cout << program_code << std::endl;
// Output the keywords and their frequencies
std::cout << "Keywords and their frequencies:\n";
for (const auto& pair : keyword_count) {
std::cout << pair.first << ": " << pair.second << std::endl;
}
return 0;
}

3. Write a Syntax Analyzer program using Yacc tool that will have grammar rules for the operators : *,/,%.
Lex Code :
%{
#include <stdio.h>
#include "exp3.tab.h"
%}
%%
[0-9]+ { yylval = atoi(yytext); return NUMBER; }
"+" { return ADD; }
"-" { return SUB; }
"*" { return MUL; }
"/" { return DIV; }
\n { return '\n'; } // Return newline to Bison for
processing
[ \t] { /* ignore whitespace */ }
. { /* ignore any other character */ }
%%
int yywrap() {
return 1;
}
Yacc Code :
Code to include * , / , % operations
%{
#include <stdio.h>
#include <stdlib.h>
void yyerror(const char *s);
int yylex(void);
%}
%token NUMBER
%token ADD SUB MUL DIV
%left ADD SUB
%left MUL DIV
%%
input:
*/ };
expression:
$3); }
$3); }
| input expression '\n' { /* Accept and ignore newline
| expression ADD expression { printf("result = %d\n", $1 +
| expression SUB expression { printf("result = %d\n", $1 -
￼ 14
CSE-A Saurav Malik 06017702722
| expression MUL expression { printf("result = %d\n", $1 *
$3); }
| expression DIV expression {
if ($3 == 0) {
yyerror("Attempted to divide by zero.");
YYERROR;
} else {
printf("result = %d\n", $1 / $3);
}
}
;
| NUMBER { $$ = $1; }
%%
void yyerror(const char *s) {
fprintf(stderr, "%s\n", s);
}
int main(void) {
printf("Enter expressions to evaluate (Ctrl+C to exit):
\n");
yyparse();
return 0;
}
commands to run:-
- bison -dy filename.yacc
- flex filename.lex
- gcc -o my_program y.tab.c lex.yy.c
- ./my_program

4. To write a C program that takes the single line production rule in a string as input and checks if it has Left-Recursion or not and give the unambiguous grammar, in case, if it has Left- Recursion.
CODE:
#include <stdio.h>
#include <string.h>
#include <stdbool.h>
#define MAX 100
void checkLeftRecursion(char* input) {
char nonTerminal = input[0];
char alpha[MAX], beta[MAX];
char* production = strstr(input, "-->") + 3;
char *token = strtok(production, "|");
bool hasLeftRecursion = false;
int alphaCount = 0, betaCount = 0;
while (token != NULL) {
if (token[0] == nonTerminal) {
hasLeftRecursion = true;
strcpy(alpha + alphaCount, token + 1);
alphaCount += strlen(token) - 1;
strcat(alpha, "|");
} else {
strcpy(beta + betaCount, token);
betaCount += strlen(token);
strcat(beta, "|");
}
token = strtok(NULL, "|");
}
if (betaCount > 0) beta[betaCount - 1] = '\0';
if (alphaCount > 0) alpha[alphaCount - 1] = '\0';
if (hasLeftRecursion) {
printf("Left Recursive Grammar\n");
printf("%c --> %s%c'\n", nonTerminal, beta, nonTerminal);
printf("%c' --> %s%c' | e\n", nonTerminal, alpha,
nonTerminal);
} else {
printf("No Left Recursion present.\n");
}
}
int main() {
char input[MAX];
printf("Enter the production rule: ");
fgets(input, MAX, stdin);
input[strcspn(input, "\n")] = 0;
checkLeftRecursion(input);
￼ 17
CSE-A Saurav Malik 06017702722
return 0;
}

5. To write a C program that takes the single line production rule in a string as input and checks if it has Left-Factoring or not and give the unambiguous grammar, in case, if it has Left- Factoring.
CODE:
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#define MAX 100
int commonPrefixLength(char *str1, char *str2) {
int i = 0;
while (str1[i] && str2[i] && str1[i] == str2[i]) {
i++;
}
return i;
}
void checkLeftFactoring(char *input) {
char nonTerminal, alpha[MAX], betas[MAX][MAX];
char *token;
int i = 0, prefixLength = 0;
int minPrefixLength = MAX;
char prefix[MAX];
nonTerminal = input[0];
token = strtok(input + 3, "|");
while (token != NULL) {
strcpy(betas[i++], token);
token = strtok(NULL, "|");
}
for (int k = 1; k < i; k++) {
prefixLength = commonPrefixLength(betas[0], betas[k]);
if (prefixLength < minPrefixLength) {
minPrefixLength = prefixLength;
}
}
if (minPrefixLength == 0) {
printf("No Left Factoring\n");
return;
}
strncpy(prefix, betas[0], minPrefixLength);
prefix[minPrefixLength] = '\0';
printf("Left Factoring Grammar Detected\n");
printf("Corrected Grammar:\n");
printf("%c --> %s%c'|", nonTerminal, prefix, nonTerminal);
for (int k = 0; k < i; k++) {
￼ 20
CSE-A Saurav Malik 06017702722
if (strncmp(betas[k], prefix, minPrefixLength) != 0) {
printf("%s", betas[k]);
}
}
printf("\n");
printf("%c' --> ", nonTerminal);
for (int k = 0; k < i; k++) {
if (k > 0) printf("|");
if (strlen(betas[k]) > minPrefixLength) {
printf("%s", betas[k] + minPrefixLength);
} else {
printf("e"); // epsilon
}
}
printf("\n");
}
int main() {
char input[MAX];
printf("Enter a production rule: ");
fgets(input, MAX, stdin);
input[strcspn(input, "\n")] = '\0';
checkLeftFactoring(input);
return 0;
}

6. Write a program to find out the FIRST of the Non-terminals in a grammar.
Code:
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <ctype.h>
#define MAX_PRODUCTIONS 100
#define MAX_SYMBOLS 100
typedef struct
{
char nonTerminal;
char productions[MAX_PRODUCTIONS][MAX_SYMBOLS];
int prodCount;
} Production;
Production productions[MAX_PRODUCTIONS];
int productionCount = 0;
typedef struct
￼ 22
CSE-A Saurav Malik 06017702722
{
char nonTerminal;
char firstSet[MAX_SYMBOLS];
int firstCount;
} FirstSet;
FirstSet firstSets[MAX_PRODUCTIONS];
int firstSetCount = 0;
int findFirstSet(char nonTerminal)
{
for (int i = 0; i < firstSetCount; i++)
{
if (firstSets[i].nonTerminal == nonTerminal)
{
return i;
}
}
return -1;
}
void addToFirstSet(int index, char symbol)
{
for (int i = 0; i < firstSets[index].firstCount; i++)
{
if (firstSets[index].firstSet[i] == symbol)
return;
}
firstSets[index].firstSet[firstSets[index].firstCount++] = symbol;
}
void computeFirst(char nonTerminal)
{
int index = findFirstSet(nonTerminal);
if (index == -1)
{
index = firstSetCount++;
firstSets[index].nonTerminal = nonTerminal;
firstSets[index].firstCount = 0;
}
for (int i = 0; i < productionCount; i++)
{
if (productions[i].nonTerminal == nonTerminal)
{
for (int j = 0; j < productions[i].prodCount; j++)
{
char *production = productions[i].productions[j];
for (int k = 0; production[k] != '\0'; k++)
{
char symbol = production[k];
if (islower(symbol) || symbol == 'e')
{
addToFirstSet(index, symbol);
break;
}
else
{
computeFirst(symbol);
int symbolIndex = findFirstSet(symbol);
for (int m = 0; m <
firstSets[symbolIndex].firstCount; m++)
￼ 23
CSE-A Saurav Malik 06017702722
{
if (firstSets[symbolIndex].firstSet[m] != 'e')
{
addToFirstSet(index,
firstSets[symbolIndex].firstSet[m]);
}
}
if (strchr(firstSets[symbolIndex].firstSet, 'e')
== NULL)
break;
}
}
}
}
}
}
int main()
{
int n;
printf("Enter the number of productions: ");
scanf("%d", &n);
getchar(); // Consume newline character
printf("Enter the productions (e.g., S-->aA|b):\n");
for (int i = 0; i < n; i++)
{
char line[MAX_SYMBOLS];
fgets(line, sizeof(line), stdin);
line[strcspn(line, "\n")] = 0; // Remove newline
productions[productionCount].nonTerminal = line[0];
char *token = strtok(line + 4, "|"); // Skip the arrow and split
by '|'
while (token)
{
strcpy(productions[productionCount].productions[productions[productionCoun
t].prodCount++], token);
token = strtok(NULL, "|");
}
productionCount++;
}
for (int i = 0; i < productionCount; i++)
{
computeFirst(productions[i].nonTerminal);
}
printf("FIRST sets:\n");
for (int i = 0; i < firstSetCount; i++)
{
printf("FIRST(%c) = { ", firstSets[i].nonTerminal);
for (int j = 0; j < firstSets[i].firstCount; j++)
{
printf("%s ", firstSets[i].firstSet[j] == 'e' ? "epsilon" :
(char[]){firstSets[i].firstSet[j], '\0'});
}
printf("}\n");
}
return 0;
}

7. Write a program to Implement Shift Reduce parsing for a String.
Code:
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#define MAX_PRODUCTIONS 100
#define MAX_SYMBOLS 100
typedef struct
{
char input[MAX_SYMBOLS];
char nonTerminal;
} Production;
Production productions[MAX_PRODUCTIONS];
int productionCount = 0;
typedef struct
{
char stack[MAX_SYMBOLS];
int top;
} Stack;
￼ 26
CSE-A Saurav Malik 06017702722
void initStack(Stack *s)
{
s->top = -1;
}
void push(Stack *s, const char *symbol)
{
if (s->top < MAX_SYMBOLS - 1)
{
strcpy(&s->stack[++(s->top)], symbol);
}
}
void pop(Stack *s, int count)
{
if (s->top >= count - 1)
{
s->top -= count;
}
}
const char *top(Stack *s)
{
return &s->stack[s->top];
}
void shift(Stack *p, const char *input, int *j, char
steps[MAX_PRODUCTIONS][MAX_SYMBOLS], int *stepCount)
{
char symbol[2] = {input[*j], '\0'};
push(p, symbol);
(*j)++;
snprintf(steps[(*stepCount)++], MAX_SYMBOLS, "Shift: %s | Stack: %s",
&input[*j], symbol);
}
int reduce(Stack *p, char steps[MAX_PRODUCTIONS][MAX_SYMBOLS], int
*stepCount)
{
for (int i = 0; i < productionCount; i++)
{
int len = strlen(productions[i].input);
char topSequence[MAX_SYMBOLS] = "";
for (int j = len - 1; j >= 0; j--)
{
if (p->top < 0)
return 0;
strncat(topSequence, top(p), 1);
pop(p, 1);
}
if (strcmp(topSequence, productions[i].input) == 0)
{
char symbol[2] = {productions[i].nonTerminal, '\0'};
push(p, symbol);
snprintf(steps[(*stepCount)++], MAX_SYMBOLS, "Reduce: %s ->
%c", topSequence, productions[i].nonTerminal);
return 1;
}
else
{
for (int k = len - 1; k >= 0; k--)
￼ 27
CSE-A Saurav Malik 06017702722
{
char symbol[2] = {topSequence[k], '\0'};
push(p, symbol);
}
}
}
return 0;
}
int srParser(const char *input, char steps[MAX_PRODUCTIONS][MAX_SYMBOLS],
int *stepCount)
{
Stack p;
initStack(&p);
int j = 0;
while (j < strlen(input) || p.top > 0)
{
if (j < strlen(input))
{
shift(&p, input, &j, steps, stepCount);
}
else if (!reduce(&p, steps, stepCount))
{
return 0;
}
}
return p.top == 0 && p.stack[0] == 'S';
}
int main()
{
int n;
printf("Enter the number of productions: ");
scanf("%d", &n);
getchar(); // Consume newline
printf("Enter the productions (e.g., S-->aA|b):\n");
for (int i = 0; i < n; i++)
{
char line[MAX_SYMBOLS];
fgets(line, sizeof(line), stdin);
line[strcspn(line, "\n")] = 0; // Remove newline
productions[productionCount].nonTerminal = line[0];
char *token = strtok(line + 4, "|"); // Skip the arrow
while (token)
{
strcpy(productions[productionCount++].input, token);
token = strtok(NULL, "|");
}
}
char input[MAX_SYMBOLS];
printf("Enter the input string: ");
scanf("%s", input);
char steps[MAX_PRODUCTIONS][MAX_SYMBOLS];
int stepCount = 0;
int accepted = srParser(input, steps, &stepCount);
for (int i = 0; i < stepCount; i++)
￼ 28
CSE-A Saurav Malik 06017702722
{
printf("%s\n", steps[i]);
}
printf("%s\n", accepted ? "Accepted" : "Not Accepted");
return 0;
}


8. Write a program to check whether a grammar is operator precedent
Code:
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#include <ctype.h>
#define MAX_PRODUCTIONS 100
#define MAX_SYMBOLS 100
// Structure to hold grammar productions
typedef struct
{
char lhs[MAX_SYMBOLS];
char rhs[MAX_SYMBOLS];
} Production;
Production grammar[MAX_PRODUCTIONS];
￼ 30
CSE-A Saurav Malik 06017702722
int productionCount = 0;
// Function to check if the grammar is operator precedence
int isOperatorPrecedent()
{
for (int i = 0; i < productionCount; i++)
{
char *rhs = grammar[i].rhs;
int len = strlen(rhs);
// Check for epsilon (represented as 'e' here)
if (strchr(rhs, 'e') != NULL)
{
return 0; // Epsilon is not allowed in operator precedence
grammar
}
// Check for consecutive non-terminals
char prevChar = '\0';
for (int j = 0; j < len; j++)
{
char ch = rhs[j];
if (isupper(ch))
{ // Non-terminal
if (isupper(prevChar))
{ // Previous was also a non-terminal
return 0; // Invalid: two non-terminals together
}
}
prevChar = ch; // Update previous character
}
}
return 1; // Passed all checks
}
int main()
{
printf("Enter grammar productions (type 'end' to finish):\n");
char line[MAX_SYMBOLS];
while (1)
{
fgets(line, sizeof(line), stdin);
line[strcspn(line, "\n")] = '\0'; // Remove newline character
if (strcmp(line, "end") == 0)
{
break; // Stop taking input when 'end' is entered
}
// Ensure the input follows the correct format
char *arrowPos = strstr(line, "-->");
if (arrowPos == NULL)
{
printf("Invalid production format. Please follow the rules.
\n");
continue;
}
// Extract lhs and rhs of the production
strncpy(grammar[productionCount].lhs, line, arrowPos - line);
grammar[productionCount].lhs[arrowPos - line] = '\0'; // Null-
terminate lhs
￼ 31
CSE-A Saurav Malik 06017702722
strcpy(grammar[productionCount].rhs, arrowPos + 3); // Copy rhs
productionCount++;
}
// Check if the grammar is operator precedence
if (isOperatorPrecedent())
{
printf("The grammar is operator precedent.\n");
}
else
{
printf("The grammar is not operator precedent.\n");
}
return 0;
}
