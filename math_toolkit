#include <stdio.h>
#include <time.h>
#include <stdlib.h>
#include <string.h>
#include <math.h>
#include <stdbool.h>


// Function to calculate factorial to use in Gregory Newton
long int fact(int n)
{
    if (n >= 1)
        return n * fact(n - 1);
    else
        return 1;
}



// ----------------------------------------------------------------------------------------------------------------------
// ----------------------------------------------------------------------------------------------------------------------
// ----------------------------------------------------------------------------------------------------------------------
// ----------------------------------------------------------------------------------------------------------------------

#define MAX_SIZE 1000

typedef struct Stack
{
    int top;
    double Number[MAX_SIZE];
} STACK;

typedef struct CharStack
{
    int top;
    char data[MAX_SIZE];
} CHAR_STACK;
//-------------------------------------------------------stack stuff------------------------------------------
int getPrecedence(char);
double func(char *, double, int *);
double pop(STACK *);
void push(STACK *, double);
bool isEmpty(STACK *);
bool isFull(STACK *);
char charPop(CHAR_STACK *);
void charPush(CHAR_STACK *, char);
bool charIsEmpty(CHAR_STACK *);
bool charIsFull(CHAR_STACK *);

double pop(STACK *stack)
{
    if (!isEmpty(stack))
    {
        return stack->Number[stack->top--];
    }
    printf("\nStack underflow.\n");
    exit(EXIT_FAILURE);
}

void push(STACK *stack, double number)
{
    if (!isFull(stack))
    {
        stack->Number[++stack->top] = number;
    }
    else
    {
        printf("Stack Overflow\n");
        exit(EXIT_FAILURE);
    }
}

bool isEmpty(STACK *stack)
{
    return stack->top == -1;
}

bool isFull(STACK *stack)
{
    return stack->top >= MAX_SIZE - 1;
}

char charPop(CHAR_STACK *stack)
{
    if (!charIsEmpty(stack))
    {
        return stack->data[stack->top--];
    }
    printf("\nChar stack underflow.\n");
    exit(EXIT_FAILURE);
}

void charPush(CHAR_STACK *stack, char c)
{
    if (!charIsFull(stack))
    {
        stack->data[++stack->top] = c;
    }
    else
    {
        printf("Char stack overflow\n");
        exit(EXIT_FAILURE);
    }
}

bool charIsEmpty(CHAR_STACK *stack)
{
    return stack->top == -1;
}

bool charIsFull(CHAR_STACK *stack)
{
    return stack->top >= MAX_SIZE - 1;
}

bool isOperator(char c)
{
    return c == '+' || c == '-' || c == '*' || c == '/' || c == '^';
}

double applyOperator(double a, double b, char op)
{
    switch (op)
    {
    case '+':
        return a + b;
    case '-':
        return a - b;
    case '*':
        return a * b;
    case '/':
        if (b == 0.0)
        {
            printf("Error: Division by zero.\n");
            exit(EXIT_FAILURE);
        }
        return a / b;
    case '^':
        return pow(a, b);
    default:
        printf("Error: Unknown operator '%c'.\n", op);
        exit(EXIT_FAILURE);
    }
}

// ----------------------------------------------------------------------------------------------------------------------
// ----------------------------------------------------------------------------------------------------------------------
// ----------------------------------------------------------------------------------------------------------------------
// ----------------------------------------------------------------------------------------------------------------------

