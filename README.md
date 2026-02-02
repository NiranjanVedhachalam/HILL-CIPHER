# HILL CIPHER
HILL CIPHER
EX. NO: 3 AIM:
 

IMPLEMENTATION OF HILL CIPHER
 
## To write a C program to implement the hill cipher substitution techniques.

## DESCRIPTION:

Each letter is represented by a number modulo 26. Often the simple scheme A = 0, B
= 1... Z = 25, is used, but this is not an essential feature of the cipher. To encrypt a message, each block of n letters is  multiplied by an invertible n × n matrix, against modulus 26. To
decrypt the message, each block is multiplied by the inverse of the m trix used for
 
encryption. The matrix used
 
for encryption is the cipher key, and it sho
 
ld be chosen
 
randomly from the set of invertible n × n matrices (modulo 26).


## ALGORITHM:

STEP-1: Read the plain text and key from the user. STEP-2: Split the plain text into groups of length three. STEP-3: Arrange the keyword in a 3*3 matrix.
STEP-4: Multiply the two matrices to obtain the cipher text of length three.
STEP-5: Combine all these groups to get the complete cipher text.

## PROGRAM 
```
#include <stdio.h>
#include <string.h>
#include <ctype.h>
#include <math.h>

#define MOD 26
#define MAX 10

int mod(int x) {
    return (x % MOD + MOD) % MOD;
}

/* Extended Euclidean Algorithm */
int modInverse(int a) {
    a = mod(a);
    for (int i = 1; i < MOD; i++)
        if ((a * i) % MOD == 1)
            return i;
    return -1;
}

/* Determinant (recursive) */
int determinant(int mat[MAX][MAX], int n) {
    if (n == 1) return mat[0][0];
    if (n == 2)
        return mat[0][0]*mat[1][1] - mat[0][1]*mat[1][0];

    int det = 0, temp[MAX][MAX];
    for (int c = 0; c < n; c++) {
        int ti = 0, tj = 0;
        for (int i = 1; i < n; i++) {
            tj = 0;
            for (int j = 0; j < n; j++) {
                if (j == c) continue;
                temp[ti][tj++] = mat[i][j];
            }
            ti++;
        }
        det += (c % 2 == 0 ? 1 : -1) * mat[0][c] * determinant(temp, n-1);
    }
    return det;
}

/* Matrix inverse modulo 26 */
void matrixInverse(int mat[MAX][MAX], int inv[MAX][MAX], int n) {
    int det = mod(determinant(mat, n));
    int invDet = modInverse(det);

    int temp[MAX][MAX], adj[MAX][MAX];

    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            int ti = 0, tj = 0;
            for (int r = 0; r < n; r++) {
                if (r == i) continue;
                tj = 0;
                for (int c = 0; c < n; c++) {
                    if (c == j) continue;
                    temp[ti][tj++] = mat[r][c];
                }
                ti++;
            }
            int sign = ((i + j) % 2 == 0) ? 1 : -1;
            adj[j][i] = mod(sign * determinant(temp, n-1));
        }
    }

    for (int i = 0; i < n; i++)
        for (int j = 0; j < n; j++)
            inv[i][j] = mod(adj[i][j] * invDet);
}

/* Matrix × Vector */
void multiply(int mat[MAX][MAX], int vec[MAX], int res[MAX], int n) {
    for (int i = 0; i < n; i++) {
        res[i] = 0;
        for (int j = 0; j < n; j++)
            res[i] += mat[i][j] * vec[j];
        res[i] = mod(res[i]);
    }
}

int main() {
    char key[100], text[100];
    int K[MAX][MAX], Kinv[MAX][MAX];
    int nums[100], block[MAX], out[MAX];
    char choice;
    int len = 0;

    printf("Enter key (length must be perfect square): ");
    scanf("%s", key);

    int klen = strlen(key);
    int n = sqrt(klen);

    if (n * n != klen) {
        printf("Invalid key length!\n");
        return 0;
    }

    int idx = 0;
    for (int i = 0; i < n; i++)
        for (int j = 0; j < n; j++)
            K[i][j] = toupper(key[idx++]) - 'A';

    printf("Encrypt or Decrypt (E/D): ");
    scanf(" %c", &choice);

    printf("Enter text: ");
    scanf("%s", text);

    for (int i = 0; text[i]; i++)
        if (isalpha(text[i]))
            nums[len++] = toupper(text[i]) - 'A';

    while (len % n != 0)
        nums[len++] = 0;   // padding A

    if (choice == 'D')
        matrixInverse(K, Kinv, n);

    printf("\nResult: ");
    for (int i = 0; i < len; i += n) {
        for (int j = 0; j < n; j++)
            block[j] = nums[i + j];

        if (choice == 'E')
            multiply(K, block, out, n);
        else
            multiply(Kinv, block, out, n);

        for (int j = 0; j < n; j++)
            printf("%c", out[j] + 'A');
    }

    return 0;
}
```
## OUTPUT
<img width="819" height="622" alt="image" src="https://github.com/user-attachments/assets/2fed2d5d-ffde-4545-8acb-40363c3f48df" />


## RESULT

Thus the python program to implement the hill cipher substitution techniques is completed and successfully executed
