# MediaServer.java
This file handles all of the server functions for this social media application. It runs a main method which waits for new users/clients to try to connect to the server. When a new client connects to the server, it creates a new thread that interacts with that client exclusively. 

This class has two fields: 
```java
    private static UserDatabase database;
    public static final Object lock = new Object();
```
The first field is just the user database which is used to find the client and verify their userinformation. The second field is just the object which is used for thread synchronization. 

This class has a run method for threading:
```java
    private static void run(Socket client, ServerSocket server);
```
The run method takes in the client socket and the server socket to be used for the networking input and output streams. Upon user connection, the server first tries to parse the user account information and create a new account needed. Then the operations are passed to the client or user handlers. Direct messages are updated periodically (currently set to 3 seconds) with a TimerTask and sent back to the user.


To handle incoming client messages, the server class handles the operations seperately depending on whether it involves the message or user database:
```java
    public void messageHandling(PrintWriter writer, String line, MessageDatabase database, User viewing);
    public void userHandling(PrintWriter writer, String line);
 ```

The message handler takes in the writer, the input line recieved from the client, the message database of the user, and the user of the DM chat that the client is looking at. It can perform the operations of getting the user's current conversation, sending a message, deleting a message, editing a message, and setting the who the client is viewing.

The enum operations for the message handler include (from Action.java): GET_CONVERSATION, SEND_MESSAGE, DELETE_MESSAGE, EDIT_MESSAGE, and SET_VIEWING

The user handler takes in the writer and the input line recieved from the client. It can perform the operations of searching for a user, adding a friend, removing a friend, blocking a user, changing a username, and unblocking a user

The enum operations for the user handler include (from Action.java): SEARCH, ADD_FRIEND, REMOVE_FRIEND, BLOCK, CHANGE_USERNAME, UNBLOCK

# Threading 
Threading in this class is handled slightly differently than threading from previous homeworks/projects. To avoid making a new object each time a client connects to the server and handling interactions within that object (in another java class), we use lambda functions to create a new thread directly with the run method included in the MediaServer.java class. More about this is described here [threading.md](threading.md).

# Running the Network IO
Start by running the Mediaserver with 
```bash 
$ MediaServer.java 8080
```
with 8080 signifying the port of the server. This can be changed but you will have to change the port that UserClient attempts to connect to (which can be done the same way). 

After this, run clients and create/login for a user account, before following the instructions on the terminal for each client for each function

# Interface
There is an interface for MediaServer.java located at [ServerInterface.java](../ServerInterface.java). This interface designates all of the methods and fields that the MediaServer.java class must handle. These methods/fields are listed below: 
```java
    static UserDatabase database = new UserDatabase();
    static final Object lock = new Object();

    static void userHandling(PrintWriter writer, String line) {
    };

    static void messageHandling(PrintWriter writer, String line, MessageDatabase database, User viewing) {
    };

    static void run(Socket client, ServerSocket server) {
    };
```
This looks a little different than the other interfaces because the methods are static, so they have an empty body. 