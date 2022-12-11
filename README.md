Blockchain Application - Todo List

Simple todo list powered by smart contracts. Understand how blockchain works and how to connect an application with a decentralized platform. Unlike traditionally todo list applications, there is no central database where data is located. The data, todo list items, are stored on a network distributed over the blockchain.

**Technology:**
Solidity - High-level language for implementing Smart Contracts
Truffle Framework - Ethereum DApps
Ganache - Personal Ethereum blockchain on your local machine
Metamask Ethereum Wallet - Chrome Extension
web3.js - Library for interacting with Ethereum blockchain
Mocha - Testing Framework
Chai - Assertion Library
Node.js
Deployment


# Install dependencies 
$ npm install -g truffle@5.0.2
$ npm install

# Migrate 
truffle migrate --reset

# Run the app
$ npm run dev
Listing Tasks
Modeling task with struct and mapping state variable tasks. Allowing for look up on any task by id.

// TodoList.sol
pragma solidity ^0.5.0;

contract TodoList {
  uint public taskCount = 0;

  struct Task {
    uint id;
    string content;
    bool completed;
  }

  mapping(uint => Task) public tasks;
}
Creating Tasks
createTask function accepts one argument, text for the task, and stores the new task on the blockchain by adding it to tasks. We want to trigger an event any time a new task is created. Solidity allows for the listening of these events inside the client-side application. TaskCreated() is triggered anytime a new task is created in createTask().

// TodoList.sol
pragma solidity ^0.5.0;

contract TodoList {

  // ...

  event TaskCreated(
    uint id,
    string content,
    bool completed
  );

  // ...

  function createTask(string memory _content) public {
    taskCount ++;
    tasks[taskCount] = Task(taskCount, _content, false);
    emit TaskCreated(taskCount, _content, false);
  }
}
new_task

Completing Tasks
Checking off tasks on the todo list will update the smart contract.

// TodoList.sol
pragma solidity ^0.5.0;

contract TodoList {

  // ...

  event TaskCompleted(
    uint id,
    bool completed
  );

  // ...

  function toggleCompleted(uint _id) public {
    Task memory _task = tasks[_id];
    _task.completed = !_task.completed;
    tasks[_id] = _task;
    emit TaskCompleted(_id, _task.completed);
  }
}
complete_task

Testing
# Run Test 
$ truffle test 
// TodoList.test.js
it ('toggles tasks completion', async () => {
  const result = await this.todoList.toggleCompleted(1)
  const task = await this.todoList.tasks(1)
  assert.equal(task.completed, true)
  const event = result.logs[0].args 
  assert.equal(event.id.toNumber(), 1)
  assert.equal(event.completed, true )
})



Ganache Personal Blockchain
Local development blockchain used to mimic the behavior of a public blockchain. Allows for deploying smart contracts, develop applications, and run tests.

https://drive.google.com/drive/u/0/folders/16ihvmFmgwUUGus8pRUEBW9qMsND6RTyo

Metamask
Google Chrome extension turning your browser into a blockchain browser. Metamask allows for managing our personal account when connecting to the blockchain, as well as manage ETH funds needed to pay for transactions.

https://drive.google.com/file/d/1Mx0j4RyD0ckBmivWy-OmoA7duRLnaI70/view?usp=share_link

Future
Upgrade User interface
