# wallet
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>WalletConnect Example</title>
    <script src="https://unpkg.com/@walletconnect/web3-provider/dist/umd/index.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/web3/1.6.1/web3.min.js"></script>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            height: 100vh;
            background-color: #f5f5f5;
        }
        #connectButton {
            padding: 10px 20px;
            font-size: 16px;
            cursor: pointer;
            border: none;
            background-color: #007bff;
            color: white;
            border-radius: 5px;
            transition: background-color 0.3s;
        }
        #connectButton:hover {
            background-color: #0056b3;
        }
        #status {
            margin-top: 20px;
            font-size: 18px;
        }
    </style>
</head>
<body>
    <h1>Connect Your Wallet</h1>
    <button id="connectButton">Connect Wallet</button>
    <div id="status"></div>

    <script>
        // Create WalletConnect Provider with your Infura API key for Polygon mainnet
        const provider = new WalletConnectProvider.default({
            infuraId: "f2c206adca0e4f6f8e25a44e28810c59" // Your Infura Project ID
        });

        // Connect button event listener
        document.getElementById('connectButton').onclick = async () => {
            try {
                // Check if already connected
                if (!provider.connected) {
                    // Enable session (triggers QR Code modal)
                    await provider.enable();
                }

                // Create Web3 instance
                const web3 = new Web3(provider);

                // Display wallet address
                const accounts = await web3.eth.getAccounts();
                document.getElementById('status').innerText = Connected: ${accounts[0]};
            } catch (error) {
                console.error("Connection failed:", error);
                document.getElementById('status').innerText = "Connection failed. Please try again.";
            }
        };

        // Subscribe to connection events
        provider.on("connect", (error, payload) => {
            if (error) {
                console.error(error);
                return;
            }
            console.log("Connected:", payload);
        });

        provider.on("disconnect", (error, payload) => {
            if (error) {
                console.error(error);
                return;
            }
            document.getElementById('status').innerText = "Disconnected";
            console.log("Disconnected:", payload);
        });
    </script>
</body>
</html>
