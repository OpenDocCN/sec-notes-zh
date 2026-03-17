# 003：Web3 - 用区块链编写YouTube复刻版

## 概述
在本节课中，我们将学习如何构建一个基于区块链的视频分享网站，类似于YouTube。这是一个面向未来的、不可阻挡的Web 3.0应用，因为它运行在以太坊区块链之上。我们将从零开始，学习如何编写以太坊智能合约、将其部署到区块链，并创建前端网站，让用户可以上传和观看视频。

## 应用架构
上一节我们介绍了项目的目标，本节中我们来看看这个去中心化应用的整体架构。

![](img/7527852448b62b40f94952190cd9680e_1.png)

![](img/7527852448b62b40f94952190cd9680e_2.png)

![](img/7527852448b62b40f94952190cd9680e_3.png)

![](img/7527852448b62b40f94952190cd9680e_4.png)

传统的YouTube采用中心化模型：用户通过浏览器连接到YouTube服务器，所有代码和数据都存储在中心化的服务器和数据库中。这种模式下，应用创建者可以随时删除视频或更改算法。

![](img/7527852448b62b40f94952190cd9680e_6.png)

我们想要构建的是一个去中心化的版本，其工作原理如下：
*   用户通过浏览器和区块链钱包（如MetaMask）连接到网站。
*   网站直接与区块链通信，应用的所有核心逻辑和数据都存储在以太坊智能合约中。
*   智能合约存储平台上视频的引用，这些视频文件实际存储在IPFS（星际文件系统）上。IPFS是一个去中心化的文件存储系统。
*   IPFS将视频文件服务回用户的浏览器。

![](img/7527852448b62b40f94952190cd9680e_8.png)

![](img/7527852448b62b40f94952190cd9680e_10.png)

![](img/7527852448b62b40f94952190cd9680e_11.png)

以下是构建应用的组件图：
*   **DApp创建者**：使用Truffle框架创建智能合约。
*   **开发区块链**：将合约发布到Ganache（个人开发区块链）。
*   **前端连接**：使用MetaMask连接我们创建的React JS网站。
*   **交互流程**：当与区块链交互时，通过MetaMask直接与Ganache通信；客户端应用（React）同时与IPFS通信。

## 环境与依赖安装
在开始编码之前，我们需要安装所有必要的依赖项。

以下是需要安装的软件和工具：
1.  **Node.js**：允许我们在计算机上安装所有软件包并运行客户端应用。可以通过官网下载或使用Homebrew等包管理器安装。
2.  **Truffle框架**：用于创建以太坊智能合约的框架。我们可以使用Solidity编程语言编写合约，用Truffle进行测试和部署。通过命令行安装：`npm install -g truffle@5.1.3`。
3.  **Ganache个人区块链**：一个可以在本地计算机上运行的区块链，用于测试和部署智能合约，无需支付真实货币。从官网下载对应操作系统的版本并运行。
4.  **MetaMask浏览器扩展**：一个以太坊钱包，可将浏览器变为区块链浏览器。从Chrome网上应用店安装MetaMask扩展，并完成设置步骤。

## 项目初始化与智能合约开发
环境配置完成后，让我们开始编码项目。我们将从智能合约开始。

![](img/7527852448b62b40f94952190cd9680e_13.png)

首先，从GitHub仓库克隆项目启动代码。进入项目目录并打开代码编辑器。

![](img/7527852448b62b40f94952190cd9680e_15.png)

快速浏览项目结构：
*   `truffle-config.js`：Truffle项目配置文件，用于连接区块链（例如Ganache）。
*   `contracts/`：存放智能合约的目录。其中`DVideo.sol`是管理应用的主要合约。
*   `migrations/`：存放部署脚本的目录。`2_deploy_contracts.js`用于将DVideo合约部署到区块链。

![](img/7527852448b62b40f94952190cd9680e_17.png)

![](img/7527852448b62b40f94952190cd9680e_19.png)

`DVideo.sol`是用Solidity语言编写的智能合约。Solidity是以太坊智能合约的主要编程语言，语法类似JavaScript。合约顶部声明了Solidity版本。合约内有一个公共状态变量`name`，其值会永久写入区块链。合约还包含一些函数。

