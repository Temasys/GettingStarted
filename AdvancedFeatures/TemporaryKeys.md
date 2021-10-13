# Temporary Keys

### Description
Temporary keys are single use keys for autenticating with Temasys services. Once used for an authentication, these temporary keys are invalidated and any subsequent authentication attempts will be rejected with an error response. Additionally, temporary keys expire after a period of time and will  be rejected with error if attempted to be used outside of their validity window independently of prior usage status. 

## How to create temporary keys using the Temasys REST API
Endpoint url: `https://api.temasys.io`

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
1. Locate your keys `secret` on the [Temasys Developer Console](https://console.temasys.io/#)
2. Install and import [CryptoJS](https://www.npmjs.com/package/crypto-js)
3. Apply HmacSHA1 to the <alias+timestamp, secret> pair <br>
   `const token = CryptoJS.enc.Hex.stringify(CryptoJS.HmacSHA1(alias+timestamp,secret));`

### Other information
- Temporary keys will not appear in your app's key list in the console, ensure that you take note of the `appid` found in response to your POST request, this is your temporary key.
- All usage for temporary keys will be attributed to the standard key used to request the key.
