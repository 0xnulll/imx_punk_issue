# IMX PUNK Presale Mint

So IMX Punk Presale Minting started on 6th Nov, 2021, soon after kick off they started having minting issues. ( https://mint.imxpunks.com/ ).

Seem fishy as there website title was "React App", they forgot to change website title ?

There minting process exposed below.
All these finding made easy because there app.js was un minified and easy to read.

# API in Use
1. Whitelist Status Api - https://2mdkcs2l0l.execute-api.us-east-2.amazonaws.com/trans/transaction [ For presale wallet whitelist fetch ]
2. Mint Api - https://mint-api.imxpunks.com/v1/presale [ To trigger minting after sending payment]

# Findings.
I figured the process of presale minting, there dev implemented below steps, all in Frontend website javascript code.

1. GET whitelist status from Whitlist api ex. https://2mdkcs2l0l.execute-api.us-east-2.amazonaws.com/trans/transaction?walletAddress=0x470299a90c371b17d6dfc101cbc7c090c1555408
    Response
```
   {
       "isMinted":true,
       "tokenId":4860,
       "walletAddress":"0x470299a90c371b17d6dfc101cbc7c090c1555408"
    }
```
     isMinted :- looks like wallet minted the nft or not.
     tokenId :- seems they pre mapped nft token id to wallet address.
2. Send eth payment to an address ( 0xb569133ed518d1c3f19ad0af75312cf894cbac7c ).
3. Send mint instruction to minting api with below data 
     ```
    {
          "price": "0.035",
          "tokenId": [4860],
          "walletAddress": "0x470299a90c371b17d6dfc101cbc7c090c1555408"
     }
     ```
    
    **is it a good practice to accept tokenId in request body from front end?**
4. Patch Request to Whitlist api https://2mdkcs2l0l.execute-api.us-east-2.amazonaws.com/trans/transaction with below data, to update wallet has minted one nft.
     ```
     {
      "isMinted":true,
      "walletAddress":"0x470299a90c371b17d6dfc101cbc7c090c1555408"
     }
     ```

    **Is it a good practice to update minting status from front end?**
# Issues 

1. Seeing patch request is allowed publically, I checked Whitlist api for **DELETE, POST** then I tried POST Reqeust to whitlist api with below data
``` 
    { 
      "isMinted":true,
      "tokenId":[#any_nft_id],
      "walletAddress":"#any_wallet_address"
    }
```
And it worked, Rest you can think what happend. :D

2. Are they checking payment transcation, not sure they are not capturing it anywhere?


This post is just to aware Community about some issues and nothing about IMX Punks team. There are lot of cheap devlopers who does not know how to write protected apis.


