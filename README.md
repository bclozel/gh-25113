# Repro project for gh-25113

To reproduce the issue, run the application and then get the main page using httpie.

The content hash for the CSS stylesheet is missing in the URL:

```shell
➜  gh-25113 git:(master) ✗ http :8080
HTTP/1.1 200
Connection: keep-alive
Content-Language: en-FR
Content-Length: 164
Content-Type: text/html;charset=UTF-8
Date: Fri, 12 Feb 2021 20:27:59 GMT
Keep-Alive: timeout=60
Set-Cookie: JSESSIONID=F3BA1DB190D6DC24F3796EA8203FF023; Path=/; HttpOnly

<!DOCTYPE html>
<html>
<head>
    <link rel="stylesheet" href="/css/style.css;jsessionid=F3BA1DB190D6DC24F3796EA8203FF023" />
</head>
<body>
TEST
</body>
</html>
```

Reverting to 2.3.7.RELEASE gives the expected:

```shell
➜  gh-25113 git:(master) ✗ http :8080
HTTP/1.1 200
Connection: keep-alive
Content-Language: en-FR
Content-Length: 197
Content-Type: text/html;charset=UTF-8
Date: Fri, 12 Feb 2021 20:30:00 GMT
Keep-Alive: timeout=60
Set-Cookie: JSESSIONID=995C5842A789798E1D9331C1556D8E5B; Path=/; HttpOnly

<!DOCTYPE html>
<html>
<head>
    <link rel="stylesheet" href="/css/style-1688c8210b6509d702b1adb96bc4d0f3.css;jsessionid=995C5842A789798E1D9331C1556D8E5B" />
</head>
<body>
TEST
</body>
</html>
```