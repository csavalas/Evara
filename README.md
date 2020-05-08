# EVARA Documentation
##### A one-time-pad messenging platform created by Christo Savalas - Harvard University
###### Email: csavalas@gmail.com

#### Installation:
***
1. Upload 'evara.zip' to your home directory on CS50 IDE (~/)
2. Unzip evara.zip to your home directory
3. Execute the following terminal commands to install prerequisite libraries:
	1. cd
	2. cd evara/server
	3. pip install -r requirements.txt

#### Running Evara:
***
1. Execute the following terminal commands to start the Evara messenging platform:
	1. cd
	2. cd evara/server
	3. python application.py
2. In the upper lefthand corner of the IDE, click 'CS50 IDE', then 'Web Server'
3. Take note of the URL in resulting tab. This is the URL clients use to access Evara

#### Using Evara
***
###### NOTE: Only Google Chrome for desktop and mobile are currently supported

##### Background
One-time-pad encryption requires the creation of a string of truly random bytes of the same length as the data which is to be encrypted. This string of random bytes, or 'one-time-pad', must be shared **in person**, in advance, by all users who wish to communicate with one another. The pad must **never be re-used** and must **never be transmitted electronically**.

##### Pad Creation
Evara includes a one-time-pad generator, although you are free to use other sources of random data for your pads. To create a one-time-pad in Evara:

###### NOTE: For the purposes of instant messaging with a one-time-pad, it is necessary to create a pad the size of which can support the **entirety** of all anticipated future communication with a user or group of users. Remember, pads should not be re-used, so once a pad runs out, a new one must be created and shared securely.

1. Execute the following terminal commands to start the create-pad tool:
	1. cd
	2. cd evara/pad_utility
	3. python create_pad.py
2. You will be prompted to:
	1. Name your pad file (otp extension will be appended if none provided)
	2. Enter pad size in MB (Max allowed size is 25MB)
3. This resulting pad will be saved in the current working directory
4. Delete the pad from the server after downloading it to your local system.
5. Share pad with people with which you wish to communicate over Evara

##### Registering a new user
To register a new user, simply visit the URL collected in the 'Running Evara' section of this document from desktop or mobile Chrome. You will be presented with a login prompt. Using the navigation bar, navigate to 'register', and enter your information on the resulting page. Upon completion, click 'Register!' to be taken to the main messenger interface.

###### NOTE: The main interface may look a little lonely without any friends or conversations! Read 'Demo Users and Pads', and then proceed to 'Exploring functionality'

##### Demo Users and Pads
To demo functionality, Evara contains 5 demo users named Alice, Bob, Carol, Dave, and Eve. While Evara enforces password strength, the passwords to the demo users are all '1'.

The project also contains two demo pads ('no_more_secrets.otp' and 'off_the_record.otp') located in '/evara/pad_utility' which correspond to two demo conversations between [Alice, Bob, Carol] AND [Alice and Carol], respectively.

###### NOTE: Neither the demo users nor the demo pads should be used outside of a demo setting.

#### Exploring Functionality
***
###### NOTE: While this section is divided into individual subsections, it is best to go through them in order

##### Logging in
To explore the different functions of Evara, let's use the built-in demo account, 'alice'.
* Visit the URL collected in the 'Running Evara' section of this document from desktop or mobile Chrome.
* Login with the username 'alice' and password '1'.

* ###### Optional: In order to experience the instant effects of one user's interactions with another user, open an addtional window in incognito mode, and login with the credentials (user: 'carol' pass: '1')

##### Friends and Requests
Alice is now logged in, and it looks like she has a friend request, let's check it out:
* Click on 'You have new friend requests'
* Click on 'Dave Deeb' and accept his friendship in the following confirmation dialog

You are now friends with Dave Deeb. Let's see if we can make some more friends!
* Click 'Find Friends'
* As your search query, enter 'Eve'

'Eve Edwards' appears in the results. Note that Eve could also have been found by her full name or email.
* Click on her entry, and confirm the following dialog to send a friend request.
* ###### Optional: To accept the request, login to the built-in 'eve' account.
* Click 'Back' to return to your main Friends/Requests list

##### Creating a conversation
Let's create a new conversation with all of our friends:
* Select Bob, Carol, and Dave from the list by tapping each of their entries
* Tap the pen icon in the lower righthand corner
* Select a one-time-pad file. If you don't already have a pad file, create one for this demo by following the instructions under 'Pad Creation' above.
* Enter a name for your conversation (Note: the name of the conversation is **NOT** encrypted with the pad, so it should not contain sensitive information)
* Tap 'Create' to create your new conversation
* You will be redirected to the messenger pane, where you will select the same one-time-pad file to access the newly created conversation.
* On mobile, you will be redirected, but not be automatically prompted. Simply tap the name of your new conversation from the list and select the file from the resulting dialog.

##### Conversing
We just created a new conversation, but there's not much to see. Let's instead take a look at an existing conversation between Alice, Bob, and Carol.
* In the CS50 IDE, download the one-time-pad file 'no_more_secrets.otp' located in 'evara/pad_utility' to your local machine
* **Mobile only:** Use the navigation bar and tap 'Conversations' to toggle the conversations pane
* Tap 'No More Secrets' in the conversations pane.
* When prompted, select the 'no_more_secrets.otp' pad file you previously downloaded.

You should now be able to see the conversation history between Alice, Bob, and Carol.
* Scroll through the messages for a sampling of different message formats

Try sending your own textual message:
* Enter text into the textarea box near the bottom of the interface
* All unicode characters are fully supported, ie. emojis, non-Western characters
* Both the paper airplane button or CTRL + ENTER will send your message.

Try sending an attachment
* Tap the paperclip icon
*  When prompted, select your desired attachment with the following notes:
	* Attachments, by default, are limited to 4 MB
	* There is no restriction on attachment file type
	* In addition to the file itself, the filename is also encrypted
	* Attachment thumbnail previews or audio previews are generated for items up to 3 MB and of type:
		* '.bmp', '.gif', '.jpg', '.jpeg', '.webp', '.png', '.svg', '.mp3', '.ogg', '.wav', '.webm', '.m4a'

##### Account Management
Let's manage Alice's account:
* Using the upper navigation bar, tap on Manage
* Change Username:
	* Enter a new username, and tap 'Change Username'
* Change Password
	* Enter a new password, confirm it, then tap 'Change Password'
* Delete Account (Including all messages, friendships/requests)
	* Enter 'delete me' exacty as it is written, then tap 'Delete Account'
	* ###### Optional: Before deleting Alice's account, login into Carol's account in an incognito window, then load up the 'No More Secrets' conversation, so you can watch Alice's messages/attachments instantly disappear when you delete her account in the main window.

###### NOTE: Should you ever wish to revert to the factory database with all demo users, delete the database located at 'evara/server/db.db' and then copy the factory backup database located at 'evara/db_utility/db.db' to the directory 'evara/server'.


#### Final Notes
***
* One-time-pads should **NEVER** be shared electronically
* One-time-pads should **NEVER** be used to encrypt data more than once.
* One-time pads should **ALWAYS** contain securely generated random bytes
* Outside of demo, one-time-pads should be generated on local client machine, **NOT** on the server
* Have fun!

© 2020 Christo Savalas

