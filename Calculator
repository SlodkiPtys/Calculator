#include <iostream>
#include <cctype>

using namespace std;

struct IntNode {
    int data;
    IntNode* next;
    IntNode(int data) : data(data), next(nullptr) {}
};

class IntStack {
private:
    IntNode* top;

public:
    // Constructor
    IntStack() : top(nullptr) {}

    // Destructor
    ~IntStack() {
        IntNode* current;
        while (top != nullptr) {
            current = top;
            top = top->next;
            delete current;
        }
    }

    // Push an element onto the stack
    void push(int data) {
        IntNode* node = new IntNode(data);
        node->next = top;
        top = node;
    }

    // Pop an element from the stack
    int pop() {
        if (top == nullptr) {
            cerr << "Stack is empty" << endl;
            exit(1);
        }
        IntNode* temp = top;
        int data = temp->data;
        top = top->next;
        delete temp;
        return data;
    }

    // Get the top element of the stack
    int peek() {
        if (top == nullptr) {
            cerr << "Stack is empty" << endl;
            exit(1);
        }
        return top->data;
    }

    // Check if the stack is empty
    bool isEmpty() {
        return top == nullptr;
    }

    // Print all elements in the stack
    void printStack() {
        IntStack tempStack;

        while (!this->isEmpty()) {
            int tempVal = this->pop();
            cout << " " << tempVal << " ";
            tempStack.push(tempVal);
        }

        // Restore the elements to the original stack
        while (!tempStack.isEmpty()) {
            this->push(tempStack.pop());
        }

        cout << endl;
    }
};

struct CharNode {
    char data;
    CharNode* next;
    CharNode(char data) : data(data), next(nullptr) {}
};

class CharStack {
private:
    CharNode* top;

public:
    CharStack() : top(nullptr) {}

    ~CharStack() {
        CharNode* current;
        while (top != nullptr) {
            current = top;
            top = top->next;
            delete current;
        }
    }

    void push(char data) {
        CharNode* node = new CharNode(data);
        node->next = top;
        top = node;
    }

    char pop() {
        if (top == nullptr) {
            cerr << "Stack is empty" << endl;
            exit(1);
        }
        CharNode* temp = top;
        char data = temp->data;
        top = top->next;
        delete temp;
        return data;
    }

    char peek() {
        return top ? top->data : '\0';
    }

    bool isEmpty() {
        return top == nullptr;
    }
};

bool isOperator(char c) {
    return c == '+' || c == '-' || c == '*' || c == '/' || c == 'I';
}

bool isFunction(char c) {
    return c == 'I' || c == 'F' || c == 'M' || c == 'G' || c == 'A' || c == 'X';
}

int getPriority(char c) {
    if (c == 'I') return 4;  // Highest priority for IF
    if (c == 'N') return 3;
    if (c == '*' || c == '/') return 2;
    if (c == '+' || c == '-') return 1;
    return 0;
}

