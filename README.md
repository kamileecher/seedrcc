<p align="center">
<img src="https://i.pinimg.com/originals/50/90/de/5090de031ef49e2b7beedab9b40ae283.png" align="center" height=205 alt="1337x" />
</p>

<h2 align='center'>Python API Wrapper of Seedr.cc</h2>

## Table of Contents
- [Installation](#installation)
- [Start Guide](#start-guide)
    - [Getting Token](#getting-token)
        - [Logging with Username and Password](#logging-with-username-and-password)
        - [Authorizing with device code](#authorizing-with-device-code)
    - [Basic Examples](#basic-examples)
    - [Managing token](#managing-token)
        - [Refreshing the token manually](#refreshing-the-token-manually)
        - [Refreshing the token automatically](#refreshing-the-token-automatically)
            - [Callback function](#callback-function)
                - [Function with single argument](#callback-function-with-single-argument)
                - [Function with multiple arguments](#callback-function-with-multiple-arguments)
- [Detailed Documentation](#documentation)
- [Contributing](#contributing)
- [Projects using this API](#projects-using-this-api)
- [License](#license)

##  Installation
- Install via [PyPi](https://www.pypi.org/project/seedrcc)
    ```bash
    pip install seedrcc
    ```

- Install from the source
    ```bash
    git clone https://github.com/hemantapkh/seedrcc && cd seedrcc && python setup.py sdist && pip install dist/*
    ```

## Start guide

----

### Getting Token

There are two methods to get the account token. You can login with email/password or by authorizing with device code (Recommended). 


#### Logging with Username and Password

This method uses the seedr Chrome extension API. The token generated by this method expires after 12 hours. You need to [refresh the token](#managing-token) after it expires.
```python
from seedrcc import Login

seedr = Login('foo@bar.com', 'password')

response = seedr.authorize()
print(response)

# Getting the token 
print(seedr.token)
```

### Authorizing with device code

This method uses the seedr kodi API. The token generated by this method expires after 1 year.

To use this method, generate a device & user code. Paste the user code in https://seedr.cc/devices and authorize with the device code.

```python
from seedrcc import Login

seedr = Login()

deviceCode = seedr.getDeviceCode()
# Go to https://seedr.cc/devices and paste the user code
print(deviceCode)

# Authorize with device code
response = seedr.authorize(deviceCode['device_code'])
print(response)

# Getting the token
seedr.token
```

----

### Basic Examples

For all available methods, please refer to the [documentation](https://seedrcc.readthedocs.org/en/latest/).

```python
from seedrcc import Seedr

account = Seedr(token='token')

# Getting user settings
print(account.getSettings())

# Adding torrent
response = account.addTorrent('magnetlink')
print(response)

#Listing folder contents
response = account.listContents()
print(response)
```

----

### Managing token

The token generated by using the username/password method expires after short period of time. So, you need to refresh the token after the token expires. You can refresh the token manually or automatically.

#### Refreshing the token manually

You can manually refresh the token by using the refreshToken method. After calling the refreshToken method, you should store the refreshed token for your future use.

```python
account = Seedr(token='token')
account.getSettings() # Error: Token expired

# Manually refreshing the token
account.refreshToken()

# Getting the new token
print(account.token)
 
# Calling the getSettings method again after refreshing the token
account.getSettings()
```

#### Refreshing the token automatically

Refreshing the token manualy can be a cumbersome task. So, you can use the auto refresh function to refresh the token automatically after it expires.

```python
account = Seedr(token='token', autoRefresh=True)

# Token will be refreshed automatically
account.getSettings()
```

----

#### Callback function

You can also set a callback function which will be called automatically each time the token is refreshed. You can use such function to deal with the refreshed token.

✏️ Note: The callback function must have at least one parameter. The first parameter of the callback function will be the updated token.

#### Callback function with single argument

Here is an example of callback function with a single argument which updates the token in a file called `token.txt`.

```python
# Read the token from token.txt
token = open('token.txt', 'r').read().strip()

# Defining the callback function
def afterRefresh(token):
    with open('token.txt', 'w') as f:
        f.write(token)

account = Seedr(token, autoRefresh=True, callbackFunc=afterRefresh)
```

#### Callback function with multiple arguments

In situations where you need to pass multiple arguments to the callback function, you can use the lambda function. Callback function with multiple arguments can be useful if your app is dealing with multiple users.

Here is an example of callback function with multiple arguments which will update the token of certain user in the database after the token of that user is refreshed.

```python
# Defining the callback function
def afterRefresh(token, userId):
    # Add your code to deal with the database
    print(f'Token of the user {userId} is updated.')

# Creating a Seedr object for user 12345
account = Seedr(token='token', autoRefresh=True, callbackFunc=lambda token: afterRefresh(token, userId='12345'))
```

----

## Documentation

The detailled documentation of each methods is available [here](https://seedrcc.readthedocs.org/en/latest/).


## Contributing

Any contributions you make are **greatly appreciated**.

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request


*Thanks to every [contributors](https://github.com/hemantapkh/1337x/graphs/contributors) who have contributed in this project.*

## Projects using this API

* Torrent Seedr - Telegram bot to download torrents ([Source code](https://github.com/hemantapkh/torrentseedr), [Link](https://t.me/torrentseedrbot)).

Want to list your project here? Just make a pull request.

## License

Distributed under the MIT License. See [LICENSE](https://github.com/hemantapkh/seedrcc/blob/main/LICENSE) for more information.

---

Author/Maintainer: [Hemanta Pokharel](https://twitter.com/hemantapkh)