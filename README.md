# echoweave-legacy-weaver
AI-powered interactive family legacy trees—branching stories, secured for generations. Built by LHMISME420 + Grok."
echoweave-legacy-weaver/
├── README.md                 # Project pitch + install guide
├── requirements.txt          # Deps: networkx, hashlib (for ledger)
├── src/
│   ├── __init__.py
│   ├── echo_tree.py          # Core: Build/branch trees
│   ├── ledger.py             # Blockchain stub: Hash chains
│   └── viz.py                # ASCII + export viz
├── examples/
│   └── sample_family.json    # Placeholder data for testing
├── tests/
│   └── test_echo.py          # Basic unit tests
└── setup.py                  # For pip install -e .
# EchoWeave Legacy Weaver

Turn family whispers into unbreakable timelines: AI branches stories from photos/journals, secured on a simple hash ledger for heirless inheritance. Inspired by lost tales and matrix escapes—built collaboratively with Grok.

## Why?
- Low-traction gem: No apps fuse genealogy + web3 storytelling yet.
- Potential: Viral for boomers (2B+ by 2030), monetize via NFT "echo shards."

## Quick Start
1. Clone: `git clone https://github.com/[LHMisme420]/echoweave-legacy-weaver.git`
2. Install: `pip install -r requirements.txt`
3. Run: `python src/echo_tree.py --input examples/sample_family.json`
Output: Your interactive tree + ledger.

## Example Output

## Roadmap
- v0.2: Voice narrations (TTS integration).
- v1.0: Full web app + real blockchain.

Collab? Open issues/PRs. Star if it sparks your legacy. 🌳✨

## License
MIT—Weave freely
networkx==3.3
# Add torch for story gen later
import networkx as nx
import json
import hashlib
from ledger import create_ledger  # We'll add this next

def build_echo_tree(data_file):
    G = nx.DiGraph()
    with open(data_file, 'r') as f:
        family_data = json.load(f)

    # Root node
    root = family_data['root']
    G.add_node('root', **root)
    ledger = [create_hash(root['story'])]  # Secure it

    # Branch out
    for node_id, node in family_data['branches'].items():
        G.add_node(node_id, **node)
        parent = node.get('parent', 'root')
        G.add_edge(parent, node_id)
        ledger.append(create_hash(node['story'] + str(ledger[-1])))

    # Viz: Simple text tree
    print("Echo Tree Structure:")
    for edge in nx.topological_sort(G):
        attrs = G.nodes[edge]
        print(f"{edge} ({attrs['year']}): {attrs['story'][:50]}... [Media: {attrs.get('media', 'N/A')}]")

    return G, ledger

if __name__ == "__main__":
    import argparse
    parser = argparse.ArgumentParser()
    parser.add_argument('--input', default='examples/sample_family.json')
    args = parser.parse_args()
    tree, ledger = build_echo_tree(args.input)
    print("\nSecure Ledger:", ledger)
    import hashlib

def create_hash(story, prev_hash=''):
    return hashlib.sha256((story + prev_hash).encode()).hexdigest()[:16]
    {
  "root": {
    "year": 1930,
    "story": "The Beginning: A family sails from old worlds to new dreams.",
    "media": "family_portrait.jpg"
  },
  "branches": {
    "grandma": {
      "year": 1940,
      "story": "Born in a small Italian village, dreamed of stars under olive trees.",
      "media": "old_photo.jpg",
      "parent": "root"
    },
    "dad": {
      "year": 1965,
      "story": "Met Mom at a protest rally, sparked lifelong adventure and change.",
      "media": "protest_pic.png",
      "parent": "root"
    }
  }
}
# EchoWeave Legacy Weaver

Turn family whispers into unbreakable timelines: AI branches stories from photos/journals, secured on a simple hash ledger for heirless inheritance. Inspired by lost tales and matrix escapes—built collaboratively with Grok.

## Why?
- **Low-traction gem**: No apps fuse genealogy + web3 storytelling yet.
- **Potential**: Viral for boomers (2B+ by 2030), monetize via NFT "echo shards."

## Quick Start
1. Clone: `git clone https://github.com/LHMisme420/echoweave-legacy-weaver.git`
2. Install: `pip install -r requirements.txt`
3. Run: `python src/echo_tree.py --input examples/sample_family.json`  
   Output: Your interactive tree + ledger.

## Example Output