double func(char *str, double x, int *index)
{
    CHAR_STACK operatorStack;
    operatorStack.top = -1;
    STACK operandStack;
    operandStack.top = -1;

    double operand = 0.0;
    bool expectOperand = true;
    bool unaryMinus = false;
    bool keepProcessing = true;

    while (keepProcessing && str[*index] != '\0' && str[*index] != ')')
    {
        if (str[*index] == ' ')
        {
            (*index)++;
        }
        else
        {
            if (expectOperand)
            {
                if (str[*index] == '(')
                {
                    (*index)++;
                    operand = func(str, x, index);
                    if (str[*index] != ')')
                    {
                        printf("Error: Missing closing parenthesis.\n");
                        exit(EXIT_FAILURE);
                    }
                    (*index)++;
                }
                else if (str[*index] == '-' && (expectOperand || (*index == 0 || isOperator(str[*index - 1]) || str[*index - 1] == '(')))
                {
                    unaryMinus = true;
                    (*index)++;
                    // Don't change expectOperand - we still need an operand
                }
                else if (str[*index] == 'x')
                {
                    operand = x;
                    (*index)++;
                }
                else if (str[*index] == 'e')
                {
                    operand = exp(1.0);
                    (*index)++;
                }
                else if (str[*index] == 'p' && str[*index + 1] == 'i')
                {
                    operand = 3.14159265358979323846;
                    (*index) += 2;
                }
                else if (str[*index] >= '0' && str[*index] <= '9')
                {
                    operand = atof(&str[*index]);
                    while ((str[*index] >= '0' && str[*index] <= '9') || str[*index] == '.')
                    {
                        (*index)++;
                    }
                }
                else if (strncmp(&str[*index], "log_", 4) == 0)
                {
                    (*index) += 4;
                    double base = atof(&str[*index]);
                    while ((str[*index] >= '0' && str[*index] <= '9') || str[*index] == '.')
                    {
                        (*index)++;
                    }
                    if (str[*index] != '(')
                    {
                        printf("Error: Missing opening parenthesis after log base.\n");
                        exit(EXIT_FAILURE);
                    }
                    (*index)++;
                    operand = func(str, x, index);
                    if (str[*index] != ')')
                    {
                        printf("Error: Missing closing parenthesis after log expression.\n");
                        exit(EXIT_FAILURE);
                    }
                    (*index)++;
                    operand = log(operand) / log(base);
                }
                else if (strncmp(&str[*index], "sin(", 4) == 0)
                {
                    (*index) += 4;
                    operand = sin(func(str, x, index));
                    if (str[*index] != ')')
                    {
                        printf("Error: Missing closing parenthesis after sin.\n");
                        exit(EXIT_FAILURE);
                    }
                    (*index)++;
                }
                else if (strncmp(&str[*index], "cos(", 4) == 0)
                {
                    (*index) += 4;
                    operand = cos(func(str, x, index));
                    if (str[*index] != ')')
                    {
                        printf("Error: Missing closing parenthesis after cos.\n");
                        exit(EXIT_FAILURE);
                    }
                    (*index)++;
                }
                else if (strncmp(&str[*index], "tan(", 4) == 0)
                {
                    (*index) += 4;
                    operand = tan(func(str, x, index));
                    if (str[*index] != ')')
                    {
                        printf("Error: Missing closing parenthesis after tan.\n");
                        exit(EXIT_FAILURE);
                    }
                    (*index)++;
                }
                else if (strncmp(&str[*index], "cot(", 4) == 0)
                {
                    (*index) += 4;
                    operand = 1.0 / tan(func(str, x, index));
                    if (str[*index] != ')')
                    {
                        printf("Error: Missing closing parenthesis after cot.\n");
                        exit(EXIT_FAILURE);
                    }
                    (*index)++;
                }
                else if (strncmp(&str[*index], "arcsin(", 7) == 0)
                {
                    (*index) += 7;
                    operand = asin(func(str, x, index));
                    if (str[*index] != ')')
                    {
                        printf("Error: Missing closing parenthesis after arcsin.\n");
                        exit(EXIT_FAILURE);
                    }
                    (*index)++;
                }
                else if (strncmp(&str[*index], "arccos(", 7) == 0)
                {
                    (*index) += 7;
                    operand = acos(func(str, x, index));
                    if (str[*index] != ')')
                    {
                        printf("Error: Missing closing parenthesis after arccos.\n");
                        exit(EXIT_FAILURE);
                    }
                    (*index)++;
                }
                else if (strncmp(&str[*index], "arctan(", 7) == 0)
                {
                    (*index) += 7;
                    operand = atan(func(str, x, index));
                    if (str[*index] != ')')
                    {
                        printf("Error: Missing closing parenthesis after arctan.\n");
                        exit(EXIT_FAILURE);
                    }
                    (*index)++;
                }
                else if (strncmp(&str[*index], "arccot(", 7) == 0)
                {
                    (*index) += 7;
                    operand = atan(1.0 / func(str, x, index));
                    if (str[*index] != ')')
                    {
                        printf("Error: Missing closing parenthesis after arccot.\n");
                        exit(EXIT_FAILURE);
                    }
                    (*index)++;
                }
                else
                {
                    printf("Error: Unexpected character '%c' at position %d.\n", str[*index], *index);
                    exit(EXIT_FAILURE);
                }

                if (!unaryMinus || str[*index - 1] != '-') // Only apply unary minus if we actually found an operand
                {
                    if (unaryMinus)
                    {
                        operand = -operand;
                        unaryMinus = false;
                    }

                    push(&operandStack, operand);
                    expectOperand = false;
                }
            }
            else
            {
                if (isOperator(str[*index]))
                {
                    char op = str[*index];
                    (*index)++;

                    // Handle operator precedence
                    while (!charIsEmpty(&operatorStack) &&
                           getPrecedence(operatorStack.data[operatorStack.top]) >= getPrecedence(op))
                    {
                        char stackOp = charPop(&operatorStack);
                        double b = pop(&operandStack);
                        double a = pop(&operandStack);
                        push(&operandStack, applyOperator(a, b, stackOp));
                    }

                    charPush(&operatorStack, op);
                    expectOperand = true;
                }
                else if (str[*index] == ')')
                {
                    keepProcessing = false;
                }
                else
                {
                    printf("Error: Expected operator at position %d\n", *index);
                    exit(EXIT_FAILURE);
                }
            }
        }
    }

    // Final evaluation
    while (!charIsEmpty(&operatorStack))
    {
        char op = charPop(&operatorStack);
        double b = pop(&operandStack);
        double a = pop(&operandStack);
        push(&operandStack, applyOperator(a, b, op));
    }

    if (isEmpty(&operandStack))
    {
        printf("Error: No result\n");
        exit(EXIT_FAILURE);
    }

    return pop(&operandStack);
}
// Helper function for operator precedence
int getPrecedence(char op)
{
    switch (op)
    {
    case '^':
        return 4;
    case '*':
    case '/':
        return 3;
    case '+':
    case '-':
        return 2;
    default:
        return 0;
    }
}

