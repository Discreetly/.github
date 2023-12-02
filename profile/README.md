Discreetly is an anonymous and federated chat application. Under the hood it uses a zero-knowledge protocol called RLN (Rate-Limiting Nullifier) to rate limit messages per user.
Read more about [RLN](https://rate-limiting-nullifier.github.io/rln-docs/).

### Features 

* Truly anonymous
  * It uses RLN to prove you are a member of a room, but not who you are.
  * If you don't trust the server, run your own
* Federated
  * Run your own server, and use the same frontend as everyone else, or run your own frontend too.
* Spammers are banned
  * If someone tries to send messages faster than the rate limit allowed, they are automatically banned, and there is no unban.


### Getting Started

* Clone the Frontend and Server

**SSH**
```
git clone git@github.com:Discreetly/server.git
git clone git@github.com:Discreetly/frontend.git
```
**HTTPS**
```
git clone https://github.com/Discreetly/server.git
git clone https://github.com/Discreetly/frontend.git
```

**Install Dependencies**

```
cd Discreetly/server && npm i
cd Discreetly/frontend && npm i
```

**Set Server Environment Variables**
```
PASSWORD=""
NODE_ENV="development"
DATABASE_URL=""
DATABASE_URL_TEST=""
THEWORD_ITERATION=''
DISCORD_PASSWORD=''
```

* **[PASSWORD](https://github.com/Discreetly/server/blob/a20d26218585a52496eb61ab78d2965235424158/src/endpoints/rooms/rooms.ts#L31C1-L32C1)** and **[DISCORD_PASSWORD](https://github.com/Discreetly/server/blob/a20d26218585a52496eb61ab78d2965235424158/src/endpoints/gateways/discord.ts#L17C1-L21C4)** set the password for express-basic-auth in [server routes](https://github.com/Discreetly/server/tree/main/src/endpoints). The default usernames are admin and discordAdmin. 
* **DATABASE_URL** and **DATABASE_URL_TEST** are [MongoDB Atlas](https://www.mongodb.com/) URL's - you can use the same for both or a seperate cluster for testing
* **[THEWORD_ITERATION](https://github.com/Mach-34/the-word/)** Is used to keep track of what iteration the_word is on for seperate rooms

**Initalize Prisma**

```
cd Discreetly/server && npx prisma db init && npx prisma db push
```

**Seed the Database**
```
npx prisma db seed
```

**Run the Frontend and Server**
```
cd Discreetly/server && npm run dev
cd Discreetly/frontend && npm run dev
```

**Using Discreetly**

The frontend is hosted at ***http://localhost:5173/***
The server is hosted at ***http://localhost:3001/***

* You'll be presented with this homepage where you can create an identity
![60e2e37d048c1c3a5d5c2957ef7e6b2f](https://i.imgur.com/GftrVqQ.png)

* Here you can join rooms through different gateways
  * With invite codes, you can join any rooms if you generate codes for them 
  * The Alpha Testers room is a general room for the public
  * Jubmoji's were presented and collected by participants at ZuConnect and DevConnect
  * Ethereum rooms currently setup are for Beacon Chain Genesis Depositors and Stateful Genesis Funders addresses
  * [The Word](https://github.com/Mach-34/the-word/)
![691469a0e9a954d3272cf21919412191](https://i.imgur.com/pcod8Zi.png)

* Chat rooms
    * Chat rooms have an epoch time (rate limit) and a message limit. Seen in the top right of the chat rooms
    * Messages are color coded by their SocketIO session ID to help keep track of the flow of conversations
 ![83b3f422315da5d0c391e4dc99733d7b](https://i.imgur.com/v0OMumd.png)


* Creating invite codes for rooms
    *    Can be generated at **https://localhost:3001/admin/invite** or:
    *    Replace the Authorization with your Base64-encoded username:password
            *    "numCodes" is the amount of codes to create
            *    "rooms" takes an array of roomId's to create codes for
            *    "all" is a boolean that will create a code for all rooms if true
            *    "expiresAt" takes UNIX time to how long until a code expires. If 0 it will expire in 3 months
            *    "usesLeft" amount of times a code can be used. If -1 it will have unlimited uses
```
curl -i -X POST \
  http://localhost:3001/admin/addcode \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Basic YWRtaW46cGFzc3dvcmQ=' \ 
  -d '{
    "numCodes": 1, 
    "rooms": [""], 
    "all": false, 
    "expiresAt": 0, 
    "usesLeft": -1
}'
```
   
* Creating rooms
    * Can be made at **https://localhost:3001/admin/newroom** or: 
    * Replace the Authorization with your Base64-encoded username:password
        * "roomName" Name of the room to be created
        * "rateLimit" Length of an epoch in MS
        * "userMessageLimit" Number of messages that can be sent in an epoch 
        * "numClaimCodes" Number of claim codes to create upon room creation
        * "approxNumMockUsers" Amount of mock users to create for a room
        * "type" PUBLIC or PRIVATE
        
         **Optional Parameters**
        * "adminIdentities" Admin identities in a room
        * "bandadaAddress" Bandada Group address
        * "bandadaGroupId" Bandada Group Id
        * "bandadaAPIKey" Bandada API Key
        * "membershipType" IDENTITY_LIST or BANDADA_GROUP
        * "roomId" Custom Room ID
        * "encrypted" AES or PLAINTEXT
```
curl -i -X POST \
  http://localhost:3001/admin/newroom \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Basic YWRtaW46cGFzc3dvcmQ=' \ 
  -d '{
  "roomName": "Test", 
  "rateLimit": 10000, 
  "userMessageLimit": 12, 
  "numClaimCodes": 0, 
  "approxNumMockUsers": 5, 
  "type": "PUBLIC", 
  "adminIdentities": [""], 
  "bandadaAddress": "", 
  "bandadaGroupId": "", 
  "bandadaAPIKey": "", 
  "membershipType": "IDENTITY_LIST", 
  "roomId": "", 
  "encrypted": "" 
}'
```