在深入开发合约之前，我们先在控制台中测试基础设置，确保项目能连接到区块链。
1.  首先安装Node模块：`npm install`。
2.  然后迁移合约到区块链：`truffle migrate`。
3.  启动Truffle控制台：`truffle console`。在控制台中，我们可以通过JavaScript与区块链和智能合约交互。

## 设计智能合约功能
现在，让我们重新审视应用的工作流程图，并设计智能合约需要实现的核心功能。

我们的智能合约需要完成以下几件事：
1.  **上传视频**：用户上传视频到IPFS，然后将IPFS返回的哈希值存储到智能合约中。
2.  **存储视频**：在区块链上保存视频的相关信息。
3.  **列出视频**：能够检索并列出所有已上传的视频。

具体流程是：用户将视频上传到IPFS，IPFS返回一个唯一哈希（即文件在IPFS上的ID）。然后，我们将这个哈希连同视频标题等信息一起存储到智能合约中。之后，我们可以通过智能合约列出所有已保存的视频。

## 实现智能合约
明确了功能后，我们开始一步步实现智能合约代码。

### 1. 定义视频数据结构
首先，我们需要定义视频的数据模型。在Solidity中，可以使用`struct`（结构体）来定义自定义数据类型。

```solidity
struct Video {
    uint id;
    string hash;
    string title;
    address author;
}
```
这个`Video`结构体包含四个字段：
*   `id`：视频在合约内的唯一标识符（无符号整数）。
*   `hash`：视频在IPFS上的哈希值（字符串）。
*   `title`：视频标题（字符串）。
*   `author`：视频上传者的以太坊地址。

### 2. 存储视频数据
接下来，我们需要一个地方来存储这些视频。在Solidity中，可以使用`mapping`（映射）数据结构，它类似于一个键值对数据库。

![](img/7527852448b62b40f94952190cd9680e_21.png)

![](img/7527852448b62b40f94952190cd9680e_23.png)

![](img/7527852448b62b40f94952190cd9680e_24.png)

```solidity
mapping(uint => Video) public videos;
```
这个映射的键是`uint`类型的ID，值是`Video`结构体。我们将它声明为`public`，这样我们就可以通过一个自动生成的函数`videos(id)`来根据ID查询单个视频。

我们还需要一个计数器来管理视频ID。

![](img/7527852448b62b40f94952190cd9680e_26.png)

![](img/7527852448b62b40f94952190cd9680e_28.png)

![](img/7527852448b62b40f94952190cd9680e_30.png)

![](img/7527852448b62b40f94952190cd9680e_32.png)

![](img/7527852448b62b40f94952190cd9680e_34.png)

```solidity
uint public videoCount;
```

![](img/7527852448b62b40f94952190cd9680e_36.png)

### 3. 实现上传视频函数
现在，我们来实现核心的`uploadVideo`函数。这个函数接收两个参数：IPFS视频哈希和视频标题。

![](img/7527852448b62b40f94952190cd9680e_38.png)

```solidity
function uploadVideo(string memory _videoHash, string memory _title) public {
    // 输入验证
    require(bytes(_videoHash).length > 0);
    require(bytes(_title).length > 0);
    require(msg.sender != address(0));

    // 增加视频计数并作为新视频ID
    videoCount ++;

    // 将新视频添加到映射中
    videos[videoCount] = Video(videoCount, _videoHash, _title, msg.sender);

    // 触发上传事件
    emit VideoUploaded(videoCount, _videoHash, _title, msg.sender);
}
```
函数内部逻辑：
1.  使用`require`语句进行输入验证，确保哈希、标题非空，且上传者地址有效。
2.  将`videoCount`加1，作为新视频的ID。
3.  创建一个新的`Video`结构体实例，并将其存入`videos`映射。
4.  使用`emit`关键字触发一个`VideoUploaded`事件，便于前端应用监听。

![](img/7527852448b62b40f94952190cd9680e_40.png)

![](img/7527852448b62b40f94952190cd9680e_42.png)

我们还需要在合约顶部定义这个事件：

![](img/7527852448b62b40f94952190cd9680e_44.png)