double FX(char *str_function, double xValue)
{
    int index = 0;
    return func(str_function, xValue, &index);
}

double F_(char *str_function, double x)
{
    return (FX(str_function, x + 0.0000001) - FX(str_function, x - 0.0000001)) / (0.0000002);
}

// ----------------------------------------------------------------------------------------------------------------------
// ----------------------------------------------------------------------------------------------------------------------
// ----------------------------------------------------------------------------------------------------------------------
// ----------------------------------------------------------------------------------------------------------------------

// Function To print the Menu
void print_Menu()
{

    printf("\n---------------------------------------------\nQuit: 0\n"
           "Bisection: 1\n"
           "Regula-Falsi: 2\n"
           "Newton-Rapshon: 3\n"
           "Inverse Matrix: 4\n"
           "Cholesky(ALU): 5\n"
           "Gauss-Seidel: 6\n"
           "Sayisal Turev: 7\n"
           "Simpson's Rule: 8\n"
           "Trapezoidal Rule: 9\n"
           "Gregory-Newton: 10\n"
           "Choice: ");
}
// ----------------------------------------------------------------------------------------------------------------------

// -----------------------------------------------Matrix Stuff--------------------------------------------------------
// Allocating space for the matrix
double **MatrixAllocate(int n, int m)
{
    int i;
    double **matrix = (double **)calloc(n, sizeof(double *));
    if (matrix == NULL)
    {
        return NULL;
    }
    for (i = 0; i < n; i++)
    {
        matrix[i] = (double *)calloc(m, sizeof(double));
        if (matrix[i] == NULL)
        {
            for (int j = 0; j < i; j++)
            {
                free(matrix[j]);
            }
            free(matrix);
            return NULL;
        }
    }
    return matrix;
}

// Taking the elements
void TakingElements(double **matrix, int n, int m)
{
    printf("Please enter the elements:\n");
    for (int i = 0; i < n; i++)
    {
        for (int j = 0; j < m; j++)
        {
            scanf("%lf", &matrix[i][j]);
        }
    }
}

// Printing the matrix
void printmatrix(double **matrix, int n, int m)
{
    for (int i = 0; i < n; i++)
    {
        for (int j = 0; j < m; j++)
        {
            printf("%lf ", matrix[i][j]);
        }
        printf("\n");
    }
    printf("\n-----------------------------------\n");
}

// Free the matrix
void FreeMatrix(double **matrix, int n)
{
    if (matrix == NULL)
        return;
    for (int i = 0; i < n; i++)
    {
        free(matrix[i]);
    }
    free(matrix);
}
// ----------------------------------------------------------------------------------------------------------------------
// ----------------------------------------------------------------------------------------------------------------------
// ----------------------------------------------------------------------------------------------------------------------
// ----------------------------------------------------------------------------------------------------------------------

