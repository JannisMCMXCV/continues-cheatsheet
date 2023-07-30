# Computers, Programming & Stuff

## Git

### rename branch: 
````
git branch -m <old> <new>
````
### GPG signed commits
> Create verified commits<br>
> #gpg #git #hithub

1. Install GPG Suite from https://gpgtools.org
2. Generate a new gpg key with GPG Keychain
3. Save the public key somewhere you will find it!
4. Add the public key to gitHub:
   1. navigate to https://github.com/settings/keys
   2. select [New GPG Key](https://github.com/settings/gpg/new)
   3. Copy and paste the public key block; -> begins with:
    ````
    -----BEGIN PGP PUBLIC KEY BLOCK-----
    ````
5. Tell git to sign commits
   1. Fire up a local terminal
   2. find the key ID of the previously generated Key
    ````
    % gpg --list-secret-keys --keyid-format=long
    `````
   3. From the list of GPG keys, copy the long form of the GPG key ID for the pubkic key you've added to gitHub. The GPG key ID looks something like `3CC7D39017658BD1` -> see this example:
   ````
    % gpg --list-secret-keys --keyid-format=long 
    /Users/exampleuser/.gnupg/pubring.kbx
    ------------------------------------
    sec   rsa4096/3CC7D39017658BD1 2023-06-25 [SC]
          BD2D9C59394D7F4F8680DEB80FA94B9E0B5C93C0
    uid                 [ultimate] github <example@email.com>
    ssb   rsa4096/CC50DC7499FF0A21 2023-06-25 [E]
    ````
    4. Set the GPG signing key ID in Git; In this example, the GPG key ID is `3CC7D39017658BD1`:
    ```
    % git config --global user.signingkey 3CC7D39017658BD1
    ``` 
    5. [optional] Configure Git to sign all commits by default:
    ````
    % git config --global commit.gpgsign true
    ````

### fetch remote branch other than master/main:

* `git fetch origin`
    > fetches all remote branches available 
  
* `git branch -v -a`

    > shows all branches ready to checkout
* -> checkout remote branch like:

    `git checkout -b <branchname> origin/<branchname>`

### Remote Repositories

* Add remote repo:

    `git remote add <repo name (best practise: origin)> <remote repository url>`


* get remote repository origin url:

    `git remote get-url origin`

* change remote repository:

    `git remote set-url origin <remote repository url>`

* update forked repository: (the circuitous way)
	```
    git remote add upstream <REPO-URL>
    git fetch upstream
    git checkout <BRANCH>
    git reset --hard upstream/<BRANCH>
    git push origin <BRANCH> --force
    ```

* fix: unable to create file \<filename\>: Filename too long
    * Alt1: `git config --system core.longpaths true`
        * Admin rights may be required
    * Alt2: open C:\Program Files\Git\etc\gitconfig -> under `[core]` add `longpaths = true`
        * Admin rights may be required.

## GitHub
### Code/PR Review
* Collapse all files:<br>
`ALT + Click` on any caret on the left of a file name
![Collapse all files in GitHub](./images/github/github-collapse-all.png)

## PostgreSQL

* start db server:
    * Environment Variable PGDATA festlegen (default [WINDOWS]: C:\Program Files\PostgreSQL\14\data)
    ```
    pg_ctl start -l logfile (startet db server im Hintergrung und schreibt loggs in "logfile")
    ```

* stop db server:
    ```
    pg_ctl stop
    ```

* check if running [WINDOWS]: 

    * Alt1:
        * WIN+R
        * `services.msc`
        * look for postgresql-[version]
    * Alt2:
        * fire up a terminal
        * `pg_ctl status`

* change pw:
    ````
    psql -U <username>
    postgres=# \password <username>
    Enter new password: <new-password>
    postgres=# \q
    ````

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
1. Locally generate key pair (rsa between 2048 and 4096 byte or ed25519)
2. registr public key
![Register public key](./images/binance/binance-testnet/register-public-key.png)



## Markdown
Pagebreak for generated PDFs: <br>
wrap the section that shall be on a new Page in: 
```
<div style="page-break-after: always;">
    Business as usual
</div>
```

## Microsoft Windows

### Shortcuts
Action | Shortcut | Version
-- | -- | --
Emoji Keyboard | `WINDOWS + .` | 10 or newer

### cmd, bat, etc...
* Alternative name for a command:
  1. create a *.bat file
  2. 
        ```bat
        @echo off
        set "NEW_COMMAND_NAME=C:\path\to\COMMAND_EXECUTABLE"
        %NEW_COMMAND_NAME% %*
        ```
     * The `%NEW_COMMAND_NAME% %*` line executes the Maven command stored in the `NEW_COMMAND_NAME` variable, and `%*` passes any additional arguments to the executable.
     * To find out where the executable is, run `where <executable>`
  3. include the directory where your *.bat is located in your (user-level) `PATH` variable.
  4. Use the new command: `new-command-name` in any Command Prompt.

## Visual Studio
### Shortcuts
Action | Shortcut
-- | --
Comment | `CTRL + K, CTRL + C`
Uncomment | `CTRL + K, CTRL + U`
Collapse all methods | `CTRL + M, CTRL + O`
Expand all methods | `CTRL + M, CTRL + P`
Collapse/expand selection | `CTRL + M, CTRL + M`

## Programming
### Java stream API vs C# LINQ


<table>
<tr>
  <th>Java</th>
  <th>C#</th>
  <th>Java Example</th>
  <th>C# example</th>
</tr>

<tr>
  <td><code>Collection.forEach(&lt;lambda&gt;)</code></td>
  <td><code>Enumerable.ForEach(&lt;lambda&gt;)</code></td>
<td>
    

```java
List<String> strings = new ArrayList<>();
strings.add("demo");
strings.add("list");
strings.add("of");
strings.add("strings");

// Print list entries
strings.forEach(System.out::println);
```

</td>
<td>
    

```c#
List<string> strings = new List<string>
{
    "demo", "list", "of", "strings"
};

// Print list entries
strings.ForEach(Console.WriteLine)
```

</td>
</tr>

<tr>
  <td><code>Collection.stream().map(&lt;lambda&gt;)</code></td>
  <td><code>Enumerable.Select(&lt;lambda&gt;)</code></td>
<td>
    

```java
List<String> strings = new ArrayList<>();
strings.add("demo");
strings.add("list");
strings.add("of");
strings.add("strings");

// convert entries to upper case
List<String> converted = strings.stream()
        .map(str -> str.toUpperCase());
        .collect(Collectors.toList());
```

</td>
<td>
    

```c#
List<string> strings = new List<string>
{
    "demo", "list", "of", "strings"
};

// convert entries to upper case
List<string> converted = strings
        .Select(str => str.ToUpper())
        .ToList();
```

</td>
</tr>

<tr>
  <td><code>Collection.stream().filter(&lt;lambda&gt;)</code></td>
  <td><code>Enumerable.Where(&lt;lambda&gt;)</code></td>
<td>
    

```java
List<String> strings = new ArrayList<>();
strings.add("demo");
strings.add("list");
strings.add("of");
strings.add("strings");

// filter for entries containing the letter 'i'
List<String> filtered = strings.stream()
        .filter(str -> str.contains("i"));
        .collect(Collectors.toList());  // -> [list, strings]
```

</td>
<td>
    

```c#
List<string> strings = new List<string>
{
    "demo", "list", "of", "strings"
};

// filter for entries containing the letter 'i'
List<string> filtered = strings
        .Where(str => str.Contains("i"))
        .ToList();  // -> [list, strings]
```

</td>
</tr>

<tr>
  <td><code>Collection.stream().takeWhile(&lt;lambda&gt;)</code></td>
  <td><code>Enumerable.TakeWhile(&lt;lambda&gt;)</code></td>
<td>
    

```java
List<Integer> numbers = new ArrayList<>();
numbers.add(1);
numbers.add(2);
numbers.add(3);

// take while number n == first entry or n is even
List<Integer> takenNumbers = numbers.stream()
        .takeWhile(n -> n.equals(numbers.get(0))  || n % 2 == 0)
        .collect(Collectors.toList());  // -> [1, 2]
```

</td>
<td>
    

```c#
List<int> numbers = new List<int> { 1, 2, 3 };

// take while number n == first entry or n is even
List<int> takenNumbers = numbers
        .TakeWhile((n, index) => index == 0 || n % 2 == 0)
        .ToList();  // -> [1, 2]
```

</td>
</tr>

</table>

## MongoDB

### cli: drop all tables
```
# mongo
> show dbs
> use <whatever db>
> db.getCollectionNames().forEach(function(c) { if(c.indexOf("system.") == -1) db.getCollection(c).drop(); })
> quit()
```

## Apache Maven

### find unused dependencies
    # mvn dependency:analyze
