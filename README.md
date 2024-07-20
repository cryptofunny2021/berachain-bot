## اول python  و pip رو نصب میکنیم
```
sudo apt update
sudo apt install python3 python3-pip
pip3 install web3
```
##  یه فایل به اسم دلخواه درست میکنیم

```
nano botbera.js
```
##  حالا تمام این دستورات رو کپی و پیست میکنیم بدون تغییری
```
const readline = require('readline');
const Web3 = require('web3');

const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout
});

rl.question('Enter your private key: ', (privateKey) => {
  rl.question('Enter recipient\'s address: ', (toAddress) => {
    rl.question('Enter range of amounts to transfer (e.g., 0.00001,0.0002): ', (amountRange) => {
      rl.question('Enter range of intervals in seconds (e.g., 10,15): ', (intervalRange) => {

        const [minAmount, maxAmount] = amountRange.split(',').map(Number);
        const [minInterval, maxInterval] = intervalRange.split(',').map(Number);

        const rpcURL = "https://bartio.rpc.berachain.com/";
        const web3 = new Web3(new Web3.providers.HttpProvider(rpcURL));

        function getRandomAmount(min, max) {
          return (Math.random() * (max - min) + min).toFixed(2);
        }

        function getRandomInterval(min, max) {
          return Math.floor(Math.random() * (max - min + 1) + min);
        }

        function sendTransaction() {
          const amount = getRandomAmount(minAmount, maxAmount);
          const interval = getRandomInterval(minInterval, maxInterval);

          const account = web3.eth.accounts.privateKeyToAccount(privateKey);
          web3.eth.accounts.wallet.add(account);
          web3.eth.defaultAccount = account.address;

          const tx = {
            from: web3.eth.defaultAccount,
            to: toAddress,
            value: web3.utils.toWei(amount, 'ether'),
            gas: 200000,
            gasPrice: web3.utils.toWei('1', 'gwei'),
            chainId: 80084
          };

          web3.eth.sendTransaction(tx)
            .then(receipt => {
              console.log(`Transaction successful with hash: ${receipt.transactionHash} | Amount sent: ${amount} ETH | Next transaction in: ${interval} seconds`);
            })
            .catch(err => {
              console.error('Error sending transaction:', err);
            });

          setTimeout(sendTransaction, interval * 1000);
        }

        sendTransaction();

        rl.close();
      });
    });
  });
});
```

##  حالا ctrl +x+y 
##  دکمه اینتر رو میزنیم
##  و در نهایت فایل رو اجرا میکنیم

```
node botbera.js
```
##  پرایوت کی رو میدیم ( ولت جدید ایجاد کردم و کمی نوکن $bera دپازیت کردم
## ادرس ولتی که توی partice دارم رو بعنوان مقصد بهش دادم
## مقدار رو به صورت 0.00001,0.0002
## زمان ارسال تراکنش 10,15


## به تعدادی که نیازی اجازه ی تراکنش میدیم و در نهایت فایل رو میبندیم
## ctrl + c