void Bisection()
{
    double a, b, c;
    int choice = 0;
    double epsilon;
    int MaxIteration, counter = 0;
    char str_function[MAX_SIZE];


    int ch;
    while ((ch = getchar()) != '\n' && ch != EOF)
        ;

    printf("Enter your function: Example-> sin(2*x) + log_2(x^4)- 2*x + e^2 + pi * x\nplease use paranthesis to seperate arguments\n");
    scanf("%[^\n]s", str_function);

    printf("Please enter the values for [a,b]\n");
    do
    {
        printf("a: ");
        scanf("%lf", &a);

        printf("b: ");
        scanf("%lf", &b);

        if (a > b || FX(str_function, a) * FX(str_function, b) > 0)
        {
            printf("Bad inputs! Try again (0), or stop (1): \n");
            scanf("%d", &choice);
        }
    } while ((a > b || FX(str_function, a) * FX(str_function, b) > 0) && choice == 0);

    if (choice != 0)
    {
        return;
    }

    printf("Enter epsilon: ");
    scanf("%lf", &epsilon);

    printf("What is the max iteration? ");
    scanf("%d", &MaxIteration);

    while (fabs(b - a) > epsilon && counter < MaxIteration)
    {
        c = (a + b) / 2;

        printf("\n------------------\nIteration: %d\n", counter + 1);

        printf("a = %lf , F(a) = %lf\n", a, FX(str_function, a));
        printf("b = %lf , F(b) = %lf\n", b, FX(str_function, b));
        printf("c = %lf , F(c) = %lf\n", c, FX(str_function, c));

        if (FX(str_function, c) == 0)
        {
            printf("Exact root found at: %lf in %d iterations\n", c, counter + 1);
            return;
        }
        if (FX(str_function, a) * FX(str_function, c) < 0)
        {
            b = c;
            printf("b updated to c\n");
        }
        else
        {
            a = c;
            printf("a updated to c\n");
        }

        counter++;
    }

    printf("Approximate root after %d iterations: %lf\n", counter, c);
}

// ----------------------------------------------------------------------------------------------------------------------
// ----------------------------------------------------------------------------------------------------------------------
// ----------------------------------------------------------------------------------------------------------------------
// ----------------------------------------------------------------------------------------------------------------------

void Regula_Falsi()
{
    double a, b, c;
    int choice = 0;
    double epsilon;
    int MaxIteration, counter = 0;
    char str_function[MAX_SIZE];
    int ch;
    while ((ch = getchar()) != '\n' && ch != EOF)
        ;

    printf("Enter your function: Example-> sin(2*x) + log_2(x^4)- 2*x + e^2 + pi * x\nplease use paranthesis to seperate arguments\n");
    scanf("%[^\n]s", str_function);

    printf("Please enter the values for [a,b]\n");
    do
    {
        printf("a: ");
        scanf("%lf", &a);

        printf("b: ");
        scanf("%lf", &b);

        if (a > b || FX(str_function, a) * FX(str_function, b) > 0)
        {
            printf("Bad inputs! Try again (0), or stop (1): ");
            scanf("%d", &choice);
        }
    } while ((a > b || FX(str_function, a) * FX(str_function, b) > 0) && choice == 0);

    if (choice == 1)
    {
        return;
    }

    printf("Enter epsilon: ");
    scanf("%lf", &epsilon);

    printf("What is the max iteration? ");
    scanf("%d", &MaxIteration);

    while (fabs(b - a) > epsilon && counter < MaxIteration)
    {
        c = (b * FX(str_function, a) - a * FX(str_function, b)) / (FX(str_function, a) - FX(str_function, b));

        printf("\n------------------\nIteration: %d\n", counter + 1);
        printf("a = %lf , F(a) = %lf\n", a, FX(str_function, a));
        printf("b = %lf , F(b) = %lf\n", b, FX(str_function, b));
        printf("c = %lf , F(c) = %lf\n", c, FX(str_function, c));

        if (fabs(FX(str_function, c)) < epsilon)
        {
            printf("Approximate  root found at: %lf in %d iterations\n", c, counter + 1);
            return;
        }

        if (FX(str_function, a) * FX(str_function, c) < 0)
        {
            b = c;
            printf("b updated to c\n");
        }
        else
        {
            a = c;
            printf("a updated to c\n");
        }

        counter++;
    }

    printf("Approximate root after %d iterations: %lf\n", counter, c);
}

// ----------------------------------------------------------------------------------------------------------------------
// ----------------------------------------------------------------------------------------------------------------------
// ----------------------------------------------------------------------------------------------------------------------
// ----------------------------------------------------------------------------------------------------------------------

