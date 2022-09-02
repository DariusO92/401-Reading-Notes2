# Day 14 Notes

## Intro to password hashing

- By dictionary definition, hashing refers to "chopping something into small pieces" to make it look like a "confused mess". That definition closely applies to what hashing represents in computing.

In cryptography, a hash function is a mathematical algorithm that maps data of any size to a bit string of a fixed size. We can refer to the function input as message or simply as input. The fixed-size string function output is known as the hash or the message digest. As stated by OWASP, hash functions used in cryptography have the following key properties:

It's easy and practical to compute the hash, but "difficult or impossible to re-generate the original input if only the hash value is known."
It's difficult to create an initial input that would match a specific desired output.
Thus, in contrast to encryption, hashing is a one-way mechanism. The data that is hashed cannot be practically "unhashed".

- Commonly used hashing algorithms include Message Digest (MDx) algorithms, such as MD5, and Secure Hash Algorithms (SHA), such as SHA-1 and the SHA-2 family that includes the widely used SHA-256 algorithm. Later on, we are going to
 learn about the strength of these algorithms and how some of them have been deprecated due to rapid computational advancements or have fallen out of use due to security vulnerabilities.
 
 Another virtue of a secure hash function is that its output is not easy to predict. The hash for dontpwnme4 would be very different than the hash of dontpwnme5, even though only the last character in the string changed and both strings would be adjacent in an alphabetically sorted list
 
 ## bcrypt overview
 
 - Salting’ the password
One could consider ‘salting’ a password before it is hashed. What does this mean? Well, a ‘salt’ adds a very long string of bytes to the password. So even though a hacker might gain access to one-way hashed passwords,
they should not be able to guess the ‘salt’ string. In theory, this is a great way to secure your data, but if a hacker has access to your source code, they will easily be able to find the ‘salt’ string for passwords.

Random ‘salt’ for each user
As an alternative, a random ‘salt’ string could be added for each user, created on the generation of the user account. This will increase encryption significantly as hackers will have to try to find a password for a single user at a time. 
Again, even though it means they will have to spend more time cracking the passwords for multiple users, they will still be able to gain access to your resources. It just takes longer.
