
# Welcome to DracoArts

![Logo](https://dracoarts-logo.s3.eu-north-1.amazonaws.com/DracoArts.png)




# Connect Metamask PC Game Unity3D Example

The LYNC Metamask PC SDK is a cutting-edge software development kit designed to empower game developers with seamless Metamask Wallet integration for blockchain-based PC games. This SDK eliminates the complexities of Web3 development, allowing studios to incorporate crypto transactions, NFT interactions, and decentralized gaming economies with minimal effort.

Built for scalability and ease of use, the LYNC Metamask PC SDK enables developers to deploy their games across multiple blockchain networks while providing robust analytics and real-time monitoring. Whether you're building a play-to-earn (P2E) game, integrating NFT assets, or enabling tokenized rewards, this SDK serves as a one-stop solution for all Web3 gaming needs.

# Key Features & Capabilities

## 1. Plug-and-Play Metamask Integration
### One-Click Wallet Connection

 - Players can securely link their Metamask wallets directly within the game.

###  Transaction Signing 
 - Supports in-game purchases, token transfers, and smart contract interactions without leaving the game.

### Cross-Platform Compatibility
-  Works seamlessly across Unity, Unreal Engine, Godot, and custom game engines.

## 2. Multi-Chain Deployment Made Simple
### Supports EVM-Compatible Blockchains 
- Deploy your game on Ethereum, Polygon, BNB Chain, Avalanche, Arbitrum, and more with just a few configuration changes.

### Automatic Network Switching 
- Players can switch between supported blockchains without disruptions.

## 3. LYNC Analytics Dashboard

### Real-Time Player Insights 
- Track wallet interactions, transaction volumes, NFT usage, and player retention.

### Custom Event Tracking 
 - Monitor in-game economic activity, such as token burns, staking, and marketplace trades.

### Data-Driven Optimization 
 - Adjust game mechanics and monetization strategies based on player behavior.

## 4. Zero-Downtime SDK Updates
### Over-the-Air (OTA) Updates 
- Receive the latest SDK enhancements without manual patches or engine modifications.

### Backward Compatibility 
- Ensures existing integrations remain functional even after updates.

## 5. Enhanced Security & Compliance
### Non-Custodial Wallet Management 
 - Players retain full control of their private keys; no centralized risks.

### Anti-Fraud & Rate Limiting 
 - Built-in safeguards against spam transactions and malicious attacks.

### Gas Fee Optimization 
 - Smart transaction batching and gas estimation for smoother user experiences.

 # For Game Developers:
## ✔ Faster Web3 Integration 

 - Reduces development time by abstracting complex blockchain interactions.
## ✔ Future-Proof Architecture

 - Adapts to new blockchain trends without requiring full redevelopment.
## ✔ Monetization Opportunities 

 - Enable NFT sales, tokenized assets, and decentralized marketplaces effortlessly.

 # For Players:
## ✔ True Ownership 
- Securely trade, sell, or use in-game assets across compatible platforms.
## ✔ Frictionless Transactions 
- No need to exit the game for crypto payments or wallet management.
## ✔ Cross-Game Compatibility 

 - Assets purchased in one LYNC-powered game may be usable in others.

# Use Cases
## Play-to-Earn (P2E) Games 
 - Reward players with tradable tokens and NFTs.

## NFT-Based Games 
-  Integrate digital collectibles, character skins, and land ownership.

## Blockchain RPGs & MMORPGs 
 - Enable player-driven economies with decentralized item trading.

## Metaverse Experiences 
 - Allow cross-platform asset portability and virtual world interactions.

 # Getting Started
- The LYNC Metamask PC SDK is designed for quick adoption, with:

 ## Comprehensive Documentation 
 - Step-by-step guides for integration.

## Developer Support 
 - Dedicated assistance for troubleshooting and optimization.

## Sandbox Testing Environment 
- Test transactions and smart contracts in a risk-free setting.


#  Prerequisites
- Before starting, ensure you have:
✅ MetaMask  (installed as a browser extension)

✅ Unity Hub (2021.3 LTS or later recommended)

✅  (Download from [GitHub](https://github.com/LYNC-WORLD/Metamask-Unity-PC-SDK/releases/tag/v1.0.0)

✅ Import into Unity

## Get your API Key
Please get your API key before downloading the SDK from [here](https://www.lync.world/form.html
)



# Step 1: Add LYNC Manager to Your Scene
- Go to Assets/LYNC-Metamask/

- Drag LYNC Manager.prefab into your first scene (e.g., MainMenu).

- LYNC Manager Prefab

## Configure Network (Testnet/Mainnet)

- Select LYNC Manager in the Hierarchy.

- In the Inspector, choose:

 #### Network: 
 Testnet (for development) or Mainnet (production).


# Final Thoughts
The LYNC Metamask PC SDK bridges the gap between traditional gaming and Web3, providing developers with the tools to build, deploy, and scale blockchain games efficiently. By simplifying wallet integration, multi-chain support, and player analytics, it allows studios to focus on gameplay innovation while leveraging the full potential of decentralized technologies.

## 🚀 Ready to take your game into the Web3 era? Integrate LYNC Metamask PC SDK today!



    using UnityEngine;
    using TMPro;
    using LYNC;
    using UnityEngine.UI;
    using UnityEngine.EventSystems;

    public class MetamaskExample : MonoBehaviour
    {
    public Button login, logout, sendTokenButton;
    public TMP_Text MetamaskWalletAddress;
    public Transform transactionResultsParent;
    public GameObject transactionResultHolder;
    [Space]
    [Header("Transaction details")]
    public Transaction SendTokenTrx;

    private void OnEnable()
    {
        LyncManager.onLyncReady += LyncReady;
    }

    private void Awake()
    {
        login.interactable = false;
        logout.interactable = false;
        sendTokenButton.interactable = false;
        Application.targetFrameRate = 30;
    }

    private void LyncReady(LyncManager Lync)
    {
        login.interactable = true;

        login.onClick.AddListener(() =>
        {
            Lync.WalletAuth.ConnectWallet((wallet) =>
            {
                OnWalletConnected(wallet);
            });
        });

        logout.onClick.AddListener(() =>
        {
            Lync.WalletAuth.Logout();
            login.interactable = true;
            logout.interactable = false;
            sendTokenButton.interactable = false;
        });

        sendTokenButton.onClick.AddListener(async () =>
        {
            TransactionResult txData = await LyncManager.Instance.TransactionsManager.SendTransaction(SendTokenTrx);
            if (txData.success)
                SuccessfullTransaction(txData.hash);
            else
                ErrorTransaction(txData.error);

            sendTokenButton.interactable = true;
        });

    }

    private void OnWalletConnected(AuthBase _authBase)
    {
        MetamaskWalletAddress.text = "Wallet address = " + _authBase.PublicAddress;

        login.interactable = false;
        logout.interactable = true;
        sendTokenButton.interactable = true;
    }

    private void SuccessfullTransaction(string hash)
    {
        var go = Instantiate(transactionResultHolder, transactionResultsParent);

        go.transform.GetComponentInChildren<TMP_Text>().text = "<color=\"green\">Success - Click to check block explorer<color=\"green\">";
        EventTrigger trigger = go.GetComponent<EventTrigger>();
        EventTrigger.Entry entry = new()
        {
            eventID = EventTriggerType.PointerClick
        };
        entry.callback.AddListener((eventData) => { Application.OpenURL("https://sepolia.etherscan.io/tx/" + hash); });
        trigger.triggers.Add(entry);
    }

    private void ErrorTransaction(string error, string txnTitle = "")
    {
        var go = Instantiate(transactionResultHolder, transactionResultsParent);
        go.transform.GetComponentInChildren<TMP_Text>().text = txnTitle + " <color=\"red\">TXN ERROR:</color=\"red\"> " + error;
    }
    }

## Images 

![](https://github.com/AzharKhemta/Gif-File-images/blob/main/Pc%20Game%20MetaMask.gif?raw=true)


## Authors

- [@MirHamzaHasan](https://github.com/MirHamzaHasan)
- [@WebSite](https://mirhamzahasan.com)


## 🔗 Links

[![linkedin](https://img.shields.io/badge/linkedin-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/company/mir-hamza-hasan/posts/?feedView=all/)
## Documentation

[MetaMask For  Pc Game  ](https://docs.lync.world/products/lync-metamask-pc-sdk)



## Tech Stack
**Client:** Unity  ,C#

**Plugin:** LYNC Metamask PC SDK



