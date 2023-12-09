Clone three repos to these folders:

```
C:\> git clone https://github.com/BigBangInfinity/ballot_shared-backend-my-api
C:\> git clone https://github.com/BigBangInfinity/ballot_shared-ballotcontract
C:\> git clone https://github.com/BigBangInfinity/ballot_shared-scaffold-eth-2
```

npm/yarn install in these folders

use npm for ballotcontract and yarn for others because this is what I used for these folders.

```
C:\ballot_shared-backend-my-api> yarn install
C:\ballot_shared-ballotcontract> npm install
C:\ballot_shared-scaffold-eth-2> yarn install
```

Compile contract
```
C:\ballot_shared-ballotcontract>npx hardhat compile 
```

Deploy token contract
```
C:\ballot_shared-ballotcontract>npx ts-node .\scripts\deployMyToken.ts
```

token contract deployed at 0xAF1bd0f58949f47f0bE98732DC3a8a79FfFB2f0e

Verify on etherscan:

```
C:\ballot_shared-ballotcontract>npx hardhat verify --network sepolia 0xAF1bd0f58949f47f0bE98732DC3a8a79FfFB2f0e
```

in C:\ballot_shared-backend-my-api\.env, update token address:
TOKEN_ADDRESS = "0xAF1bd0f58949f47f0bE98732DC3a8a79FfFB2f0e"

in C:\ballot_shared-scaffold-eth-2\packages\nextjs\.env.local, update token address (environment variable has to start with NEXT_PUBLIC):
NEXT_PUBLIC_TOKEN_ADDRESS = "0xAF1bd0f58949f47f0bE98732DC3a8a79FfFB2f0e"

```
C:\ballot_shared-backend-my-api>yarn run start:dev
```      

Launches Swagger API on 
http://localhost:3001/api

```
C:\ballot_shared-scaffold-eth-2>yarn start
```

Launches Scaffold ETH on 
http://localhost:3000

Request tokens on the frontend

![image](https://github.com/BigBangInfinity/ballot_shared-main/assets/37957341/0c13ecb0-b85f-4be3-afde-6900b68b51e6)


delegate vote to yourself or to someone else.

![image](https://github.com/BigBangInfinity/ballot_shared-main/assets/37957341/c6705a8d-a8b5-491a-97b1-1aa6e731db5d)


After delegation, deploy ballot. Votes have to be delegated before deploying the ballot, otherwise they don't count.


In  C:\ballot_shared-ballotcontract\scripts\deployTokenizedBallot.ts,
update token name and target blocknumber in ballot.
Target blocknumber should be the block of the last delegation or after.

const myTokenAddress = "0xAF1bd0f58949f47f0bE98732DC3a8a79FfFB2f0e";
const targetBlockNumber = 4853193;

Deploy tokenized ballot
```
C:\ballot_shared-ballotcontract>npx ts-node .\scripts\deployTokenizedBallot.ts
```
Ballot contract deployed on 0xB76315720DbE71718ad670A5E938C86a26555A7c

To verify with hardhat, we have to pass the constructor arguments into the verifier, and the proposal names have to be converted to bytes.
To get the bytes representation, run 
```
C:\ballot_shared-ballotcontract>npx ts-node .\scripts\encodeStrings.ts 
```
The strings ["Proposal1", "Proposal2", "Proposal3"] are converted to 
```
[
  '0x50726f706f73616c310000000000000000000000000000000000000000000000',
  '0x50726f706f73616c320000000000000000000000000000000000000000000000',
  '0x50726f706f73616c330000000000000000000000000000000000000000000000'
]
```
Create arguments.json file which contains the constructir arguments which are needed for Etherscan verification:
```
[
    [
        "0x50726f706f73616c310000000000000000000000000000000000000000000000",
        "0x50726f706f73616c320000000000000000000000000000000000000000000000",
        "0x50726f706f73616c330000000000000000000000000000000000000000000000"
    ],
    "0xAF1bd0f58949f47f0bE98732DC3a8a79FfFB2f0e",
    "4853193"
]
```

verify on etherscan:

```
C:\ballot_shared-ballotcontract>npx hardhat verify --network sepolia --constructor-args arguments.json 0xB76315720DbE71718ad670A5E938C86a26555A7c 
```




in ballot_shared-backend-my-api\.env, update token address:
BALLOT_ADDRESS = "0xB76315720DbE71718ad670A5E938C86a26555A7c"

in ballot_shared-scaffold-eth-2\packages\nextjs\.env.local, update token address (environment variable has to start with NEXT_PUBLIC):
NEXT_PUBLIC_BALLOT_ADDRESS =  "0xB76315720DbE71718ad670A5E938C86a26555A7c

Terminate and relaunch Swagger API
```
C:\backend\my-api>yarn run start:dev
```

Relaunch scaffold-eth-2
```
C:\scaffold-eth-2>yarn start
```

Vote for proposals (index numbers 0, 1 or 2), 
put in number of votes,
![image](https://github.com/BigBangInfinity/ballot_shared-main/assets/37957341/9bd89b6f-9a9a-4504-a1bf-91259cc4f708)


and then read results.

![image](https://github.com/BigBangInfinity/ballot_shared-main/assets/37957341/412cfd01-7eda-4f56-a162-add8e90f81ae)
