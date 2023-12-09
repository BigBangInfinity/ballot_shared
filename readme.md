Clone three repos to these folders:

```
C:\backend\my-api> git clone https://github.com/BigBangInfinity/ballot_shared-backend-my-api
C:\ballotcontract> git clone https://github.com/BigBangInfinity/ballot_shared-ballotcontract
C:\scaffold-eth-2> git clone https://github.com/BigBangInfinity/ballot_shared-scaffold-eth-2
```

C:\ballotcontract>npx hardhat compile 
C:\ballotcontract>npx ts-node .\scripts\deployMyToken.ts

token contract deployed at 0xAF1bd0f58949f47f0bE98732DC3a8a79FfFB2f0e
C:\ballotcontract>npx hardhat verify --network sepolia 0xAF1bd0f58949f47f0bE98732DC3a8a79FfFB2f0e


in backend\my-api\.env, update token address:
TOKEN_ADDRESS = "0xAF1bd0f58949f47f0bE98732DC3a8a79FfFB2f0e"

in scaffold-eth-2\packages\nextjs\.env.local, update token address (environment variable has to start with NEXT_PUBLIC):
NEXT_PUBLIC_TOKEN_ADDRESS = "0xAF1bd0f58949f47f0bE98732DC3a8a79FfFB2f0e"



C:\backend\my-api>yarn run start:dev

Launches Swagger API on 
http://localhost:3001/api

C:\scaffold-eth-2>yarn start
Launches Scaffold ETH on 
http://localhost:3000

Request tokens on the frontend
delegate vote to yourself or to someone else.

After delegation, deploy ballot.
In  C:\ballotcontract\scripts\deployTokenizedBallot.ts,
update token name and target blocknumber in ballot.
Target blocknumber should be the block of the last delegation or after.

const myTokenAddress = "0xAF1bd0f58949f47f0bE98732DC3a8a79FfFB2f0e";
const targetBlockNumber = 4853193;

Deploy tokenized ballot

C:\ballotcontract>npx ts-node .\scripts\deployTokenizedBallot.ts

Ballot contract deployed on 0xB76315720DbE71718ad670A5E938C86a26555A7c

To verify with hardhat, we have to pass the constructor arguments into the verifier, and the proposal names have to be converted to bytes.
To get the bytes representation, run 

C:\ballotcontract>npx ts-node .\scripts\encodeStrings.ts 

The strings ["Proposal1", "Proposal2", "Proposal3"] are converted to 

[
  '0x50726f706f73616c310000000000000000000000000000000000000000000000',
  '0x50726f706f73616c320000000000000000000000000000000000000000000000',
  '0x50726f706f73616c330000000000000000000000000000000000000000000000'
]

Create arguments.json file which contains the constructir arguments which are needed for Etherscan verification:

[
    [
        "0x50726f706f73616c310000000000000000000000000000000000000000000000",
        "0x50726f706f73616c320000000000000000000000000000000000000000000000",
        "0x50726f706f73616c330000000000000000000000000000000000000000000000"
    ],
    "0xAF1bd0f58949f47f0bE98732DC3a8a79FfFB2f0e",
    "4853193"
]


C:\ballotcontract>npx hardhat verify --network sepolia --constructor-args arguments.json 0xB76315720DbE71718ad670A5E938C86a26555A7c 





in backend\my-api\.env, update token address:
BALLOT_ADDRESS = "0xB76315720DbE71718ad670A5E938C86a26555A7c"

in scaffold-eth-2\packages\nextjs\.env.local, update token address (environment variable has to start with NEXT_PUBLIC):
NEXT_PUBLIC_BALLOT_ADDRESS =  "0xB76315720DbE71718ad670A5E938C86a26555A7c

Terminate and relaunch Swagger API
C:\backend\my-api>yarn run start:dev

Relaunch scaffold-eth-2

C:\scaffold-eth-2>yarn start

