# 004：Web3 - 用区块链编写DROPBOX复刻版

![](img/42765643d30b926b61467dd8d1f5adec_1.png)

在本节课中，我们将学习如何使用以太坊区块链和星际文件系统（IPFS）构建一个去中心化的文件存储网站，类似于Dropbox。我们将从零开始，逐步创建智能合约、部署到区块链，并构建一个完整的React前端应用。

## 概述

![](img/42765643d30b926b61467dd8d1f5adec_3.png)

![](img/42765643d30b926b61467dd8d1f5adec_4.png)

![](img/42765643d30b926b61467dd8d1f5adec_5.png)

![](img/42765643d30b926b61467dd8d1f5adec_6.png)

我们将构建一个名为“DStorage”的去中心化文件存储应用。用户可以通过网页界面上传文件，文件将被存储在IPFS上，而文件的元数据（如位置哈希）则通过以太坊智能合约记录在区块链上。这种方式确保了文件的抗审查性和长期存储。

![](img/42765643d30b926b61467dd8d1f5adec_8.png)

## 项目架构与依赖安装

上一节我们介绍了项目的整体目标，本节中我们来看看实现这个目标所需的技术栈和开发环境。

![](img/42765643d30b926b61467dd8d1f5adec_10.png)

![](img/42765643d30b926b61467dd8d1f5adec_12.png)

以下是构建项目前需要安装的核心依赖：

1.  **Node.js**：用于运行JavaScript环境和安装项目包。
    *   检查安装：在终端输入 `node -v`。
    *   建议版本：v10.16.0或更高。
2.  **Truffle框架**：用于开发、测试和部署以太坊智能合约。
    *   安装命令：`npm install -g truffle@5.1.3`
3.  **Ganache**：一个本地以太坊区块链，用于开发和测试，无需消耗真实加密货币。
    *   从官网下载并安装对应操作系统的版本。
4.  **MetaMask**：浏览器扩展钱包，将普通浏览器变为区块链浏览器。
    *   在Chrome网上应用店搜索并安装。

安装完成后，运行Ganache，你会看到10个预充值了100个测试以太币的账户。同时，在浏览器中设置MetaMask，连接到Ganache的RPC服务器（通常是 `http://127.0.0.1:7545`），并导入一个Ganache的账户私钥。

![](img/42765643d30b926b61467dd8d1f5adec_14.png)

## 初始化项目与后端智能合约开发

![](img/42765643d30b926b61467dd8d1f5adec_16.png)

环境配置好后，我们开始初始化项目并编写核心的智能合约。

首先，克隆项目启动代码并安装依赖：

![](img/42765643d30b926b61467dd8d1f5adec_18.png)

```bash
git clone -b starter-code https://github.com/dappuniversity/dstorage
cd dstorage
npm install
```

![](img/42765643d30b926b61467dd8d1f5adec_20.png)

![](img/42765643d30b926b61467dd8d1f5adec_22.png)

![](img/42765643d30b926b61467dd8d1f5adec_24.png)

项目结构中的 `contracts/` 目录用于存放Solidity智能合约。我们将在此创建 `DStorage.sol` 文件。

智能合约的核心功能是充当一个去中心化的数据库，记录上传到IPFS的文件的哈希值。我们使用 `mapping` 数据结构来建立文件ID和文件详情之间的映射。

```solidity
// 定义文件结构体
struct File {
    uint fileId;
    string fileHash;
    uint fileSize;
    string fileType;
    string fileName;
    string fileDescription;
    uint uploadTime;
    address payable uploader;
}

// 创建映射：文件ID => 文件详情
mapping(uint => File) public files;
```

接下来，我们编写 `uploadFile` 函数，用于处理文件上传逻辑。该函数接收文件哈希、大小、类型、名称和描述作为参数，并执行以下操作：
1.  使用 `require` 语句进行输入验证。
2.  递增文件计数器 `fileCount` 以生成新文件ID。
3.  使用传入的参数和全局变量 `msg.sender`（上传者地址）、`now`（当前时间戳）创建一个新的 `File` 结构体实例。
4.  将新文件存入 `files` 映射。
5.  触发一个 `FileUploaded` 事件，以便前端应用监听。

```solidity
event FileUploaded(
    uint fileId,
    string fileHash,
    uint fileSize,
    string fileType,
    string fileName,
    string fileDescription,
    uint uploadTime,
    address payable uploader
);

function uploadFile(string memory _fileHash, uint _fileSize, string memory _fileType, string memory _fileName, string memory _fileDescription) public {
    // 输入验证
    require(bytes(_fileHash).length > 0);
    require(bytes(_fileType).length > 0);
    require(bytes(_fileDescription).length > 0);
    require(bytes(_fileName).length > 0);
    require(msg.sender != address(0));
    require(_fileSize > 0);

    // 增加文件ID
    fileCount ++;

    // 添加文件到映射
    files[fileCount] = File(fileCount, _fileHash, _fileSize, _fileType, _fileName, _fileDescription, now, msg.sender);

    // 触发事件
    emit FileUploaded(fileCount, _fileHash, _fileSize, _fileType, _fileName, _fileDescription, now, msg.sender);
}
```