void Newton_Raphson()
{

    double x, x2;
    double epsilon;
    int MaxIteration, counter = 0;
    char str_function[MAX_SIZE];

    int ch;
    while ((ch = getchar()) != '\n' && ch != EOF)
        ;

    printf("Enter your function: Example-> sin(2*x) + log_2(x^4)- 2*x + e^2 + pi * x\nplease use paranthesis to seperate arguments\n");
    scanf("%[^\n]s", str_function);

    printf("enter the value of X0 (the starting value): ");
    scanf("%lf", &x);

    printf("Enter epsilon: ");
    scanf("%lf", &epsilon);

    printf("What is the max iteration? ");
    scanf("%d", &MaxIteration);

    while (counter < MaxIteration)
    {
        x2 = x - (FX(str_function, x) / F_(str_function, x));

        printf("\n------------------\nIteration: %d\n", counter + 1);
        printf("X_n = %lf , F(X_n) = %lf\n", x, FX(str_function, x));
        printf("X_n+1 = %lf , F(X_n+1) = %lf\n", x2, FX(str_function, x2));
        printf("Epsilon = %lf\n", fabs(x2 - x));

        if (fabs(x2 - x) <= epsilon)
        {
            printf("Approximate  root found at: %lf in %d iterations\n", x2, counter + 1);
            return;
        }

        x = x2;
        counter++;
    }
}

// ----------------------------------------------------------------------------------------------------------------------
// ----------------------------------------------------------------------------------------------------------------------
// ----------------------------------------------------------------------------------------------------------------------
// ----------------------------------------------------------------------------------------------------------------------

// INVERSE OF MATRIX

void Matrix_Inverse(double **matrix, int n)
{
    double **Identity;
    int i, j, s; // to use in for loop
    double pivot;
    double *tmp; // to swap raws
    int swap = 0;
    // int flag = 1;
    double z;
    matrix = MatrixAllocate(n, n);
    printf("The matrix will be takin at once\nexample:\n"
           "a b c d\n"
           "e f g h\n"
           "i j k l\nand so on\n");
    TakingElements(matrix, n, n);
    printf("\n");
    printmatrix(matrix, n, n);
    // making Identity Matrix
    Identity = MatrixAllocate(n, n);
    for (i = 0; i < n; i++)
    {
        Identity[i][i] = 1;
    }
    //printmatrix(Identity, n, n); // if you want to see the identity matrix

    // making the pivot = 1
    for (i = 0; i < n; i++)
    {
        pivot = matrix[i][i];
        if (matrix[i][i] == 0)
        {
            swap = -1;
            for (j = i + 1; j < n; j++)
            {
                if (matrix[j][i] != 0)
                {
                    swap = j; // the raw to swap
                    j = n;    // this is to break out of the loop
                }
            }
            if (swap == -1)
            {
                printf("This is a singular matrix!! CAN NOT BE DONE ):");
                return; // if you can use exit go ahead
            }

            // swap in the main matrix
            tmp = matrix[i];
            matrix[i] = matrix[swap];
            matrix[swap] = tmp;
            // swap in the identity matrix
            tmp = Identity[i];
            Identity[i] = Identity[swap];
            Identity[swap] = tmp;
            pivot = matrix[i][i];
        }
        // making the first raw's pivot = 1
        if (pivot != 1)
        {
            for (j = 0; j < n; j++)
            {
                matrix[i][j] = (matrix[i][j]) / pivot;
                Identity[i][j] = (Identity[i][j]) / pivot;
            }
        }
        // Eliminate other entries in column i
        for (s = 0; s < n; s++)
        {
            if (s != i)
            {
                z = matrix[s][i];
                for (j = 0; j < n; j++)
                {
                    matrix[s][j] -= z * matrix[i][j];
                    Identity[s][j] -= z * Identity[i][j];
                }
            }
        }
        // if you want to see the matrix after each iteration
        //printf("this is the original matrix\n");
        //printmatrix(matrix, n, n);
        //printf("this is the identity matrix\n");

    }
    printf("\nThis is your inversed matrix \n");
    printmatrix(Identity, n, n);
    // free the matrix after finishing
    FreeMatrix(matrix, n);
}
// ----------------------------------------------------------------------------------------------------------------------
// ----------------------------------------------------------------------------------------------------------------------
// ----------------------------------------------------------------------------------------------------------------------
// ----------------------------------------------------------------------------------------------------------------------
void Gauss_Seidel(double **matrix, int n)
{

    int i, j, swap = 0;
    double *answer = calloc(n, sizeof(double));
    double *oldAnswer = calloc(n, sizeof(double));
    matrix = MatrixAllocate(n, n);
    double *tmp;
    double max, x;
    double epsilon;
    int MaxIteration, counter = 0;
    int done = 0;
    double sum;

    printf("The matrix will be taken at once\nexample "
           "a b c d\n"
           "e f g h\n"
           "i j k l\nand so on\n");
    TakingElements(matrix, n, n);
    printf("\n");
    printmatrix(matrix, n, n);

    printf("enter the answer matrix: ");
    for (i = 0; i < n; i++)
    {
        scanf("%lf", &answer[i]);
    }

    printf("Enter starting values for the unknowns: ");
    for (i = 0; i < n; i++)
    {
        scanf("%lf", &oldAnswer[i]);
    }

    printf("what is epsilon value ? ");
    scanf("%lf", &epsilon); //
    printf("what is the max iteration ? ");
    scanf("%d", &MaxIteration);

    // Make the diagonal the largest value by swapping rows
    for (i = 0; i < n; i++)
    {
        max = fabs(matrix[i][i]);
        swap = i;

        for (j = i + 1; j < n; j++)
        {
            if (fabs(matrix[j][i]) > max)
            {
                max = fabs(matrix[j][i]);
                swap = j;
            }
        }

        if (max == 0)
        {
            printf("This is a singular matrix!! CAN NOT BE DONE ):\n");
            return;
        }

        if (swap != i)
        {
            x = answer[i];
            answer[i] = answer[swap];
            answer[swap] = x;
            tmp = matrix[i];
            matrix[i] = matrix[swap];
            matrix[swap] = tmp;
        }
    }

    while (counter < MaxIteration && done == 0)
    {
        done = 1;

        for (i = 0; i < n; i++)
        {
            sum = answer[i];
            for (j = 0; j < n; j++)
            {
                if (i != j)
                    sum = sum - (matrix[i][j] * oldAnswer[j]);
            }
            sum = sum / matrix[i][i];

            if (fabs(sum - oldAnswer[i]) > epsilon)
                done = 0;

            oldAnswer[i] = sum;
        }

        counter++;
        printf("\nResult after %d iterations:\n", counter);
        for (i = 0; i < n; i++)
        {
            printf("x%d = %lf\n", i + 1, oldAnswer[i]);
        }
    }

    printf("\n---------------------\nFinal Result after %d iterations:\n", counter);
    for (i = 0; i < n; i++)
    {
        printf("x%d = %lf\n", i + 1, oldAnswer[i]);
    }

    free(answer);
    free(oldAnswer);
    FreeMatrix(matrix, n);
}

