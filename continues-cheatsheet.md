# Computers, Programming & Stuff

## create ssh/rsa keys
> Use Cases: ssh-key for github, rsa key for self generated binance api key<br>
> #binance #ssh #encryption #encoding
1. create ssh key pair (example)
    ````
    % ssh-keygen -t rsa -b 4096 -f <keyname>
    ````
2. convert key format (example) -> e.g., to use rsa public key for binance self-generated api keys
    ```
    % ssh-keygen -f <keyname>.pub -e -m PKCS8 > <converted filename>
    ```
### Notes:
* Addition to 1.: If switch `-f <keyname>` is provided, the keypair will be saved to `~/.ssh` esle you'll be prompted to specify a location and filename.
* `~/.ssh` resolves to `/Users/<user>/.ssh/`
* documentation (POSIX)
    ````
    man ssh-keygen
    ````
* Useful Links:
  * https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent
  * https://www.atlassian.com/git/tutorials/git-ssh
  * https://www.binance.com/en/my/settings/api-management

## create rsa key pair for binance [for POSIX Systems]:
1. create ssh key pair (this only formats the private key with the PKCS#8 format) (example)
    ````
    % ssh-keygen -t rsa -b 4096  -m PKCS8 -f <keyname>
    ````
2. convert public key to PKCS#8 format (example)
    ```
    % ssh-keygen -f <keyname>.pub -e -m PKCS8 > <converted filename>
    ```
### Notes:
* Useful Links:
  * https://binance-docs.github.io/apidocs/spot/en/#introduction
  * https://www.binance.com/en/my/settings/api-management
