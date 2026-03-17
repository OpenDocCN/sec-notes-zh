# 001：Web3 - 用区块链编写Instagram复刻版 🚀

## 概述

在本节课中，我们将学习如何构建一个基于区块链的去中心化图片分享网站，类似于Instagram。这个应用将允许用户发布图片并赚取加密货币。我们将使用以太坊区块链和星际文件系统（IPFS）来创建一个抗审查、不可阻挡的Web 3.0应用。即使你之前没有编程或区块链经验，本教程也将一步步引导你完成整个过程。

![](img/3240d77ebb9dd9a14111406bce3a787e_1.png)

## 应用架构

![](img/3240d77ebb9dd9a14111406bce3a787e_3.png)

![](img/3240d77ebb9dd9a14111406bce3a787e_4.png)

上一节我们介绍了项目的目标，本节中我们来看看应用的整体架构。

![](img/3240d77ebb9dd9a14111406bce3a787e_5.png)

### 传统中心化应用的问题

![](img/3240d77ebb9dd9a14111406bce3a787e_7.png)

![](img/3240d77ebb9dd9a14111406bce3a787e_8.png)

![](img/3240d77ebb9dd9a14111406bce3a787e_9.png)

![](img/3240d77ebb9dd9a14111406bce3a787e_11.png)

![](img/3240d77ebb9dd9a14111406bce3a787e_13.png)

首先，让我们看看像Instagram这样的传统应用是如何工作的。当你使用Instagram时，你的移动设备会与一个后端Web服务器通信。这个服务器存储了应用的所有代码、数据和图片。当你请求时间线图片时，数据会从Web服务器传回你的设备。这种架构存在几个主要问题，最大的问题是应用创建者对向用户展示哪些内容拥有完全控制权。社交媒体算法臭名昭著，其最大的问题之一是它们可以通过隐藏某些内容、突出显示应用创建者认为最适合用户看到的内容，从而有效地审查平台上的用户。

![](img/3240d77ebb9dd9a14111406bce3a787e_15.png)

![](img/3240d77ebb9dd9a14111406bce3a787e_17.png)

我们不希望以这种中心化的、容易受到此类操纵的方式构建图片分享网站。我们希望以一种去中心化的方式构建它，采用一个非常不同、直接、透明且免于审查的算法。

![](img/3240d77ebb9dd9a14111406bce3a787e_19.png)

![](img/3240d77ebb9dd9a14111406bce3a787e_21.png)

![](img/3240d77ebb9dd9a14111406bce3a787e_23.png)

### 去中心化应用架构

![](img/3240d77ebb9dd9a14111406bce3a787e_25.png)

![](img/3240d77ebb9dd9a14111406bce3a787e_26.png)

那么，去中心化版本是什么样的呢？我们将不再让移动设备与Web服务器通信，而是构建一个基于区块链的应用。你将通过带有区块链钱包的网页浏览器访问它。这个应用将直接与用HTML、CSS和JavaScript编写的网站交互，然后这个网站将直接与区块链通信。我们的代码将以以太坊智能合约的形式编写，并部署到以太坊区块链上。所有图片将存储在一个名为IPFS的去中心化文件存储系统中，这样图片就永远不会被审查或删除。这将是一个完全透明的算法，我们知道它每次都以相同的方式工作，因为智能合约代码是不可变的，无法更改，用户可以通过阅读代码来验证其工作原理。

![](img/3240d77ebb9dd9a14111406bce3a787e_28.png)

![](img/3240d77ebb9dd9a14111406bce3a787e_29.png)

以下是应用架构的图示：

```
[用户浏览器 + MetaMask钱包] <-> [React前端应用] <-> [以太坊智能合约] + [IPFS文件存储]
```

## 环境与依赖安装

在开始编码之前，我们需要设置开发环境并安装必要的依赖项。

以下是构建此项目所需的工具和库：

1.  **Node.js**：允许我们在计算机上安装所有软件包并运行客户端应用。
    *   你可以通过在终端输入 `node -v` 来检查是否已安装Node.js。本教程推荐使用版本 `9.10.0` 以获得最佳效果，但你也可以使用最新版本。
    *   你可以直接从官网下载Node.js，或使用像Homebrew这样的命令行工具来安装。

2.  **Truffle框架**：这是一个用于创建、测试和部署以太坊智能合约的开发框架。
    *   你可以通过命令行安装：`npm install -g truffle@5.1.39`

3.  **Ganache**：这是一个在本地计算机上运行的以太坊区块链模拟器。我们可以用它来部署智能合约和运行交易，而无需支付真实的加密货币。
    *   前往Ganache官网，下载适合你操作系统的版本并安装。运行后，你将看到一个全新的区块链在本地运行，其中包含多个预充值了100个假以太币的账户。

