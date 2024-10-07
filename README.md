# How to Use cURL With Proxy

[![Oxylabs promo code](https://user-images.githubusercontent.com/129506779/250792357-8289e25e-9c36-4dc0-a5e2-2706db797bb5.png)](https://oxylabs.go2cloud.org/aff_c?offer_id=7&aff_id=877&url_id=112)

[![](https://dcbadge.vercel.app/api/server/eWsVUJrnG5)](https://discord.gg/GbxmdGhZjq)

[<img src="https://img.shields.io/static/v1?label=&message=Curl&color=brightgreen" />](https://github.com/topics/curl) [<img src="https://img.shields.io/static/v1?label=&message=Proxy&color=important" />](https://github.com/topics/proxy)

- [What is cURL?](#what-is-curl)
- [Installation](#installation)
- [What you need to connect to a proxy](#what-you-need-to-connect-to-a-proxy)
- [Command line argument to set proxy in cURL](#command-line-argument-to-set-proxy-in-curl)
- [Using environment variables](#using-environment-variables)
- [Configure cURL to always use proxy](#configure-curl-to-always-use-proxy)
- [Ignore or override proxy for one request](#ignore-or-override-proxy-for-one-request)
- [Bonus tip – turning proxies off and on quickly](#bonus-tip--turning-proxies-off-and-on-quickly)
- [cURL socks proxy](#curl-socks-proxy)

This step-by-step guide will explain how to use cURL or, simply, curl with proxy servers. It covers all the aspects, beginning from installation to explaining various options to set the proxy.

For a detailed explanation, see our [blog post](https://oxy.yt/ArRn).

## What is cURL?

cURL is a command line tool for sending and receiving data using the URL. The following command gets the HTML of the page and prints it in the console:

```shell
curl https://www.google.com
```

## Installation

cURL is provided with many Linux distributions and MacOS. Now, it is also provided with Windows 10. You can check whether your computer has curl installed by opening your terminal and running the following command:

```shell
curl --version
```

## What you need to connect to a proxy

You’ll need the following information to anonymize curl behind a proxy:

- proxy server address
- port
- protocol
- username (if authentication is required)
- password (if authentication is required)

In this tutorial, let's assume that the proxy server is **127.0.0.1**, the port is **1234**, the user name is **user**, and the password is **pwd**. We will look into multiple examples covering various protocols.

## Command line argument to set proxy in cURL

Open the terminal, type the following command, and press Enter:

```shell
curl --help
```

The output is going to be a huge list of options. One of them is going to look like this:

```shell
-x, --proxy [protocol://]host[:port] 
```

Note that **x** is lowercase, and it is case-sensitive. The proxy details can be supplied using the **-x** or **–proxy** switch. Both mean the same thing:

```shell
curl -x "http://user:pwd@127.0.0.1:1234" "http://httpbin.org/ip"
```

or

```shell
curl --proxy "http://user:pwd@127.0.0.1:1234" "http://httpbin.org/ip"
```

**NOTE.** If there are SSL certificate errors, add **-k** (lowercase) to the **curl** command. This will allow insecure server connections when using SSL:

```shell
curl --proxy "http://user:pwd@127.0.0.1:1234" "http://httpbin.org/ip" -k
```

Another interesting thing to note here is that the default proxy protocol is HTTP. Thus, the following two commands will do exactly the same:

```shell
curl --proxy "http://user:pwd@127.0.0.1:1234" "http://httpbin.org/ip"
curl --proxy "user:pwd@127.0.0.1:1234" "http://httpbin.org/ip"
```

## Using environment variables

Another way to use proxy with curl is to set the environment variables **http_proxy** and **https_proxy**:

```shell
export http_proxy="http://user:pwd@127.0.0.1:1234"
export https_proxy="http://user:pwd@127.0.0.1:1234"
```

After running these two commands, run **curl** normally:

```shell
curl "http://httpbin.org/ip"
```

To stop using a proxy, turn off the global proxy by unsetting these two variables:

```shell
unset http_proxy
unset https_proxy
```

## Configure cURL to always use a proxy

If you want a proxy for curl but not for other programs, you can create a [curl config file](https://everything.curl.dev/cmdline/configfile.html).

For Linux and MacOS, open the terminal and navigate to your home directory. If there is already a **.curlrc** file, open it. If there is none, create a new file. Here are the set of commands that can be run:

```shell
cd ~
nano .curlrc
```

In this file, add a proxy authorization line:

```shell
proxy="http://user:pwd@127.0.0.1:1234"
```

Save the file. Now, curl with proxy is ready to be used. 

Simply run **curl** normally and it will read the proxy from **.curlrc** file.

```shell
curl "http://httpbin.org/ip"
```

On Windows, the file is named **_curlrc**. This file can be placed in the **%APPDATA%** directory.

To find the exact path of **%APPDATA%**, open the command prompt and run the following command:

```shell
echo %APPDATA%
```

This directory will be something like **C:\Users\<your_user>\AppData\Roaming**. Now go to this directory, create a new file **_curlrc**, and set the proxy by adding this line:

```shell
proxy="http://user:pwd@127.0.0.1:1234"
```

It works exactly the same way in Linux, MacOS, and Windows.

## Ignore or override proxy for one request

To override the proxy for one request, set the new proxy using the **-x** or **–proxy** switch as usual:

```shell
curl --proxy "http://user:pwd@1.0.0.1:8090" "http://httpbin.org/ip"
```

## Bonus tip – turning proxies off and on quickly

You can create an alias in your **.bashrc** file to set proxies and unset proxies. For example, open the **.bashrc** file using any editor and add these lines:

```shell
alias proxyon="export http_proxy=' http://user:pwd@127.0.0.1:1234';export https_proxy=' http://user:pwd@127.0.0.1:1234'"
alias proxyoff="unset http_proxy;unset https_proxy"
```

After adding the lines, save the **.bashrc** and update the shell to read this **.bashrc**. To do this, run this command in the terminal:

```shell
. ~/.bashrc
```

Now, you can quickly turn on the proxy, run one or more curl commands, and then turn off the proxies like this:

```shell
proxyon
curl "http://httpbin.org/ip"
curl "http://google.com"
proxyoff 
```

## cURL SOCKS proxy

If the proxy server is using the SOCKS protocol, the syntax remains the same:

```shell
curl -x "socks5://user:pwd@127.0.0.1:1234" "http://httpbin.org/ip"
```

If you want to learn more about using cURL with proxy, see our [blog post](https://oxy.yt/ArRn).
