<img src="https://avatars.githubusercontent.com/u/96357480?s=400&u=f54a2fab0e5faaf6bccf57b993e0a28ca2102001&v=4" width="100">  |  [◀️ GoraNetwork](https://github.com/GoraNetwork)|
| -------- | ------- |
## Gora Feeds Guide

### Price Feed Oracle Consumer Example

This is a code snippet that shows the consumption of an Algoracle global state price pair oracle. This example reads from two smart contracts: the price feed index contract that stores the current oracle appId for each price pair, and the corresponding contract for the desired price pair oracle.

These institutional data providers are currently feeding the price feeds:

- Kaiko
- Amberdata
- dxFeed
- Brave New Coin
- Crypto Compare
- Twelve Data
  
To view the index contract and look up the appId for a price pair on testnet:

https://testnet.algoexplorer.io/application/70820731

To view the algo/usd oracle being udpated, see the following app:

https://testnet.algoexplorer.io/application/53083112

Requirements:
Python3
Python modules: pyteal, python-dotenv, py-algorand-sdk
Running the App
To run the app, follow these steps:
1- Clone the repo
2- Create a .env file with the following:
```
secret_key = <The secret key of an algorand account>
creator_address = <The algorand address for the secret_key>
sender_secret_key = <The secret key of another algorand address , used to call theapp
```
                     
From the directory where you cloned the application, run the following command:
```
python3 main.py
```

You will see the following output:

```
------
1 - created new appid : 58165098
------
2 - price has been updated to global state. Here is your transaction ID:  EAUIGDX4OM6D5GH2WL5CFDQYG6GHVYIWG4RUSUMA2PFYCUOD3IEQ
------
3 - Thank your for using the price oracle. Your app has now been deleted. Deleted app-id: 58165098
```

To run the applications multiple times, you can remove the following line on
main.py line 66
```
delete_app(algod_client, os.environ.get('secret_key'),  created_app_id)
```

This last line immediately deletes the created application, so as not to use up the account's allowed limit of 10 apps.

For any questions about the above procedure, email us at communications@gora.io and one of Gora developer relations will be in touch with you!