编写完成后，使用Truffle编译并部署合约到Ganache网络：

```bash
truffle migrate --reset
```

你可以使用 `truffle console` 进入控制台与合约进行交互测试。

## 前端应用开发与区块链连接

![](img/42765643d30b926b61467dd8d1f5adec_26.png)

智能合约部署成功后，我们开始构建与之交互的React前端应用。

![](img/42765643d30b926b61467dd8d1f5adec_28.png)

![](img/42765643d30b926b61467dd8d1f5adec_30.png)

首先，确保开发服务器运行：`npm run start`。应用的核心是 `src/components/App.js` 文件。

![](img/42765643d30b926b61467dd8d1f5adec_32.png)

![](img/42765643d30b926b61467dd8d1f5adec_34.png)

前端需要完成两件事：1. 连接浏览器到区块链（通过MetaMask）；2. 连接React应用到区块链（通过Web3.js库）。

![](img/42765643d30b926b61467dd8d1f5adec_36.png)

![](img/42765643d30b926b61467dd8d1f5adec_38.png)

![](img/42765643d30b926b61467dd8d1f5adec_40.png)

![](img/42765643d30b926b61467dd8d1f5adec_42.png)

![](img/42765643d30b926b61467dd8d1f5adec_43.png)

我们在 `loadWeb3` 函数中初始化Web3连接，并在 `loadBlockchainData` 函数中加载区块链数据：
1.  获取当前MetaMask账户。
2.  获取已部署的 `DStorage` 合约实例（需要合约ABI和地址）。
3.  从合约中读取文件总数和所有文件详情，并存入React的 `state` 中。

![](img/42765643d30b926b61467dd8d1f5adec_45.png)

```javascript
async loadBlockchainData() {
    const web3 = window.web3;
    const accounts = await web3.eth.getAccounts();
    this.setState({ account: accounts[0] });

    const networkId = await web3.eth.net.getId();
    const networkData = DStorage.networks[networkId];

    if(networkData) {
        const dstorage = new web3.eth.Contract(DStorage.abi, networkData.address);
        this.setState({ dstorage });
        const filesCount = await dstorage.methods.fileCount().call();
        this.setState({ filesCount });

        // 加载文件数据...
    } else {
        window.alert('智能合约未部署到当前网络。');
    }
}
```

![](img/42765643d30b926b61467dd8d1f5adec_47.png)

![](img/42765643d30b926b61467dd8d1f5adec_49.png)

账户信息通过 `props` 传递给 `Navbar` 组件显示在页面上。

![](img/42765643d30b926b61467dd8d1f5adec_51.png)

![](img/42765643d30b926b61467dd8d1f5adec_53.png)

![](img/42765643d30b926b61467dd8d1f5adec_55.png)

![](img/42765643d30b926b61467dd8d1f5adec_57.png)

![](img/42765643d30b926b61467dd8d1f5adec_59.png)

## 集成IPFS与文件上传功能

![](img/42765643d30b926b61467dd8d1f5adec_61.png)

![](img/42765643d30b926b61467dd8d1f5adec_63.png)

![](img/42765643d30b926b61467dd8d1f5adec_65.png)

现在，我们将IPFS集成到应用中，实现完整的文件上传流程。

![](img/42765643d30b926b61467dd8d1f5adec_67.png)

![](img/42765643d30b926b61467dd8d1f5adec_69.png)

![](img/42765643d30b926b61467dd8d1f5adec_71.png)

![](img/42765643d30b926b61467dd8d1f5adec_73.png)

![](img/42765643d30b926b61467dd8d1f5adec_75.png)

![](img/42765643d30b926b61467dd8d1f5adec_77.png)

首先，在应用中初始化IPFS客户端（这里使用Infura的公共网关）：

![](img/42765643d30b926b61467dd8d1f5adec_79.png)

![](img/42765643d30b926b61467dd8d1f5adec_80.png)

![](img/42765643d30b926b61467dd8d1f5adec_81.png)

```javascript
const ipfs = new IPFS({ host: 'ipfs.infura.io', port: 5001, protocol: 'https' });
```

![](img/42765643d30b926b61467dd8d1f5adec_82.png)

文件上传分为三个步骤，在 `Main` 组件中实现：

![](img/42765643d30b926b61467dd8d1f5adec_84.png)

![](img/42765643d30b926b61467dd8d1f5adec_85.png)

![](img/42765643d30b926b61467dd8d1f5adec_87.png)

![](img/42765643d30b926b61467dd8d1f5adec_89.png)