void Cholesky(int n)
{
    double **A = MatrixAllocate(n, n);
    double **U = MatrixAllocate(n, n);
    double **L = MatrixAllocate(n, n);

    double **C = MatrixAllocate(n, 1);
    double **X = MatrixAllocate(n, 1);
    double **Y = MatrixAllocate(n, 1);
    int i, j, k, f;
    // double pivot;
    double tmp = 0, tmp1;
    // int flag = 1;

    printf("The matrix will be takin at once\nexample"
           "a b c d\n"
           "e f g h\n"
           "i j k l\nand so on\n");
    TakingElements(A, n, n);
    printf("\n");
    printmatrix(A, n, n);

    printf("Enter The Result Matrix :\n ");
    TakingElements(C, n, 1);

    // printmatrix(C, n, 1);

    // setting the diagonal for U to 1

    for (i = 0; i < n; i++)
    {
        U[i][i] = 1;
    }

    // calculating L and U
    for (i = 0; i < n; i++)
    {
        // For The Diagonal elements in L
        tmp = 0;
        for (k = 0; k < i; k++)
        {
            tmp = tmp + L[i][k] * U[k][i];
        }

        L[i][i] = A[i][i] - tmp;

        // upper elements in raw for U
        for (j = i + 1; j < n; j++)
        {
            tmp = 0;
            for (k = 0; k < i; k++)
            {
                tmp = tmp + L[i][k] * U[k][j];
            }
            U[i][j] = (A[i][j] - tmp) / L[i][i];
        }
        // L’s lower elements in column
        for (j = i + 1; j < n; j++)
        {
            tmp = 0;
            for (k = 0; k < i; k++)
            {
                tmp = tmp + L[j][k] * U[k][i];
            }
            L[j][i] = A[j][i] - tmp;
        }
    }

    printf("This is matrix L \n\n");
    printmatrix(L, n, n);
    printf("This is matrix U \n\n");
    printmatrix(U, n, n);

    // Now you have matrix L and U , you need to calculate y and x

    // starting with L Y = C

    for (i = 0; i < n; i++)
    {
        tmp1 = 0;
        for (f = 0; f < i; f++)
        {
            tmp1 += L[i][f] * Y[f][0];
        }

        Y[i][0] = (C[i][0] - tmp1) / L[i][i];
    }

    // now U X = Y

    for (i = n - 1; i >= 0; i--)
    {
        tmp1 = 0;
        for (f = i + 1; f < n; f++)
        {
            tmp1 += U[i][f] * X[f][0];
        }

        X[i][0] = (Y[i][0] - tmp1);
    }

    printf("This is matrix Y \n\n");
    printmatrix(Y, n, 1);

    for (i = 0; i < n; i++)
    {
        printf("X%d = %lf\n", i + 1, X[i][0]);
    }

    FreeMatrix(A, n);
    FreeMatrix(L, n);
    FreeMatrix(U, n);
    FreeMatrix(C, n);
    FreeMatrix(X, n);
    FreeMatrix(Y, n);
}

