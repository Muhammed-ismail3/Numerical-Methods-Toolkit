# Numerical Methods Toolkit

The **Numerical Methods Toolkit** is a comprehensive C-based command-line application designed to solve complex mathematical problems using standard numerical analysis algorithms. From finding roots of nonlinear equations to performing matrix operations and numerical integration, this tool provides a centralized suite for engineering and scientific calculations.

---

## ## Core Features

### 1. Root-Finding Algorithms
The toolkit can find the roots (zeros) of a user-defined function using several iterative methods:
* **Bisection Method**: A reliable "bracketed" method that repeatedly bisects an interval where a root exists.
* **Regula-Falsi (False Position)**: Uses linear interpolation between two points to converge on a root.
* **Newton-Raphson**: A fast-converging method that utilize the function's derivative to find approximations.

### 2. Linear Algebra & Matrix Operations
The program handles systems of linear equations and complex matrix manipulations:
* **Matrix Inversion**: Calculates the inverse of an $n \times n$ matrix using row reduction techniques.
* **Cholesky Decomposition (ALU)**: Decomposes a matrix into lower and upper triangular forms to solve for unknowns.
* **Gauss-Seidel Method**: An iterative technique used to solve large systems of linear equations by updating estimates.

### 3. Calculus & Interpolation
* **Numerical Differentiation (Sayisal Turev)**: Calculates derivatives using forward, backward, and central difference formulas.
* **Numerical Integration**:
    * **Simpson’s 1/3 and 3/8 Rules**: High-accuracy parabolic and cubic interpolation for finding the area under a curve.
    * **Trapezoidal Rule**: Approximates integration by dividing the area into trapezoids.
* **Gregory-Newton Interpolation**: Estimates unknown values between known data points using finite difference tables.

---

## ## Technical Implementation

### **Function Parser**
A standout feature of this toolkit is the **recursive descent parser**. Unlike basic programs that require hard-coded equations, this tool allows you to type functions directly into the console, such as: 
`sin(2*x) + log_2(x^4) - 2*x + e^2 + pi * x`.

### **Data Structures**
* **Stacks**: Two custom stack structures (`STACK` for numbers and `CHAR_STACK` for operators) are used to handle mathematical precedence (PEMDAS) during evaluation.
* **Dynamic Memory**: Matrices are allocated dynamically using `calloc` to handle varying user-defined dimensions safely.

---

## ## How to Use

### 1. Compilation
Use any standard C compiler (like GCC) to compile the source code:
```bash
gcc tmp.c -o math_toolkit -lm
