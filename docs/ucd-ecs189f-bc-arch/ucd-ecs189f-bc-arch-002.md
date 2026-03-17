# 002：逐步编写区块链游戏

![](img/84667de9984b90e1ba962183151f4b9e_1.png)

## 概述

在本节课中，我们将学习如何创建一个基于区块链的记忆匹配游戏。我们将从零开始，逐步构建一个完整的Web3应用。课程将涵盖如何编写以太坊智能合约、使用Solidity编程语言、为合约编写测试、将其部署到区块链，以及如何创建一个客户端应用，让用户能够玩游戏并与智能合约交互。即使你没有编程或区块链经验，也能跟上本教程。

## 游戏演示

我们将构建一个基于区块链的记忆游戏，目标是找到匹配的卡片。

![](img/84667de9984b90e1ba962183151f4b9e_3.png)

![](img/84667de9984b90e1ba962183151f4b9e_4.png)

![](img/84667de9984b90e1ba962183151f4b9e_1.png)

一旦找到匹配项，你就可以永久地将它们保存在区块链上。这是因为每个卡片都由一个基于区块链的代币表示。每当你找到并匹配代币时，你就可以将它们保存在你的钱包里。这些代币可以转移到游戏之外，因为它们永久存在于区块链上，你可以在市场上出售它们，或作为收藏品保存。

![](img/84667de9984b90e1ba962183151f4b9e_6.png)

![](img/84667de9984b90e1ba962183151f4b9e_7.png)

![](img/84667de9984b90e1ba962183151f4b9e_8.png)

这个应用的灵感来源于另一位YouTube开发者Anyao的教程，我们已将其适配用于区块链和现代React JS Web开发。

![](img/84667de9984b90e1ba962183151f4b9e_10.png)

## 应用架构

![](img/84667de9984b90e1ba962183151f4b9e_12.png)

以下是应用程序的工作原理图：

![](img/84667de9984b90e1ba962183151f4b9e_14.png)

![](img/84667de9984b90e1ba962183151f4b9e_3.png)

![](img/84667de9984b90e1ba962183151f4b9e_4.png)

![](img/84667de9984b90e1ba962183151f4b9e_16.png)

![](img/84667de9984b90e1ba962183151f4b9e_17.png)

我们将首先构建一个客户端网站，大部分游戏逻辑将在这里运行。我们将使用JavaScript和React JS编写这个游戏。当我们在记忆游戏中匹配代币时，我们实际上会在区块链上创建新的代币。这些代币由以太坊智能合约驱动，智能合约将充当转移代币和管理代币所有权的业务逻辑。拥有代币的用户账本也将存在于区块链上，它将充当我们的后端数据库。一旦代币存在于区块链的公共账本上，它们就可以被发送到任何其他区块链钱包，在市场上出售等。

## 代币类型：非同质化代币

在本教程中，我们将逐步构建的代币不是货币，它是一种不同的类型，称为**非同质化代币**。你可能听说过数字收藏品，比如CryptoKitties，这就是一个例子。

为了理解这个概念，我们需要区分**同质化代币**和**非同质化代币**。

*   **同质化代币**：可以互换，每个代币价值相同。例如，加密货币如UNI代币，每个UNI代币的价值都与其他UNI代币相同，可以一对一交换。
*   **非同质化代币**：每个代币都是独一无二的，价值不一定相同。一个代币可能因为稀有而比另一个代币价值更高，这使其非常适合作为收藏品。

在本教程中，我们将创建的就是这种非同质化代币。

## 使用ERC-721标准

我们将使用**ERC-721标准**。你可能听说过用于基于以太坊的代币的ERC-20标准，而ERC-721就是用于非同质化代币的标准。它基本上描述了代币必须实现的函数和事件类型。这只是一个用Solidity编程语言编写的以太坊智能合约，它描述了必须存在的Solidity函数类型，例如：
*   `balanceOf`：查看个人拥有多少代币。
*   `ownerOf`：查看每个代币的所有者。
*   `transferFrom`：转移代币的能力。
*   `approve`：批准他人发送代币的能力。

我们将在本视频中深入所有这些细节，但这就是对我们将要构建的ERC-721标准的快速概述。

