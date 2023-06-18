## Solution
1) This is a continuation of the "Cookies" challenge, which I did not write up since it is very simple. However, this challenge is fairly difficult despite the point value.



2) There is a cookie called auth_name with the value UjhNMXIwdEJrbUVNcGVlbXFyUmRXSEhFbjUzZlhCKzYzRWJramlNc0c2YlhJKzZiVlBDZUhVa245ZzNzbWFLS0dLbk5reUlSdkpyaWFNNWh4VGVNeTBRT0tlSW1QU1RsdTA2b3dqRUJLWFVUZk9ZeHR6MkZSZ0NxM0o1VEhSb0Y=. Decoding this as base64 using CyberChef produces gibberish since it is encrypted as per the challenge description.
 
 
 
3) The letters C, B, and C are capitalized in the challenge description which is a hint that cipher block chaining (CBC) is used. CBC is vulnerable to a bit flip. This answer on the Crypto StackExchange extensively explains this attack. Essentially, there is a single bit that determines if the user is an admin. Maybe there is a parameter like admin=0 and if we change the correct bit then we can set admin=1. However, the position of this bit is unknown, so we can try every position until we get the flag.



4) We write an improved Python cookies.py to complete this bruteforce attack. The script loops through all the bits in the cookie and flips each one until the flag is shown. See the comments in the script for more details.