4.  **MetaMask浏览器扩展**：大多数现代网页浏览器默认无法连接到区块链，因此我们需要安装一个特殊的浏览器扩展，MetaMask就是一个以太坊钱包，可以将我们的浏览器变成区块链浏览器。
    *   前往Chrome网上应用店，搜索并安装MetaMask扩展。安装完成后，按照设置步骤完成初始化。

![](img/3240d77ebb9dd9a14111406bce3a787e_31.png)

![](img/3240d77ebb9dd9a14111406bce3a787e_33.png)

## 项目初始化与代码结构

现在，让我们开始初始化项目并查看代码结构。

我们将从一个包含启动代码的特定Git分支开始，而不是从头开始设置一切。

![](img/3240d77ebb9dd9a14111406bce3a787e_35.png)

1.  克隆启动代码仓库：
    ```bash
    git clone -b starter-code https://github.com/dappuniversity/decentagram.git decentagram
    ```
2.  进入项目目录并安装依赖：
    ```bash
    cd decentagram
    npm install
    ```
3.  确保Ganache正在运行。这是我们将用于本教程的测试区块链。
4.  在另一个终端标签页中，启动Web服务器以确保一切设置正确：
    ```bash
    npm run start
    ```

![](img/3240d77ebb9dd9a14111406bce3a787e_37.png)

![](img/3240d77ebb9dd9a14111406bce3a787e_38.png)

![](img/3240d77ebb9dd9a14111406bce3a787e_40.png)

现在，你应该能在浏览器中看到一个基础的应用界面，包含导航栏，但尚未连接任何功能。

![](img/3240d77ebb9dd9a14111406bce3a787e_42.png)

![](img/3240d77ebb9dd9a14111406bce3a787e_44.png)

让我们快速浏览一下项目结构：
*   `src/contracts/Decentagram.sol`：我们将在这里编写智能合约的Solidity代码。
*   `src/contracts/Decentagram.json`：这是由Truffle生成的智能合约ABI和地址信息文件。
*   `test/decentagram.test.js`：我们将在这里为智能合约编写JavaScript测试。
*   `src/components/App.js`：这是React应用的主组件文件。
*   `src/components/Main.js`：这是应用主要内容区域的组件。
*   `src/components/Navbar.js`：这是导航栏组件。

![](img/3240d77ebb9dd9a14111406bce3a787e_46.png)

## 编写智能合约（第一部分）

![](img/3240d77ebb9dd9a14111406bce3a787e_48.png)

上一节我们搭建好了环境，本节中我们来编写第一个智能合约。

我们将首先在 `src/contracts/Decentagram.sol` 文件中编写业务逻辑。Solidity是以太坊智能合约的主要编程语言。

### 基础合约结构

首先，我们定义合约的基本框架和一个状态变量。

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Decentagram {
    string public name = "Decentagram";
}
```
*   `pragma solidity ^0.8.0;` 指定了Solidity编译器的版本。
*   `contract Decentagram { ... }` 定义了一个名为 `Decentagram` 的智能合约。
*   `string public name = "Decentagram";` 定义了一个公开的字符串状态变量 `name`，其值存储在区块链上，并且可以从合约外部读取。

### 部署与测试

为了验证我们的设置，让我们部署这个基础合约并进行测试。

1.  首先，我们需要创建一个迁移文件来告诉Truffle如何部署合约。在 `migrations/` 目录下创建 `2_deploy_contracts.js`：
    ```javascript
    const Decentagram = artifacts.require("Decentagram");

    module.exports = function(deployer) {
      deployer.deploy(Decentagram);
    };
    ```
2.  在终端中编译并部署合约到Ganache：
    ```bash
    truffle migrate --reset
    ```
    `--reset` 标志确保我们部署的是最新版本的合约。
3.  打开Truffle控制台与合约交互：
    ```bash
    truffle console
    ```
4.  在控制台中，获取合约实例并读取 `name` 变量：
    ```javascript
    let decentagram = await Decentagram.deployed()
    decentagram.address // 查看合约地址
    let name = await decentagram.name()
    name // 应输出 "Decentagram"
    ```

### 自动化测试

为了更高效地开发，我们将为合约编写自动化测试。打开 `test/decentagram.test.js` 文件。

![](img/3240d77ebb9dd9a14111406bce3a787e_50.png)

![](img/3240d77ebb9dd9a14111406bce3a787e_52.png)

我们将编写一个测试来验证合约能成功部署并且 `name` 变量正确。

```javascript
const Decentagram = artifacts.require("Decentagram");

require('chai')
  .use(require('chai-as-promised'))
  .should()