## 环境与依赖安装

![](img/84667de9984b90e1ba962183151f4b9e_19.png)

在开始构建之前，我们需要安装一些必要的工具。

以下是安装步骤：

![](img/84667de9984b90e1ba962183151f4b9e_21.png)

1.  **Node.js**：允许我们在计算机上安装所有软件包并运行客户端应用程序。你可以在终端输入 `node -v` 来检查是否已安装。建议使用Node 9.10.0版本以获得最佳效果，但你也可以使用最新版本。可以从官网下载Node.js，或使用Homebrew等命令行工具安装。
2.  **Truffle框架**：用于创建以太坊智能合约的框架。我们可以用Solidity编程语言编写智能合约，为其编写测试，并使用Truffle将其部署到区块链。可以通过命令行安装：`npm install -g truffle@5.1.39`。
3.  **Ganache个人区块链**：一个运行在我们计算机上的区块链。我们可以简单地下载它并在区块链上运行交易、部署智能合约，而无需支付任何真实货币。请根据你的操作系统下载相应版本。运行后，你会看到一个全新的区块链在计算机上运行，其中有许多账户，每个账户都有100个假的以太币。
4.  **MetaMask浏览器扩展**：大多数现代网络浏览器默认无法连接到区块链，因此我们需要安装一个特殊的浏览器扩展来实现这一点。MetaMask就是一个以太坊钱包，它将我们的浏览器变成了区块链浏览器。请前往Google Chrome网上应用店，找到MetaMask并点击安装。安装后，你会在右上角看到一个狐狸图标，按照设置步骤完成即可。

## 项目初始化与代码获取

现在让我们开始构建项目。为了快速启动，我将提供一个GitHub仓库链接，其中包含应用的初始代码。

![](img/84667de9984b90e1ba962183151f4b9e_23.png)

在终端中执行以下命令来获取代码：

![](img/84667de9984b90e1ba962183151f4b9e_25.png)

![](img/84667de9984b90e1ba962183151f4b9e_26.png)

![](img/84667de9984b90e1ba962183151f4b9e_27.png)

![](img/84667de9984b90e1ba962183151f4b9e_28.png)

![](img/84667de9984b90e1ba962183151f4b9e_29.png)

```bash
git clone -b starter-code https://github.com/dappuniversity/blockchain-game.git blockchain-game
```

进入新创建的目录：
```bash
cd blockchain-game
```

接下来，安装所有依赖项：
```bash
npm install
```

安装完成后，启动开发服务器：
```bash
npm run start
```

现在，你应该能在浏览器中看到应用的基本模板。它有一个导航栏，标题为“Memory Tokens”，一个图标，以及一个显示以太坊地址的位置。游戏区域和代币收集区域将在后续教程中构建。

## 启动本地区块链

![](img/84667de9984b90e1ba962183151f4b9e_31.png)

![](img/84667de9984b90e1ba962183151f4b9e_32.png)

在开始构建之前，我们需要启动Ganache本地区块链，以便我们可以向其部署智能合约。

打开Ganache应用程序，确保它正在运行。你会看到许多账户，每个账户都有100个假的以太币。

![](img/84667de9984b90e1ba962183151f4b9e_34.png)

![](img/84667de9984b90e1ba962183151f4b9e_35.png)

## 项目结构概览

让我们在文本编辑器中打开项目，查看其结构。

项目主要包含以下目录和文件：
*   `truffle-config.js`：配置我们的项目以连接到区块链。
*   `contracts/`：智能合约存放的目录。我们已经有一个设置好的`MemoryToken.sol`智能合约。
*   `tests/`：为智能合约编写测试的目录。我们已经有一个基本的测试框架。
*   `src/`：客户端代码目录，包含React JS组件。

## 编写智能合约：MemoryToken

现在，让我们开始编写代币的智能合约。我们将创建一个ERC-721代币，用于在游戏中收集记忆代币。

首先，我们打开`contracts/MemoryToken.sol`文件。我们将继承OpenZeppelin库中的ERC-721标准实现，这样可以免费获得许多标准功能。

以下是智能合约的基本结构：

![](img/84667de9984b90e1ba962183151f4b9e_37.png)

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "./ERC721Full.sol";