![](img/7527852448b62b40f94952190cd9680e_46.png)

![](img/7527852448b62b40f94952190cd9680e_48.png)

![](img/7527852448b62b40f94952190cd9680e_49.png)

![](img/7527852448b62b40f94952190cd9680e_51.png)

```solidity
event VideoUploaded(uint indexed id, string hash, string title, address author);
```

![](img/7527852448b62b40f94952190cd9680e_53.png)

## 测试智能合约
智能合约编写完成后，进行测试至关重要。虽然本教程因时间关系不逐步编写测试，但项目提供了完整的测试文件。

![](img/7527852448b62b40f94952190cd9680e_55.png)

![](img/7527852448b62b40f94952190cd9680e_56.png)

![](img/7527852448b62b40f94952190cd9680e_58.png)

![](img/7527852448b62b40f94952190cd9680e_59.png)

在`test/`目录下的`DVideo.test.js`文件中，包含了用Mocha和Chai编写的测试用例。这些测试会验证合约部署是否成功、能否创建视频以及能否正确列出视频。

![](img/7527852448b62b40f94952190cd9680e_61.png)

![](img/7527852448b62b40f94952190cd9680e_63.png)

![](img/7527852448b62b40f94952190cd9680e_65.png)

![](img/7527852448b62b40f94952190cd9680e_67.png)

![](img/7527852448b62b40f94952190cd9680e_69.png)

![](img/7527852448b62b40f94952190cd9680e_71.png)

运行测试命令：`truffle test`。如果所有测试通过，说明我们的智能合约逻辑正确。

![](img/7527852448b62b40f94952190cd9680e_73.png)

![](img/7527852448b62b40f94952190cd9680e_75.png)

![](img/7527852448b62b40f94952190cd9680e_77.png)

## 部署更新后的智能合约
测试通过后，我们需要将更新后的智能合约部署到区块链。由于区块链的不可变性，要更新合约代码必须部署一份新的副本。

![](img/7527852448b62b40f94952190cd9680e_79.png)

![](img/7527852448b62b40f94952190cd9680e_81.png)

![](img/7527852448b62b40f94952190cd9680e_83.png)

使用命令`truffle migrate --reset`来重新部署合约。这会生成一份新的合约副本到区块链上。

![](img/7527852448b62b40f94952190cd9680e_85.png)

![](img/7527852448b62b40f94952190cd9680e_87.png)

![](img/7527852448b62b40f94952190cd9680e_89.png)

![](img/7527852448b62b40f94952190cd9680e_91.png)

## 开发客户端应用（前端）
智能合约部署完毕，现在开始构建客户端部分，即用户交互的网站。

![](img/7527852448b62b40f94952190cd9680e_93.png)

![](img/7527852448b62b40f94952190cd9680e_95.png)

![](img/7527852448b62b40f94952190cd9680e_97.png)

![](img/7527852448b62b40f94952190cd9680e_99.png)

![](img/7527852448b62b40f94952190cd9680e_101.png)

### 1. 启动开发服务器
首先，在新的终端标签页中启动React开发服务器：`npm run start`。这将在浏览器中打开一个React应用，目前是一个空白模板，包含导航栏和空的内容区域。

![](img/7527852448b62b40f94952190cd9680e_103.png)

### 2. 连接前端与区块链
默认情况下，浏览器无法直接连接区块链，我们需要通过MetaMask来实现。同时，前端应用需要使用Web3.js库与以太坊通信。

![](img/7527852448b62b40f94952190cd9680e_105.png)

![](img/7527852448b62b40f94952190cd9680e_107.png)

**连接步骤：**
1.  在MetaMask中添加Ganache网络（使用Ganache提供的RPC URL）并导入一个测试账户（使用Ganache中某个账户的私钥）。
2.  在前端代码中，我们已经在`App.js`组件里配置了Web3.js，它通过MetaMask注入的`ethereum`提供者来实例化与区块链的连接。

![](img/7527852448b62b40f94952190cd9680e_109.png)

![](img/7527852448b62b40f94952190cd9680e_111.png)

![](img/7527852448b62b40f94952190cd9680e_113.png)