void infixToPostfix(const char* infix, char* postfix) {
    CharStack stack;
    IntStack stackNArg;
    int iNArg;
    int TmpNArg;
    int j = 0;
    for (int i = 0; infix[i] != '\0'; ++i) {
        if (isspace(infix[i])) continue;

        if (isdigit(infix[i]) || (infix[i] == '-' && isdigit(infix[i + 1]))) {
            if (infix[i] == '-') {
                postfix[j++] = infix[i++];
            }
            while (isdigit(infix[i])) {
                postfix[j++] = infix[i++];
            }
            postfix[j++] = ' ';
            --i;
        }
        else if (infix[i] == 'N') {
            // Ensure that 'N' is treated like an operator with the highest precedence.
            while (!stack.isEmpty() && getPriority(stack.peek()) >= getPriority('N') && isOperator(infix[i])) {
                postfix[j++] = stack.pop();
                postfix[j++] = ' ';
            }
            stack.push(infix[i]);
        }
        else if (isFunction(infix[i])) {  // Assume 'I' represents 'IF'
            // Push 'F' to stack directly since it has the highest precedence
            while (isFunction(infix[i])) {
                i++;
            }
            if (infix[i] == 'N')
            {
                stack.push('G');
            }
            else
            {
                stack.push(infix[i - 1]);
            }
            stack.push(' ');
            stackNArg.push(1);
        }
        else if (infix[i] == '(') {
            stack.push(infix[i]);
        }
        else if (infix[i] == ',') {
            int topLAgr = stackNArg.pop();
            stackNArg.push(++topLAgr);
            while (!stack.isEmpty() && stack.peek() != '(') {
                postfix[j++] = stack.pop();
                postfix[j++] = ' ';
            }
            //stack.pop(); // Remove the '(' from the stack
        }
        else if (infix[i] == ')') {
            while (!stack.isEmpty() && stack.peek() != '(') {
                postfix[j++] = stack.pop();
                postfix[j++] = ' ';
            }
            stack.pop(); // Remove the '(' from the stack     
            while (!stack.isEmpty() && isspace(stack.peek())) {
                stack.pop();
            }
            int k;
            if (!stack.isEmpty() && isFunction(stack.peek())) {
                switch (stack.peek()) {
                case 'F':                       //IF
                    postfix[j++] = 'I';
                    postfix[j++] = stack.pop();
                    postfix[j++] = ' ';
                    stackNArg.pop();
                    break;
                case 'G':                       //MIN
                    postfix[j++] = 'M';
                    postfix[j++] = 'I';
                    stack.pop();
                    postfix[j++] = 'N';
                    TmpNArg = stackNArg.peek();
                    iNArg = 0;
                    while (TmpNArg) {
                        TmpNArg /= 10;
                        iNArg++;
                    }
                    TmpNArg = stackNArg.pop();
                    for (k = iNArg - 1; k >= 0; k--) {
                        postfix[j + k] = (char)((TmpNArg % 10) | 48);
                        TmpNArg /= 10;
                    }
                    j += iNArg;
                    postfix[j++] = ' ';
                    break;
                case 'X':                       //MAX
                    postfix[j++] = 'M';
                    postfix[j++] = 'A';
                    postfix[j++] = stack.pop();
                    TmpNArg = stackNArg.peek();
                    iNArg = 0;
                    while (TmpNArg) {
                        TmpNArg /= 10;
                        iNArg++;
                    }
                    TmpNArg = stackNArg.pop();
                    for (k = iNArg - 1; k >= 0; k--) {
                        postfix[j + k] = (char)((TmpNArg % 10) | 48);
                        TmpNArg /= 10;
                    }
                    j += iNArg;
                    postfix[j++] = ' ';
                    break;
                }
            }
        }
        else if (isOperator(infix[i])) {
            while (!stack.isEmpty() && getPriority(stack.peek()) >= getPriority(infix[i])) {
                postfix[j++] = stack.pop();
                postfix[j++] = ' ';

            }
            stack.push(infix[i]);
        }
    }

    while (!stack.isEmpty()) {
        postfix[j++] = stack.pop();
        postfix[j++] = ' ';
    }

    postfix[j] = '\0';
}

int charArrayToInt(const char* str) {
    int result = 0;
    bool isNegative = false;

    if (*str == '-') {
        isNegative = true;
        ++str;
    }

    while (*str >= '0' && *str <= '9') {
        result = result * 10 + (*str - '0');
        ++str;
    }

    return isNegative ? -result : result;
}

