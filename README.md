# auth-passport-jwt


A [Passport](http://passportjs.org/) strategy for authenticating with a
[JSON Web Token](http://jwt.io).

This module lets you authenticate endpoints using a JSON web token. It is
intended to be used to secure RESTful endpoints without sessions.

## Supported By

If you want to quickly add secure token-based authentication to Node.js apps, feel free to check out Auth0's Node.js SDK and free plan at [auth0.com/overview](https://auth0.com/overview?

## Install

    npm install passport-jwt

## Usage


### Extracting the JWT from the request

There are a number of ways the JWT may be included in a request.  In order to remain as flexible as
possible the JWT is parsed from the request by a user-supplied callback passed in as the
`jwtFromRequest` parameter.  This callback, from now on referred to as an extractor,
accepts a request object as an argument and returns the encoded JWT string or *null*.

#### Included extractors

A number of extractor factory functions are provided in passport-jwt.ExtractJwt. These factory
functions return a new extractor configured with the given parameters.

* ```fromHeader(header_name)``` creates a new extractor that looks for the JWT in the given http
  header
* ```fromBodyField(field_name)``` creates a new extractor that looks for the JWT in the given body
  field.  You must have a body parser configured in order to use this method.
* ```fromUrlQueryParameter(param_name)``` creates a new extractor that looks for the JWT in the given
  URL query parameter.
* ```fromAuthHeaderWithScheme(auth_scheme)``` creates a new extractor that looks for the JWT in the
  authorization header, expecting the scheme to match auth_scheme.
* ```fromAuthHeaderAsBearerToken()``` creates a new extractor that looks for the JWT in the authorization header
  with the scheme 'bearer'
* ```fromExtractors([array of extractor functions])``` creates a new extractor using an array of
  extractors provided. Each extractor is attempted in order until one returns a token.

### Writing a custom extractor function

If the supplied extractors don't meet your needs you can easily provide your own callback. For
example, if you are using the cookie-parser middleware and want to extract the JWT in a cookie
you could use the following function as the argument to the jwtFromRequest option:

```js
var cookieExtractor = function(req) {
    var token = null;
    if (req && req.cookies) {
        token = req.cookies['jwt'];
    }
    return token;
};
// ...
opts.jwtFromRequest = cookieExtractor;
```

### Authenticate requests

Use `passport.authenticate()` specifying `'JWT'` as the strategy.

```js
app.post('/profile', passport.authenticate('jwt', { session: false }),
    function(req, res) {
        res.send(req.user.profile);
    }
);
```

### Include the JWT in requests

The method of including a JWT in a request depends entirely on the extractor
function you choose. For example, if you use the `fromAuthHeaderAsBearerToken`
extractor, you would include an `Authorization` header in your request with the
scheme set to `bearer`. e.g.

    Authorization: bearer JSON_WEB_TOKEN_STRING.....

## Migrating

The the [Migration Guide](docs/migrating.md) for help upgrading to the latest
major version of passport-jwt

## Tests

    npm install
    npm test

To generate test-coverage reports:

    npm install -g istanbul
    npm run-script testcov
    istanbul report