![](img/84667de9984b90e1ba962183151f4b9e_39.png)

contract MemoryToken is ERRC721Full {
    constructor() ERRC721Full("Memory Token", "MEMORY") public {
    }

    function mint(address _to, string memory _tokenURI) public returns (bool) {
        uint256 _tokenId = totalSupply().add(1);
        _mint(_to, _tokenId);
        _setTokenURI(_tokenId, _tokenURI);
        return true;
    }
}
```

**代码解释**：
1.  **导入与继承**：我们导入`ERC721Full.sol`文件，并让`MemoryToken`合约继承它。这使我们能够使用所有标准的ERC-721函数。
2.  **构造函数**：在合约部署时调用。我们传入代币的名称`"Memory Token"`和符号`"MEMORY"`。
3.  **`mint`函数**：用于创建（铸造）新代币。
    *   它接受两个参数：接收代币的地址`_to`，以及代币元数据（如图片）的链接`_tokenURI`。
    *   函数内部，我们首先获取当前总供应量并加1，作为新代币的ID。
    *   然后调用继承的`_mint`函数来铸造代币给指定地址。
    *   接着调用`_setTokenURI`函数来设置该代币的元数据链接。
    *   最后返回`true`表示成功。

这个简单的智能合约就是我们游戏所需的核心。

## 部署智能合约

![](img/84667de9984b90e1ba962183151f4b9e_41.png)

![](img/84667de9984b90e1ba962183151f4b9e_42.png)

![](img/84667de9984b90e1ba962183151f4b9e_44.png)

在编写测试之前，让我们先尝试将智能合约部署到区块链，以确保一切设置正确。

首先，我们需要创建一个迁移文件来告诉Truffle如何部署合约。在`migrations/`目录下，创建一个名为`2_deploy_contracts.js`的文件，内容如下：

```javascript
const MemoryToken = artifacts.require("MemoryToken");

module.exports = function(deployer) {
  deployer.deploy(MemoryToken);
};
```

![](img/84667de9984b90e1ba962183151f4b9e_46.png)

![](img/84667de9984b90e1ba962183151f4b9e_48.png)

然后，在终端中运行以下命令来部署合约：

```bash
truffle migrate --reset
```

`--reset`标志确保我们部署的是最新版本的合约。部署成功后，我们可以打开Truffle控制台与合约交互：

![](img/84667de9984b90e1ba962183151f4b9e_50.png)

```bash
truffle console
```

![](img/84667de9984b90e1ba962183151f4b9e_52.png)

![](img/84667de9984b90e1ba962183151f4b9e_54.png)

![](img/84667de9984b90e1ba962183151f4b9e_56.png)

![](img/84667de9984b90e1ba962183151f4b9e_58.png)

在控制台中，我们可以获取合约实例并读取其名称：

```javascript
let token = await MemoryToken.deployed()
token.address
let name = await token.name()
name
```

![](img/84667de9984b90e1ba962183151f4b9e_60.png)

![](img/84667de9984b90e1ba962183151f4b9e_61.png)

![](img/84667de9984b90e1ba962183151f4b9e_62.png)

![](img/84667de9984b90e1ba962183151f4b9e_63.png)

![](img/84667de9984b90e1ba962183151f4b9e_65.png)

![](img/84667de9984b90e1ba962183151f4b9e_67.png)

![](img/84667de9984b90e1ba962183151f4b9e_68.png)

如果一切顺利，你应该能看到合约地址和名称`"Memory Token"`。恭喜你，你已经成功将智能合约部署到了区块链！

![](img/84667de9984b90e1ba962183151f4b9e_70.png)

![](img/84667de9984b90e1ba962183151f4b9e_72.png)

## 为智能合约编写测试

![](img/84667de9984b90e1ba962183151f4b9e_74.png)

![](img/84667de9984b90e1ba962183151f4b9e_75.png)

上一节我们成功部署了智能合约，本节我们将为其编写自动化测试。测试对于智能合约开发至关重要，因为一旦部署，合约代码就无法更改。

![](img/84667de9984b90e1ba962183151f4b9e_77.png)

![](img/84667de9984b90e1ba962183151f4b9e_78.png)

![](img/84667de9984b90e1ba962183151f4b9e_79.png)

![](img/84667de9984b90e1ba962183151f4b9e_80.png)

![](img/84667de9984b90e1ba962183151f4b9e_82.png)

![](img/84667de9984b90e1ba962183151f4b9e_84.png)

我们将使用Mocha测试框架和Chai断言库，它们已与Truffle捆绑。

打开`test/MemoryToken.test.js`文件。我们将编写测试来验证合约的部署、名称、符号以及代币铸造功能。

![](img/84667de9984b90e1ba962183151f4b9e_86.png)

![](img/84667de9984b90e1ba962183151f4b9e_88.png)

![](img/84667de9984b90e1ba962183151f4b9e_90.png)

![](img/84667de9984b90e1ba962183151f4b9e_91.png)

以下是测试的核心内容：

![](img/84667de9984b90e1ba962183151f4b9e_93.png)

![](img/84667de9984b90e1ba962183151f4b9e_94.png)

![](img/84667de9984b90e1ba962183151f4b9e_96.png)

![](img/84667de9984b90e1ba962183151f4b9e_97.png)

```javascript
const MemoryToken = artifacts.require('MemoryToken')

