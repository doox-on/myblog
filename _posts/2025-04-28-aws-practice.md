---
title: Cloud Computing
categories:
feature_text: |
  
feature_image: "https://picsum.photos/2560/600?image=872"


---

This project involves working with cloud services such as AWS CloudFormation, EC2, Docker, and MySQL.
I deployed actual servers and integrated cloud storage, APIs, and remote connections.
Below is a detailed log of the tasks and technologies I worked with during the project.


* Chapter 1.
  
This project involved implementing DELETE, tag-based search, and update functionalities for a photo application using AWS services like Lambda, API Gateway, and DynamoDB. Key challenges included resolving complex CORS errors and discovering that the API Gateway INTEGRATIONHTTPMETHOD must be POST when integrating with Lambda, regardless of the client-facing HTTPMETHOD. These learnings were crucial for successfully creating dedicated functions and APIs to handle specific database operations for each new feature.

```

This week, I worked on modifying the template.yaml and process.js files and added updatephoto.html.
 I utilized S3 Bucket, API Gateway, CloudFormation, Lambda, DynamoDB, and CloudWatch for this project.

CORS Configuration Issues

At first, I struggled a lot with configuring CORS (Cross-Origin Resource Sharing). I encountered preflight CORS errors, which I resolved by:
	1.	Setting the Integration Method’s Access-Control-Allow-Methods to either ' * ' or 'DELETE, GET, OPTIONS, POST' for
     all OPTIONS API methods, even if they seemed irrelevant.
	2.	Checking the Lambda function’s permissions and ensuring the API method had access to the correct resources.
	3.	Manually configuring CORS settings in the AWS API Gateway console by clicking the appropriate options.

The key takeaway was that setting '*' for ALL OPTIONS methods was necessary to resolve the issue.

Internal Server Error (500)

After fixing the CORS issue, I ran into a 500 Internal Server Error, which was caused by inconsistencies between the Lambda function,
 API method, and process.js file.

The problem stemmed from incompatible syntax in the Lambda function. I referred to AWS documentation and various online examples,
 but most of them used a different format instead of .yaml, which made it difficult to apply directly.

1. DELETE feature
Implementing the DELETE function alone took me more than 10 hours. One of the most critical discoveries was understanding 
the difference between HTTPMETHOD and INTEGRATIONHTTPMETHOD in the API method definition within the template.yaml file:
	•	The HTTPMETHOD could be DELETE, PUT, GET, etc.
	•	The INTEGRATIONHTTPMETHOD must be POST, regardless of the API method type.

It took me almost a day of experimenting before I figured this out, but once I did, I was finally able to implement 
the DELETE function successfully.

-Related code line in template.yaml-

[DELETE Data in DB]
*[LambdaPermission13] gives accees permission to [LambdaFunction00] to accees the API delete gateway.
I tried to make a DELETE api under /photos/{id}, but some errors keep occured so I just decided to make it
under /photos API directory.
*[LambdaFunction00] takes json file as a parameter and find the photoId info in the jsonfile. 
With the photoId, scanned with table.scan() method through DynamoDB to find the data we are willing to delete.
With table.delete_item() method, deleted the item that was searched.
Returned status code 200 for success, and item for debugging purpose. 
*[ApiGatewayMethod00] uses DELETE HTTPMETHOD, and POST IntegrationHttpMethod. I used the same Request template
as one of the POST API methond in the template. Since this API method is interacting with Lambdafunction, I set
URI endpoint to [LambdaFunction00] at Uri part.
*[ApiGatewayResource5] refers the API directory of /photos. So we can access to DELETE API under /photos.

[DELETE Data in S3 bucket]
*[ApiGatewayMethod01] directly access to S3 photo bucket. So URI goes to the S3 photo bucket address. 
In this case, I used DELETE for both HttpMethod and IntegrationHttpMethod, and it worked fine.
I guess it's because the API does not communicate with Lambda function.


-Related code line in process.js-
*[processDeletePhoto] checks Dynamo DB first. When a user press the delete button, the button sends over the photoId
to [processDeletePhoto] as a parameter. Then a confirm window pops up to check a user really wants to delete the photo.
If a user confirm, put photoId into a json format, and send it to related API method. Since the URL is /photos and 
method is DELETE, it will go to [ApiGatewayMethod00]. If successfully connected and deleted the metadata of the photo in DynamoDB,
Lambdafunction will return a data as json form to JavaScript in the name of 'data'. In this case, I set it to return photo URL to get
the name of photo, which is also a key for the S3 photo bucket. I used 'split' method to divide URL into two parts,
and the latter contains the name of the photo. So passed the photo name to [deletePhoto] to delete photo in S3 photobucket.
*[deletePhoto] takes the photoname as a parameter, which is a key for the bucket. It directly goes to the address of 
${API_URL}/uploadphoto/${key}, and acceess to the S3 bucket with the key. If an item is founded, delete it! 


2.Tag-Based Search Feature

Implementing search by tag was slightly easier since I had already learned from my previous mistakes.

The challenge was that tags were dynamic, so I had to handle them differently when fetching data.
Initially, I passed the tag value directly to the Search function, and it seemed to work.
However, I later realized that if a photo’s description or title contained the same word as the tag,
it would also appear in the search results, which was not the intended behavior.

To fix this, I created a new JavaScript function called searchByTag.
	•	This function worked similarly to the existing search method, but it specifically searched only the
     “Tag” section instead of querying across Title, Description, and Tags all at once.
	•	In the Lambda function, the default search method searched across all fields. I initially tried modifying it 
    using if conditions, but it became too complicated.
	•	Instead of modifying the existing function, I created a new API and Lambda function dedicated to tag-based searches.
	•	Since the Search API already had a POST method, I implemented the new function using PUT.

-Related code line in template.yaml-
*[LambdaFunction51] works almost the same as [LambdaFunction5], which is just search method, but it only scans 'Tags' section.
*[LambdaPermission81] gives a permission to [LambdaFunction51] to access search/PUT.
*[ApiGatewayMethod71] works the same as search API, but use PUT HttpMethod, and use [LambdaFunction51] as a Uri endpoint.

-Related code line in process.js-

*[searchByTag] it is almost the same as [searchPhotos] fucntion, but put 'tag' value and chaned the method type to 'put'.


3. Update Photo Feature 

The update photo feature was particularly challenging due to difficulties in understanding the UpdateExpression syntax in DynamoDB.
	•	Clicking the update button opens a new updatephoto.html page, which is essentially a modified version of addphoto.html.
	•	I passed the photo ID through the URL, and upon submission, it used the photo ID to locate the target file in the database and update it accordingly.

Summary of Challenges

Most of the difficulties I encountered were due to:
	1.	New syntax and configurations I wasn’t familiar with.
	2.	Permission issues when setting up API Gateway and Lambda.
	3.	Incorrect resource targeting, which caused errors in API calls.

-Related code line in template.yaml-
*[LambdaFunction02] uses the similar format as [LambdaFunction00], but it is update a photo inforamtion (title, tags, description).
It uses specific 'table.update_item' function and syntax for that. 'UpdateExpression' sets the attributes that would be updated,
and 'ExpressionAttributeValues' actually updates the values into the attributes.
*[ApiGatewayMethod02] is the same as upload photo's API method. Connected with [LambdaFunction02] to access to DynamoDB.

-Related code line in process.js-
*[updatePhoto] is being called when a user clikced the update buttton. I made a new updatephoto.html, which is copied from
addphoto.html, but a few things are modified. I made the updatephoto.html contains the phtoID click the button, so 
[updatePhoto] does not have an argument of phtoID. Similar as [uploadphoto], it takes title, tags, and description, with
photoID since it does not need a photoData. When upload is successful, automatically goes to the main page for user convenience.
```

