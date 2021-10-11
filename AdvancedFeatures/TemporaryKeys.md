# Temporary Keys

### Description
Temporary keys are designed to be single use keys that can only authenticate once with Temasys servers. Once used, the keys are not longer valid and further connections with the same key will be rejected. Temporary keys also have an expiry that will result in failure to connect to Temasys servers even if the the key has not been used.

## How to create temporary keys using the Temasys REST API
Endpoint url: `https://api,temasys.io`

#### Step 1: Start a current logged in session with the API server.

**POST/rest/login**
> |Parameter|Type|Optional|Description|
> |-|-|-|-|
> |username|String|No|The account username.|
> |password|String|No|The account password.|

#### Step 2: Create a temporary key.

**POST/rest/apps**
> |Parameter|Type|Optional|Description|
> |-|-|-|-|
> |alias|String|No|Key Id - A key of the App that will be used as a template to create the temporary key. All configurations form the alias key will be applied to the temporary key unless specified in the POST parameters.|
> |timestamp|String|No|An arbitrary date in ISO8601 format. <br> A common method in JavaScript is to call `new Date().toISOString()`.|
> |token|String|No|A Hex string generated using HmacSHA1 protocol. *Refer to steps below.* |
> |isTemporary|Boolean|No|The flag indicating if the key is a temporary key.|
> |duration|Integer|Yes|The duration, in minutes, from the time of key creation to expiry. <br> Minimum of 5 minutes to a maximum of 7200 minutes (5 days). <br> Default is set to 240 minutes (4 hours).|
> |corsurl|String|Yes|The app whitelisted CORS domains. <br> If the parameter is not provided, the corsurl values from the alias key will be applied to the temporary key.|
> |turn|String|Yes|The flag indicating if `TURN` servers should be used for this key. <br> Values are `"ON"` or `"OFF"`.|

## How to generate the token
1. Obtain `secret` from the [Temasys Developer Console](https://console.temasys.io/#)
2. Install and import [CryptoJS](https://www.npmjs.com/package/crypto-js)
3. Apply HmacSHA1 to the <alias+timestamp, secret> pair <br>
   `const token = CryptoJS.enc.Hex.stringify(CryptoJS.HmacSHA1(alias+timestamp,secret));`

### Other information
- Temporary keys will not show up in the developers console so do save the `appid` in the response to the POST request.
- All usages for the temporary key will be attributed to the alias key.
