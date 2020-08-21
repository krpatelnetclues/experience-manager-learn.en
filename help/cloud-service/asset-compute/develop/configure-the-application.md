
## Configure the application

Before beginning development of the Asset Compute workers, first ensure the project is configured with the account information provided by Adobe I/O and your Cloud Storage provider in order for the application to deploy to and execute within the Adobe I/O Runtime environment.

### Create and configure credentials.yml

Create a new `credentials.yml` file to contain the Adobe I/O Service Account credentials and the private key corresponding to the public key registered in Adobe I/O with your Asset Compute Firefly project's Development workspace. __Save this file someplace safe, outside of your project. This is only used for local development and should NOT be checked in to Git.__

Fill out the `technicalAccount` information using the Adobe I/O Service Account values, and the private key contents.

+ [Download the template credentials.yml](assets/configure-the-application/credentials.yml)

```
metascopes:
  - event_receiver_api
  - ent_adobeio_sdk
  - asset_compute_meta
imsEndpoint:  https://ims-na1.adobelogin.com
technicalAccount:
  id:           console.adobe.io > Your Asset Compute Firefly project > Workspaces @ Development > Service Account (JWT) > Technical Account ID
  org:          console.adobe.io > Your Asset Compute Firefly project > Workspaces @ Development > Service Account (JWT) > Organization ID
  clientId:     console.adobe.io > Your Asset Compute Firefly project > Workspaces @ Development > Service Account (JWT) > Client ID
  clientSecret: console.adobe.io > Your Asset Compute Firefly project > Workspaces @ Development > Service Account (JWT) > Client Secret
  privateKey: |
    -----BEGIN PRIVATE KEY-----

    *** INDENTATION MATTERS! ***
    Ensure -----BEGIN PRIVATE KEY----- is indented at the same level as the sample -----BEGIN PRIVATE KEY----- on line 12 above!

    The contents of the Private key registered for the Asset Compute Firefly project. 
    If this was generated by Adobe I/O, it is in the auto-downloaded config.zip > private.key.
    If you provided the public key to Adobe I/O, then you should also be in possesion of the matching private key.

    If you do not have these key pairs, you can generate new keypairs or upload new public keys at the bottom of:
    console.adobe.io > Your Asset Compute Firefly project > Workspaces @ Development > Service Account (JWT) 

    Copy the contents of the private key file into this yaml key, including the -----BEGIN PRIVATE KEY----- and -----END PRIVATE KEY-----,
    and paste/replace this contents.

    + macOS: pbcopy < /path/to/private.key
    + Windows: echo \path\to\private.key | clip


    -----END PRIVATE KEY-----
```


## Configure .env

Once a project has been created, it must be configured with Adobe I/O and cloud storage credentials. These values are set in the generated `.env` file in the root of the project as key/value pairs.

Note that other custom parameters and secrets can be stored in the `.env` file as well, such as credentials to 3rd party web services the Asset Compute application connects to.

![\.env file](assets/asset-compute-project/env-file.png)

### Reference `credentials.yml`

Open the `.env` file, uncomment the following key, and provide the absolute path the `credentials.yml`.

Remember that the `credentials.yml` files should not be checked into Git as it contains secrets, rather it should be stored in a safe place outside the project.

For example, on macOS this might look like:

```
...
ASSET_COMPUTE_INTEGRATION_FILE_PATH=/Users/example-user/credentials/aem-guides-wknd-asset-compute/credentials.yml
...
```

### Configure Cloud Storage credentials

The credentials used by the Asset Compute application to interact with your Cloud Storage provider are provided in the `.env` file. Provide either the Azure Blob Storage credentials. The values can be obtained from either the Azure Portal or Amazon S3 console.

#### Using Azure Blob Storage cloud storage

If you are using [Microsoft Azure Blov Storage] uncomment and populate the following keys in the `.env` file. 
If you are NOT using Microsoft Azure Blov Storage, leave these commented out (by prefixing with `#`).

For example, this might look like (values for illustration only):

```
...
AZURE_STORAGE_ACCOUNT=aemguideswkndassetcomput
AZURE_STORAGE_KEY=Ba9CnisgabdsNJEJBqCYyNbYppbGbZ2VbhcUIcQEw+xxRUDxOUNY/eExll0vwoLsPt/OvbM+B7pkUdpEe7zJhg==
AZURE_STORAGE_CONTAINER_NAME=asset-compute
...
```

#### Using Amazon S3 cloud storage

If you are using [Amazon S3 cloud storage] uncomment and populate the following keys in the `.env` file. 
If you are NOT using Amazon S3, leave these commented out (by prefixing with `#`).

For example, this might look like (values for illustration only):

```
...
S3_BUCKET=
AWS_ACCESS_KEY_ID=
AWS_SECRET_ACCESS_KEY=
AWS_REGION=
...
```


## Validating the project configuration

Once the generated Asset Compute project has been configured, validate the configuration prior to making code changes to ensure the supporting services are provisioned, and configuration elements properly defined in the `credentials.yml` and `.env` files.

Open a shell terminal in the project root, and start up the Asset Conmpute Dev Tool to by executing the command:

```
$ aio app run
```

This starts the local Asset Compute Local Dev Tool at [http://localhost:9000](http://localhost:9000]) which opens in a new Web browser.

Watch the terminal output as the Asset Compute Dev Tool intializes and the Web browser window for error messages.

To stop the Asset Compute Local Dev Tool, tap `Ctrl-C` in the terminal window that executed `aio app run` to terminate the process.


## Troubleshooting

### Asset Compute Local Dev tools cannot start due to missing credentials.yml

+ __Error message:__ Local Dev ServerError: Missing required files (via standard out from `aio app run` command)
+ __Cause:__ The `ASSET_COMPUTE_INTEGRATION_FILE_PATH` value in `.env` file, does not point to `credentials.yml` or `credentials.yml` is not read-able by the current user.
+ __Resolution:__ Review the `ASSET_COMPUTE_INTEGRATION_FILE_PATH` value in `.env` file, and ensure it contains the full, absolute path to the `credentials.yml` on your file system.


### Asset Compute Local Dev tools cannot start due to malformed credentials.yml

+ __Error message:__ Local Dev Server(node:#####) UnhandledPromiseRejectionWarning: TypeError: Cannot read property 'metascopes' of undefined
+ __Cause:__ YAML files are white-space sensitive, and an improperly indented [credentials.yml] can result it being unparse-able. Malformatting of the `credentials.yml` typically happens when the private key contents is copy/pasted into it, and 
it's multiple line must be indented 4 spaces. 
+ __Resolution:__ Review and correct `credentials.yml` for whitespace and indentation.
