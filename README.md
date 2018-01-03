## Purpose
This API is designed based on RESTful principles. Currently it is focused on back-end functionality, with a CLI intended as a future improvement. It enables users to transform an 8 bit or 16 bit WAV audio file using one of the provided functions (see 'Transforms').

<!-- we can add in encryption if we reach that point -->

## Set Up
To set up ScrambleVox on your own machine, follow the steps below.

1. Fork or clone the repository into whatever directory you want.
2. Run 'npm i' from within the repo folder.
  * To install jest for testing purposes run 'npm i jest' as well

3. Open a new tab in the terminal and run 'npm run dbon' to open a connection with the mongoDB database.
4. Finally run 'node index.js' to initialize the program


## Tests
ScrambleVox implements continuous integration (CI) via Travis CI and continuous deployment via Heroku. Only modules which pass all tests and build properly will be automatically deployed on Heroku.

Tests examine both proper behavior for each route as well as behavior when errors occur. The following tests can be executed by running 'npm test' after following the steps under 'Set Up'. Note: failure to set up the API properly before running tests will prevent tests from running.

1. Wave Parser Tests
  * Tests success case in which file meets all requirements and a new Constructed Wave File is created
  * Tests failure cases in which
    * The file is the incorrect format (not RIFF or not WAV)
    * The file size is too large
    * The file has additional pieces of data that are unexpected (subchunk id 1 or subchunk id 2 do not match the expected values of 'fmt' and 'data' respectively)
    * The file is not linear PCM encoded (i.e. the file is compressed)
    * The file has more than two channels (it is not mono or stereo)
    * The sample rate is too high (above 48k)
    * The file has a bit depth other than 8 or 16 bits

2. Bitcrusher Tests
  * Tests success cases for 8 bit and 16 bit files in which the second part of each sample is "crushed", flattening out waves into a more rectangular shape

3. Sample Rate Tests
  * Tests success case in which the sample rate of the file is reduced 50%

## Transforms
1. Bitcrusher: Reduces the resolution of the audio from 8 or 16 bits to 2 bits without affecting bit depth.

2. Sample Rate Reduction: Reduces the sample rate of the audio file by half, reducing the maximum possible frequency of the recording.

## Routes
### Account setup
1. POST /signup: Creates a new account and responds with a token. You must include a username (String), email (String), and password (String).
2. GET /login: Accesses an account and returns a new token. You must send the account's username and password in the authorization header of the request. If no basic authentication is included a 400 error will occur.

### Transforming files
1. POST /waves/bitcrusher: Transforms an audio file and returns a url to the modified file. You must send the token for your account in the authorization header of the request. If no bearer authorization is included a 401 error will occur.
<!-- 2. POST /waves/transform2 -->

## Internal Infrastructure/Code Examples
1. models (user and wave)
2. middleware (express, fs-extra?, error, logger, basic auth, bearer auth)

## Technologies Used
### For production
* ES6
* node
* aws-sdk
* bcrypt
* dotenv
* express
* fs-extra
* http-errors
* jsonwebtoken
* mongodb
* mongoose
* multer
* winston

### For development
* aws-sdk-mock
* eslint
* faker
* jest
* superagent

<!-- * libraries we used, if any -->

## Contribute
If you would like to help improve this API you can do so by opening an issue under the 'Issues' tab on the repo. Please tell us what your feedback is related to by including a label (i.e. 'bug' to report a problem).

## License
MIT (see License file)

## Credits
Thank you to Vinicio Vladimir Sanchez Trejo, Steve Geluso, Izzy Baer, Joshua Evans, and Ron Dunphy for help problem solving and identifying useful tools to examine WAV files.
