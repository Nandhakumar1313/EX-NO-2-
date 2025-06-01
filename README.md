## EX. NO:2 IMPLEMENTATION OF PLAYFAIR CIPHER

 

## AIM:
 

 

To write a C program to implement the Playfair Substitution technique.

## DESCRIPTION:

The Playfair cipher starts with creating a key table. The key table is a 5×5 grid of letters that will act as the key for encrypting your plaintext. Each of the 25 letters must be unique and one letter of the alphabet is omitted from the table (as there are 25 spots and 26 letters in the alphabet).

To encrypt a message, one would break the message into digrams (groups of 2 letters) such that, for example, "HelloWorld" becomes "HE LL OW OR LD", and map them out on the key table. The two letters of the diagram are considered as the opposite corners of a rectangle in the key table. Note the relative position of the corners of this rectangle. Then apply the following 4 rules, in order, to each pair of letters in the plaintext:
1.	If both letters are the same (or only one letter is left), add an "X" after the first letter
2.	If the letters appear on the same row of your table, replace them with the letters to their immediate right respectively
3.	If the letters appear on the same column of your table, replace them with the letters immediately below respectively
4.	If the letters are not on the same row or column, replace them with the letters on the same row respectively but at the other pair of corners of the rectangle defined by the original pair.
## EXAMPLE:
![image](https://github.com/Hemamanigandan/EX-NO-2-/assets/149653568/e6858d4f-b122-42ba-acdb-db18ec2e9675)

 

## ALGORITHM:

STEP-1: Read the plain text from the user.
STEP-2: Read the keyword from the user.
STEP-3: Arrange the keyword without duplicates in a 5*5 matrix in the row order and fill the remaining cells with missed out letters in alphabetical order. Note that ‘i’ and ‘j’ takes the same cell.
STEP-4: Group the plain text in pairs and match the corresponding corner letters by forming a rectangular grid.
STEP-5: Display the obtained cipher text.

## Program:
```
SIZE = 5

def generate_key_matrix(key):
    key = key.upper().replace('J', 'I')
    seen = set()
    filtered_key = ''

    for char in key:
        if char.isalpha() and char not in seen:
            seen.add(char)
            filtered_key += char

    for ch in map(chr, range(ord('A'), ord('Z') + 1)):
        if ch == 'J': continue
        if ch not in seen:
            filtered_key += ch

    matrix = [[None]*SIZE for _ in range(SIZE)]
    index = 0
    for i in range(SIZE):
        for j in range(SIZE):
            matrix[i][j] = filtered_key[index]
            index += 1

    return matrix

def find_position(matrix, ch):
    if ch == 'J':
        ch = 'I'
    for i in range(SIZE):
        for j in range(SIZE):
            if matrix[i][j] == ch:
                return i, j
    return None, None

def process_digraph(a, b, matrix, encrypt=True):
    row1, col1 = find_position(matrix, a)
    row2, col2 = find_position(matrix, b)

    if row1 == row2:  # Same row
        if encrypt:
            return matrix[row1][(col1 + 1) % SIZE], matrix[row2][(col2 + 1) % SIZE]
        else:
            return matrix[row1][(col1 - 1) % SIZE], matrix[row2][(col2 - 1) % SIZE]
    elif col1 == col2:  # Same column
        if encrypt:
            return matrix[(row1 + 1) % SIZE][col1], matrix[(row2 + 1) % SIZE][col2]
        else:
            return matrix[(row1 - 1) % SIZE][col1], matrix[(row2 - 1) % SIZE][col2]
    else:  # Rectangle
        return matrix[row1][col2], matrix[row2][col1]

def preprocess_text(text):
    text = text.upper().replace('J', 'I')
    cleaned = ''.join([ch for ch in text if ch.isalpha()])
    result = ''
    i = 0
    while i < len(cleaned):
        a = cleaned[i]
        b = ''
        if i + 1 < len(cleaned):
            b = cleaned[i + 1]
            if a == b:
                b = 'X'
                i += 1
            else:
                i += 2
        else:
            b = 'X'
            i += 1
        result += a + b
    return result

def encrypt_decrypt_text(text, matrix, encrypt=True):
    processed = preprocess_text(text)
    result = ''
    for i in range(0, len(processed), 2):
        a, b = processed[i], processed[i+1]
        res_a, res_b = process_digraph(a, b, matrix, encrypt)
        result += res_a + res_b
    return result

def print_matrix(matrix):
    print("Key Matrix:")
    for row in matrix:
        print(' '.join(row))

def main():
    key = input("Enter the key: ")
    text = input("Enter text to encrypt: ")

    matrix = generate_key_matrix(key)
    print_matrix(matrix)

    encrypted = encrypt_decrypt_text(text, matrix, encrypt=True)
    print("Encrypted Text:", encrypted)

    decrypted = encrypt_decrypt_text(encrypted, matrix, encrypt=False)
    print("Decrypted Text:", decrypted)

if __name__ == "__main__":
    main()

```
## Output:
![image](https://github.com/user-attachments/assets/fbb59576-72b4-4a1f-ab2e-6aaa4462e9bb)

## Result:
the implementation of playfair cipher was executed successfully
