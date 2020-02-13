# hypefury

## Usage

### Update configuration files with your Firebase testing project configuration
In the files `.firebaserc` and `src/firebase.js`.

### Google cloud configuration
Install `gcloud`.

`npm install -g firebase-tools`


### Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build && firebase deploy
```

Most of the times, you want to deploy with the `firebase deploy --only hosting` option.

### Run your tests
```
firebase emulators:start --only firestore
```

```
FIRESTORE_EMULATOR_HOST=localhost:8082 npm run test:unit
```