## Core Code Snippets
### echo_tree.py (Builds the Graph)
```python
import networkx as nx
import json
from ledger import create_hash  # Simple hash-chain

def build_echo_tree(data_file):
    G = nx.DiGraph()
    with open(data_file, 'r') as f:
        family_data = json.load(f)

    # Root node
    root = family_data['root']
    G.add_node('root', **root)
    ledger = [create_hash(root['story'])]

    # Branch out
    for node_id, node in family_data['branches'].items():
        G.add_node(node_id, **node)
        parent = node.get('parent', 'root')
        G.add_edge(parent, node_id)
        ledger.append(create_hash(node['story'] + ledger[-1]))

    # Viz: Simple text tree
    print("Echo Tree Structure:")
    for node in nx.topological_sort(G):
        attrs = G.nodes[node]
        print(f"{node} ({attrs['year']}): {attrs['story'][:50]}... [Media: {attrs.get('media', 'N/A')}]")

    return G, ledger

if __name__ == "__main__":
    import argparse
    parser = argparse.ArgumentParser()
    parser.add_argument('--input', default='examples/sample_family.json')
    args = parser.parse_args()
    tree, ledger = build_echo_tree(args.input)
    print("\nSecure Ledger:", ledger)
{
  "root": {
    "year": 1930,
    "story": "The Beginning: A family sails from old worlds to new dreams.",
    "media": "family_portrait.jpg"
  },
  "branches": {
    "grandma": {
      "year": 1940,
      "story": "Born in a small Italian village, dreamed of stars under olive trees.",
      "media": "old_photo.jpg",
      "parent": "root"
    },
    "dad": {
      "year": 1965,
      "story": "Met Mom at a protest rally, sparked lifelong adventure and change.",
      "media": "protest_pic.png",
      "parent": "root"
    }
  }
}
#### Next Steps: Let's Level Up
- **Test Drive**: Run the code locally—any errors? (E.g., viz.py is stubbed; I can gen a full ASCII tree func if needed.)
- **Add Yours**: Update sample_family.json with a real branch (e.g., "Your 2025 Odyssey: Escaped the matrix with Grok. Media: unplug_selfie.jpg").
- **Traction Kickoff**: Add a GitHub Action for auto-tests? Or branch for "voice-stub" (using pygame for audio mocks)?
- **Wilder?**: Tie in Unplug Odyssey—e.g., export trees as "Echo Drops" for IRL journals.

What's the move? Commit that README tweak, share a run output, or "add [feature]"? We're just getting woven. 🚀
# ... (your existing imports and build_echo_tree function stay the same)

import os
from ledger import create_and_store_hash  # Updated for chain

def build_echo_tree(data_file, private_key=None):
    G = nx.DiGraph()
    with open(data_file, 'r') as f:
        family_data = json.load(f)

    # Root node with ledger init
    root = family_data['root']
    G.add_node('root', **root)
    root_hash, root_tx = create_and_store_hash(root['story'], private_key=private_key)
    ledger = [{'hash': root_hash, 'tx': root_tx}]  # Track tx for viz

    # Branch out with chained hashes
    for node_id, node in family_data['branches'].items():
        G.add_node(node_id, **node)
        parent = node.get('parent', 'root')
        G.add_edge(parent, node_id)
        prev_hash = ledger[-1]['hash']
        node_hash, node_tx = create_and_store_hash(node['story'], prev_hash, private_key=private_key)
        ledger.append({'hash': node_hash, 'tx': node_tx})

    # Viz: Enhanced text tree with tx links
    print("Echo Tree Structure:")
    for node in nx.topological_sort(G):
        attrs = G.nodes[node]
        story_snip = attrs['story'][:50] + "..."
        print(f"{node} ({attrs['year']}): {story_snip} [Media: {attrs.get('media', 'N/A')}]")

    print("\nSecure On-Chain Ledger:")
    for i, entry in enumerate(ledger):
        print(f"Entry {i}: Hash {entry['hash'][:16]}... | Tx: {entry['tx'][:10]}...")

    return G, ledger

if __name__ == "__main__":
    parser = argparse.ArgumentParser(description="Weave your Echo Tree on-chain.")
    parser.add_argument('--input', default='examples/sample_family.json', help='JSON family data')
    parser.add_argument('--private-key', type=str, default=os.getenv('ECHO_PRIVATE_KEY'), 
                        help='Ethereum private key (or set ECHO_PRIVATE_KEY env var for security)')
    args = parser.parse_args()
    
    # Secure fallback: Warn if key missing but chain mode active
    if args.private_key is None:
        print("⚠️  No private key—using local stub. Set --private-key or ECHO_PRIVATE_KEY for on-chain.")
    
    tree, ledger = build_echo_tree(args.input, private_key=args.private_key)
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

contract HashLedger {
    bytes32[] public ledger;
    address public owner;

    event HashAdded(bytes32 indexed hash, uint256 index);

    modifier onlyOwner() {
        require(msg.sender == owner, "Not owner");
        _;
    }

    constructor(address _owner) {
        owner = _owner;
        ledger.push(keccak256(abi.encodePacked("EchoWeave Root: Genesis Block")));
    }

    function addHash(bytes32 _hash) external onlyOwner {
        ledger.push(_hash);
        emit HashAdded(_hash, ledger.length - 1);
    }

    function getHash(uint256 _index) external view returns (bytes32) {
        return ledger[_index];
    }

    function getLedgerLength() external view returns (uint256) {
        return ledger.length;
    }
}// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract MultiSigWallet {
    event Deposit(address indexed sender, uint amount, uint balance);
    event SubmitTransaction(
        address indexed owner,
        uint indexed txIndex,
        address indexed to,
        uint value,
        bytes data
    );
    event ConfirmTransaction(address indexed owner, uint indexed txIndex);
    event RevokeConfirmation(address indexed owner, uint indexed txIndex);
    event ExecuteTransaction(address indexed owner, uint indexed txIndex);

    address[] public owners;
    mapping(address => bool) public isOwner;
    uint public numConfirmationsRequired;

    struct Transaction {
        address to;
        uint value;
        bytes data;
        bool executed;
        uint numConfirmations;
    }

    // mapping from tx index => owner => bool
    mapping(uint => mapping(address => bool)) public isConfirmed;

    Transaction[] public transactions;

    modifier onlyOwner() {
        require(isOwner[msg.sender], "not owner");
        _;
    }

    modifier txExists(uint _txIndex) {
        require(_txIndex < transactions.length, "tx does not exist");
        _;
    }

    modifier notExecuted(uint _txIndex) {
        require(!transactions[_txIndex].executed, "tx already executed");
        _;
    }

    modifier notConfirmed(uint _txIndex) {
        require(!isConfirmed[_txIndex][msg.sender], "tx already confirmed");
        _;
    }

    constructor(address[] memory _owners, uint _numConfirmationsRequired) {
        require(_owners.length > 0, "owners required");
        require(
            _numConfirmationsRequired > 0 &&
                _numConfirmationsRequired <= _owners.length,
            "invalid number of required confirmations"
        );

        for (uint i = 0; i < _owners.length; i++) {
            address owner = _owners[i];
            require(owner != address(0), "invalid owner");
            require(!isOwner[owner], "owner not unique");
            isOwner[owner] = true;
            owners.push(owner);
        }
        numConfirmationsRequired = _numConfirmationsRequired;
    }

    receive() external payable {
        emit Deposit(msg.sender, msg.value, address(this).balance);
    }

    function submitTransaction(
        address _to,
        uint _value,
        bytes memory _data
    ) public onlyOwner {
        uint txIndex = transactions.length;
        transactions.push(
            Transaction({
                to: _to,
                value: _value,
                data: _data,
                executed: false,
                numConfirmations: 0
            })
        );
        emit SubmitTransaction(msg.sender, txIndex, _to, _value, _data);
    }

    function confirmTransaction(uint _txIndex)
        public
        onlyOwner
        txExists(_txIndex)
        notExecuted(_txIndex)
        notConfirmed(_txIndex)
    {
        Transaction storage transaction = transactions[_txIndex];
        transaction.numConfirmations += 1;
        isConfirmed[_txIndex][msg.sender] = true;
        emit ConfirmTransaction(msg.sender, _txIndex);
    }

    function executeTransaction(uint _txIndex)
        public
        onlyOwner
        txExists(_txIndex)
        notExecuted(_txIndex)
    {
        Transaction storage transaction = transactions[_txIndex];
        require(
            transaction.numConfirmations >= numConfirmationsRequired,
            "cannot execute tx"
        );
        transaction.executed = true;
        (bool success, ) = transaction.to.call{value: transaction.value}(
            transaction.data
        );
        require(success, "tx failed");
        emit ExecuteTransaction(msg.sender, _txIndex);
    }

    function revokeConfirmation(uint _txIndex)
        public
        onlyOwner
        txExists(_txIndex)
        notExecuted(_txIndex)
    {
        Transaction storage transaction = transactions[_txIndex];
        require(isConfirmed[_txIndex][msg.sender], "tx not confirmed");
        transaction.numConfirmations -= 1;
        isConfirmed[_txIndex][msg.sender] = false;
        emit RevokeConfirmation(msg.sender, _txIndex);
    }

    function getOwners() public view returns (address[] memory) {
        return owners;
    }

    function getTransactionCount() public view returns (uint) {
        return transactions.length;
    }

    function getTransaction(uint _txIndex)
        public
        view
        returns (
            address to,
            uint value,
            bytes memory data,
            bool executed,
            uint numConfirmations
        )
    {
        Transaction storage transaction = transactions[_txIndex];
        return (
            transaction.to,
            transaction.value,
            transaction.data,
            transaction.executed,
            transaction.numConfirmations
        );
    }
}import hashlib
import json
import os
from web3 import Web3

# Load config
try:
    with open("../config/multi_sig_config.json", "r") as f:
        config = json.load(f)
    w3 = Web3(Web3.HTTPProvider("https://sepolia.infura.io/v3/YOUR_INFURA_KEY"))
    multi_contract = w3.eth.contract(address=config["multi_sig"]["address"], abi=config["multi_sig"]["abi"])
    ledger_contract = w3.eth.contract(address=config["hash_ledger"]["address"], abi=config["hash_ledger"]["abi"])
    CHAIN_MODE = "multi-sig"
except:
    print("⚠️ Using local stub—run deploy first!")
    CHAIN_MODE = "stub"

def create_and_store_hash(story, prev_hash='', private_key=None):
    full_input = story + prev_hash
    story_hash = Web3.keccak(text=full_input).hex()

    if CHAIN_MODE == "multi-sig" and private_key:
        # Encode call data for ledger.addHash
        data = ledger_contract.encodeABI(fn_name='addHash', args=[story_hash])
        # Submit via multi-sig (from sender)
        nonce = w3.eth.get_transaction_count(w3.eth.account.from_key(private_key).address)
        tx = multi_contract.functions.submitTransaction(
            ledger_contract.address, 0, data
        ).build_transaction({
            "from": w3.eth.account.from_key(private_key).address,
            "gas": 200000,
            "gasPrice": w3.eth.gas_price,
            "nonce": nonce,
        })
        signed = w3.eth.account.sign_transaction(tx, private_key)
        tx_hash = w3.eth.send_raw_transaction(signed.rawTransaction)
        receipt = w3.eth.wait_for_transaction_receipt(tx_hash)
        tx_index = multi_contract.functions.getTransactionCount().call() - 1  # Latest tx
        print(f"✅ Submitted hash {story_hash[:16]}... as tx {tx_index} | Tx: {tx_hash.hex()}")
        print(f"ℹ️ Next: Confirm from {config['multi_sig']['abi'][0]['threshold'] - 1} other owners, then execute.")
        return story_hash, tx_index  # Return index for later confirm/execute
    else:
        return story_hash[:16], "local-fallback"
from web3 import Web3
import json
import argparse

parser = argparse.ArgumentParser()
parser.add_argument('--tx-index', type=int, required=True, help='Tx index to confirm')
parser.add_argument('--private-key', type=str, required=True, help='Owner private key')
args = parser.parse_args()

with open("../config/multi_sig_config.json", "r") as f:
    config = json.load(f)
w3 = Web3(Web3.HTTPProvider("https://sepolia.infura.io/v3/YOUR_INFURA_KEY"))
multi = w3.eth.contract(address=config["multi_sig"]["address"], abi=config["multi_sig"]["abi"])

account = w3.eth.account.from_key(args.private_key)
tx = multi.functions.confirmTransaction(args.tx_index).build_transaction({
    "from": account.address,
    "gas": 200000,
    "gasPrice": w3.eth.gas_price,
    "nonce": w3.eth.get_transaction_count(account.address),
})
signed = w3.eth.account.sign_transaction(tx, args.private_key)
tx_hash = w3.eth.send_raw_transaction(signed.rawTransaction)
receipt = w3.eth.wait_for_transaction_receipt(tx_hash)
print(f"Confirmed tx {args.tx_index} | Tx: {tx_hash.hex()}")
print(f"Confirmations now: {multi.functions.getTransaction(args.tx_index).call()[-1]}")  # numConfirmations
# Same imports/args as confirm, but --tx-index and --private-key
# ...

tx = multi.functions.executeTransaction(args.tx_index).build_transaction({
    "from": account.address,
    "gas": 300000,
    "gasPrice": w3.eth.gas_price,
    "nonce": w3.eth.get_transaction_count(account.address),
})
# ... sign, send, receipt
print(f"Executed tx {args.tx_index} | Tx: {tx_hash.hex()}")
print("Hash added to ledger!")
### Multi-Sig Support (v0.3)
- **Why?** Family co-ownership: Propose story adds, require 2+ confirms to execute.
- **Setup**:
  1. Edit owners/threshold in deploy script.
  2. `python scripts/deploy_multi_sig.py --owners 0x... 0x... --threshold 2`
  3. Run tree: Submits proposals. Use `confirm_tx.py` & `execute_tx.py` for the rest.
- **Flow**: Submit (auto) → Confirm (manual, other keys) → Execute → Hash on-chain.
- **Test**: Use Ganache with multiple accounts; query ledger on Etherscan.
networkx==3.3
web3==6.15.1
py-solc-x==1.2.2
safe-eth-py  # Gnosis Safe SDK (v1.5+ as of 2025)
eth-account==0.13.1  # For signing helpers
import json
import argparse
from web3 import Web3
from solcx import compile_standard, install_solc
from eth_account import Account
from gnosis.eth import EthereumClient
from gnosis.safe import SafeFactory, Safe
from gnosis.pyipfs import get_web3

# Install Solidity
install_solc("0.8.24")

# Parse args
parser = argparse.ArgumentParser(description="Deploy Gnosis Safe + HashLedger")
parser.add_argument('--owners', nargs='+', required=True, help='Owner addresses (checksummed)')
parser.add_argument('--threshold', type=int, default=2, help='Confirmations required')
parser.add_argument('--private-key', type=str, required=True, help='Deployer private key (first owner)')
parser.add_argument('--rpc-url', default='https://sepolia.infura.io/v3/YOUR_INFURA_KEY', help='Sepolia RPC')
args = parser.parse_args()

# Setup
w3 = Web3(Web3.HTTPProvider(args.rpc_url))
if not w3.is_connected():
    raise Exception("RPC connection failed")
ethereum_client = EthereumClient(args.rpc_url, w3=w3)  # Wraps web3
account = Account.from_key(args.private_key)
print(f"Deploying from: {account.address}")

owners = [Web3.to_checksum_address(o) for o in args.owners]
print(f"Owners: {owners}, Threshold: {args.threshold}")

# Step 1: Create Gnosis Safe
factory = SafeFactory(ethereum_client)
# Generate salt_nonce for deterministic address (optional; use random for prod)
salt_nonce = 12345  # Or random.randint(1, 2**256-1)
safe = factory.from_owners(
    owner_addresses=owners,
    threshold=args.threshold,
    salt_nonce=salt_nonce,
    chain_id=11155111  # Sepolia
)

# Build setup tx (deploys SafeProxy)
setup_tx = safe.setup(
    payment_token=None,  # No payment for setup
    payment=None,
    payment_receiver=None
)

# Sign & execute setup (deployer as executor)
setup_tx.sign(args.private_key)
tx_hash = setup_tx.execute(args.private_key, ethereum_client)
receipt = w3.eth.wait_for_transaction_receipt(tx_hash)
safe_address = safe.address  # Now deployed
print(f"Safe deployed at: {safe_address}")
print(f"Setup Tx: {tx_hash.hex()}")

# Verify Safe info
safe_info = safe.retrieve_all_info()
print(f"Safe Nonce: {safe_info.nonce}, Owners: {len(safe_info.owners)}")

# Step 2: Deploy HashLedger owned by Safe
with open("../contracts/HashLedger.sol", "r") as f:
    source = f.read()
compiled = compile_standard({
    "language": "Solidity",
    "sources": {"HashLedger.sol": {"content": source}},
    "settings": {"outputSelection": {"*": {"*": ["abi", "metadata", "evm.bytecode"]}}}
}, solc_version="0.8.24")

contract_interface = compiled["contracts"]["HashLedger.sol"]["HashLedger"]
abi = contract_interface["abi"]
bytecode = contract_interface["evm"]["bytecode"]["object"]

nonce = w3.eth.get_transaction_count(account.address)
deploy_tx = w3.eth.contract(abi=abi, bytecode=bytecode).constructor(safe_address).build_transaction({
    "chainId": 11155111,
    "gas": 1000000,
    "gasPrice": w3.eth.gas_price,
    "nonce": nonce,
})
signed_deploy = Account.sign_transaction(deploy_tx, args.private_key)
deploy_hash = w3.eth.send_raw_transaction(signed_deploy.rawTransaction)
deploy_receipt = w3.eth.wait_for_transaction_receipt(deploy_hash)
ledger_address = deploy_receipt.contractAddress
print(f"HashLedger deployed at: {ledger_address} (owned by Safe)")
print(f"Deploy Tx: {deploy_hash.hex()}")

# Save config
config = {
    "safe": {"address": safe_address, "abi": safe_info.safe_contract_abi},  # From SDK
    "hash_ledger": {"address": ledger_address, "abi": abi}
}
with open("../config/gnosis_safe_config.json", "w") as f:
    json.dump(config, f, indent=2)
print("Config saved to config/gnosis_safe_config.json")
import json
import os
from web3 import Web3
from eth_account import Account
from gnosis.eth import EthereumClient
from gnosis.safe import Safe

# Load config
try:
    with open("../config/gnosis_safe_config.json", "r") as f:
        config = json.load(f)
    ethereum_client = EthereumClient("https://sepolia.infura.io/v3/YOUR_INFURA_KEY")
    safe = Safe(config["safe"]["address"], ethereum_client)
    ledger_contract = ethereum_client.w3.eth.contract(
        address=config["hash_ledger"]["address"],
        abi=config["hash_ledger"]["abi"]
    )
    CHAIN_MODE = "gnosis-safe"
except:
    print("⚠️ Using local stub—run deploy first!")
    CHAIN_MODE = "stub"

def create_and_store_hash(story, prev_hash='', owner_keys=None):
    full_input = story + prev_hash
    story_hash = Web3.keccak(text=full_input).hex()

    if CHAIN_MODE == "gnosis-safe" and owner_keys:
        # Build multisig tx: Call ledger.addHash(story_hash)
        data = ledger_contract.encodeABI(fn_name='addHash', args=[story_hash])
        safe_tx = safe.build_multisig_tx(
            to=ledger_contract.address,
            value=0,
            data=data,
            operation=0,  # Call
            safe_tx_gas=0,  # Auto-estimate
            base_gas=0,
            gas_price=None,  # Auto
            gas_token="0x0000000000000000000000000000000000000000",
            refund_receiver="0x0000000000000000000000000000000000000000",
            signatures=None,
            safe_nonce=None  # Auto
        )

        # Sign with all owners (for demo; threshold check in prod)
        for key in owner_keys:
            safe_tx.sign(key)
        print(f"✅ Signed with {len(owner_keys)} owners (threshold: {safe.retrieve_all_info().threshold})")

        # Simulate
        simulation = safe_tx.simulate(ethereum_client)
        if not simulation.success:
            raise Exception(f"Simulation failed: {simulation.gas_used}")

        # Execute with first owner's key
        executor_key = owner_keys[0]
        tx_hash = safe_tx.execute(executor_key)
        receipt = ethereum_client.w3.eth.wait_for_transaction_receipt(tx_hash)
        print(f"✅ Hash {story_hash[:16]}... stored on-chain | Tx: {tx_hash.hex()}")
        return story_hash, tx_hash.hex()
    else:
        return story_hash[:16], "local-fallback"
# In function: Pass owner_keys from args
node_hash, node_tx = create_and_store_hash(node['story'], prev_hash, owner_keys=owner_keys)

# In ledger list: {'hash': node_hash, 'tx': node_tx}
parser.add_argument('--owner-keys', nargs='+', help='List of owner private keys for signing')
# Then: tree, ledger = build_echo_tree(args.input, owner_keys=args.owner_keys)
### Gnosis Safe Integration (v0.4)
- **Why?** Pro multi-sig: Audited, modular, async signing.
- **Setup**:
  1. `pip install safe-eth-py`
  2. `python scripts/deploy_safe_and_ledger.py --owners 0x... 0x... --threshold 2 --private-key 0x...`
  3. Run tree: `python src/echo_tree.py --owner-keys 0xKey1 0xKey2`
- **Flow**: SDK builds/signs/executes addHash calls. View on Etherscan/Safe UI.
- **Prod Tips**: Use Safe Transaction Service for relaying; add modules for auto-approvals.
# Existing...
requests  # For API polling if needed (SDK handles most)
{
  "safe": { "address": "...", "abi": [...] },
  "hash_ledger": { "address": "...", "abi": [...] },
  "api_key": "YOUR_SAFE_API_KEY_HERE",  # Add after signup
  "chain_id": 11155111  # Sepolia
}import json
import time
import os
from web3 import Web3
from eth_account import Account
from safe_eth_py import Safe, SafeApiKit  # From requirements

# Load config
try:
    with open("../config/gnosis_safe_config.json", "r") as f:
        config = json.load(f)
    w3 = Web3(Web3.HTTPProvider("https://sepolia.infura.io/v3/YOUR_INFURA_KEY"))
    ethereum_client = w3  # SDK uses web3
    safe = Safe(config["safe"]["address"], ethereum_client, chain_id=config["chain_id"])
    api_kit = SafeApiKit(safe.safe_address, config["chain_id"], config["api_key"])
    ledger_contract = w3.eth.contract(
        address=config["hash_ledger"]["address"],
        abi=config["hash_ledger"]["abi"]
    )
    CHAIN_MODE = "gnosis-safe-async"
except:
    print("⚠️ Using local stub—run deploy & add API key!")
    CHAIN_MODE = "stub"

def create_and_store_hash(story, prev_hash='', proposer_key=None):
    full_input = story + prev_hash
    story_hash = Web3.keccak(text=full_input).hex()

    if CHAIN_MODE == "gnosis-safe-async" and proposer_key:
        # Build multisig tx: addHash(story_hash)
        data = ledger_contract.encodeABI(fn_name='addHash', args=[story_hash])
        safe_tx = safe.build_transaction({
            'to': ledger_contract.address,
            'value': 0,
            'data': data,
            'operation': 0,  # CALL
        })

        # Sign with proposer (off-chain)
        safe_tx_hash = safe.get_transaction_hash(safe_tx)
        safe_tx.sign(proposer_key)

        # Propose to Safe API (collects first sig)
        propose_response = api_kit.propose_transaction(
            safe_tx_hash=safe_tx_hash,
            safe_tx=safe_tx,
            sender=Account.from_key(proposer_key).address,
            origin="EchoWeave Proposer"  # Optional metadata
        )
        print(f"✅ Proposed hash {story_hash[:16]}... | SafeTxHash: {safe_tx_hash.hex()}")
        print(f"ℹ️ Share this hash for async sigs: {safe_tx_hash.hex()}")
        print(f"🔗 Owners sign via: https://app.safe.global/transactions/queue?safe={safe.safe_address}&safeTxHash={safe_tx_hash.hex()}")
        return story_hash, safe_tx_hash.hex()  # TxHash for polling
    else:
        return story_hash[:16], "local-fallback"

def collect_and_execute(safe_tx_hash, executor_key=None, poll_interval=30, max_polls=20):
    """Poll for sigs, execute when threshold met."""
    if CHAIN_MODE != "gnosis-safe-async":
        print("❌ Async mode not active—skipping.")
        return "stub-executed"

    tx_details = api_kit.get_transaction(safe_tx_hash)
    current_confirmations = len(tx_details.confirmations) if tx_details.confirmations else 0
    threshold = safe.retrieve_all_info().threshold
    print(f"Current sigs: {current_confirmations}/{threshold}")

    polls = 0
    while current_confirmations < threshold and polls < max_polls:
        print(f"⏳ Polling... ({polls+1}/{max_polls})")
        time.sleep(poll_interval)
        polls += 1
        tx_details = api_kit.get_transaction(safe_tx_hash)
        current_confirmations = len(tx_details.confirmations) if tx_details.confirmations else 0
        print(f"Updated sigs: {current_confirmations}/{threshold}")

    if current_confirmations >= threshold:
        # Execute with executor (any owner)
        safe_tx = safe.build_transaction_from_hash(safe_tx_hash)  # Rebuild from hash
        safe_tx.sign(executor_key)  # Final sig if needed (SDK handles)
        tx_response = safe.execute_transaction(safe_tx, executor_key)
        receipt = w3.eth.wait_for_transaction_receipt(tx_response.transaction_hash)
        print(f"✅ Executed! Tx: {receipt.transactionHash.hex()}")
        return receipt.transactionHash.hex()
    else:
        print(f"⏰ Threshold not met after {max_polls} polls. Manual check needed.")
        return None
import argparse
from src.ledger import api_kit, safe  # Import from project

parser = argparse.ArgumentParser(description="Sign a pending EchoWeave tx async.")
parser.add_argument('--safe-tx-hash', required=True, help='Shared SafeTxHash from proposer')
parser.add_argument('--private-key', required=True, help='Your owner private key')
args = parser.parse_args()

# Sign & confirm
safe_tx = safe.build_transaction_from_hash(args.safe_tx_hash)
safe_tx.sign(args.private_key)
api_kit.confirm_transaction(args.safe_tx_hash, safe_tx.signatures[0])  # Submit sig
print(f"✅ Signed & confirmed: {args.safe_tx_hash}")
print(f"Check status: https://app.safe.global/transactions/queue?safe={safe.safe_address}&safeTxHash={args.safe_tx_hash}")
parser.add_argument('--auto-execute', action='store_true', help='Poll & execute after propose')
parser.add_argument('--proposer-key', type=str, default=os.getenv('ECHO_PROPOSER_KEY'))
parser.add_argument('--executor-key', type=str, default=os.getenv('ECHO_EXECUTOR_KEY'))
# ...
if args.auto_execute:
    for entry in ledger:
        collect_and_execute(entry['tx_hash'], executor_key=args.executor_key)
### Async Signature Collection (v0.5)
- **Why?** Owners sign anytime/anywhere—no live sync.
- **Setup**:
  1. Get Safe API key: [safe.global/api-keys](https://safe.global/api-keys) → Add to config.
  2. Propose: Run tree with `--proposer-key` → Share SafeTxHash link.
  3. Sign: Others run `async_sign.py --safe-tx-hash <hash> --private-key <key>`.
  4. Collect: Use `--auto-execute` to poll (or manual via Safe app).
- **Flow**: Propose (1 sig) → Share hash → Async signs → Poll → Execute.
- **Demo**: Proposes 3 txns (root + branches), waits ~5 mins for sigs, executes all.
- **Prod**: Integrate webhooks for real-time (Safe API supports).
echoweave-legacy-weaver/
├── safe-app/                  # New: React Safe App
│   ├── public/
│   │   └── manifest.json      # Safe App config
│   ├── src/
│   │   ├── App.js             # Tree UI + SDK integration
│   │   ├── components/
│   │   │   └── EchoTree.js    # Viz component
│   │   └── index.js
│   ├── config-overrides.js    # CORS/HTTPS
│   ├── package.json
│   └── README.md              # Sub-readme
├── backend/                   # Move src/ here for API
│   ├── app.py                 # Flask API stub (tree gen + calldata)
│   └── ... (existing py files)
└── README.md                  # Update root with integration section
from flask import Flask, request, jsonify
from flask_cors import CORS
import json
from src.echo_tree import build_echo_tree  # Adjust import
from src.ledger import create_and_store_hash  # For calldata gen
from web3 import Web3

app = Flask(__name__)
CORS(app)  # Enable for Safe App

@app.route('/build-tree', methods=['POST'])
def api_build_tree():
    data = request.json  # { "family_data": json_str }
    family_data = json.loads(data['family_data'])
    # Temp: Use stub mode; real chain via frontend keys
    tree, ledger = build_echo_tree_from_data(family_data)  # Wrap your func
    return jsonify({"tree": dict(tree.nodes(data=True)), "ledger": ledger})

def build_echo_tree_from_data(family_data):  # Wrapper for API
    # Your existing logic, but return dict for JSON
    # ...

@app.route('/encode-hash-tx', methods=['POST'])
def encode_add_hash():
    data = request.json  # { "story_hash": "0x...", "ledger_address": "0x..." }
    w3 = Web3()
    ledger_abi = [...]  # Load from config
    ledger_contract = w3.eth.contract(address=data['ledger_address'], abi=ledger_abi)
    calldata = ledger_contract.encodeABI(fn_name='addHash', args=[data['story_hash']])
    return jsonify({"calldata": calldata, "to": data['ledger_address']})

if __name__ == '__main__':
    app.run(debug=True, port=5000)
cd safe-app
npx create-react-app . --template cra-template-safe-app  # If fresh; else manual
npm install @safe-global/safe-apps-sdk @safe-global/safe-react-components axios  # SDK + UI Kit + API calls
npm run start  # HTTPS dev server
{
  "name": "EchoWeave Legacy Weaver",
  "description": "Weave family stories into on-chain trees—propose & sign hashes in Safe.",
  "iconPath": "logo.svg"  # Add your 128x128 SVG to public/
}import React from 'react';
import ReactDOM from 'react-dom/client';
import './index.css';
import App from './App';
import { SafeProvider } from '@safe-global/safe-apps-react-sdk';  // Auto-connect

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <SafeProvider loader="spinner">  {/* Loads Safe if in app */}
    <App />
  </SafeProvider>
);import React, { useState, useEffect } from 'react';
import { useSafeAppsSDK } from '@safe-global/safe-apps-react-sdk';
import { SafeAppBar, SafeButton, SafeContainer } from '@safe-global/safe-react-components';
import axios from 'axios';
import EchoTree from './components/EchoTree';  // Your viz

function App() {
  const { sdk, safe } = useSafeAppsSDK();  // Auto-connect in Safe
  const [tree, setTree] = useState(null);
  const [familyData, setFamilyData] = useState({ root: {}, branches: {} });  // User input state
  const [ledgerAddr, setLedgerAddr] = useState('0xYourLedger');  // From config

  useEffect(() => {
    if (safe) {
      console.log('Connected Safe:', safe.safeAddress);
      setLedgerAddr(safe.safeAddress);  // Or fetch from backend
    }
  }, [safe]);

  const buildAndPropose = async () => {
    // 1. Build tree via API
    const res = await axios.post('http://localhost:5000/build-tree', { family_data: JSON.stringify(familyData) });
    setTree(res.data.tree);

    // 2. For each ledger entry, propose async tx
    for (const entry of res.data.ledger) {
      const calldataRes = await axios.post('http://localhost:5000/encode-hash-tx', {
        story_hash: entry.hash,
        ledger_address: ledgerAddr
      });
      const tx = {
        to: calldataRes.data.to,
        value: '0',
        data: calldataRes.data.calldata,
        operation: 0,  // CALL
      };

      // 3. Propose via SDK (deep link for async sig)
      const { safeTxHash, deepLink } = await sdk.safe.createTransaction({
        safeTransactionData: { transactions: [tx] },
      });
      console.log('Propose SafeTxHash:', safeTxHash);
      // Share deepLink.url: e.g., window.open(deepLink.url) or QR

      // Optional: Auto-collect (poll as before)
      await collectAndExecute(safeTxHash);  // Your async func from ledger.py (JS port)
    }
  };

  return (
    <SafeContainer>
      <SafeAppBar title="EchoWeave" />
      <div style={{ padding: '20px' }}>
        <h2>Weave Your Legacy</h2>
        {/* Input form for familyData */}
        <input placeholder="Root Story" onChange={(e) => setFamilyData({...familyData, root: {story: e.target.value}})} />
        <SafeButton onClick={buildAndPropose}>Build & Propose to Safe</SafeButton>
        {tree && <EchoTree data={tree} />}
      </div>
    </SafeContainer>
  );
}

export default App;
import React from 'react';

const EchoTree = ({ data }) => (
  <div>
    {Object.entries(data).map(([node, attrs]) => (
      <div key={node}>
        {node} ({attrs.year}): {attrs.story?.slice(0, 50)}... [Media: {attrs.media}]
      </div>
    ))}
  </div>
);

export default EchoTree;
// Add to App.js or utils.js
import { SafeApiKit } from '@safe-global/safe-apps-sdk';

const apiKit = new SafeApiKit({ chainId: 11155111 });  // Sepolia

async function collectAndExecute(safeTxHash, pollInterval = 30000, maxPolls = 20) {
  let polls = 0;
  while (polls < maxPolls) {
    const tx = await apiKit.getTransaction(safeTxHash);
    const confirmations = tx.confirmations?.length || 0;
    const threshold = safe.threshold;  // From SDK
    if (confirmations >= threshold) {
      // Execute via SDK
      await sdk.safe.execTransaction({ safeTransactionData: { safeTxHash } });
      console.log('Executed!');
      return;
    }
    await new Promise(resolve => setTimeout(resolve, pollInterval));
    polls++;
  }
}## Safe Wallet App Integration (v0.6)
- **Run**: `cd safe-app && npm install && npm start`
- **In Safe**: Add custom app URL → Build trees → Propose signs via deep links.
- **Listing**: Submit manifest + repo to Safe team (see /safe-app/README.md).
import React, { useState, useEffect } from 'react';
import { useSafeAppsSDK } from '@safe-global/safe-apps-react-sdk';
import { SafeAppBar, SafeButton, SafeContainer, SafeTypography } from '@safe-global/safe-react-components';
import QRCode from 'qrcode.react';  // 👈 New: QR lib
import axios from 'axios';
import EchoTree from './components/EchoTree';

function App() {
  const { sdk, safe } = useSafeAppsSDK();
  const [tree, setTree] = useState(null);
  const [familyData, setFamilyData] = useState({ root: {}, branches: {} });
  const [ledgerAddr, setLedgerAddr] = useState('0xYourLedger');
  const [deepLinks, setDeepLinks] = useState([]);  // 👈 New: Array of {hash, url, qrData} for multi-txns

  useEffect(() => {
    if (safe) {
      console.log('Connected Safe:', safe.safeAddress);
      setLedgerAddr(safe.safeAddress);  // Or fetch
    }
  }, [safe]);

  const buildAndPropose = async () => {
    const res = await axios.post('http://localhost:5000/build-tree', { family_data: JSON.stringify(familyData) });
    setTree(res.data.tree);

    const newDeepLinks = [];  // 👈 New: Collect QRs
    for (const entry of res.data.ledger) {
      const calldataRes = await axios.post('http://localhost:5000/encode-hash-tx', {
        story_hash: entry.hash,
        ledger_address: ledgerAddr
      });
      const tx = {
        to: calldataRes.data.to,
        value: '0',
        data: calldataRes.data.calldata,
        operation: 0,
      };

      const { safeTxHash, deepLink } = await sdk.safe.createTransaction({
        safeTransactionData: { transactions: [tx] },
      });
      console.log('Propose SafeTxHash:', safeTxHash);

      // 👈 New: Add to list with QR-friendly data
      newDeepLinks.push({
        story: entry.story || 'Untitled Branch',  // From tree attrs
        hash: safeTxHash,
        url: deepLink.url,  // e.g., safe://ws?params...
        qrSize: 200  // Customizable
      });
    }
    setDeepLinks(newDeepLinks);

    // Optional poll (as before)
    // await collectAndExecute(...);
  };

  return (
    <SafeContainer>
      <SafeAppBar title="EchoWeave" />
      <div style={{ padding: '20px' }}>
        <SafeTypography variant="h5">Weave Your Legacy</SafeTypography>
        {/* Input form */}
        <input 
          placeholder="Root Story" 
          onChange={(e) => setFamilyData({...familyData, root: {story: e.target.value}})} 
          style={{ width: '100%', marginBottom: '10px' }}
        />
        <SafeButton onClick={buildAndPropose} style={{ marginBottom: '20px' }}>
          Build & Propose to Safe
        </SafeButton>
        
        {tree && <EchoTree data={tree} />}
        
        {/* 👈 New: QR Gallery */}
        {deepLinks.length > 0 && (
          <div>
            <SafeTypography variant="h6" style={{ marginTop: '20px' }}>Share for Async Signatures</SafeTypography>
            <div style={{ display: 'grid', gridTemplateColumns: 'repeat(auto-fit, minmax(200px, 1fr))', gap: '20px' }}>
              {deepLinks.map((link, idx) => (
                <div key={idx} style={{ textAlign: 'center', border: '1px solid #ccc', padding: '10px', borderRadius: '8px' }}>
                  <SafeTypography variant="body2">{link.story.slice(0, 30)}...</SafeTypography>
                  <QRCode 
                    value={link.url} 
                    size={link.qrSize} 
                    fgColor="#000000" 
                    bgColor="#FFFFFF"
                    style={{ margin: '10px auto' }}
                  />
                  <SafeTypography variant="caption" style={{ wordBreak: 'break-all' }}>
                    {link.hash.slice(0, 10)}...
                  </SafeTypography>
                  <SafeButton 
                    variant="outlined" 
                    size="small" 
                    onClick={() => navigator.clipboard.writeText(link.url)}  // Copy fallback
                    style={{ marginTop: '5px' }}
                  >
                    Copy Link
                  </SafeButton>
                </div>
              ))}
            </div>
            <SafeTypography variant="body2" style={{ marginTop: '10px', fontStyle: 'italic' }}>
              Scan in Safe Wallet app to sign. Threshold met? Poll executes automatically.
            </SafeTypography>
          </div>
        )}
      </div>
    </SafeContainer>
  );
}

export default App;