// ----------------------------------------------------------------------------------------------------------------------
// ----------------------------------------------------------------------------------------------------------------------
// ----------------------------------------------------------------------------------------------------------------------
// ----------------------------------------------------------------------------------------------------------------------

void Sayisal_Turev()
{
    double x, h, tmp;
    char str_function[MAX_SIZE];
    int ch;
    while ((ch = getchar()) != '\n' && ch != EOF)
        ;

    printf("Enter your function: Example-> sin(2*x) + log_2(x^4)- 2*x + e^2 + pi * x\nplease use paranthesis to seperate arguments\n");
    scanf("%[^\n]s", str_function);

    printf("please enter the value of X : ");
    scanf("%lf", &x);
    printf("\nplease enter the value of h : ");
    scanf("%lf", &h);

    // Geri Farklar İle Sayısal Türev

    tmp = (FX(str_function, x) - FX(str_function, x - h)) / h;
    printf("Geri Farklar ile Sayisal Turev for X = %lf\n----------------\n", tmp);

    tmp = (FX(str_function, x + h) - FX(str_function, x)) / h;
    printf("ileri Farklar ile Sayisal Turev for X = %lf\n----------------\n", tmp);

    tmp = (FX(str_function, x + h) - FX(str_function, x - h)) / (2 * h);
    printf("Merkezi Farklar ile Sayisal Turev for X = %lf\n----------------\n", tmp);
}

// ----------------------------------------------------------------------------------------------------------------------
// ----------------------------------------------------------------------------------------------------------------------
// ----------------------------------------------------------------------------------------------------------------------
// ----------------------------------------------------------------------------------------------------------------------
void Simpson_Kurallari()
{
    double a, b, h;
    int n;
    int n38; // total number of sub-intervals for 3/8
    double simpson38 = 0.0, sum38 = 0.0;
    double simpson13 = 0.0, sum13 = 0.0;
    int i;
    double x;
    char str_function[MAX_SIZE];
    int ch;
    while ((ch = getchar()) != '\n' && ch != EOF)
        ;

    printf("Enter your function: Example-> sin(2*x) + log_2(x^4)- 2*x + e^2 + pi * x\nplease use paranthesis to seperate arguments\n");
    scanf("%[^\n]s", str_function);

    printf("Enter lower limit a: ");
    scanf("%lf", &a);
    printf("Enter upper limit b: ");
    scanf("%lf", &b);

    printf("Enter a single n value: ");
    scanf("%d", &n);

    if (n < 1)
    {
        printf("[!] n must be at least 1.\n");
        return;
    }

    // Simpson 1/3 Rule
    if (n >= 2 && n % 2 == 0)
    {
        h = (b - a) / n;
        sum13 = FX(str_function, a) + FX(str_function, b);
        for (i = 1; i <= n - 1; i++)
        {
            x = a + i * h;
            if (i % 2 == 0)
                sum13 += 2 * FX(str_function, x);
            else
                sum13 += 4 * FX(str_function, x);
        }
        simpson13 = (h / 3) * sum13;
        printf("\nSimpson's 1/3 Rule Result  = %lf\n", fabs(simpson13));
    }
    else
    {
        printf("\n[!] Simpson's 1/3 Rule not applied. Reason: n must be even and at least 2.\n");
    }

    // Simpson 3/8 Rule
    n38 = 3 * n;
    h = (b - a) / n38;
    sum38 = FX(str_function, a) + FX(str_function, b);
    for (i = 1; i < n38; i++)
    {
        x = a + i * h;
        if (i % 3 == 0)
            sum38 += 2 * FX(str_function, x);
        else
            sum38 += 3 * FX(str_function, x);
    }
    simpson38 = (3 * h / 8) * sum38;
    printf("\nSimpson's 3/8 Rule Result = %lf\n", fabs(simpson38));
}
// ----------------------------------------------------------------------------------------------------------------------
// ----------------------------------------------------------------------------------------------------------------------
// ----------------------------------------------------------------------------------------------------------------------
// ----------------------------------------------------------------------------------------------------------------------