int evaluatePostfix(const char* postfix) {
    IntStack stack;
    IntStack argStack;
    int highestNum;
    int lowestNum;
    bool errorOccurred = false;
    int lastResult = 0; // Variable to store the last result
    int NArg = 0;

    for (int i = 0; postfix[i]; ++i) {
        if (postfix[i] == ' ') continue;

        if (isdigit(postfix[i]) || (postfix[i] == '-' && isdigit(postfix[i + 1]))) {
            int start = i;
            if (postfix[i] == '-') {
                i++;
            }
            while (isdigit(postfix[i])) { i++; }
            stack.push(charArrayToInt(&postfix[start]));
            if (postfix[i] == '\0') break;
        }
        else if (isFunction(postfix[i])) {
            while (isFunction(postfix[i])) {
                i++;
            }
            // IF function handling
            if (stack.isEmpty()) {
                cout << "Insufficient operands for Function." << endl;
                errorOccurred = true;
                break;
            }
            --i;
            int valC;
            int valB;
            int valA;
            switch (postfix[i]) {
            case 'F':
                valC = stack.pop();
                valB = stack.pop();
                valA = stack.pop();
                cout << "IF " << valC << " " << valB << " " << valA;
                if (!stack.isEmpty()) {
                    stack.printStack();
                }
                else {
                    cout << endl;
                }
                stack.push(valA > 0 ? valB : valC);
                break;
            case 'I':
                i++;
                NArg = charArrayToInt(&postfix[i + 1]);
                while (!isspace(postfix[i])) {
                    i++;
                }
                cout << "MIN" << NArg << " ";
                lowestNum = stack.peek();
                for (int Z = 0; Z < NArg; Z++)
                {
                    cout << stack.peek() << " ";
                    if (lowestNum > stack.peek())
                    {
                        lowestNum = stack.peek();
                    }
                    stack.pop();
                }
                if (!stack.isEmpty()) {
                    stack.printStack();
                }
                else {
                    cout << endl;
                }
                stack.push(lowestNum);
                break;
            case 'X':
                NArg = charArrayToInt(&postfix[i + 1]);
                while (!isspace(postfix[i])) {
                    i++;
                }
                cout << "MAX" << NArg << " ";
                highestNum = stack.peek();
                for (int Z = 0; Z < NArg; Z++)
                {
                    cout << stack.peek() << " ";
                    if (highestNum < stack.peek())
                    {
                        highestNum = stack.peek();
                    }
                    stack.pop();
                }
                if (!stack.isEmpty()) {
                    stack.printStack();
                }
                else {
                    cout << endl;
                }
                stack.push(highestNum);
                break;
            }
        }
        else if (postfix[i] == 'N') {
            if (stack.isEmpty()) {
                cout << "No operand for negation." << endl;
                errorOccurred = true;
                break;
            }
            int operand = stack.pop();
            cout << "N " << operand << " ";  // Show the negation operation and the operand
            operand = -operand; // Negate the operand
            if (!stack.isEmpty()) {
                stack.printStack();
            }
            else {
                cout << endl;  // Ensure we end the line after printing the negation operation
            }
            stack.push(operand); // Push the negated value back on the stack
        }
        else {
            if (stack.isEmpty()) {
                cout << "Insufficient operands." << endl;
                errorOccurred = true;
                break;
            }

            int val2 = stack.pop();

            if (stack.isEmpty()) {
                cout << "Insufficient operands." << endl;
                errorOccurred = true;
                break;
            }

            int val1 = stack.pop();

            switch (postfix[i]) {
            case '+':
                lastResult = val1 + val2;
                cout << "+ " << val2 << " " << val1;
                if (!stack.isEmpty()) {
                    stack.printStack();
                }
                else {
                    cout << endl;
                }
                stack.push(lastResult);
                break;
            case '-':
                lastResult = val1 - val2;
                cout << "- " << val2 << " " << val1;
                if (!stack.isEmpty()) {
                    stack.printStack();
                }
                else {
                    cout << endl;
                }
                stack.push(lastResult);
                break;
            case '*':
                cout << "* " << val2 << " " << val1;

                // Check if there's another number on the stack before performing multiplication
                if (!stack.isEmpty()) {
                    stack.printStack();
                }
                else {
                    cout << endl;
                }
                lastResult = val1 * val2;
                stack.push(lastResult);
                break;
            case '/':
                int rightOperand = val2;  // This is the right-hand operand (second popped)
                int leftOperand = val1;   // This is the left-hand operand (first popped)
                cout << "/ " << rightOperand << " " << leftOperand;

                if (rightOperand == 0) {
                    if (!stack.isEmpty()) {
                        stack.printStack();
                    }
                    cout << endl << "ERROR" << endl;
                    errorOccurred = true;
                }
                else {
                    // Check if there's another number on the stack before performing division
                    if (!stack.isEmpty()) {
                        stack.printStack();
                    }
                    else {
                        cout << endl;
                    }
                    lastResult = leftOperand / rightOperand;
                    stack.push(lastResult);
                }
                break;
            }
        }

        if (errorOccurred) {
            break;
        }
    }

    if (errorOccurred) {
        return INT_MIN; // Use INT_MIN to indicate an error occurred
    }
    return stack.isEmpty() ? 0 : stack.pop(); // Return 0 if stack is empty
}

int main() {
    int n;
    cin >> n;
    cin.ignore();

    char** infixExpressions = new char* [n];
    char** postfixExpressions = new char* [n];
    int* results = new int[n];  // Array to store results of the evaluations

    // First, convert all infix expressions to postfix and store them
    for (int i = 0; i < n; ++i) {
        infixExpressions[i] = new char[999999];
        cin.getline(infixExpressions[i], 999999);

        postfixExpressions[i] = new char[999999];
        infixToPostfix(infixExpressions[i], postfixExpressions[i]);
    }

    // Now, evaluate all postfix expressions and store the results
    bool errorOccurred = false;
    for (int i = 0; i < n; ++i) {
        cout << endl << postfixExpressions[i] << endl;
        results[i] = evaluatePostfix(postfixExpressions[i]);
        if (results[i] == INT_MIN) {
            // Error encountered, print an error message if you want
            errorOccurred = true;
            continue; // Exit the loop as we do not want to process further
        }
        cout << results[i] << endl;
    }

    // Clean up
    for (int i = 0; i < n; ++i) {
        delete[] infixExpressions[i];
        delete[] postfixExpressions[i];
    }
    delete[] infixExpressions;
    delete[] postfixExpressions;
    delete[] results;

    return errorOccurred ? 1 : 0; // Return a non-zero exit code if an error occurred
}
