# EVARA Design
##### A one-time-pad messenging platform created by Christo Savalas - Harvard University
###### Email: csavalas@gmail.com

#### Background:
***
EVARA is an instant-messaging platform utilizing one-time-pad encryption. EVARA supports group chat, emojis, all unicode characters, attachments, previews of select MIME types, and instant, asynchronous updating of all connected clients for all inter-user actions. Decrypted messages, attachments, MIME-types, and filenames are never seen by the server; nor are any user utilized one-time-pad files.

#### One-Time-Pad Generation and Client/Server Handling:
***
As a convenience, EVARA contains a one-time-pad generator in the distribution (evara/pad_utility/create_pad.py), although an end-user can likely find more secure sources of random data. The included python script 'create_pad.py' generates random bytes (0-255) 1KB at a time using the token_bytes function from the 'secrets' library (which is substantially more secure than the built-in random library).

To keep track of which one-time-pad files correspond to which conversation, as well as to prevent re-use by clients, the EVARA client will generate the MD5 hash of the one time pad file upon creation of a conversation, and the server will store it.

#### Encryption:
***
In this type of encryption, the bytes of binary data one wishes to encrypt are XOR'd one-to-one with the bytes of the one-time-pad. To decrypt, one simply takes the ciphered bytes, and XOR's them again with the same bytes of the one-time-pad. For encrypting attachments, EVARA simply reads the binary data of the desired file and XOR's it with corresponding bytes from the pad. The process is slightly more involved for encrypting message data.

Javascript encodes strings as UTF-16 using two bytes per character (0-65535) and does not provide direct byte-by-byte binary access to the string data. The UTF-16 strings must therefore be converted character by character to integers, bitwise operators (bitshift, and, or) must be employed to split each 16 bit integer into two bytes (2 x 8 bits), and the resulting pair of bytes must then be XOR'd  with two corresponding bytes of the one-time-pad. This process is then reversed for decryption, using bitwise operators to reassamble the characters after XORing with pad data. Simply XOR'ing each character integer value of the UTF-16 string with one byte of pad data  would not have created a one to one relationship between the size of the pad and plaintext. (Rendering the encryption insecure by leaving the 8 most significant plaintext bits untouched per character.)

#### Back End (Server):
***
The back-end of EVARA is written in **Python** and utilizes the following libraries (among others):
* **Flask** - for basic web server functionality
* **Socketio** - for asynchronous communication between server and client
* **CS50-SQL (modified)** - In the distribution is a version of the CS50 SQL library I modified to not log results of database queries to the console, as it heavily slowed down queries with long string values.
* **login_required** - Detailed on pythonprogramming.net and utilized in CS50 Finance - used to wrap functions with the login requirement

##### NOTES:
* Since the server never gets to see the MIME type, file name, or data of any attachment, the server needed an alternative (but safe) way to decide whether or not to auto-download the attachment to the client when a given conversation containing said attachment is requested (for the purposes of displaying a thumbnail preview or an HTML5 audio player control). So instead of it being purely the server's decision, the client and server decide together:
	* When a client uploads an attachment, the client determines if it has a MIME type which should be _potentially_ auto-downloaded. If so, it indicates such with a boolean 1 to the server.
	* The server, given this information then checks to see if the attachment is within the size limit set out for auto-downloaded attachments. If and only if it is, will it be auto-downloaded by clients when requests are made of the attachment's parent conversation.

#### Front End (Client):
***
The front-end of EVARA is written in **Javascript** and utilizes the following libraries:
* **Bootstrap** - for minimal responsiveness (heavy supplementary js code was required to create responsiveness for the main messenger interface on mobile)
* **SPARKMD5** - for generating MD5 checksums of one-time-pad files
* **Socketio** - for asynchronous communication between server and client, and through the use of container divs and javascript, asynchronous updating of otherwise static HTML
* **Base64MIT** - for transmitting client's encrypted binary data to server without having to worry about invalid characters, escape sequences, etc.

##### NOTES:
* To mitigate against malicious user input (bad input or too large), I use client-side javascript validation, in addition to eventual server side validation, and finally input sanitation before any insert/update queries are performed.
* When the client sends a message to the server, it merely proposes an index with which it should have encrypted its data. The server can reject it, and instruct the client to resend at a new index to avoid pad overlap and race conditions.
* In order to display the attachment previews, the client downloads the encrypted data from the server, decrypts it, then uses the resulting byte array to create a blob, creates a URL of its address in memory, then uses that url as the value for the src tag in the img object or for an audio object.

#### Database Design:
***

The EVARA data from all users/conversations is stored in an SQL database with the following tables:
* conversations (Stores info on conversations)
	* id
	* name
	* current_index (index of the one-time-pad, clients need this to correctly encrypt data)
	* total_size (size of one-time-pad)
	* pad_hash (MD5 of pad, used to prevent duplicates and to validate correct pad usage client-side)
	* last_sent (Unix timestamp of last sent message in conversation)
* friendships (Care must be taken to correctly query this table given the lopsided relationship of the two users per row)
	* mutual
	* requester_id
	* requestee_id
* messages (Stores info on individual messages)
	* id
	* conv_id (Conversation membership)
	* from_id (Message is from which user id?)
	* pad_index (index of pad used to encrypt the plaintext)
	* msg_length
	* ciphertext (Encrypted data, base64 encoded)
	* unix_time
	* file_name (Encrypted filename, or NULL if not an attachment)
	* thumbable (Should the attachment be auto-downloaded?)
* participants (Used to grant conversation membership)
	* user_id
	* conv_id
	* last_read (last time this user sent a read receipt to server for this conversation)
* users
	* id
	* username
	* name
	* password (Stored as a salted hash)
	* email

##### NOTES:
* In order to store the raw binary data in the database, data is first encoded with base64 client-side, mitigating any encoding issues (base64 utilizes ASCII characters).


***

© 2020 Christo Savalas

