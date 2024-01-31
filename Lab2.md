# Lab Report 2 - Servers and SSH Keys (Week 3)

## Part 1

*Implementation of the ChatServer class:*
```
import java.io.IOException;
import java.net.URI;
import java.util.ArrayList;

class ChatHandler implements URLHandler {
    ArrayList<String> chatLog = new ArrayList<>();

    public String handleRequest(URI url) {
        if (url.getPath().contains("/add-message")) {
            String[] parameters = url.getQuery().split("=");    //s will be the first element 
            String user = parameters[2];
            String[] message = parameters[1].split("&user");
            chatLog.add( user + ": " + message[0]);

            String formatedChatLog = "";

            for (int i = 0; i < chatLog.size(); i++){
                formatedChatLog += chatLog.get(i) + "\n";
            }

            return formatedChatLog;

        }
        return "404 Not Found!";
    }
}

class ChatServer {
    public static void main(String[] args) throws IOException {
        if(args.length == 0){
            System.out.println("Missing port number! Try any number between 1024 to 49151");
            return;
        }

        int port = Integer.parseInt(args[0]);

        Server.start(port, new ChatHandler());
    }
}
```

In the program above, the main method of `~/wavelet/ChatServer.java` first calls the method `parseInt()` of the Integer class build into java, in order to take the frist command line argument and cast it to an int. This argument provided by the user when running `~/wavelet/ChatServer.java` is the port number the user intends the program to use to host the sever. 

The `start()` method from the Server class: `~/wavelet/Server.java` is then used to start the server the server on the local system. It takes 2 arguments, the port number provided by the command line argument, and an object of the ChatHandler class, located within `~/wavelet/ChatServer.java`; As mentioned previously, the interger argument is used to proivde a specific port number for the system to use. The `ChatHandle` object is used to read the server's URL, and determine the users desier message by reading their additions to the browser's path.


![Screenshot_20240129_113635](https://github.com/Sa-Rangaraj/cse15l-lab-reports/assets/158000497/6cd7ae68-8b8a-41ee-9a88-de66911c0101)

In the screen shot above, the user has added the path `/add-message` and the qeuery `?s=This is an example of the server working&user=Sahana`. The `handleRequest()` method of the ChatHandler object takes in the new argument and recognizes that the substring `"/add-message"` can now be found within the URL. As a result, the program then aims determine the user and the message they choose to give. This is done by creating the array field `parameters`, which stores the contents of the URL's query (using the `getQuery()` method of the URI class) split at every occurance of a `"="` (which is done using the (`split()` method of the string class passing the char `'='` as an argument). In the example above, the first (0 index) element of `paparameters` would be `"s"`, as it is the substring from the index after the query `"?"` up until the `"="`. Following this logic, the second (1 index) element contains `"This is an example of the server working&user"` and the third (2 index) `"Sahana"`. As this is where the URL string ends, this is where the array ends as well. 

Following this, the program now "sorts" the input to make it more managable to code with later on. The third element (2 index) of `parameter` is direcly copied into a string field called `user`, as the element stores nothing other than the username of the chatter. However, the second element (1 index) which holds the user's message also has an aditional substring "&user" at the end, due to the way the query was split up. In order to get the message alone, the `split()` method is called once again, this time with `"&user"` as the argument, to intitalize an array field `message`, which only holds one element: the message we just split from the string. In the example above, this element would hold `"This is an example of the server working"`.

These fields are then appended into the ArrayList `ArrayList<String> chatLog`, whose functions is to store all messages made with the server, in the format: "User: message" (in the program user and message replaced with the fields `user` and `message[0]` respectivly. The `chatLog` field  having been changed now reflects the new message added, which in this case would've been `"Sahana: This is an example of the server working"`, is returned to the `start()` method. This prints it to the server's browser so the user may see the new message reflected on their screen. 

*Note that in practice, all the spaces characters within the message have been replaced by `+`, as that is how Linux, the user's system, chooses to represent spaces in the URL, and no measueres were taken in the program replace these with spaces again. The `+` characters are excluded from the previous and future explanations for the sake of readability.


Example 2 utilizing Chat Server
![Screenshot_20240129_113538](https://github.com/Sa-Rangaraj/cse15l-lab-reports/assets/158000497/7c1888a8-ba5c-4546-ac0c-76bbaa749ed6)

Everytime a change is made to the URL, the `handleRequest()` is called an additional time to handle the new changes made by the user. For example, in the screenshot above, the URL was changed an altered an additional two times. As a result, the `handleRequest()` method would have been called twice, and each time the program would have found the new `user` and `message` used the `getQuery()` and `split()` methods, and appended this information into the `chatLog` field. 

To be more specific, when the user adds the path `/add-message?s=This is an example of the server storing multiple messages&user=Sahana` the `parameter` field now contains the elements `{"s", "This is an example of the server storing multiple messages&user", "Sahana"}`. Note that although the first and last element are visually the same (because `"?s="` is the format of the querey and the user hasn't happened to changed) the values wuthin the code are being updated and repleaced each time. After the helper fields are updated: `user` once again is newly set to contain `"Sahana"` and `message[0]` contains `"This is an example of the server storing multiple messages&user"`. These fields are appended to the Arraylist chatLog which now contains `{"Sahana: This is an example of the server working", "Sahana: This is an example of the server storing multiple messages"}`. Note that unlike the previous fields it has not changed it's value completely to reflect the new message, rather the new message has just been appended after the previous one. This is so the field maintains its functionality as a proper chat log and keep track of all the messages, not just the most recent one. After, the `chatLog` ArrayList is returned back to the `start()` method where it is printed in it's entierty, resulting in the browser showing the user a list of all the messages that have been created while the server has been active. 

This process takes place once more when the user changes the URL a third time. The changes in the arguments and fields are as follows: 
    
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;New Path and Query: `"/add-message?s=This is an example of the server working with different users&user=Someone else"`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`parameter` array: `{"s", "This is an example of the server working with different users", "Someone else"}`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`user` string: `"Someone else"`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`message` array: `{"This is an example of the server working with different users"}`

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`chatLog` ArrayList: `{"Sahana: This is an example of the server working", "Sahana: This is an example of the server storing multiple messages", "This is an example of the server working with different users"}`
    


In short, the same methods are called in this example as the first, but the `handleRequest()` is called more times in order to reflect the changes made to the server's URL. 

## Part 2
*Private key*

![image](https://github.com/Sa-Rangaraj/cse15l-lab-reports/assets/158000497/fcfb9e92-eada-4690-a95d-eda6c5553135)

*Public key*

![image](https://github.com/Sa-Rangaraj/cse15l-lab-reports/assets/158000497/73abc3af-9194-4f6e-b8cd-677b3b8f2604)

*Logging in with ssh without password*

![image](https://github.com/Sa-Rangaraj/cse15l-lab-reports/assets/158000497/05f915ee-5bd5-41c9-bc3a-f43004edf3b7)


## Part 3

Something new I learned in lab from weeks 2-3 was how to log into remote computers from one's terminal. I learned that by using the `ssh` command and providing it our server username, we can connect to that server remotly. Once logged in, our terminal acts as if we are in a completely different system; we no longer have acess to our previous directories, but we now have acess to directories that we were not on our machine before. Additionally, I learned that by running the command `ssh-keygen` we could create a public key, which allows us to log into our desired server without having to provide a password each time. 
