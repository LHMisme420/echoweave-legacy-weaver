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