contract('Decentagram', (accounts) => {
  let decentagram

  before(async () => {
    decentagram = await Decentagram.deployed()
  })

  describe('deployment', async () => {
    it('deploys successfully', async () => {
      const address = await decentagram.address
      assert.notEqual(address, 0x0)
      assert.notEqual(address, '')
      assert.notEqual(address, null)
      assert.notEqual(address, undefined)
    })

    it('has a name', async () => {
      const name = await decentagram.name()
      assert.equal(name, 'Decentagram')
    })
  })
})
```
运行测试：
```bash
truffle test
```
如果一切正常，测试应该通过。

## 设计数据模型与存储

现在我们已经验证了基础设置，接下来设计应用的核心数据模型。

我们的应用需要存储图片帖子。每个帖子将包含以下信息：
1.  唯一ID
2.  IPFS上的图片哈希值（即图片的地址）
3.  图片描述
4.  收到的打赏总额
5.  作者的钱包地址

在Solidity中，我们使用 `struct` 来定义自定义数据类型，使用 `mapping` 来存储键值对数据，类似于数据库。

![](img/3240d77ebb9dd9a14111406bce3a787e_54.png)

![](img/3240d77ebb9dd9a14111406bce3a787e_56.png)

### 定义数据结构和存储

让我们更新 `Decentagram.sol` 合约。

```solidity
contract Decentagram {
    string public name = "Decentagram";

    // 存储图片数量，也用作ID生成器
    uint public imageCount = 0;

    // 定义图片结构体
    struct Image {
        uint id;
        string hash;
        string description;
        uint tipAmount;
        address payable author;
    }

    // 使用映射来存储图片，键是ID，值是Image结构体
    mapping(uint => Image) public images;
}
```
*   `uint public imageCount = 0;`：一个公共的无符号整数，用于追踪图片总数，并作为生成新图片ID的计数器。
*   `struct Image { ... }`：定义了一个名为 `Image` 的结构体，包含我们需要的所有字段。注意 `author` 被声明为 `address payable`，这意味着我们可以向这个地址发送以太币。
*   `mapping(uint => Image) public images;`：定义了一个公共的映射。键是 `uint` 类型的ID，值是 `Image` 结构体。声明为 `public` 后，Solidity会自动生成一个 `images(id)` 函数，允许我们通过ID查询单个图片。

## 实现核心功能：上传图片

有了数据模型，我们现在可以实现第一个核心功能：上传图片。

### 上传图片函数

我们将创建一个函数，允许用户将图片的IPFS哈希和描述上传到区块链。

```solidity
// 事件：用于在前端监听图片创建
event ImageCreated(
    uint id,
    string hash,
    string description,
    uint tipAmount,
    address payable author
);