![](img/84667de9984b90e1ba962183151f4b9e_99.png)

![](img/84667de9984b90e1ba962183151f4b9e_101.png)

require('chai')
  .use(require('chai-as-promised'))
  .should()

contract('MemoryToken', (accounts) => {
  let token

  before(async () => {
    token = await MemoryToken.deployed()
  })

  describe('deployment', async () => {
    it('deploys successfully', async () => {
      const address = token.address
      assert.notEqual(address, 0x0)
      assert.notEqual(address, '')
      assert.notEqual(address, null)
      assert.notEqual(address, undefined)
    })

    it('has a name', async () => {
      const name = await token.name()
      assert.equal(name, 'Memory Token')
    })

    it('has a symbol', async () => {
      const symbol = await token.symbol()
      assert.equal(symbol, 'MEMORY')
    })
  })

  describe('token distribution', async () => {
    it('mints tokens', async () => {
      await token.mint(accounts[0], 'https://www.token-uri.com/nft')

      // 应增加总供应量
      let result = await token.totalSupply()
      assert.equal(result.toString(), '1', 'total supply is correct')

      // 应增加所有者余额
      result = await token.balanceOf(accounts[0])
      assert.equal(result.toString(), '1', 'balance is correct')

      // 代币应属于所有者
      let owner = await token.ownerOf('1')
      assert.equal(owner, accounts[0])

      // 所有者应拥有所有代币
      let balanceOf = await token.balanceOf(accounts[0])
      let tokenIds = []
      for (let i = 0; i < balanceOf; i++) {
        let id = await token.tokenOfOwnerByIndex(accounts[0], i)
        tokenIds.push(id.toString())
      }
      let expected = ['1']
      assert.equal(tokenIds.toString(), expected.toString(), 'tokenIds are correct')

      // 代币URI应正确设置
      let tokenURI = await token.tokenURI('1')
      assert.equal(tokenURI, 'https://www.token-uri.com/nft')
    })
  })
})
```

![](img/84667de9984b90e1ba962183151f4b9e_103.png)

![](img/84667de9984b90e1ba962183151f4b9e_105.png)

![](img/84667de9984b90e1ba962183151f4b9e_107.png)

**测试解释**：
1.  **部署测试**：确保合约成功部署，并且具有正确的名称和符号。
2.  **代币铸造测试**：
    *   调用`mint`函数为第一个账户铸造一个代币。
    *   检查总供应量是否增加到1。
    *   检查所有者余额是否增加到1。
    *   检查代币ID 1的所有者是否正确。
    *   使用循环获取所有者拥有的所有代币ID，并验证其正确性。
    *   检查代币的URI是否设置正确。

![](img/84667de9984b90e1ba962183151f4b9e_108.png)

![](img/84667de9984b90e1ba962183151f4b9e_110.png)

![](img/84667de9984b90e1ba962183151f4b9e_112.png)

运行测试：
```bash
truffle test
```

![](img/84667de9984b90e1ba962183151f4b9e_114.png)

![](img/84667de9984b90e1ba962183151f4b9e_115.png)

如果所有测试通过，说明我们的智能合约功能正常。

## 构建客户端应用

![](img/84667de9984b90e1ba962183151f4b9e_117.png)

![](img/84667de9984b90e1ba962183151f4b9e_118.png)

![](img/84667de9984b90e1ba962183151f4b9e_120.png)

现在智能合约部分已经完成，让我们开始构建客户端应用。我们将使用React JS来创建用户界面，并使用Web3.js库与区块链交互。

![](img/84667de9984b90e1ba962183151f4b9e_122.png)

首先，确保你的开发服务器仍在运行（`npm run start`），Ganache正在运行，并且MetaMask已配置连接到Ganache网络（RPC URL为`http://127.0.0.1:7545`）。

