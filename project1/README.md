### High-level approach
This program requires to take 2 necessary arguments and 2 optional arguments. Argparse module is required for parsing input arguments. If there is no port given, 27993 would be the default port, and if -s flag is given, it uses SSL encrypted socket connection. If input incorrect number of arguments, it raises error. 
Then, it tries to connect to the given host and port. If connection fails, it raises error and exits.
Otherwise, it connects successfully and sends HELLO message to the server. 
While server is sending back FIND message, it keeps counting the number of given ASCII symbol in the given random characters, and send COUNT message to server. 
The loop process ends when server sends back BYE message, and it prints out the secret flag sent along with BYE message.
If the given husky id is not recogzied by the server, it prints out unknown husky id.
If encounter any incorrect number of fields or wrong message type in received message from server, it asserts error.      

### Challenges
This is the first time that I use python to programming. I always want to learn Python through truely implementing it and I think using it in this class will be a good practice.
The first chanllenge for me is to familiarize with the syntax of python and luckily, it was not bad.
Then, importing argparse module to parse 2 required arguments and 2 optional arguments accordingly took a bit time to implement as I never worked with this module before. 
Other than that, it's kind of tricky to read received message from the server as we need to read one message until '\n' appears. We need to continuingly append received message until we receive '\n'. 
Another thing that needs to be considered is where to throw errors. Thoroughly taking every edge case into consideration is kind of challenging.         

### Overview of testing code
I tested my code with inputting invalid arguments.  