function uploadImage(string memory _imgHash, string memory _description) public {
    // 验证输入
    require(bytes(_imgHash).length > 0, 'Image hash is required');
    require(bytes(_description).length > 0, 'Description is required');
    require(msg.sender != address(0), 'Author address is required');

    // 增加图片计数
    imageCount ++;

    // 将图片添加到映射中
    images[imageCount] = Image(
        imageCount,
        _imgHash,
        _description,
        0, // 初始打赏金额为0
        payable(msg.sender) // 调用者即为作者
    );

    // 触发事件
    emit ImageCreated(imageCount, _imgHash, _description, 0, payable(msg.sender));
}
```
**代码解释：**
1.  `event ImageCreated(...)`：定义了一个事件。当图片创建时，会触发这个事件，前端应用可以监听它。
2.  `function uploadImage(...) public`：这是一个公开函数，任何人都可以调用。
3.  `require(...)`：这些是条件检查。如果条件不满足，交易将回滚，状态不会改变。我们确保哈希、描述和发送者地址有效。
4.  `imageCount ++;`：每次上传，图片总数加1。新的 `imageCount` 值就是新图片的ID。
5.  `images[imageCount] = Image(...);`：在映射中创建一个新的 `Image` 条目。
6.  `emit ImageCreated(...);`：触发事件，通知所有监听者一个新的图片已被创建。

### 为上传功能编写测试

现在，让我们为上传功能编写更全面的测试。回到 `test/decentagram.test.js` 文件。

我们将添加测试来验证图片创建、输入验证以及事件触发。

```javascript
describe('images', async () => {
  let result
  const imgHash = 'abc123'
  const description = 'Hello World'

  before(async () => {
    // 使用第一个测试账户作为作者上传一张图片
    result = await decentagram.uploadImage(imgHash, description, { from: accounts[0] })
  })

  it('creates images', async () => {
    // 检查图片总数增加
    const imageCount = await decentagram.imageCount()
    assert.equal(imageCount, 1)

    // 检查事件参数
    const event = result.logs[0].args
    assert.equal(event.id.toNumber(), imageCount.toNumber(), 'ID is correct')
    assert.equal(event.hash, imgHash, 'Hash is correct')
    assert.equal(event.description, description, 'Description is correct')
    assert.equal(event.tipAmount.toNumber(), 0, 'Tip amount is correct')
    assert.equal(event.author, accounts[0], 'Author is correct')
  })

  it('lists images', async () => {
    // 通过ID获取图片
    const image = await decentagram.images(1)
    assert.equal(image.id.toNumber(), 1, 'ID is correct')
    assert.equal(image.hash, imgHash, 'Hash is correct')
    assert.equal(image.description, description, 'Description is correct')
    assert.equal(image.tipAmount.toNumber(), 0, 'Tip amount is correct')
    assert.equal(image.author, accounts[0], 'Author is correct')
  })

  it('fails for empty image hash', async () => {
    // 尝试用空哈希上传，应该被拒绝
    await decentagram.uploadImage('', description, { from: accounts[0] }).should.be.rejected
  })

  it('fails for empty description', async () => {
    // 尝试用空描述上传，应该被拒绝
    await decentagram.uploadImage(imgHash, '', { from: accounts[0] }).should.be.rejected
  })
})
```
运行测试以确保所有功能正常工作：
```bash
truffle test
```

![](img/3240d77ebb9dd9a14111406bce3a787e_58.png)

![](img/3240d77ebb9dd9a14111406bce3a787e_60.png)

## 实现核心功能：打赏图片

图片分享的另一个关键功能是允许用户为他们喜欢的图片打赏。接下来我们实现这个功能。

### 打赏图片函数

![](img/3240d77ebb9dd9a14111406bce3a787e_62.png)

![](img/3240d77ebb9dd9a14111406bce3a787e_63.png)

![](img/3240d77ebb9dd9a14111406bce3a787e_64.png)

![](img/3240d77ebb9dd9a14111406bce3a787e_65.png)

![](img/3240d77ebb9dd9a14111406bce3a787e_66.png)

我们将创建一个函数，允许用户向特定图片的作者发送以太币作为打赏。

![](img/3240d77ebb9dd9a14111406bce3a787e_68.png)

![](img/3240d77ebb9dd9a14111406bce3a787e_69.png)

![](img/3240d77ebb9dd9a14111406bce3a787e_71.png)

```solidity
// 事件：用于在前端监听打赏
event ImageTipped(
    uint id,
    string hash,
    string description,
    uint tipAmount,
    address payable author
);

![](img/3240d77ebb9dd9a14111406bce3a787e_73.png)

![](img/3240d77ebb9dd9a14111406bce3a787e_74.png)

