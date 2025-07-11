# Online inference with Snowpark Container Services


## Inference from Local Machine
### Prerequisites
To set up, make sure you have [uv installed](https://docs.astral.sh/uv/getting-started/installation/)

### Setup
Run the following commands
```
chmod +x create-keys.sh 
sh create-keys.sh
```
This will create a public and private key pair on your local machine. 

Next, you'll need to associate your Snowflake user with your public key. Open up your public key (ending in .pub, likely located at ~/.snowflake/keys). Select the text before and after the long dashes, and run the command below in your Snowflake account.
```
ALTER USER <YOUR_USER> SET RSA_PUBLIC_KEY = 'MI...'
```

Next, we'll create a virtual environment from which to run the python request code. Run the following in your terminal.
```
uv init
uv sync
source .venv/bin/activate
uv add requests pyJWT cryptography
```

### Running inference
Make sure your inference endpoint is live. Then, open up `api-call.sh`. Change the account, user, role, endpoing, and snowflake account url on the first command. Copy and paste into your terminal, and you should get a response back. 


## Running inference from Postman
To run in Postman, run the `jwt_to_oauth.py` file. Something like the following
```
python jwt_to_oauth.py \
  --account <account> \
  --user <user> \
  --private_key_file_path ~/.snowflake/keys/rsa_key.p8 \
  --endpoint <container endpoint>
```
This will output an OAuth token. Put this into a header in Postman with the key `Authorization` and the value `Snowflake Token="ey..."`. 

Make sure you have `Content-Type` set to `application/json` and `Content-Length` enabled. Paste in the request body from the `api-call.sh` python examples as raw JSON body. 

Set the url to your container endpoint url. Don't forget to add https:// and /predict at the end. 