---

* Chapter 2.
* User and Photo Management System with Secure Authentication and Cloud Storage Integration
  
Developed a full-featured web application enabling secure user management, including email verification, hashed password storage, and session-based authentication. Implemented robust photo and album deletion functionalities integrated with AWS S3 for storage management. Added account deletion and session logout features to enhance user control and security. The system supports both SQL and NoSQL databases, ensuring flexible backend compatibility.

```

//////////////////////////////////////
1. USER MANAGEMENT

1-1. Each user entry has a unique user ID. 

When a user registers an account, the signup() function checks whether the userID already exists
 in the database. If the user already exists but their confirmation value is 0, the existing account
 is deleted, and the new account is registered.
 SQL: Lines 232–238.
 NoSQL: Lines 231–236.

1-2. Each user entry has a unique email address. Multiple users cannot sign up using the same email address.
 
If the provided email is already in use, the system either returns a blank page or aborts with a 400 error.
 If the email exists but the confirmation value is 0, the existing account is deleted, and the new account is registered.
 SQL: Lines 217–226.
 NoSQL: Lines 218–225.

1-3. The password must not be stored in plain text in the database.

For security reasons, passwords are hashed before being stored.
The user-entered password from the HTML page is hashed using the generate_password_hash() function.
The werkzeug.security library is used for hashing. When verifying passwords, check_password_hash() is used.
The hashed password is stored in the database along with other user details.
SQL: Line 242.
NoSQL: Line 240.

1-4. When a user registers, a validation email is sent.

After successful account registration in signup(), a verification token is generated by hashing the userID.
 The token is generated using the generate_token() function provided in the HW2 PDF.
A verification link is created by combining the root URL with /confirm/ and the generated token.
The send_email() function is then used to send a confirmation email to the user.
After sending the email, the user is redirected to confirm.html, where they are informed that the verification email has been sent.
SQL: Lines 240–250.
NoSQL: Lines 238–255.

1-5. The verification link must be used within one hour.

 The Source email address in the send_email() function must be an AWS SES-registered email, 
 which is set to jmoon318@gatech.edu. When the user clicks the link in the email, 
 the @app.route function extracts the token from the URL and converts it into a string.
The confirm_email() function processes the token by decoding it using validate_token(),
checking whether it has expired more than 1 hour old. If the token is valid, the corresponding user is retrieved from the database,
 and their confirmation status and updatedAt fields are updated accordingly. Finally, the user is redirected to the /login page. 
 SQL: Lines 257–286.
 NoSQL: Lines 262–293.

1-6. To log in, the user must enter their email address and password.

 If the user has not completed the email verification confirmation != 1, or if the email/password combination is invalid, 
 the login attempt is rejected. The user enters their login credentials and submits the form.
  The login_page() function processes the request POST method. The system checks if the email exists in the database,
if confirmation = 1 account is verified, and if the entered password matches the hashed password using check_password_hash().
 If all conditions are met, the user is successfully logged in, and their userID is stored in the session.
The user is redirected to the main page.
SQL: Lines 148–197.
NoSQL: Lines 156–200.

To access the application, the session must contain logged_in = True.
 Before processing any HTTP request, the require_login() function is executed to verify the current session state.
If the session is not set, the user is automatically redirected to the login page /login.
The only pages accessible without logging in are /login, /signup, and /confirm_email.
SQL: Lines 137–145.
NoSQL: Lines 143–153.

1-7. After a successful login, the user may access the application. 
The homepage displays the currently logged-in user by retrieving their username from the session.






/////////////////////////
2.PHOTO DELETION
A delete button was added to Viewphotos.html to allow users to delete photos.
When the button is clicked, the photoURL and albumURL are sent to delete_photo().
The filename is extracted from photoURL to locate the photo in S3. 
The photo data is deleted from the database table, and the s3_delete() function is called to remove the photo from the S3 bucket.

SQL: Lines 288–306
NoSQL: Lines 296–318

In s3_delete(), “photos” is passed along with the filename to delete a regular photo,
 while “thumbnails” is passed to delete a thumbnail.
 The server access key allows any user to delete photos. The function locates the file in the bucket and deletes it.
If the file is successfully deleted, the album page is reloaded without the removed photo.

SQL: Lines 119–132
NoSQL: Lines 120–133






/////////////////////////
3.ALBUM DELETION

Album Deletion

A delete button was added to index.html to allow users to delete albums.
 When the button is clicked, the albumID is sent to delete_album(). 
 The function retrieves all photos with the same albumID from the database. 
 The regular photos are deleted first, followed by the thumbnail. 
 The s3_delete() function is used simultaneously to remove files as the data is deleted.

SQL: Lines 308-349
NoSQL: Lines 320–357

** Although it is possible to retrieve and delete all database entries with the same album ID at once,
 the implementation follows a two-step search approach for better code reusability between SQL and NoSQL versions.



 ///////////////////////
4. USER ACCOUNT DELETION
A delete account button was added to the navigation bar at the top of each HTML page, which renders accountDelete.html.
 To prevent accidental account deletion, a confirmation button was implemented.
  When the user clicks the delete account button, a POST method request is sent to execute the delete_account() function. 
  The function retrieves the userID stored in the current session and deletes all albums in the database that match the same userID. 
  The user’s albums and all photos within those albums are deleted.

** To track which user uploaded which photo or album, 
the userID from the session is now stored in the table when creating a photo or album. 
In SQL, a new column was added using:
ALTER TABLE `Photo` ADD `userID` varchar(100) NOT NULL; (Refer to sqlcommands.sql for details).
In NoSQL (DynamoDB), new columns can be dynamically added as needed.

The function then deletes all photos that the user uploaded to other users’ albums.

Finally, the user is removed from the USER table, and the system redirects to the login page.
If the user is still logged in, the session is cleared upon accessing the login page.

** Additionally, for convenience, a log-out button was added to the navigation bar.
 Clicking the button clears the session and redirects the user to the login page.

///////////////////
END 


```

