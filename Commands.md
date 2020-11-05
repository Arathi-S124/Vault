
### Installation in Linux

1. Add the Hasicorp  GPG Key
       
       curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
 
2. Add the official linux directory of HashiCorp
  
       sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
  
3. Now update and start install
  
       sudo apt-get update && sudo apt-get install vault
  
Type the "vault" command to check the installation.

### Starting Server
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

### Secrets
We shall write a secret by command

     vault kv put <path> <key>=<value>  [Path is secret/, key can be value and value is assigned to this key]
     vault kv put secret/hello foo=world
   
To retrieve the secret
     
     vault kv get secret/hello
To this you can add just to print the value of a given field, using the -field=<key_name> flag.


Delete the secret

    vault kv delete secret/hello

Enable the secret engine
   
    vault secrets enable -path=kv kv
    Each path is completely isolated and cannot talk to other paths. 
 
To list out information about seret engines

    vault secrets list

Disabling a secret engine

    vault secrets disable <secret-engine>/
    
### Deploy Vault

  For configuring vault HCL files are used.
  ```
  storage "raft" {
  path    = "./vault/data"
  node_id = "node1"
  }

  listener "tcp" {
  address     = "127.0.0.1:8200"
  tls_disable = 1
  }
  disable_mlock = true
  api_addr = "http://127.0.0.1:8200"
  cluster_addr = "https://127.0.0.1:8201"
  ui = true
  ```
  
  Set the -config flag to point to the proper path where you saved the configuration above.

       vault server -config=config.hcl
       
  During initialization, the encryption keys are generated, unseal keys are created, and the initial root token is setup. To initialize Vault use
      
       vault init 
   
  Every initialized Vault server starts in the sealed state.  So we shall unseal it.
   
       vault operator unseal
       
  Finally, authenticate as the initial root token
   
       vault login <token>

### AWS AND Secret
  Configuring Vault to AWS

        vault secrets enable -path=<path> aws
        vault write aws/config/root access_key <access key> secret_key <secret key> region=<region>  
        
   Create role and attach a policy
        
        vault write aws/role/<role name> credential_type=<credential type> policy_document=-<<EOF <policy> EOF  
   
   You can generate the new access and secret access key
        
        vault read aws/role/<role name>           
   
   Revoking the specified secret
        
        vault lease revoke <lease id>                                                                        

