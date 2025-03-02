
## Assignment 01 (6p)
# Programming With C language


Install gcc

Make a small C program "mycalc.c" to ask user 2 numbers, and print the sum
Install Node.js and npm. Make a new directory "myserver", cd there and install Express web server
Copy example javascript code to "myserver.js" and modify it to use also route "/user" that returns information about client user (Hint: use process.env)
Install python3 and pip
install bpytop (, run, and take screenshot)
Return in one zip file containing mycalc.c and myserver.js, and screenshot (jpg/png) from bpytop screen

1. Install GCC (GNU Compiler Collection)

To install GCC, use the following commands based on your operating system:

        sudo apt update
        
        sudo apt install gcc

![image](https://github.com/user-attachments/assets/e3cc9394-a4f7-4924-baf3-307ff8a97c9f)


Install gcc

Already have installed gcc compiler we do not need to install the compiler again.

        - gcc --version

![image](https://github.com/user-attachments/assets/57cc7fcc-b916-4b21-a1e4-c613c96105a2)


Creating and Writing the C Program (mycalc.c)

Navigate to your directory 
        mkdir -p calculator && cd calculator

![image](https://github.com/user-attachments/assets/698865ff-223e-49b9-86a3-b0b81b9e59e1)


Create the mycalc.c file using nano:
        nano mycalc.c

![image](https://github.com/user-attachments/assets/a5df0db8-4220-42fa-9080-4d7564239d5e)



Write a basic Hello World program first:

#include <stdio.h>
int main() {
    printf("Hello, World!\n");
    return 0;
}

        Save (Ctrl + S) and exit (Ctrl + X).

Compile the Hello World program:

        gcc mycalc.c -o mycalc

![image](https://github.com/user-attachments/assets/2ddf02c3-8e95-4f94-8945-571023ce2d8e)



## Now we can modify to add our calculator codes.


Updating mycalc.c to Perform Addition

Open mycalc.c 

        nano mycalc.c

Modify the code to:

#include <stdio.h>

int main() {
    int num1, num2, sum;
    printf("Enter two numbers: ");
    scanf("%d %d", &num1, &num2);
    sum = num1 + num2;
    printf("Sum: %d\n", sum);
    return 0;
}

Save (Ctrl + S) and exit (Ctrl + X).

![image](https://github.com/user-attachments/assets/e4f5c6fc-6957-484b-8691-b6a5d5204c1e)


Recompile:

        gcc mycalc.c -o mycalc

Run the updated program:

        ./mycalc

![image](https://github.com/user-attachments/assets/8fb4c5d7-f588-4e75-8eb5-af23f0b94b2d)

### Install Node.js and npm

```bash
sudo apt-get install nodejs npm
```
### Set up Express web server
- To create a new directory:
```bash
mkdir myserver && cd myserver
```
- Initialize npm:
```bash
npm init -y
```
- to install Express:
```bash
npm install express
```
![image](https://github.com/user-attachments/assets/c1ff2b88-e3af-4444-b8b4-f5988c46971d)

### Create myserver.js and add the following code:
```bash
const express = require('express');
const app = express();
const port = 3000;

app.get('/', (req, res) => {
    res.send('Hello World!');
});

app.get('/user', (req, res) => {
    res.json({
        user: process.env.USER || 'unknown',
        home: process.env.HOME || 'unknown'
    });
});

app.listen(port, () => {
    console.log(`Server running at http://localhost:${port}/`);
});
```
- To run the server:
```bash
node myserver.js
```


### Install Python3 and pip
```bash
sudo apt-get update
```
![image](https://github.com/user-attachments/assets/9fbe461d-13e4-4fab-90a6-55b4b005b6c0)


```bash
sudo apt-get install python3 python3-pip
```


```bash
 pip install bpytop
```
![image](https://github.com/user-attachments/assets/419c3078-8c5f-4f2c-ba4e-869e84f4f7cd)

