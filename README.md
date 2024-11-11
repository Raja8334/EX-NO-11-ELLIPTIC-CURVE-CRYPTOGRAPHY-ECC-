# EX-NO-11-ELLIPTIC-CURVE-CRYPTOGRAPHY-ECC

## Aim:
To Implement ELLIPTIC CURVE CRYPTOGRAPHY(ECC)


## ALGORITHM:

1. Elliptic Curve Cryptography (ECC) is a public-key cryptography technique based on the algebraic structure of elliptic curves over finite fields.

2. Initialization:
   - Select an elliptic curve equation \( y^2 = x^3 + ax + b \) with parameters \( a \) and \( b \), along with a large prime \( p \) (defining the finite field).
   - Choose a base point \( G \) on the curve, which will be used for generating public keys.

3. Key Generation:
   - Each party selects a private key \( d \) (a random integer).
   - Calculate the public key as \( Q = d \times G \) (using elliptic curve point multiplication).

4. Encryption and Decryption:
   - Encryption: The sender uses the recipient’s public key and the base point \( G \) to encode the message.
   - Decryption: The recipient uses their private key to decode the message and retrieve the original plaintext.

5. Security: ECC’s security relies on the Elliptic Curve Discrete Logarithm Problem (ECDLP), making it highly secure with shorter key lengths compared to traditional methods like RSA.

## Program:

```c
#include <stdio.h> 
#include <stdint.h> 
#define PRIME 17 // Define a small prime for the field 
// ECC Parameters for the curve y^2 = x^3 + ax + b 
const int a = 2; 
const int b = 2; 
// Point structure 
typedef struct { 
int x; 
int y; 
} Point; 
// Modular inverse function using the Extended Euclidean Algorithm 
int mod_inverse(int k, int p) { 
k = k % p; 
for (int x = 1; x < p; x++) { 
    if ((k * x) % p == 1)
        return x; 
  } 
   return -1; // Return -1 if no modular inverse exists 
 } 
// Point doubling function 
Point point_double(Point P) { 
if (P.y == 0)
  return (Point){0, 0}; 
int m = ((3 * P.x * P.x + a) * mod_inverse(2 * P.y, PRIME)) % PRIME; 
int x3 = (m * m - 2 * P.x) % PRIME; 
int y3 = (m * (P.x - x3) - P.y) % PRIME; 
   if (x3 < 0) x3 += PRIME; 
   if (y3 < 0) y3 += PRIME; 
return (Point){x3, y3}; 
} 
// Point addition function 
Point point_add(Point P, Point Q) { 
   if (P.x == 0 && P.y == 0)
     return Q; 
   if (Q.x == 0 && Q.y == 0) 
     return P; 
   if (P.x == Q.x && P.y == -Q.y)
     return (Point){0, 0}; 
int m = ((Q.y - P.y) * mod_inverse(Q.x - P.x, PRIME)) % PRIME; 
int x3 = (m * m - P.x - Q.x) % PRIME; 
int y3 = (m * (P.x - x3) - P.y) % PRIME; 
if (x3 < 0) x3 += PRIME; 
if (y3 < 0) y3 += PRIME; 
  return (Point){x3, y3}; 
} 
// Scalar multiplication function 
Point scalar_mult(int k, Point P) { 
Point R = {0, 0}; // Initialize to the point at infinity 
Point Q = P; 
while (k > 0) { 
if (k % 2 == 1) { 
R = point_add(R, Q); 
} 
Q = point_double(Q); 
k /= 2; 
} 
return R; 
} 
int main() { 
Point G = {5, 1}; // Base point on the curve 
int k = 2; // Scalar multiplier 
Point R = scalar_mult(k, G); 
printf("Result of %d * G = (%d, %d)\n", k, R.x, R.y); 
return 0; 
}
```

## Output:
![Screenshot_2024-11-11_102636 1](https://github.com/user-attachments/assets/ff3e6691-bf97-4af6-9913-85de78f510b9)


## Result:
The program is executed successfully

