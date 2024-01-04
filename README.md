## Foundry FundMe - My Notes

### Setup

- Create a new directory within a folder using 'mkdir'. Utilize 'ls' to view folders in the directory.
- Navigate back a folder using 'cd ..'.
- Employ the tab key to autocomplete folder names when using 'cd'.
- Initialize Forge using 'forge init'.
- Complete your contracts.
- Obtain Chainlink contracts via 'forge install smartcontractkit/chainlink-brownie-contracts@0.8.0 —no-commit'. The version can be found on GitHub under releases.
- Note: Chainlink contracts may point to the wrong folder; either adjust this or create a remapping in 'foundry.toml'.
  - Example remapping: remappings = ["@chainlink/contracts/=lib/chainlink-brownie-contracts/contracts/"]
- Compile with 'forge build' or 'forge compile'.

### Testing

- Deployment Process:
  - Normally write deploy script 1st.
- Testing:
  - Use 'FundMeTest.t.sol' for testing.
  - Include a 'set up' function for test preparation.
- Running Tests:
  - Execute a general test using 'forge test -vvvvv' (v specifies console.logging visibility).
  - Run a specific test with 'forge test --match-test testPriceFeedVersionIsAccurate'.
  - In Foundry, tests spin up an Anvil chain and delete it after completion.
- Test Configuration:
  - Address errors like pointing to a non-existent SEPOLIA etc/USD price feed during the 'testPriceFeedVersionIsAccurate' test.
  - Use a fork URL for simulation; paste SEPOLIA_RPC_URL from Alchemy into a .env file.
- Environment Setup:
  - Execute 'source .env' and 'echo $SEPOLIA_RPC_URL' in the command line to verify the URL.
  - Run tests with 'forge test --match-test testPriceFeedVersionIsAccurate -vvv —fork-url $SEPOLIA_RPC_URL' to simulate transactions on the SEPOLIA chain.
  - This will spin up an anvil chain but simulate all transactions as if they are running on the sepolia chain
- Additional Testing Notes:
  - Check Alchemy node information.
  - Fork URL simulates testing on the actual chain.
  - Be cautious about API calls to Alchemy as it can accumulate costs. Want to write s many tests as you can without forking. Some tests can only be done using a fork or using mocking.
- Coverage Testing:
  - Assess coverage with 'forge coverage --fork-url $SEPOLIA_RPC_URL'.
- Importing Deployment Script:
  - Import the deploy script into the test script for setup.
- Mock Tests:
  - Create mock tests to simulate interactions with the Chainlink price feed contract.
- Final Testing:
  - Run tests with 'forge test --fork-url $MAINNET_RPC_URL'.
  - Alternatively, use 'forge test --fork-url $SEPOLIA_RPC_URL'.
- Integration test:
  - forge install Cyfrin/foundry-devops --no-commit (helps you work with most recent deployment)
  - Set ‘fyi = true’ in foundry.toml. (usually better off, but using to get most recent deployment in integration test)

### Deployment

- Deployment Script:
  - Utilize the deployment script named Script > 'DeployFundMe.s.sol'.
  - Execute the deployment script using 'forge script script/DeployFundMe.s.sol'.
- Modular Deployment:
  - Aim to deploy contracts in a modular way with external systems, avoiding hardcoding addresses etc.
- Helper Configuration:
  - Create a 'helperconfig' to eliminate the need for hardcoding addresses.
  - This configuration should seamlessly work on the local chain, forked chain, or live chain.
- Make File and Shortcuts:
  - Refer to the Make file for command shortcuts.
  - Explore the deploy script for verification purposes.
- Deployment Command:
  - The final deployment command involves using the 'make deploy ARGS="--network sepolia”' command.

### Gas Optimization

- Gas snapshot:
  - Conduct snapshot using 'forge snapshot —match-test testWithDrawFromMultipleFunders'.
  - Check the '.gas-snapshot' folder for gas consumption details.
- Storage Variables:
  - Storage variables are added to a storage spot, impacting gas usage.
- Constant and Immutable Variables:
  - Constant and immutable variables are part of the byte code, incurring less gas consumption.
- String Handling:
  - Strings require the 'memory' keyword, as they are treated as arrays. Solidity needs clarification on whether it's dealing with storage or memory to optimize space usage.
- Storage Layout Inspection:
  - Use 'forge inspect FundMe storageLayout' to obtain the exact storage layout of the contract.
- Viewing Storage Slots:
  - Alternatively, view storage slots by executing the following commands:
    - forge script script/DeployFundMe.s.sol --rpc-url $SEPOLIA_RPC_URL --private-key insert private key --broadcast
    - cast storage “paste contract address” 2
    - Choose storage slot numbers (e.g., 0, 1, 2, 3) to inspect the stored information.
- Public Nature of Blockchain:
  - Everything on the blockchain is public, even when using the private keyword.
- Gas Costs of Op Codes:
  - Visit www.evm.codes to explore the different gas costs associated with various opcodes.
- Snapshot Comparison:
  - Utilize 'forge snapshot' to compare gas usage for different tests.

### Additional Notes

- Error Naming Convention:
  - Best practice recommends naming errors with the contract name followed by 2 underscores, e.g., error FundeMe\_\_NotOwner();.
- Code Refactoring:
  - "Refactoring" involves changing the architecture of the code without altering its functionality. For instance, deploy contracts in a modular way with external systems, avoiding hardcoding addresses. This approach enhances code maintainability.
- Account List Creation:
  - To create a list of accounts, type 'anvil'.
- Testing Code in Terminal:
  - Type 'chisel' in the command line to test code directly in the terminal. Use 'Ctrl + C' and 'Ctrl + C' to exit.
- Command Palette and File Viewer:
  - Use 'Command + Shift + P' to open the command palette. Remove '>' to access the file viewer.

## Foundry

**Foundry is a blazing fast, portable and modular toolkit for Ethereum application development written in Rust.**

Foundry consists of:

- **Forge**: Ethereum testing framework (like Truffle, Hardhat and DappTools).
- **Cast**: Swiss army knife for interacting with EVM smart contracts, sending transactions and getting chain data.
- **Anvil**: Local Ethereum node, akin to Ganache, Hardhat Network.
- **Chisel**: Fast, utilitarian, and verbose solidity REPL.

## Documentation

https://book.getfoundry.sh/

## Usage

### Build

```shell
$ forge build
```

### Test

```shell
$ forge test
```

### Format

```shell
$ forge fmt
```

### Gas Snapshots

```shell
$ forge snapshot
```

### Anvil

```shell
$ anvil
```

### Deploy

```shell
$ forge script script/Counter.s.sol:CounterScript --rpc-url <your_rpc_url> --private-key <your_private_key>
```

### Cast

```shell
$ cast <subcommand>
```

### Help

```shell
$ forge --help
$ anvil --help
$ cast --help
```
# foundry-fund-me