void trapez_yontemi()
{
    double a, b, h, toplam;
    int n, i;
    char str_function[MAX_SIZE];
    int ch;
    while ((ch = getchar()) != '\n' && ch != EOF)
        ;

    printf("Enter your function: Example-> sin(2*x) + log_2(x^4)- 2*x + e^2 + pi * x\nplease use paranthesis to seperate arguments\n");
    scanf("%[^\n]s", str_function);
    // User input
    printf("Enter the lower limit (a): ");
    scanf("%lf", &a);
    printf("Enter the upper limit (b): ");
    scanf("%lf", &b);
    printf("Enter the Value of (n): ");
    scanf("%d", &n);

    h = (b - a) / n;
    toplam = (FX(str_function, a) + FX(str_function, b)) / 2.0;

    for (i = 1; i < n; i++)
    {
        toplam += FX(str_function, a + i * h);
    }

    toplam *= h;

    printf("The result is : %lf\n", toplam);
}
// ----------------------------------------------------------------------------------------------------------------------
// ----------------------------------------------------------------------------------------------------------------------
// ----------------------------------------------------------------------------------------------------------------------
// ----------------------------------------------------------------------------------------------------------------------

void Gregory_Newton()
{
    int n, i;
    double wanted, h, u, result;
    double u_term = 1;

    printf("Please enter how many X values you got: ");
    scanf("%d", &n);

    double *x = calloc(n, sizeof(double));
    double *fx = calloc(n, sizeof(double));
    double diff[n][n];

    for (i = 0; i < n; i++)
    {
        printf("The value for X%d: ", i);
        scanf("%lf", &x[i]);
        printf("The value for F(X_%d): ", i);
        scanf("%lf", &fx[i]);
    }

    printf("Please enter the value you want to calculate: ");
    scanf("%lf", &wanted);

    h = x[1] - x[0];
    u = (wanted - x[0]) / h;

    // fill first column with fx
    for (i = 0; i < n; i++)
    {
        diff[i][0] = fx[i];
    }

    // calculate SONLU FARKALR
    for (int j = 1; j < n; j++)
    {
        for (int i = 0; i < n - j; i++)
        {
            diff[i][j] = diff[i + 1][j - 1] - diff[i][j - 1];
        }
    }

    // print table
    printf("\nSONLU FARKLAR TABLU :\n");
    for (i = 0; i < n; i++)
    {
        for (int j = 0; j < n - i; j++)
        {
            printf("%lf   ", diff[i][j]);
        }
        printf("\n\n");
    }

    h = x[1] - x[0];
    u = (wanted - x[0]) / h;

    // apply Gregory-Newton
    result = diff[0][0];

    for (int i = 1; i < n; i++)
    {
        u_term *= (u - (i - 1));
        result += (u_term * diff[0][i]) / fact(i);
    }

    printf("Estimated value at x = %lf is %lf\n", wanted, result);

    free(x);
    free(fx);
}

// ----------------------------------------------------------------------------------------------------------------------
int main()
{
    int choice;
    int n;
    double **matrix;
    // scanf("%d",&choice);

    while (1)
    {

        do
        {
            print_Menu();
            scanf("%d", &choice);
            if (choice < 0 || choice > 10)
            {
                printf("Invalid choice! Please try again.\n\n");
            }

        } while (choice < 0 || choice > 10);

        switch (choice)
        {
        case 0:
            printf("End of the program");
            return 0;
            break;
        case 1:
            Bisection();
            break;
        case 2:
            Regula_Falsi();
            break;
        case 3:
            Newton_Raphson(); // rememberr to edit
            break;
        case 4:
            printf("Please enter the value of NxN N: ");
            scanf("%d", &n);
            Matrix_Inverse(matrix, n);
            break;
        case 5:
            printf("Please enter the value of NxN N: ");
            scanf("%d", &n);
            Cholesky(n);
            break;
        case 6:

            printf("Please enter the value of NxN N: ");

            scanf("%d", &n);
            Gauss_Seidel(matrix, n);
            break;
        case 7:
            Sayisal_Turev();
            // system("cls");
            break;
        case 8:
            Simpson_Kurallari();
            // system("cls");
            break;
        case 9:
            trapez_yontemi();
            // system("cls");
            break;
        case 10:
            Gregory_Newton();
            // system("cls");
            break;
        default:
            printf("Invalid Choice!  Try again\n");
            break;
        }
    };

    return 0;
}
