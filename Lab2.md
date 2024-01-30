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

In the screen shot above, the user has added the path `/add-message` and the qeuery `?s=This is an example of the server working&user=Sahana`. The `handleRequest()` method of the ChatHandler object takes in the new argument and recognizes that the substring "/add-message" can now be found within the URL. Consequently, the program uses multiple methods of the Url and String classes (the `getQuery()` and `split()` methods respectfully) to determine the user and the message they choose to give. These values are then appended into the ArrayList `ArrayList<String> chatLog`, whose functions is to store all messages given to the server, in the format: `User: message`". The `chatLog` field  having been changed now reflects the new message added, which in this case would've been "Sahana: This is an example of the server working, is returned to the `start()` method. This prints it to the server's browser so the user may see the new message reflected on their screen. 

*Note that in practice, all the spaces characters within the message have been replaced by `+`, as that is how Linux, the user's system, chooses to represent spaces in the URL, and no measueres were taken in the program replace these with spaces again. The `+` characters are excluded from the previous and future explanations for the sake of readability.


Example 2 utilizing Chat Server
![Screenshot_20240129_113538](https://github.com/Sa-Rangaraj/cse15l-lab-reports/assets/158000497/7c1888a8-ba5c-4546-ac0c-76bbaa749ed6)

Everytime a change is made to the URL, the `handleRequest()` is called an additional time to handle the new changes made by the user. For example, in the screenshot above, the URL was changed an altered an additional two times. As a result, the `handleRequest()` method would have been called twice, and each time the program would have found the `user` and `message` using the `getQuery()` and `split()` methods, and appended this information into the `chatLog` field. Each time this is done, the `chatLog` ArrayList is returned back to the `start()` method where it is printed in it's entierty, resulting in the browser showing the user a list of all the messages that have been created while the server has been active. 

In short, the same methods are called in this example as the first, but the `handleRequest()` is called more times in order to reflect the changes made to the server's URL. 

## Part 2
