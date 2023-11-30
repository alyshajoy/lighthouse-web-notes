# HTTP Cookies and User Authentication Lecture

### HTTP
* request/response protocol
* stateless protocol -> neither party is required to remember any previous interactions

### Cookie
* key/value pair stored in the browser
* send the cookie with every HTTP request
* domain-specific (will only be given to the host and port that they got the cookies from)



# Security & Real World HTTP Servers

### Plain Text...

Why not plain text?

* if someone gains access to the database they'll see all the passwords
* even the admin can see the passwords if the check the DB

### What can we do instead of plain text?

* encryption -> read...lock-up...read
* hashing -> lock-up... (don't want to ever be able to read it in its original form)

### Hashing Process...

(plaintext_password + salt) -> hashing algorithm -> hashed_password

bcryptjs is an npm framework that provides algorithm to hash passwords