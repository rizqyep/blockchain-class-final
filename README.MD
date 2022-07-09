
# Blockchain Case Study - Todo List Dapp

Project ini dibuat untuk keperluan edukasi (Educational Purpose) pada Mata Kuliah Blockchain, S1 Teknik Komputer, Fakultas Teknik Elektro, Telkom University.

Anggota Tim:

- Rizqy Eka Putra Rizaldy (1103184149)
- Aditya Rohman (1103184090)


## Overview

Dapp adalah sebuah aplikasi yang berjalan pada peer-to-peer network atau blockchain. Pada case study ini, aplikasi yang dikembangkan adalah sebuah Todo List sederhana dengan blockchain.


| Web Traditional | Web3 |
| :---: | :---: |
| ![](https://raw.githubusercontent.com/codestronaut/blockchain-class-final/master/screenshots/web.png) | ![](https://raw.githubusercontent.com/codestronaut/blockchain-class-final/master/screenshots/web3.png) |

![Preview](https://raw.githubusercontent.com/codestronaut/blockchain-class-final/master/screenshots/dapp_preview.png)

## Dependencies

Dalam pengembangan project ini digunakan beberapa tools atau dependencies yaitu:

- Ganache (https://trufflesuite.com/ganache)
    
    Ganache adalah sebuah blockchain network yang berjalan pada server lokal (localhost) pada sebuah komputer. Ganache dimanfaatkan untuk proses pengembangan dan pengujian aplikasi blockchain.
    
- Truffle (https://trufflesuite.com/truffle)

    Truffle digunakan untuk mengembangkan dan menguji Ethereum Smart Contract dengan Solidity sebagai bahasa pemrogramannya. Smart Contract adalah program yang ada dalam sebuah blockchain. Pada smart contract kode business-logic diletakkan.

- Metamask Browser Extension (https://metamask.io)

    Digunakan untuk terhubung dengan blockchain network dan berinteraksi dengan smart contract yang dikembangkan.
    
- Bootstrap 4, jQuery, Web3.js, Lite-Server for Hot Reloading and Bundler

    Digunakan untuk membuat antarmuka pengguna untuk berinteraksi dengan Todo List smart contract

## TodoList Smart Contract

Berikut adalah kode backend untuk TodoList decentralized application (dapp), nantinya smart contract ini akan dideploy ke blockchain network dan berinteraksi dengan Front-End.

```solidity
pragma solidity ^0.5.0;

contract TodoList {
	/* State Variable: untuk menyimpan progress TodoList
	Dideklarasikan dengan uint256 (unsigned integer)
	Karena taskCount tidak akan bernilai negatif */
    uint256 public taskCount = 0;
		
	// Model
	// Untuk memudahkan menyimpan data dari task
    struct Task {
        uint256 id;
        string content;
        bool completed;
    }
		
    mapping(uint256 => Task) public tasks;
		
	/* Constructor akan dieksekusi ketika 
	smart contract pertama kali dijalankan */
    constructor() public {
        createTask("Init new task added");
    }
		
	// Fungsi untuk membuat task baru
    event TaskCreated(uint256 id, string content, bool completed);
		function createTask(string memory _content) public {
        taskCount++;
        tasks[taskCount] = Task(taskCount, _content, false);
        emit TaskCreated(taskCount, _content, false);
    }
		
	// Fungsi untuk menyelesasikan sebuah task
    event TaskCompleted(uint256 id, bool completed);
    function toggleCompleted(uint256 _id) public {
        Task memory _task = tasks[_id];
        _task.completed = !_task.completed;
        tasks[_id] = _task;
        emit TaskCompleted(_id, _task.completed);
    }
}
```

## Compile & Test Smart Contract

Berikut adalah perintah untuk meng-compile smart contract:

```bash
truffle compile

-> output:
Compiling your contracts...
===========================
> Compiling ./contracts/Migrations.sol
> Compiling ./contracts/TodoList.sol
> Artifacts written to /Users/aditya/Desktop/blockchain-class-final/build/contracts
```

Setelah berhasil dicompile, selanjutnya adalah untuk mencoba smart contract yang telah dibuat. Untuk mencoba berinteraksi dengan smart contract pada local network, pastikan Ganache sudah berjalan. Berikut adalah perintah untuk deploy smart contract:

```bash
truffle migrate --reset
```

Flag `—reset` pada perintah di atas dapat digunakan untuk mendeploy perubahan pada smart contract. Perlu diingat, untuk mendeploy smart contract ke blockchain network dibutuhkan sejumlah biaya yang dibayarkan dengan Ethereum. Biaya deploy ini dikenal dengan istilah Gas fee.

Tapi karena masih dalam proses development, Ganache dapat dimanfaatkan sebagai blockchain network virtual untuk mendeploy dan testing smart contract tanpa dikenakan Gas fee.

Untuk melihat apakah smart contract sudah deployed pada local blockchain network dapat dilakukan pada truffle console seperti berikut:

![Smart Contract Check](https://raw.githubusercontent.com/codestronaut/blockchain-class-final/master/screenshots/smart_contract_check.png)

![Ganache Status](https://raw.githubusercontent.com/codestronaut/blockchain-class-final/master/screenshots/ganache_deploy_status.png)

Berikut adalah hasil dari mencoba fungsi untuk mendapatkan task pada TodoList contract. Terdapat 1 task di sini karena pada smart contract terdapat constructor yang didalamnya memanggil fungsi untuk membuat sebuah task baru.

![Smart Contract Test](https://raw.githubusercontent.com/codestronaut/blockchain-class-final/master/screenshots/smart_contract_test.png)


## How to Run

- Install necessary dependencies:

  > npm install

- Compile smart contract :

  > truffle compile

- Deploy Smart contract:

  > truffle migrate (add --reset flags to reset on each deployment)

### How to run unit test :

> truffle test

<b>Important notes: make sure you connected to meta mask to the correct network and account as displayed on your ganache (account can be imported by acquiring the private key on ganache) </b>
