
#### Installation in Linux

1. Add the Hasicorp  GPG Key
 curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
 
2. Add the official linux directory of HashiCorp
  sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
  
3. Now update and start install
  sudo apt-get update && sudo apt-get install vault
  
Type the "vault" command to check the installation.

#### Starting Server
For beginning we have chosen to setup a dev server.
It is a built-in, pre-configured server that is not very secure but useful for exploring Vault locally.

    vault server -dev
 
After getting started launch new session.

    export VAULT_ADDR='http://127.0.0.1:8200'
 
Vault CLI determines which Vault servers to send requests using the VAULT_ADDR environment variable. So save the unseal key which is displayed on your screen.

Set the VAULT_TOKEN environment variable value to the generated Root Token value displayed in the terminal output.

    export VAULT_TOKEN="<your token>"
 
To check your server run
     
     vault status

We shall write a secret by command

     vault kv put <path> <key>=<value>  [Path is secret/, key can be value and value is assigned to this key]
     vault kv put secret/hello foo=world
   
To retrieve the secret
     
     vault kv get secret/hello
To this you can add just to print the value of a given field, using the -field=<key_name> flag.


Delete the secret

    vault kv delete secret/hello

   