### 连接应用与区块链

我们需要将React应用连接到区块链。这主要通过Web3.js库实现。

![](img/84667de9984b90e1ba962183151f4b9e_124.png)

![](img/84667de9984b90e1ba962183151f4b9e_125.png)

在`src/components/App.js`文件中，我们首先导入Web3，然后创建函数来加载Web3并获取区块链数据。

![](img/84667de9984b90e1ba962183151f4b9e_127.png)

![](img/84667de9984b90e1ba962183151f4b9e_128.png)

关键步骤包括：
1.  **加载Web3**：检查浏览器是否注入了Web3实例（由MetaMask提供）。
2.  **加载区块链数据**：获取当前账户、智能合约实例、总供应量以及用户拥有的代币。
3.  **更新状态**：将获取到的数据（如账户地址、合约实例、代币列表）存入React组件的状态中，以便在UI中显示。

以下是`loadBlockchainData`函数的核心部分：

```javascript
async loadBlockchainData() {
  const web3 = window.web3
  // 加载账户
  const accounts = await web3.eth.getAccounts()
  this.setState({ account: accounts[0] })
  // 网络ID
  const networkId = await web3.eth.net.getId()
  const networkData = MemoryToken.networks[networkId]
  if(networkData) {
    const abi = MemoryToken.abi
    const address = networkData.address
    const token = new web3.eth.Contract(abi, address)
    this.setState({ token })
    // 总供应量
    const totalSupply = await token.methods.totalSupply().call()
    this.setState({ totalSupply })
    // 加载用户代币
    const balanceOf = await token.methods.balanceOf(this.state.account).call()
    for (let i = 0; i < balanceOf; i++) {
      const tokenId = await token.methods.tokenOfOwnerByIndex(this.state.account, i).call()
      const tokenURI = await token.methods.tokenURI(tokenId).call()
      this.setState({
        tokenURIs: [...this.state.tokenURIs, tokenURI]
      })
    }
  } else {
    window.alert('Smart contract not deployed to detected network.')
  }
}
```

完成这些步骤后，你的应用应该能够显示连接的以太坊账户地址。

![](img/84667de9984b90e1ba962183151f4b9e_130.png)

![](img/84667de9984b90e1ba962183151f4b9e_132.png)

### 构建游戏逻辑

![](img/84667de9984b90e1ba962183151f4b9e_134.png)

![](img/84667de9984b90e1ba962183151f4b9e_135.png)

现在，让我们构建记忆游戏本身。游戏的核心是一个网格卡片，玩家点击翻转两张卡片，尝试找到匹配项。

我们需要在状态中管理以下数据：
*   `cardArray`：所有卡片的数组，包含卡片名称和图片路径。
*   `cardsChosen`：当前被选中的卡片ID数组。
*   `cardsChosenIds`：当前被选中的卡片在数组中的索引数组。
*   `cardsWon`：已匹配成功的卡片ID数组。
*   `tokenURIs`：用户收集到的代币URI数组。

游戏流程如下：
1.  **初始化**：在组件加载时，创建并随机排序卡片数组。
2.  **渲染卡片**：根据状态映射卡片数组，显示卡片图片（未翻转时是背面，翻转后是正面，匹配成功后是空白或标记）。
3.  **卡片点击**：当玩家点击一张卡片时，触发处理函数。
    *   如果这是第一次选择（`cardsChosen`长度为0），则将卡片ID加入`cardsChosen`。
    *   如果这是第二次选择（`cardsChosen`长度为1），则检查两张卡片是否匹配。
