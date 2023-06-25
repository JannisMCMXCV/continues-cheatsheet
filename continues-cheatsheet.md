# Computers, Programming & Stuff

## key pairs with `openssl` [POSIX]

### generate rsa key pair

> #openssl #rsa #ed25519 #ssl
1. generate private rsa key [stores the private key in the current directory]
    ````
    % openssl genrsa -out <private key name> <size in bytes>
    ````
2. generate public key
    ````
    % openssl rsa -in <private key name> -pubout -outform <output format (e.g. PEM)> -out <private key name>
    ````
> **Notes**
> * documentation (POSIX)
>     ````
>     % man openssl
>     ````

### generate ed25519 key pair
> #binance test net
1. generate private ed25519 key [stores the private key in the current directory]
    ````
    % openssl genpkey -algorithm ed25519 -out <private key name>
    ````
2. generate public key
    ````
    % openssl pkey -pubout -in <private key name> -out <public key name>
    ````
> **Notes**
> * Useful Links:
>   * https://testnet.binance.vision

## ssh key pairs with `ssh-keygen` [POSIX]

### generate ssh key pair

> #ssh #encryption #encoding #POSIX #github
1. generate ssh key pair (example)
    ````
    % ssh-keygen -t rsa -b 4096 -f <keyname>
    ````
2. [optional] convert key format (example) -> e.g., to use rsa public key with PKCS8 format
    ```
    % ssh-keygen -f <keyname>.pub -e -m PKCS8 > <converted filename>
    ```
> Notes:
> * Addition to 1.: If switch `-f <keyname>` is provided, the keypair will be saved to `~/.ssh` esle you'll be prompted to specify a location and filename.
> * `~/.ssh` resolves to `/Users/<user>/.ssh/`
> * documentation (POSIX)
>     ````
>     % man ssh-keygen
>     ````
> * Useful Links:
>   * https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent
>   * https://www.atlassian.com/git/tutorials/git-ssh
>   * https://www.binance.com/en/my/settings/api-management

### utilize ssh-keygen pair for binance:

> #binance
1. generate ssh key pair (this only formats the private key with the PKCS#8 format) (example)
    ````
    % ssh-keygen -t rsa -b 4096  -m PKCS8 -f <keyname>
    ````
2. convert public key to PKCS#8 format (example)
    ```
    % ssh-keygen -f <keyname>.pub -e -m PKCS8 > <converted filename>
    ```
> **Notes:**
> * Useful Links:
>   * https://binance-docs.github.io/apidocs/spot/en/#introduction
>   * https://www.binance.com/en/my/settings/api-management

## Setup binance testnet
1. Visit https://testnet.binance.vision/
2. Log in with GitHub (GitHub account required)
![Authenticate with GitHub](./images/binance/binance-testnet/authenticate-with-github.png)
3. Generate locally key pair (rsa between 2048 and 4096 byte or ed25519)
4. registr public key
![Register public key](./images/binance/binance-testnet/register-public-key.png)