* Chapter 3.
* Microservices-Based User Profile and Chatroom File Management System with Authentication
Implemented a microservices architecture featuring user profile management with secure password hashing, user validation, and session logout functionality. Developed a file upload and attachment system for chatrooms using base64-encoded files saved to Docker volumes, with metadata stored in a MySQL database. Integrated services communicate via REST APIs, supporting seamless user authentication and real-time chat features including image sharing.

```
//////////////////////////////////////
1. Profile Service

1.1. /new_profile
/register function sends /new_profile a request with json file. The json file includes user name, password, and bio.
Password is hashed with werkzeug. 
If username or password is missing, return an error. 
Make a connection to the mySQL table and insert the information.
Maybe createdAt is not necessary, but I included it.
cursor.lastrowid retrieves the lasts uid.
Fianlly returns the profile_id to /register

+ 

Init.sql has a new table User for profile_service.
profile_id is the primary key and auto increase. 
Password(based), bio, createdAt, updatedAt are located. (But createdAt and updatedAt are not really used) 



1.2. /validate_user
/login calls /validate_user to check if the user is validate(registered).
/login receives json with username and password. 
The default response message is none.
If username or password is missing, send None with error code 400.
Connect to mySQL database and fetch the target user information.
If nothing is found, response None
Check_password_hash compares the fetched password with the user typed password.
If matched, put profile_id into response and return it.
If not matched, send None.


1.3. /get_username
/lookup_useraname sends a request with profile_id to find username. 
Connect to User table and fetch the target profile.
If nothing is found, return error message.
If found, return username. 



2.Database management interface

Implemented phpmyadmin as the PDF file guided in docker-compose.



3.Logout button
Base.html has the logout button and it uses go() to go login_service port. Since I am using 5007 as 
login_service port, changed it from 5002 to 5007.
/logout gets profile_id from the URL arguments. If user tries to log out in log out status, show "already logged out" message. 
Send request to /delete_access_token with profile_id.
/delete_access_token service is in auth_service, and does literally the reverse way of what /create_access_token does. 
If authentication record doesn't exist, abort it with 404. If exists, delete that row in Auth table and return message and the profile_id.
If successfully removed token, redirect to login page.



4.Message attachments and media

file_service is implemented in docker-compose.yaml. Docker volume is located /app/uploads and declare volume with mysql_data. 
Dockerfile and requirements are basically the same as other services so I just copied with name modifications.
File.py
Upload_folder is the volume location. 

4.1 /upload_file in chatroom.py 
When user upload files, the files are encoded in the bas64 form with header.
I thought the encoding was only for html and reading the files, so tried to send the whole file as a dataForm at the first time.
But the fetch() in chatroom.html forces me to use only json format! So I copied the send message's fetch format.
Base64 is text, so put it in the json body and send it to /uploadfile in chatroom_service. 
/upload_file(line120 ~) in chatroom.py, do the authentication like /send_message. 
Take the values from html and put them in another json request to add_photo in file.py to save the photos.

4.2 /Add_photo 
/Add_photo takes file(in 64encoded), room_id, and profile id. 
The file has [data:image/jpeg;base64,ABCDEF] form, split it into header part and data part. 
Codeline 38-40 extract file extension.
To save the encoded file into volume as a file, decode it with b64decode().
Then generate random file name with uuid4() in hexadecimal and append the file extension. (Line47)
Merge volume path and file name to save the file into correct location. (Line48)
Image_path is going to be saved in the ChatroomPhotos table as a metadata. This is where the file is located in the docker volume. 
Open the file location, and write decoded file data into the memory location. (Line 50-54) 
And update mySQL ChatroomPhotos table.
Return file_id and image_url to /upload_file in chatroom.py

Now /upload_file updates ChatroomMessages table. I added a new column called image_url. 
This new row has empty message but image_url.
Return Ok if is successful.

4.3 chatroom.html
- uploadFile(input) (line43-77) does the same thing as send message. But if user uploads several files together, it will iterate and send the images as separate messages. 

- refresh() (line79-103) refresh and retrieve the messages in time order.
Since the image_url is saved in the Chatroom table, if the image_url exist, get the url and put it in img src="". If not, put regular message.
Then put the content in text!

(I did not use ChatroomPhotos even though I created it this time, but making a separate table will help further implementation) 


**Overall message upload process**

Chatroom.html (User upload files) -> /upload_file in Chatroom.py (Send file to /add_photo) -> /add_photo in file.py (save file in
file_service volume and metadata in ChatroomPhotos. Return saved file address (URL) -> /upload)file in Chatroom.py (update URL in ChatroomMessages)


**



///////////////////
END 

```
