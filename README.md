# ByteHouse CLI

ByteHouse CLI is a command-line application for the most direct way to interact with [Bytehouse Services](https://bytehouse.cloud/)

## Installation

### MacOS

To install the ByteHouse CLI, make sure you have Homebrew installed on your machine and run the following commands:

```sh
brew tap bytehouse-cloud/homebrew-core
brew install bytehouse-cli
```
To upgrade the ByteHouse CLI, run the following command:

```sh
brew upgrade bytehouse-cli
```

### Linux

You can get the most updated version here: https://github.com/bytehouse-cloud/cli/releases/

```sh
curl -o bytehouse-cli -L https://github.com/bytehouse-cloud/cli/releases/download/v1.5.17.1.1/bytehouse-v1.5.17.1.1-linux-amd64
chmod +x bytehouse-cli 

# You might want to add this binary executable to your `~/.bashrc` as alias, or `~/.zshrc`l
echo "alias bytehouse-cli=\"$(pwd)/bytehouse-cli\"" > ~/.bashrc
```

### Windows
Download the latest installer (bytehouse-vX.X.XX.X-windows-amd64) from https://github.com/bytehouse-cloud/cli/releases

# Getting Started

## Credentials
There are few credentials you would need to get started

- Account Name
- User Name
- Password
- Region

> Note: The information you would need is the same as how you would log in with Web UI.

If you are unsure, you can also check your details on the [Web Console](https://console.bytehouse.cloud/account/details).

## Using the CLI

The simplest way to start the application is to run it in your command line or PowerShell.

#### With Flags

When specifying flag and its value when starting the application, the format is `--<flag> value` , eg `--user mary`
Flags also have an alias, see in Reference: Alias
An example of starting bytehouse-cli is shown below:
```sh
bytehouse-cli --user <user name> --account <account name> --password <password> --region <region name> --secure
# Example
$ bytehouse-cli --user bob --account AWSXXX --password coolbob --region cn-north-1 --secure
```
> Note: --secure flag is needed when connecting to bytehouse's public domain

### With Configuration File

Sometimes it's neater and more manageable to keep all flags in a configuration file. With the configuration file, you can also specify query settings in it. You can use the -cf flag with the path to configuration file as value.

For full usage of configuration File, see in Reference: Configuration File
An example of configuration file and usage is shown below:

```sh
$ cat bytehouse_conf.toml
# Settings for connection
account = "AWSXXXXX"
user = "bob"
password = "coolbob"
region = "cn-north-1"
secure = true

# Settings for query Settings
ansi_sql = true 

$ bytehouse-cli -cf bytehouse_conf.toml
```

## Non-Interactive Mode

Sometimes you could be writing shell script and it could be impractical to get into interactive mode. Bytehouse CLI allows the user to execute a SQL command and exit automatically.

### With Query Flag

If you launch bytehouse-cli with `-q` or `--query` flag, that SQL statement will be executed and bytehouse-cli will exit immediately after the execution.

```sh
$ bytehouse-cli -q "select 1"
```

### With stdin
Users can also allow bytehouse-cli to take in input from stdin .
```
$ echo "select 1" | bytehouse-cli
```

### Scripting
Users can also write a SQL script and pipe the input to bytehouse-cli

- Queries are separated by `;`
- Queries will be run sequentially
- Stops further execution when first query execution returns with an error

```sh
$ cat example.sql
CREATE DATABASE bob_db;
USE bob_db;
CREATE TABLE bob_numbers
(
   i Int32
)
ENGINE = CnchMergeTree
ORDER BY i;
SHOW CREATE TABLE bob_numbers;

$ bytehouse-cli < example.sql

# This is also accepted
$ cat example.sql | bytehouse-cli
```

## Data Insertion
It is very common to load data from a file, below shows some examples on how to do so.

### From query

#### Interactive Mode
```sh
Bytehouse » INSERT INTO bob_db.bob_number VALUES (1), (2), (3)
```
#### Non-Interactive Mode
```sh
$ bytehouse-cli -q "INSERT INTO bob_db.bob_number VALUES (1), (2), (3)"
```

### From a local file

#### Interactive Mode
```sh
Bytehouse » INSERT INTO bob_db.bob_number FORMAT csv INFILE 'path/to/data.csv'
```
#### Non-Interactive Mode
```sh
$ bytehouse-cli -q "INSERT INTO bob_db.bob_number FORMAT csv" < 'path/to/data.csv'
```

## Data Export
You can use the `INTO OUTFILE` syntax after your query to save your results to a local file.
```sh
Bytehouse » SELECT * FROM bob_db.bob_number INTO OUTFILE 'out.csv' format csv
```

## Version Check
You can check the version of the ByteHouse CLI using the -v or --version flag. When flag is specified, ByteHouse CLI does not start
```sh
$ bytehouse-cli -v
v1.5.2
```
## Help
You can show all the flags supported by using -h flag or --help
```
#To display all options and their alias
bytehouse-cli -h    
```
