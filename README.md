# Test-CI-CD
CI CD with react and discord messages

# Encrypt/Decrypt fields with SOPS

Here we have a yaml file with sensitive keys in clear text: <mark>password</mark>

1. Open terminal

```
$ vim test.yaml
username: geordirs.example
password: GiorgioExample3
description: test example
```
2. Generate a GPG key pair
```
$ gpg --full-generate-key
```
Verify that your selections are correct.

3. Find the public fingerprint (<mark>GPG_key_ID</mark>) for the newly generated key:
```
$ gpg --list-keys "geordirs@gmail.com" | grep pub -A 1 | grep -v pub
```
or 
```
$ gpg --list-secret-keys --keyid-format=long
```
```
/home/grdy/.gnupg/pubring.kbx
-----------------------------
sec   rsa3072/3AA5C34371567BD2 2022-02-20 [SC]
GPG_key_ID
uid                 [ultimate] geordirs <geordirs@gmail.com>
```

4. Now let’s use sops to encrypt the sensitive fields in the yaml file

```
$ sops --encrypt --in-place --encrypted-regex 'password|pin' --pgp GPG_key_ID test.yaml
```
The values of all keys in the yaml file would get encrypted

The yaml file now looks like
```
$ vim test.yaml
username: geordirs.example
password: ENC[AES256_GCM,data:9E9H2ikNC8hrsg==,iv:Z6FziJ65LYsUOb7ZynItd+9U5m7elJSAJAYCq6uUBRw=,tag:mtpbYLGAEBt/8DuiJkzNbg==,type:str]
pin: ENC[AES256_GCM,data:6GH35g==,iv:5g9o700U5aHvfgCWIUzbKQxMwDvOazy+atz2+pRTX2g=,tag:6T7bN99xO9adlcgIDQt3dg==,type:str]
description: test
sops:
    kms: []
    gcp_kms: []
    azure_kv: []
    hc_vault: []
    age: []
    lastmodified: "2022-02-20T01:38:54Z"
    mac: ENC[AES256_GCM,data:fv+GC/1Ce94XIIvGW4GP7mbTtl3VG/It0YrNXekHW4JX0sgyu5KQa/o6pMjTuyd7YFe/JLRiTq9ar9y+1kStUp6NjM3lftSxuf4kggbM1klzyit5gjeTKztcfVKtbH1puQEZPn6xiWpLhRCPuCh4rUN9F9QuK+ZHZxaiUxbOo9s=,iv:/t9tsi6jgtd99HiO/hJxJM/nwBJpiy5sTR1CdjtBqYo=,tag:F9vX2xRlUhZ4Z0dSBqE6mQ==,type:str]
    pgp:
        - created_at: "2022-02-20T01:38:53Z"
          enc: |
            -----BEGIN PGP MESSAGE-----

            hQGMAxeOimkUSssRAQwAqrWhh/8iA8mjkk6zumkKce+2WrqOuyGfnf5DrVxpJWNa
            P1f5/8w/yWhvT7n+PoqBxNOuP0WwmeicllutFJjtUyHxOp2+8TmWqZah1QrQChwP
            mkY6fGEBBQXy/f1R3HmGzrw4PV9+cMjYfpKjepjqFNSfeE74spv8gke3IGX1eTNk
            spQcL2J81wPEcoxRDmfXP4172/BMSs8aAUHkBJFjEeHhUVdRxW6cauGNr96mGQnX
            9evp6Hi241rPF9yKOgbLTDzikE/MwtuanUiIsgXFRHBnh0OqExu8gZ0qs8E/3eY3
            QmFoARfj4sRChiYBaw6D9N+QfaLvArpPHF7wo9tTDbg9cp8wpvWS3i6EP7aKSGWx
            fGYHYY4MK3IdL2SlmZaBBHFYpSSB8vfCcJ8y/Fv1/cofuBPFLd8t9ltmuRFr2uYf
            CGnNeb91aP92cedohJnMcxgLZXvxKk7la2/hgUAm9PHwaexCTnC2GmdMLxYpM5Ui
            xfngn7auCJR1ynbR+6+I0l4BoFhkG659iDSsDhbIIWA/YyOsl5pE8XwW5IWKmfsb
            z/CbmIHGOrttY7qyWqHC+ScgWmI5YGb+PmTzH6onov0PxPdGZid8zjwJHVKI1hX3
            zZRHDOXIxJq59GTdACLu
            =wosU
            -----END PGP MESSAGE-----
          fp: CDE308E72D0290811881B90B05EAE3FD6F03653E
    encrypted_regex: password|pin
    version: 3.7.1

```

5. To decrypt it to STDOUT on the same machine/as the same user, we could run

```
$ sops --decrypt test.yaml
username: geordirs.example
password: GiorgioExample3
description: test example
```

# Getting Started with Create React App

This project was bootstrapped with [Create React App](https://github.com/facebook/create-react-app).

## Available Scripts

In the project directory, you can run:

### `npm start`

Runs the app in the development mode.\
Open [http://localhost:3000](http://localhost:3000) to view it in your browser.

The page will reload when you make changes.\
You may also see any lint errors in the console.

### `npm test`

Launches the test runner in the interactive watch mode.\
See the section about [running tests](https://facebook.github.io/create-react-app/docs/running-tests) for more information.

### `npm run build`

Builds the app for production to the `build` folder.\
It correctly bundles React in production mode and optimizes the build for the best performance.

The build is minified and the filenames include the hashes.\
Your app is ready to be deployed!

See the section about [deployment](https://facebook.github.io/create-react-app/docs/deployment) for more information.

### `npm run eject`

**Note: this is a one-way operation. Once you `eject`, you can't go back!**

If you aren't satisfied with the build tool and configuration choices, you can `eject` at any time. This command will remove the single build dependency from your project.

Instead, it will copy all the configuration files and the transitive dependencies (webpack, Babel, ESLint, etc) right into your project so you have full control over them. All of the commands except `eject` will still work, but they will point to the copied scripts so you can tweak them. At this point you're on your own.

You don't have to ever use `eject`. The curated feature set is suitable for small and middle deployments, and you shouldn't feel obligated to use this feature. However we understand that this tool wouldn't be useful if you couldn't customize it when you are ready for it.

## Learn More

You can learn more in the [Create React App documentation](https://facebook.github.io/create-react-app/docs/getting-started).

To learn React, check out the [React documentation](https://reactjs.org/).

### Code Splitting

This section has moved here: [https://facebook.github.io/create-react-app/docs/code-splitting](https://facebook.github.io/create-react-app/docs/code-splitting)

### Analyzing the Bundle Size

This section has moved here: [https://facebook.github.io/create-react-app/docs/analyzing-the-bundle-size](https://facebook.github.io/create-react-app/docs/analyzing-the-bundle-size)

### Making a Progressive Web App

This section has moved here: [https://facebook.github.io/create-react-app/docs/making-a-progressive-web-app](https://facebook.github.io/create-react-app/docs/making-a-progressive-web-app)

### Advanced Configuration

This section has moved here: [https://facebook.github.io/create-react-app/docs/advanced-configuration](https://facebook.github.io/create-react-app/docs/advanced-configuration)

### Deployment

This section has moved here: [https://facebook.github.io/create-react-app/docs/deployment](https://facebook.github.io/create-react-app/docs/deployment)

### `npm run build` fails to minify

This section has moved here: [https://facebook.github.io/create-react-app/docs/troubleshooting#npm-run-build-fails-to-minify](https://facebook.github.io/create-react-app/docs/troubleshooting#npm-run-build-fails-to-minify)