function tipImageOwner(uint _id) public payable {
    // 验证ID有效
    require(_id > 0 && _id <= imageCount, 'Invalid image ID');

    // 获取图片
    Image memory _image = images[_id];

    // 获取作者地址
    address payable _author = _image.author;

    // 确保作者地址有效
    require(_author != address(0), 'Author address is invalid');

    // 向作者转账（msg.value 是随交易发送的以太币数量）
    _author.transfer(msg.value);

    // 更新图片的打赏总额
    _image.tipAmount = _image.tipAmount + msg.value;

    // 将更新后的图片存回映射
    images[_id] = _image;

    // 触发打赏事件
    emit ImageTipped(_id, _image.hash, _image.description, _image.tipAmount, _author);
}
```
**代码解释：**
1.  `function tipImageOwner(uint _id) public payable`：这是一个 `payable` 函数，意味着调用它可以附带以太币。`_id` 是要打赏的图片ID。
2.  `require(_id > 0 && _id <= imageCount, ...)`：验证提供的ID在有效范围内。
3.  `Image memory _image = images[_id];`：从存储中读取图片数据到内存中。`memory` 关键字表示这是一个临时变量。
4.  `address payable _author = _image.author;`：获取图片作者的支付地址。
5.  `_author.transfer(msg.value);`：这是最关键的一行。它将调用者随交易发送的以太币（`msg.value`）转账给作者。
6.  更新图片的 `tipAmount` 并将其存回映射。
7.  触发 `ImageTipped` 事件，通知前端打赏已完成。

![](img/3240d77ebb9dd9a14111406bce3a787e_76.png)

![](img/3240d77ebb9dd9a14111406bce3a787e_78.png)

![](img/3240d77ebb9dd9a14111406bce3a787e_79.png)

### 为打赏功能编写测试

![](img/3240d77ebb9dd9a14111406bce3a787e_81.png)

![](img/3240d77ebb9dd9a14111406bce3a787e_82.png)

![](img/3240d77ebb9dd9a14111406bce3a787e_84.png)

![](img/3240d77ebb9dd9a14111406bce3a787e_85.png)

![](img/3240d77ebb9dd9a14111406bce3a787e_87.png)

现在，为打赏功能添加测试。我们需要检查作者在打赏后是否收到了资金。

![](img/3240d77ebb9dd9a14111406bce3a787e_89.png)

![](img/3240d77ebb9dd9a14111406bce3a787e_91.png)

```javascript
describe('tipping', async () => {
  let result
  const tipAmount = web3.utils.toWei('1', 'Ether') // 1个以太币，转换为Wei单位

  before(async () => {
    // 假设我们已经有一张ID为1的图片，由accounts[0]上传
    // 现在由另一个账户（accounts[1]）来打赏
    result = await decentagram.tipImageOwner(1, { from: accounts[1], value: tipAmount })
  })

  it('allows users to tip images', async () => {
    // 获取打赏后作者的余额
    let oldAuthorBalance = await web3.eth.getBalance(accounts[0])
    oldAuthorBalance = new web3.utils.BN(oldAuthorBalance)

    // 执行打赏
    result = await decentagram.tipImageOwner(1, { from: accounts[1], value: tipAmount })

    // 检查事件
    const event = result.logs[0].args
    assert.equal(event.id.toNumber(), 1, 'ID is correct')
    assert.equal(event.tipAmount, tipAmount, 'Tip amount is correct')

    // 检查作者余额是否增加
    let newAuthorBalance = await web3.eth.getBalance(accounts[0])
    newAuthorBalance = new web3.utils.BN(newAuthorBalance)

    const tip = new web3.utils.BN(tipAmount)
    const expectedBalance = oldAuthorBalance.add(tip)

    assert.equal(newAuthorBalance.toString(), expectedBalance.toString(), 'Author received the tip')
  })

  it('fails for invalid image ID', async () => {
    // 尝试打赏一个不存在的ID（例如99），应该被拒绝
    await decentagram.tipImageOwner(99, { from: accounts[1], value: tipAmount }).should.be.rejected
  })
})
```
运行所有测试，确保打赏功能按预期工作。

## 部署完整智能合约

在开始构建前端之前，让我们确保完整的智能合约已部署到我们的本地区块链。

![](img/3240d77ebb9dd9a14111406bce3a787e_93.png)

在终端中运行：
```bash
truffle migrate --reset
```
这将编译并部署我们刚刚编写的完整 `Decentagram` 合约到Ganache。

![](img/3240d77ebb9dd9a14111406bce3a787e_95.png)

![](img/3240d77ebb9dd9a14111406bce3a787e_97.png)

## 构建React前端应用（第一部分）

![](img/3240d77ebb9dd9a14111406bce3a787e_99.png)

智能合约的后端逻辑已经完成，现在我们将构建用户与之交互的前端界面。我们将使用React框架。

![](img/3240d77ebb9dd9a14111406bce3a787e_101.png)

![](img/3240d77ebb9dd9a14111406bce3a787e_103.png)

![](img/3240d77ebb9dd9a14111406bce3a787e_104.png)

### 项目结构与组件

![](img/3240d77ebb9dd9a14111406bce3a787e_106.png)

![](img/3240d77ebb9dd9a14111406bce3a787e_108.png)

![](img/3240d77ebb9dd9a14111406bce3a787e_110.png)

我们的前端主要包含三个组件：
1.  `App.js`：主应用组件，负责初始化区块链连接和管理全局状态。
2.  `Navbar.js`：导航栏组件，显示应用名称和用户账户。
3.  `Main.js`：主内容组件，显示图片流和上传表单。

![](img/3240d77ebb9dd9a14111406bce3a787e_112.png)

![](img/3240d77ebb9dd9a14111406bce3a787e_113.png)

![](img/3240d77ebb9dd9a14111406bce3a787e_114.png)

![](img/3240d77ebb9dd9a14111406bce3a787e_116.png)

### 连接区块链与MetaMask

![](img/3240d77ebb9dd9a14111406bce3a787e_118.png)

首先，我们需要将React应用连接到以太坊区块链。这需要两个步骤：使用Web3.js库，并通过MetaMask钱包提供连接。

![](img/3240d77ebb9dd9a14111406bce3a787e_120.png)

![](img/3240d77ebb9dd9a14111406bce3a787e_122.png)

![](img/3240d77ebb9dd9a14111406bce3a787e_124.png)

在 `App.js` 中，我们添加初始化代码。

![](img/3240d77ebb9dd9a14111406bce3a787e_126.png)

![](img/3240d77ebb9dd9a14111406bce3a787e_128.png)

1.  **加载Web3**：检查浏览器中是否注入了Web3实例（由MetaMask提供）。
    ```javascript
    async loadWeb3() {
      if (window.ethereum) {
        window.web3 = new Web3(window.ethereum)
        await window.ethereum.enable()
      } else if (window.web3) {
        window.web3 = new Web3(window.web3.currentProvider)
      } else {
        window.alert('Non-Ethereum browser detected. You should consider trying MetaMask!')
      }
    }
    ```
2.  **加载区块链数据**：获取用户账户、智能合约实例，并加载图片数据。
    ```javascript
    async loadBlockchainData() {
      const web3 = window.web3
      // 加载账户
      const accounts = await web3.eth.getAccounts()
      this.setState({ account: accounts[0] })

      // 网络ID
      const networkId = await web3.eth.net.getId()

      // 加载智能合约
      const decentagramData = Decentagram.networks[networkId]
      if (decentagramData) {
        const decentagram = new web3.eth.Contract(Decentagram.abi, decentagramData.address)
        this.setState({ decentagram })

        // 获取图片总数
        const imageCount = await decentagram.methods.imageCount().call()
        this.setState({ imageCount })

        // 加载所有图片
        let images = []
        for (let i = 1; i <= imageCount; i++) {
          const image = await decentagram.methods.images(i).call()
          images.push(image)
        }
        // 按打赏金额排序，最高的排前面
        images.sort((a, b) => b.tipAmount - a.tipAmount)
        this.setState({ images })
      } else {
        window.alert('Decentagram contract not deployed to detected network.')
      }
      // 数据加载完成
      this.setState({ loading: false })
    }
    ```
3.  **在组件挂载时调用**：使用React的生命周期方法 `componentDidMount` 来触发初始化。
    ```javascript
    async componentDidMount() {
      await this.loadWeb3()
      await this.loadBlockchainData()
    }
    ```
4.  **管理应用状态**：在构造函数中初始化状态。
    ```javascript
    constructor(props) {
      super(props)
      this.state = {
        account: '',
        decentagram: null,
        images: [],
        loading: true
      }
      this.loadWeb3 = this.loadWeb3.bind(this)
      this.loadBlockchainData = this.loadBlockchainData.bind(this)
      this.captureFile = this.captureFile.bind(this)
      this.uploadImage = this.uploadImage.bind(this)
      this.tipImageOwner = this.tipImageOwner.bind(this)
    }
    ```

![](img/3240d77ebb9dd9a14111406bce3a787e_130.png)

### 导入账户到MetaMask并显示

![](img/3240d77ebb9dd9a14111406bce3a787e_132.png)

为了进行测试，我们需要将Ganache中的账户导入MetaMask。
1.  在Ganache界面中，点击第一个账户旁边的钥匙图标，复制其私钥。
2.  在MetaMask中，点击账户图标，选择“导入账户”。
3.  粘贴私钥并导入。现在你的MetaMask中应该有了一个带有测试以太币的账户。
4.  刷新前端页面，导航栏上应该会显示你的以太坊地址和一个Identicon（根据地址生成的头像）。

![](img/3240d77ebb9dd9a14111406bce3a787e_134.png)

## 构建React前端应用（第二部分）：上传图片到IPFS

![](img/3240d77ebb9dd9a14111406bce3a787e_136.png)

![](img/3240d77ebb9dd9a14111406bce3a787e_138.png)

现在，让我们实现上传图片的功能。这涉及两个步骤：先将图片上传到去中心化的IPFS网络，然后将返回的哈希值存储到区块链智能合约中。

![](img/3240d77ebb9dd9a14111406bce3a787e_140.png)

![](img/3240d77ebb9dd9a14111406bce3a787e_141.png)

### 集成IPFS客户端

![](img/3240d77ebb9dd9a14111406bce3a787e_143.png)

![](img/3240d77ebb9dd9a14111406bce3a787e_145.png)

首先，我们需要在应用中集成IPFS。我们已经通过 `ipfs-http-client` 包安装了IPFS客户端。

![](img/3240d77ebb9dd9a14111406bce3a787e_147.png)

![](img/3240d77ebb9dd9a14111406bce3a787e_149.png)

在 `App.js` 顶部导入并初始化IPFS客户端：
```javascript
const ipfsClient = require('ipfs-http-client')
const ipfs = ipfsClient({ host: 'ipfs.infura.io', port: 5001, protocol: 'https' })
```

![](img/3240d77ebb9dd9a14111406bce3a787e_151.png)

![](img/3240d77ebb9dd9a14111406bce3a787e_153.png)

### 处理文件上传

![](img/3240d77ebb9dd9a14111406bce3a787e_155.png)

![](img/3240d77ebb9dd9a14111406bce3a787e_157.png)

我们需要一个函数来处理用户选择的图片文件，并将其转换为IPFS可以接受的格式（Buffer）。

在 `App.js` 中添加：
```javascript
captureFile = event => {
  event.preventDefault()
  const file = event.target.files[0]
  const reader = new window.FileReader()
  reader.readAsArrayBuffer(file)
  reader.onloadend = () => {
    // 将文件转换为Buffer，这是IPFS需要的格式
    this.setState({ buffer: Buffer(reader.result) })
    console.log('buffer', this.state.buffer)
  }
}
```
这个函数在用户选择文件后被调用，它将文件内容读入一个 `Buffer` 对象并存储在组件状态中。

![](img/3240d77ebb9dd9a14111406bce3a787e_159.png)

![](img/3240d77ebb9dd9a14111406bce3a787e_161.png)

### 上传到IPFS和区块链

![](img/3240d77ebb9dd9a14111406bce3a787e_163.png)

![](img/3240d77ebb9dd9a14111406bce3a787e_165.png)

接下来，创建 `uploadImage` 函数，它首先将Buffer上传到IPFS，获取哈希，然后调用智能合约的 `uploadImage` 函数。

![](img/3240d77ebb9dd9a14111406bce3a787e_167.png)

![](img/3240d77ebb9dd9a14111406bce3a787e_169.png)

```javascript
uploadImage = description => {
  this.setState({ loading: true })
  console.log("Submitting file to IPFS...")

  // 将文件上传到IPFS
  ipfs.add(this.state.buffer, (error, result) => {
    console.log('IPFS result', result)
    if(error) {
      console.error(error)
      this.setState({ loading: false })
      return
    }
    // 调用智能合约的 uploadImage 方法
    this.state.decentagram.methods.uploadImage(result[0].hash, description)
      .send({ from: this.state.account })
      .on('transactionHash', (hash) => {
        this.setState({ loading: false })
        // 可以在这里添加成功提示或重载图片列表
        window.location.reload()
      })
      .on('error', (e) => {
        window.alert('Error uploading image')
        this.setState({ loading: false })
      })
  })
}
```

### 构建上传表单

![](img/3240d77ebb9dd9a14111406bce3a787e_171.png)

![](img/3240d77ebb9dd9a14111406bce3a787e_172.png)

现在，我们需要在 `Main.js` 组件中构建一个表单来触发这些函数。

`Main.js` 中的表单部分大致如下：
```jsx
<form onSubmit={(event) => {
  event.preventDefault()
  const description = this.imageDescription.value
  this.props.uploadImage(description)
}}>
  <input type='file' accept=".jpg, .jpeg, .png, .bmp, .gif" onChange={this.props.captureFile} />
  <input type='text' ref={(input) => { this.imageDescription = input }} placeholder="Image description..." required />
  <button type='submit'>Upload Image</button>