![](img/42765643d30b926b61467dd8d1f5adec_91.png)

![](img/42765643d30b926b61467dd8d1f5adec_93.png)

1.  **准备文件**：通过 `captureFile` 函数，使用JavaScript的 `FileReader` API将用户选择的文件转换为Buffer格式，以便IPFS处理。
2.  **上传至IPFS**：在表单提交时，调用IPFS客户端的 `add` 方法上传Buffer数据。成功后会返回一个唯一的文件内容哈希（CID）。
3.  **存储到区块链**：获得IPFS哈希后，调用智能合约的 `uploadFile` 函数，将该哈希、文件大小、类型、名称和描述等信息发送到区块链上。这需要用户通过MetaMask确认并支付一笔极小的Gas费用。

![](img/42765643d30b926b61467dd8d1f5adec_95.png)

![](img/42765643d30b926b61467dd8d1f5adec_97.png)

![](img/42765643d30b926b61467dd8d1f5adec_99.png)

![](img/42765643d30b926b61467dd8d1f5adec_100.png)

```javascript
// 步骤2和3的示例代码
const result = await this.state.ipfs.add(this.state.buffer);
const fileHash = result[0].hash;

![](img/42765643d30b926b61467dd8d1f5adec_102.png)

![](img/42765643d30b926b61467dd8d1f5adec_104.png)

![](img/42765643d30b926b61467dd8d1f5adec_106.png)

this.setState({ loading: true });

![](img/42765643d30b926b61467dd8d1f5adec_108.png)

// 调用智能合约
this.state.dstorage.methods.uploadFile(fileHash, fileSize, fileType, fileName, fileDescription).send({ from: this.state.account })
.on('transactionHash', (hash) => {
    this.setState({ loading: false });
});
```

![](img/42765643d30b926b61467dd8d1f5adec_110.png)

![](img/42765643d30b926b61467dd8d1f5adec_112.png)

![](img/42765643d30b926b61467dd8d1f5adec_114.png)

![](img/42765643d30b926b61467dd8d1f5adec_116.png)

## 文件列表展示与项目完成

![](img/42765643d30b926b61467dd8d1f5adec_118.png)

![](img/42765643d30b926b61467dd8d1f5adec_119.png)

![](img/42765643d30b926b61467dd8d1f5adec_121.png)

![](img/42765643d30b926b61467dd8d1f5adec_123.png)

最后一步是在前端页面上展示所有已上传的文件。

![](img/42765643d30b926b61467dd8d1f5adec_125.png)

我们已经在 `loadBlockchainData` 中从合约获取了所有文件数据并存入 `state`。现在，在 `Main` 组件中，我们通过一个表格来遍历 `this.props.files` 并展示每个文件的详细信息，包括ID、名称、描述、类型、大小、上传时间、上传者以及一个可点击的IPFS哈希链接。

![](img/42765643d30b926b61467dd8d1f5adec_127.png)

![](img/42765643d30b926b61467dd8d1f5adec_129.png)

```jsx
{this.props.files.map((file, key) => {
    return(
        <tr key={key}>
            <td>{file.fileId}</td>
            <td>{file.fileName}</td>
            <td>{file.fileDescription}</td>
            <td>{file.fileType}</td>
            <td>{file.fileSize}</td>
            <td>{file.uploadTime}</td>
            <td>{file.uploader}</td>
            <td><a href={`https://ipfs.infura.io/ipfs/${file.fileHash}`} target="_blank">{file.fileHash}</a></td>
        </tr>
    )
})}
```

至此，一个完整的去中心化文件存储应用就构建完成了。用户可以上传文件，文件被永久存储在IPFS网络上，而其存证则不可篡改地记录在以太坊区块链上。

![](img/42765643d30b926b61467dd8d1f5adec_131.png)

![](img/42765643d30b926b61467dd8d1f5adec_133.png)

## 总结

![](img/42765643d30b926b61467dd8d1f5adec_135.png)

![](img/42765643d30b926b61467dd8d1f5adec_137.png)

本节课中我们一起学习了如何构建一个基于Web3技术的去中心化Dropbox复刻版。我们涵盖了以下核心内容：
*   使用 **Solidity** 编写智能合约，作为去中心化数据库。
*   使用 **Truffle** 框架编译和部署合约到 **Ganache** 本地区块链。
*   使用 **React** 构建前端用户界面。
*   使用 **Web3.js** 库连接前端与以太坊区块链。
*   使用 **MetaMask** 处理用户账户和交易签名。
*   集成 **IPFS** 用于去中心化文件存储。
*   实现了完整的文件上传、存储和列表展示流程。

![](img/42765643d30b926b61467dd8d1f5adec_139.png)

通过这个项目，你掌握了构建一个完整DApp（去中心化应用）的核心技能组合。你可以在此基础上继续扩展功能，例如添加文件访问权限控制、付费下载或社交分享等特性。