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

export default App;

```
{% endtab %}

{% tab title="src/index.js" %}
```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import * as fcl from "@onflow/fcl"
import './index.css';
import App from './App';
import * as serviceWorker from './serviceWorker';

fcl.config()
  .put("accessNode.api", "https://access-testnet.onflow.org") // connect to Flow testnet

ReactDOM.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
  document.getElementById('root')
);

// If you want your app to work offline and load faster, you can change
// unregister() to register() below. Note this comes with some pitfalls.
// Learn more about service workers: https://bit.ly/CRA-PWA
serviceWorker.unregister();

```
{% endtab %}
{% endtabs %}

When user clicks the button, `runGetLatestBlock` sends a request to get information for the latest block on Flow and display the result in `<Code>` block.

You can run the app and see the results on `http://localhost:3000`

```bash
$ npm start
```

### Step 3 - Connect to Blocto wallet

Now, let's add login functionality to your dApp.

1. Create `src/Authenticate.js`
2. Add the component to `src/App.js`
3. Add config for FCL in `src/index.js` so FCL knows which wallet to use



## Demo Project

More example interactions with FCL can be found in this project: [https://github.com/portto/fcl-demo](https://github.com/portto/fcl-demo)  
Hosted online demo: [http://fcl-demo.portto.io/](http://fcl-demo.portto.io/)

{% hint style="success" %}
The demo project also shows how you can run local **Flow emulator** and **dev-wallet**
{% endhint %}

## Other Examples

**NFT Minter** by Blockchain@Berkeley: [https://berkeley-blockchain-demo.vercel.app/](https://berkeley-blockchain-demo.vercel.app/)