</form>
```
*   文件输入框的 `onChange` 事件调用 `captureFile`。
*   表单的 `onSubmit` 事件调用 `uploadImage`，并传递描述文本。

现在，你可以尝试选择一张图片，输入描述，然后点击上传。MetaMask会弹出交易确认窗口（需要支付Gas费）。确认后，你的图片将被上传到IPFS，其哈希值会被记录在区块链上。

![](img/3240d77ebb9dd9a14111406bce3a787e_174.png)

![](img/3240d77ebb9dd9a14111406bce3a787e_176.png)

![](img/3240d77ebb9dd9a14111406bce3a787e_178.png)

## 构建React前端应用（第三部分）：显示图片与打赏功能

![](img/3240d77ebb9dd9a14111406bce3a787e_179.png)

![](img/3240d77ebb9dd9a14111406bce3a787e_180.png)

最后一步是在前端显示图片流，并实现打赏功能。

![](img/3240d77ebb9dd9a14111406bce3a787e_182.png)

![](img/3240d77ebb9dd9a14111406bce3a787e_184.png)

### 显示图片流

![](img/3240d77ebb9dd9a14111406bce3a787e_186.png)

![](img/3240d77ebb9dd9a14111406bce3a787e_187.png)

![](img/3240d77ebb9dd9a14111406bce3a787e_189.png)

在 `App.js` 的 `loadBlockchainData` 函数中，我们已经循环获取了所有图片并存储在 `this.state.images` 中。我们需要将这个状态传递给 `Main` 组件。

![](img/3240d77ebb9dd9a14111406bce3a787e_191.png)

![](img/3240d77ebb9dd9a14111406bce3a787e_193.png)

在 `App.js` 的render方法中：
```jsx
<Main
  images={this.state.images}
  captureFile={this.captureFile}
  uploadImage={this.uploadImage}
  tipImageOwner={this.tipImageOwner}
