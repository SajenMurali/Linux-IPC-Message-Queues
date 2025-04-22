# Linux-IPC-Message-Queues
Linux IPC-Message Queues
# Developed by: purajith s
# Register number: 212223040158

# AIM:
To write a C program that receives a message from message queue and display them

# DESIGN STEPS:

### Step 1:

Navigate to any Linux environment installed on the system or installed inside a virtual environment like virtual box/vmware or online linux JSLinux (https://bellard.org/jslinux/vm.html?url=alpine-x86.cfg&mem=192) or docker.

### Step 2:

Write the C Program using Linux message queues API 

### Step 3:

Execute the C Program for the desired output. 

# PROGRAM:

## C program that receives a message from message queue and display them
writer.c
```
#include <stdio.h> 
#include <sys/ipc.h> 
#include <sys/msg.h> 

// structure for message queue 
struct mesg_buffer { 
    long mesg_type; 
    char mesg_text[100]; 
} message; 

int main() { 
    key_t key; 
    int msgid; 

    // ftok to generate unique key 
    key = ftok("progfile", 65); 

    // msgget creates a message queue and returns identifier 
    msgid = msgget(key, 0666 | IPC_CREAT); 
    message.mesg_type = 1; 

    printf("Write Data : "); 
    fgets(message.mesg_text, sizeof(message.mesg_text), stdin);

    // msgsnd to send message 
    msgsnd(msgid, &message, sizeof(message), 0); 

    // display the message 
    printf("Data send is : %s\n", message.mesg_text); 
    return 0; 
}
```
reader.c
```
#include <stdio.h>
#include <sys/ipc.h>
#include <sys/msg.h>

// structure for message queue
struct mesg_buffer {
    long mesg_type;
    char mesg_text[100];
} message;

int main() {
    key_t key;
    int msgid;

    // ftok to generate unique key
    key = ftok("progfile", 65);

    // msgget creates a message queue and returns identifier
    msgid = msgget(key, 0666 | IPC_CREAT);

    // msgrcv to receive message
    msgrcv(msgid, &message, sizeof(message), 1, 0);

    // display the message
    printf("Data Received is : %s\n", message.mesg_text);

    // to destroy the message queue
    msgctl(msgid, IPC_RMID, NULL);

    return 0;
}
```



## OUTPUT
![image](https://github.com/user-attachments/assets/f007ce57-4942-4b5b-a1a5-a3c6ca452bae)


# RESULT:
The programs are executed successfully.