![](img/7527852448b62b40f94952190cd9680e_114.png)

### 3. 获取并显示用户账户
接下来，我们要从MetaMask获取当前用户账户并显示在页面上。

![](img/7527852448b62b40f94952190cd9680e_116.png)

![](img/7527852448b62b40f94952190cd9680e_117.png)

![](img/7527852448b62b40f94952190cd9680e_119.png)

![](img/7527852448b62b40f94952190cd9680e_121.png)

![](img/7527852448b62b40f94952190cd9680e_123.png)

在`App.js`的`loadBlockchainData`函数中，我们添加以下代码：
```javascript
const accounts = await web3.eth.getAccounts();
this.setState({ account: accounts[0] });
```
这会将第一个账户地址保存到React组件的状态（state）中。然后，我们将这个状态作为属性（props）传递给`Navbar`组件，最终在导航栏中显示用户地址和一个由Identicon库生成的头像。

![](img/7527852448b62b40f94952190cd9680e_125.png)

![](img/7527852448b62b40f94952190cd9680e_127.png)

![](img/7527852448b62b40f94952190cd9680e_129.png)

### 4. 加载智能合约实例
为了在前端调用智能合约的函数，我们需要创建合约的JavaScript实例。这需要合约的ABI（应用二进制接口）和部署地址。

![](img/7527852448b62b40f94952190cd9680e_131.png)

![](img/7527852448b62b40f94952190cd9680e_133.png)

在`App.js`中，我们根据当前连接的网络ID，从Truffle构建的`DVideo.json`文件中获取ABI和对应的部署地址，然后使用Web3.js创建合约实例：
```javascript
const networkId = await web3.eth.net.getId();
const networkData = DVideo.networks[networkId];
if(networkData) {
    const dvideo = new web3.eth.Contract(DVideo.abi, networkData.address);
    this.setState({ dvideo });
    // ... 后续加载视频数据
} else {
    window.alert('智能合约未部署到当前网络。');
}
```
创建合约实例后，我们将其保存到组件状态中。

![](img/7527852448b62b40f94952190cd9680e_135.png)

### 5. 从区块链加载视频数据
有了合约实例，我们就可以调用其函数来获取数据。在`loadBlockchainData`函数中，我们获取视频总数，然后通过循环获取每个视频的详细信息，并存入状态。

![](img/7527852448b62b40f94952190cd9680e_137.png)

```javascript
const videoCount = await dvideo.methods.videoCount().call();
this.setState({ videoCount });

![](img/7527852448b62b40f94952190cd9680e_139.png)

![](img/7527852448b62b40f94952190cd9680e_141.png)

![](img/7527852448b62b40f94952190cd9680e_143.png)

![](img/7527852448b62b40f94952190cd9680e_145.png)

![](img/7527852448b62b40f94952190cd9680e_147.png)

// 加载视频
for (var i = videoCount; i >= 1; i--) {
    const video = await dvideo.methods.videos(i).call();
    this.setState({
        videos: [...this.state.videos, video]
    });
}
// 设置最新视频为当前播放视频
this.setState({
    currentHash: this.state.videos[0].hash,
    currentTitle: this.state.videos[0].title
});
```

![](img/7527852448b62b40f94952190cd9680e_149.png)

![](img/7527852448b62b40f94952190cd9680e_151.png)

![](img/7527852448b62b40f94952190cd9680e_153.png)

### 6. 实现视频上传功能
前端需要提供一个表单让用户上传视频。这是一个两步过程：
1.  将视频文件上传到IPFS，获取文件哈希。
2.  调用智能合约的`uploadVideo`函数，将IPFS哈希和标题保存到区块链。

![](img/7527852448b62b40f94952190cd9680e_154.png)

![](img/7527852448b62b40f94952190cd9680e_156.png)

**步骤一：处理文件并上传至IPFS**
在`Main.js`组件中，我们创建一个表单，包含文件输入框和标题输入框。当用户选择文件时，`captureFile`函数会将文件转换为Buffer格式，为上传到IPFS做准备。表单提交时，`uploadVideo`函数被调用。

![](img/7527852448b62b40f94952190cd9680e_158.png)