/>
```
在 `Main.js` 中，我们可以遍历 `this.props.images` 数组来渲染每张图片。

![](img/3240d77ebb9dd9a14111406bce3a787e_195.png)

![](img/3240d77ebb9dd9a14111406bce3a787e_197.png)

```jsx
{this.props.images.map((image, key) => {
  return(
    <div className="card" key={key}>
      <div className="card-header">
        <Identicon string={image.author} size={30} /> {image.author}
      </div>
      <img src={`https://ipfs.infura.io/ipfs/${image.hash}`} alt={image.description} />
      <div className="card-body">
        <p>{image.description}</p>
        <p>Tip Amount: {window.web3.utils.fromWei(image.tipAmount, 'Ether')} ETH</p>
        <button
          name={image.id}
          onClick={(event) => {
            let tipAmount = window.web3.utils.toWei('0.1', 'Ether')
            this.props.tipImageOwner(event.target.name, tipAmount)
          }}
        >
          TIP 0.1 ETH
        </button>
      </div>
    </div>
  )
})}
```
*   使用 `Identicon` 组件根据作者地址生成头像。
*   图片的 `src` 通过IPFS网关链接构建：`https://ipfs.infura.io/ipfs/${image.hash}`。
*   显示描述和当前打赏总额（从Wei单位转换回Ether）。
*   点击按钮会调用打赏函数，固定打赏0.1 ETH。

