---
description: Run a simple dApp that uses Blocto wallet service
---

# Tutorial

## Hello World

Let's build a simple project that sends a simple transaction on Flow testnet with Blocto FCL wallet.

{% hint style="warning" %}
You need to have [Node.js](https://nodejs.org/zh-tw/download/package-manager/) and [yarn](https://classic.yarnpkg.com/en/docs/install/) installed for the rest of the tutorial.
{% endhint %}

### Step 1 - Create app and setup dependencies

```bash
$ npx create-react-app hello-world
```

In the `hello-world` folder you've just created, install dependencies necessary for this project.

```
$ yarn add @onflow/fcl
$ yarn add styled-components
```

{% hint style="danger" %}
FCL is under heavy developments and the versions are not always backward compatible. We recommend that you use `@onflow/fcl@^0.0.52` for now.
{% endhint %}

You can start the app and see it running on `http://localhost:3000`

```bash
$ yarn start
```

### Step 2 - Read from Flow

1. Create `src/GetLatestBlock.js`
2. Add the component to `src/App.js`
3. Add config for FCL in `src/index.js` so FCL knows which access node to read data from

{% tabs %}
{% tab title="src/GetLatestBlock.js" %}
```jsx
import React, {useState} from "react"
import * as fcl from "@onflow/fcl"
import styled from 'styled-components'

const Card = styled.div`
  margin: 10px 5px;
  padding: 10px;
  border: 1px solid #c0c0c0;
  border-radius: 5px;
`

const Code = styled.pre`
  background: #f0f0f0;
  border-radius: 5px;
  max-height: 150px;
  overflow-y: auto;
  padding: 5px;
`

const GetLatestBlock = () => {
  const [data, setData] = useState(null)

  const runGetLatestBlock = async (event) => {
    event.preventDefault()

    const response = await fcl.send([
      fcl.getLatestBlock(),
    ])
    
    setData(await fcl.decode(response))
  }

  return (
    <Card>
      <button onClick={runGetLatestBlock}>
        Get Latest Block
      </button>
      
      {data && <Code>{JSON.stringify(data, null, 2)}</Code>}
    </Card>
  )
}

export default GetLatestBlock

```
{% endtab %}

{% tab title="src/App.js" %}
```jsx
import React from 'react';
import styled from 'styled-components'

import GetLatestBlock from './GetLatestBlock'

const Wrapper = styled.div`
  font-size: 13px;
  font-family: Arial, Helvetica, sans-serif;
`;

function App() {
  return (
    <Wrapper>
      <GetLatestBlock />
    </Wrapper>
  );
}

export default App

```
{% endtab %}

{% tab title="src/index.js" %}
```jsx
import React from 'react'
import ReactDOM from 'react-dom'
import * as fcl from "@onflow/fcl"
import './index.css'
import App from './App'
import * as serviceWorker from './serviceWorker'

fcl.config()
  .put("accessNode.api", "https://access-testnet.onflow.org") // connect to Flow testnet

ReactDOM.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
  document.getElementById('root')
)

// If you want your app to work offline and load faster, you can change
// unregister() to register() below. Note this comes with some pitfalls.
// Learn more about service workers: https://bit.ly/CRA-PWA
serviceWorker.unregister()

```
{% endtab %}
{% endtabs %}

When user clicks the button, `runGetLatestBlock` sends a request to get information for the latest block on Flow and display the result in `<Code>` block.

### Step 3 - Connect to Blocto wallet

Now, let's add login functionality to your dApp.

1. Create `src/Authenticate.js`
2. Add the component to `src/App.js`
3. Add config for FCL in `src/index.js` so FCL knows which wallet to use

{% tabs %}
{% tab title="src/Authenticate.js" %}
```jsx
import React, {useState, useEffect} from "react"
import styled from "styled-components"
import * as fcl from "@onflow/fcl"

const Card = styled.div`
  margin: 10px 5px;
  padding: 10px;
  border: 1px solid #c0c0c0;
  border-radius: 5px;
`

const Profile = styled.div`
  margin-bottom: 10px;
`
const Img = styled.img`
  width: 50px;
  height: 50px;
`

const SignInOutButton = ({ user: { loggedIn } }) => {
  const signInOrOut = async (event) => {
    event.preventDefault()

    if (loggedIn) {
      fcl.unauthenticate()
    } else {
      fcl.authenticate()
    }
  }

  return (
    <button onClick={signInOrOut}>
      {loggedIn ? 'Sign Out' : 'Sign In/Up'}
    </button>
  )
}

const UserProfile = ({ user }) => (
  <Profile>
    {user.identity.avatar && <Img src={user.identity.avatar} />}

    <div>
      <b>Name</b>: {user.identity.name || "Anonymous"}
    </div>
    <div>
      <b>Address</b>: {user.addr || ""}
    </div>
  </Profile>
)

const CurrentUser = () => {
  const [user, setUser] = useState({})

  useEffect(() =>
    fcl
      .currentUser()
      .subscribe(user => setUser({...user}))
  , [])

  return (
    <Card>
      {user.loggedIn && <UserProfile user={user} />}

      <SignInOutButton user={user} />
    </Card>
  )
}

export default CurrentUser

```
{% endtab %}

{% tab title="src/App.js" %}
```jsx
import React from 'react';
import styled from 'styled-components'

import GetLatestBlock from './GetLatestBlock'
import Authenticate from './Authenticate'

const Wrapper = styled.div`
  font-size: 13px;
  font-family: Arial, Helvetica, sans-serif;
`;

function App() {
  return (
    <Wrapper>
      <GetLatestBlock />
      <Authenticate />
    </Wrapper>
  );
}

export default App;

```
{% endtab %}

{% tab title="src/index.js" %}
```jsx
import React from 'react'
import ReactDOM from 'react-dom'
import * as fcl from "@onflow/fcl"
import './index.css'
import App from './App'
import * as serviceWorker from './serviceWorker'

fcl.config()
  .put("accessNode.api", "https://access-testnet.onflow.org") // connect to Flow testnet
  .put("challenge.handshake", "https://flow-wallet-testnet.blocto.app/authn") // use Blocto testnet wallet

ReactDOM.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
  document.getElementById('root')
)

// If you want your app to work offline and load faster, you can change
// unregister() to register() below. Note this comes with some pitfalls.
// Learn more about service workers: https://bit.ly/CRA-PWA
serviceWorker.unregister()

```
{% endtab %}
{% endtabs %}

When user clicks the login button, FCL calls out to Blocto wallet and the user can either register a new Blocto account or login to their existing Blocto accounts. Once the login process completes and user chooses to use the Blocto account on the dApp, the dApp gets the connected account information and show it in `<UserProfile>`.

### Step 4 - Send a simple transaction

Finally, we can use the connected Blocto Flow account to send a transaction.

1. Create `src/SendTransaction.js`
2. Add the component to `src/App.js`

{% tabs %}
{% tab title="src/SendTransaction.js" %}
```jsx
import React, {useState} from "react"
import * as fcl from "@onflow/fcl"
import styled from 'styled-components'

const Card = styled.div`
  margin: 10px 5px;
  padding: 10px;
  border: 1px solid #c0c0c0;
  border-radius: 5px;
`

const Header = styled.div`
  font-size: 16px;
  font-weight: 600;
  margin-bottom: 5px;
`

const Code = styled.pre`
  background: #f0f0f0;
  border-radius: 5px;
  max-height: 300px;
  overflow-y: auto;
  padding: 5px;
`

const simpleTransaction = `\
transaction {
  execute {
    log("Hello World!!")
  }
}
`

const SendTransaction = () => {
  const [status, setStatus] = useState("Not started")
  const [transaction, setTransaction] = useState(null)

  const sendTransaction = async (event) => {
    event.preventDefault()
    
    setStatus("Resolving...")

    const blockResponse = await fcl.send([
      fcl.getLatestBlock(),
    ])

    const block = await fcl.decode(blockResponse)
    
    try {
      const tx = await fcl.send([
        fcl.transaction(simpleTransaction),
        fcl.proposer(fcl.currentUser().authorization),
        fcl.payer(fcl.currentUser().authorization),
        fcl.ref(block.id),
      ])

      const { transactionId } = tx

      setStatus("Transaction sent, waiting for confirmation")

      const unsub = fcl
        .tx({ transactionId })
        .subscribe(transaction => {
          setTransaction(transaction)

          if (fcl.tx.isSealed(transaction)) {
            setStatus("Transaction is Sealed")
            unsub()
          }
        })
    } catch (error) {
      console.error(error)
      setStatus("Transaction failed")
    }
  }

  return (
    <Card>
      <Header>send transaction</Header>

      <Code>{simpleTransaction}</Code>

      <button onClick={sendTransaction}>
        Send
      </button>

      <Code>Status: {status}</Code>

      {transaction && <Code>{JSON.stringify(transaction, null, 2)}</Code>}
    </Card>
  )
}

export default SendTransaction

```
{% endtab %}

{% tab title="src/App.js" %}
```jsx
import React from 'react';
import styled from 'styled-components'

import GetLatestBlock from './GetLatestBlock'
import Authenticate from './Authenticate'
import SendTransaction from './SendTransaction'

const Wrapper = styled.div`
  font-size: 13px;
  font-family: Arial, Helvetica, sans-serif;
`;

function App() {
  return (
    <Wrapper>
      <GetLatestBlock />
      <Authenticate />
      <SendTransaction />
    </Wrapper>
  );
}

export default App;

```
{% endtab %}
{% endtabs %}

When user clicks the `send` button, FCL summons Blocto wallet and prompts user to either approve the transaction or reject it. If user approves of the transaction, Blocto wallet signs the message with the key in custodial and pass the signature back to FCL, where the transaction and the signature is sent to Flow network.

{% hint style="info" %}
Awesome! You have sent a transaction to Flow testnet with Blocto wallet!
{% endhint %}

You can find the complete code of the Hello World project here: [https://github.com/portto/flow-hello-world](https://github.com/portto/flow-hello-world)

## Demo Project

More example interactions with FCL can be found in this project: [https://github.com/portto/fcl-demo](https://github.com/portto/fcl-demo)  
Hosted online demo: [http://fcl-demo.portto.io/](http://fcl-demo.portto.io/)

{% hint style="success" %}
The demo project also shows how you can run local **Flow emulator** and **dev-wallet**
{% endhint %}

## Other Examples

* **NFT Minter** by Blockchain@Berkeley: [https://berkeley-blockchain-demo.vercel.app/](https://berkeley-blockchain-demo.vercel.app/)

