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