![](img/3240d77ebb9dd9a14111406bce3a787e_199.png)

### 实现打赏函数

在 `App.js` 中实现 `tipImageOwner` 函数：
```javascript
tipImageOwner = (id, tipAmount) => {
  this.setState({ loading: true })
  this.state.decentagram.methods.tipImageOwner(id)
    .send({ from: this.state.account, value: tipAmount })
    .on('transactionHash', (hash) => {
      this.setState({ loading: false })
      window.location.reload() // 刷新以更新打赏金额和排序
    })
    .on('error', (e) => {
      window.alert('Error tipping image')
      this.setState({ loading: false })
    })
}
```
这个函数调用智能合约的 `tipImageOwner` 方法，并附带一定数量的以太币（`value: tipAmount`）。

![](img/3240d77ebb9dd9a14111406bce3a787e_201.png)

### 测试完整流程

![](img/3240d77ebb9dd9a14111406bce3a787e_203.png)

![](img/3240d77ebb9dd9a14111406bce3a787e_205.png)

1.  **切换账户**：在MetaMask中切换到另一个Ganache账户（例如第二个账户），以模拟另一个用户。
2.  **打赏图片**：找到你想要打赏的图片，点击“TIP 0.1 ETH”按钮。MetaMask会弹出确认窗口。
3.  **确认交易**：确认后，等待交易完成。页面刷新后，你会看到该图片的打赏金额增加了0.1 ETH。
4.  **排序生效**：由于我们在 `loadBlockchainData` 中按打赏金额降序排序了图片，打赏金额最高的图片会自动跳到顶部。

![](img/3240d77ebb9dd9a14111406bce3a787e_207.png)

## 总结

![](img/3240d77ebb9dd9a14111406bce3a787e_209.png)

在本节课中，我们一起学习并完成了一个完整的去中心化图片分享应用——Decentagram。我们回顾一下所完成的工作：

![](img/3240d77ebb9dd9a14111406bce3a787e_211.png)

1.  **智能合约开发**：我们使用Solidity编写了 `Decentagram` 智能合约，实现了图片上传、存储和打赏的核心逻辑，并确保了代码的安全性和可验证性。
2.  **自动化测试**：我们为智能合约编写了全面的JavaScript测试，确保其功能在各种情况下都能按预期工作，这对于不可变的区块链代码至关重要。
3.  **本地区块链环境**：我们使用Ganache搭建了本地测试网络，并使用Truffle框架进行合约的编译、部署和迁移。
4.  **前端应用构建**：我们使用React构建了用户界面，并通过Web3.js库将其与以太坊区块链连接起来。
5.  **集成IPFS**：我们实现了将用户上传的图片文件存储到去中心化的IPFS网络，仅将内容哈希存储在高效的区块链上，解决了区块链存储成本高的问题。
6.  **钱包集成**：我们通过MetaMask浏览器扩展管理用户身份、签名交易并支付Gas费。
7.  **核心功能实现**：最终的应用允许用户连接钱包、上传图片到IPFS和区块链、浏览按打赏排序的图片流，并为喜欢的图片打赏加密货币。

![](img/3240d77ebb9dd9a14111406bce3a787e_213.png)

你已成功构建了一个真正的Web 3.0应用：它**去中心化**（逻辑在链上，文件在IPFS）、**抗审查**（无人能删除你的帖子）、**透明**（算法由公开的智能合约定义）且**由用户拥有**（打赏直接给创作者）。

![](img/3240d77ebb9dd9a14111406bce3a787e_215.png)

这只是区块链开发的起点。你可以在此基础上扩展更多功能，例如用户评论、关注系统、将前端也部署到IPFS以实现完全不可阻挡，或者将其部署到真实的以太坊测试网甚至主网。继续探索，构建更强大的去中心化未来吧！