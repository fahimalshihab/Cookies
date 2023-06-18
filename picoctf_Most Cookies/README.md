## Most Cookies
Most Cookies
 | 150 points
Tags: 
AUTHOR: MADSTACKS

Description
Alright, enough of using my own encryption. Flask session cookies should be plenty secure! server.py http://mercury.picoctf.net:35697/


Hints 
How secure is a flask cookie?
## Solution
1) Looking at the server script we can see that the app’s secret key is set to a random cookie name:

<code> cookie_names = ["snickerdoodle", "chocolate chip", "oatmeal raisin", "gingersnap", "shortbread", "peanut butter", "whoopie pie", "sugar", "molasses", "kiss", "biscotti", "butter", "spritz", "snowball", "drop", "thumbprint", "pinwheel", "wafer", "macaroon", "fortune", "crinkle", "icebox", "gingerbread", "tassie", "lebkuchen", "macaron", "black and white", "white chocolate macadamia"]
app.secret_key = random.choice(cookie_names)</code>

The app’s secret key is used to sign a flask session cookie so that it cannot be modified. However, since we know the secret key is one of the 28 cookie names, we can simply try them all until we successfully decrypt the cookie.

2) So, the first step is to go to the website and copy a session cookie: <code>eyJ2ZXJ5X2F1dGgiOiJibGFuayJ9.ZI9RUA.m8SdRNUSWyRR-H0lvcUPOTw-BMQ</code>

We can write a script that uses the logic from Flask’s SecureCookieSessionInterface(https://github.com/pallets/flask/blob/020331522be03389004e012e008ad7db81ef8116/src/flask/sessions.py#L304)to decode and encode cookies.

But first we need to determine what value we should set in the cookie. We can find this on lines 45-47 of the server code.

<code> check = session["very_auth"]
if check == "admin":
    resp = make_response(render_template("flag.html", value=flag_value, title=title)) </code>
So, we need to store {"very_auth": "admin"} in the cookie.

3) Running the solve script will try each secret key and then once it successfully fins the key by decoding a know cookie, it will encode the above cookie data.

Secret Key: whoopie pie
Admin Cookie:<code>  eyJ2ZXJ5X2F1dGgiOiJhZG1pbiJ9.ZI9S6g.lZrLO6-g4mGxC80i-H081HH7Bm0 </code>
We can replace the cookie on the website with our admin cookie, refresh the page, and the flag will be shown.