4.  **检查匹配**：
    *   如果两张卡片相同，则匹配成功。将卡片ID加入`cardsWon`，并调用智能合约的`mint`函数为用户铸造一个新的NFT代币。
    *   如果两张卡片不同，则匹配失败。重置`cardsChosen`和`cardsChosenIds`，让玩家继续尝试。
5.  **更新UI**：根据游戏状态（选中、匹配成功等）动态更新每张卡片显示的图片。

![](img/84667de9984b90e1ba962183151f4b9e_137.png)

![](img/84667de9984b90e1ba962183151f4b9e_139.png)

关键函数`flipCard`和`checkForMatch`实现了上述逻辑。当匹配成功时，我们调用智能合约的`mint`方法，并传入当前账户地址和卡片图片的URL作为代币URI。

### 显示收集的代币

![](img/84667de9984b90e1ba962183151f4b9e_141.png)

![](img/84667de9984b90e1ba962183151f4b9e_143.png)

最后，我们需要在页面底部显示用户通过游戏收集到的NFT代币。

我们可以在`render`函数中映射`tokenURIs`状态数组，为每个代币URI创建一个图片元素来显示。

![](img/84667de9984b90e1ba962183151f4b9e_145.png)

```jsx
<div className="row">
  <h2>Tokens Collected: {this.state.tokenURIs.length}</h2>
  {this.state.tokenURIs.map((tokenURI, key) => {
    return (
      <div className="col-md-3 mb-3" key={key}>
        <img src={tokenURI} alt={`NFT Token ${key}`} width="100%" />
      </div>
    )
  })}
</div>
```

![](img/84667de9984b90e1ba962183151f4b9e_147.png)

## 测试完整应用

![](img/84667de9984b90e1ba962183151f4b9e_149.png)

![](img/84667de9984b90e1ba962183151f4b9e_151.png)

现在，所有部分都已就绪。你可以玩这个记忆游戏了。

![](img/84667de9984b90e1ba962183151f4b9e_153.png)

1.  点击翻转两张卡片。
2.  如果找到匹配项，MetaMask会弹出交易确认窗口，要求你支付Gas费来铸造代币（使用Ganache的假以太币）。
3.  确认交易后，新的NFT代币就会被铸造并归属于你的账户。
4.  匹配成功的卡片会从游戏板上消失，新收集的代币会显示在页面底部。

![](img/84667de9984b90e1ba962183151f4b9e_155.png)

你可以刷新页面，之前收集的代币仍然会显示，因为它们被永久记录在区块链上。

![](img/84667de9984b90e1ba962183151f4b9e_157.png)

## 总结

在本节课中，我们一起学习了如何从零开始构建一个完整的区块链游戏。我们涵盖了以下核心内容：

1.  **项目规划**：了解了我们将要构建的记忆匹配游戏及其基于区块链的奖励机制。
2.  **环境搭建**：安装了Node.js、Truffle、Ganache和MetaMask等必要工具。
3.  **智能合约开发**：使用Solidity编写了符合ERC-721标准的非同质化代币合约，并利用OpenZeppelin库加速开发。
4.  **测试驱动**：为智能合约编写了自动化测试，确保其功能正确性，这是区块链开发中至关重要的一步。
5.  **客户端开发**：使用React JS构建了游戏前端界面。
6.  **区块链集成**：通过Web3.js库将React应用连接到以太坊区块链（Ganache），实现了账户读取、合约交互和交易发送。
7.  **游戏逻辑实现**：编写了记忆游戏的完整逻辑，包括卡片翻转、匹配检测和状态管理。
8.  **代币铸造与展示**：在玩家匹配成功时，调用智能合约铸造NFT代币，并在UI中展示用户收集的所有代币。

![](img/84667de9984b90e1ba962183151f4b9e_159.png)

通过这个项目，你不仅学会了如何创建智能合约和DApp前端，还亲身体验了如何将区块链技术融入传统应用，为用户创造拥有真正数字资产所有权的游戏体验。这为探索更复杂的Web3应用奠定了坚实的基础。