![](img/7527852448b62b40f94952190cd9680e_160.png)

在`App.js`中，我们定义了`uploadVideo`函数的核心逻辑：
```javascript
// 1. 上传到IPFS
ipfs.add(this.state.buffer, (error, result) => {
    if(error) {
        console.error(error);
        return;
    }
    // 2. 保存到区块链
    this.state.dvideo.methods.uploadVideo(result[0].hash, _title)
    .send({ from: this.state.account })
    .on('transactionHash', (hash) => {
        this.setState({ loading: false });
    });
});
```
这里使用了之前配置的IPFS客户端。上传成功后，我们调用智能合约的`uploadVideo`方法，使用`.send()`发送一个交易，并指定发送者账户。交易被确认后，将加载状态设为false。

![](img/7527852448b62b40f94952190cd9680e_162.png)

![](img/7527852448b62b40f94952190cd9680e_164.png)

![](img/7527852448b62b40f94952190cd9680e_166.png)

![](img/7527852448b62b40f94952190cd9680e_168.png)

![](img/7527852448b62b40f94952190cd9680e_170.png)

![](img/7527852448b62b40f94952190cd9680e_172.png)

![](img/7527852448b62b40f94952190cd9680e_174.png)

![](img/7527852448b62b40f94952190cd9680e_176.png)

### 7. 显示视频列表与播放器
最后，我们需要在页面上显示视频播放器和侧边栏的视频列表。

![](img/7527852448b62b40f94952190cd9680e_178.png)

![](img/7527852448b62b40f94952190cd9680e_179.png)

![](img/7527852448b62b40f94952190cd9680e_181.png)

![](img/7527852448b62b40f94952190cd9680e_183.png)

![](img/7527852448b62b40f94952190cd9680e_185.png)

![](img/7527852448b62b40f94952190cd9680e_187.png)

![](img/7527852448b62b40f94952190cd9680e_189.png)

*   **播放器**：我们将状态中的`currentHash`和`currentTitle`传递给`Main`组件。在`Main.js`中，使用HTML5的`<video>`标签，其`src`属性指向IPFS网关URL加上当前视频的哈希（例如：`https://ipfs.io/ipfs/${currentHash}`）。
*   **视频列表**：我们将状态中的`videos`数组传递给`Main`组件。在`Main.js`中，使用`.map()`方法遍历视频数组，为每个视频在侧边栏生成一个预览项。点击某个视频项时，会调用一个函数来更新状态中的`currentHash`和`currentTitle`，从而切换播放器中的视频。

![](img/7527852448b62b40f94952190cd9680e_191.png)

![](img/7527852448b62b40f94952190cd9680e_193.png)

![](img/7527852448b62b40f94952190cd9680e_195.png)

![](img/7527852448b62b40f94952190cd9680e_197.png)

![](img/7527852448b62b40f94952190cd9680e_199.png)

![](img/7527852448b62b40f94952190cd9680e_201.png)

![](img/7527852448b62b40f94952190cd9680e_203.png)

## 总结
本节课中，我们一起学习并完成了一个基于区块链的去中心化视频分享网站（YouTube复刻版）的构建。我们涵盖了以下核心内容：
1.  **项目架构**：理解了中心化与去中心化应用的区别，以及如何利用以太坊智能合约和IPFS构建去中心化应用。
2.  **智能合约开发**：使用Solidity语言编写了`DVideo`合约，实现了定义数据结构、存储映射、上传视频和事件触发等功能。
3.  **前端开发**：使用React框架构建了用户界面，并通过Web3.js和MetaMask实现了前端与以太坊区块链的交互。
4.  **文件存储**：集成IPFS，实现了视频文件去中心化存储与检索。
5.  **完整流程**：实现了从用户上传视频（到IPFS并记录哈希至区块链）到在页面列表展示和播放视频的完整闭环。

![](img/7527852448b62b40f94952190cd9680e_205.png)

通过这个项目，你创建了一个抗审查的视频平台，因为视频内容存储在IPFS，交易记录在不可篡改的区块链上。这为你进一步探索Web3和去中心化应用开发奠定了坚实的基础。