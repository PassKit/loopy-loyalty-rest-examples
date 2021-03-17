# loopy-loyalty-rest-examples
Repository contains reference files for the Loopy Loyalty API Developer Documentation:
https://developer.loopyloyalty.com

# Postman
The quickest way to get started with the API is by using our Postman Collection with the below prerequest snippet.

## Prerequest snippet
The prerequest snippet takes care of generating a JWT for you to place in the `Authorization` header:

__Step 1: Copy and paste below in the prerequest section of your request:__
```
var apiKey = postman.getEnvironmentVariable("LL_API_KEY"),
    apiSecret = postman.getEnvironmentVariable("LL_API_SECRET"),
    username = postman.getEnvironmentVariable("LL_USERNAME");

// Generates the jwt token from an api key, secret &username
postman.setEnvironmentVariable('jwt', generateJWT(apiKey, apiSecret, username));

function generateJWT(key, secret, username) {
    var body = {
        "uid": key,
        "exp": Math.floor(new Date().getTime() / 1000) + 3600,
        "iat": Math.floor(new Date().getTime() / 1000) - 10,
        "username": username
    };

    header = {
        "alg": "HS256",
        "typ": "JWT"
    };
    var token = [];
    token[0] = base64url(JSON.stringify(header));
    token[1] = base64url(JSON.stringify(body));
    token[2] = genTokenSign(token, secret);

   return token.join(".");
}

function genTokenSign(token, secret) {
    if (token.length != 2) {
        return;
    }
    var hash = CryptoJS.HmacSHA256(token.join("."), secret);
    var base64Hash = CryptoJS.enc.Base64.stringify(hash);
    return urlConvertBase64(base64Hash);
}


function base64url(input) {
    var base64String = btoa(input);
    return urlConvertBase64(base64String);
}

function urlConvertBase64(input) {
    var output = input.replace(/=+$/, '');
    output = output.replace(/\+/g, '-');
    output = output.replace(/\//g, '_');

   return output;
}
```

![Prerequest](img/step1-prerequest.png)

__Step 2: Create a new environment with your API key and Secret:__
- `LL_API_KEY`: Your API key.
- `LL_API_SECRET`: Your API secret.
- `LL_USERNAME`: Your Loopy Loyalty username (or subuser name).

![Environment](img/step1-environment.png)

__Step 3: Use `{{jwt}}` in your auth header:__

![Prerequest](img/step3-auth-header.png)

__Step 4: Execute your API call with environment set:__

![Run](img/step1-run.png)

## Postman Collection
[![Run in Postman](https://run.pstmn.io/button.svg)](https://app.getpostman.com/run-collection/d3d2753c6072ae2c5b18)