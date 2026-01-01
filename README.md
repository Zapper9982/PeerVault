# NebulaFS 🌌

**NebulaFS** is a decentralized, encrypted, peer-to-peer file storage system written in Go. It splits files into encrypted chunks, distributes them across a network of nodes using a Kademlia-based Distributed Hash Table (DHT), and allows for secure retrieval.

## 🚀 Features

- **Decentralized**: No central server; nodes form a mesh network.
- **Encrypted**: All data is encrypted with AES-256 before leaving your machine.
- **Content Addressed**: Files and chunks are identified by their SHA-1 hash.
- **Distributed**: File chunks are replicated to the closest peers in the network.
- **Resilient**: Automatic peer discovery and routing via Kademlia DHT.
- **Simple CLI**: Easy-to-use command line interface.

## 🛠️ Installation

Requirements: Go 1.25+

```bash
# Clone the repository
git clone https://github.com/tanmaydeobhankar/nebulafs.git
cd nebulafs

# Build the binary
go build -o nebulafs ./cmd/nebulafs
```

## 📖 Usage

### 1. Start a Bootstrap Node
Start the first node in your network (the "lighthouse").
```bash
./nebulafs start --port 3000
```

### 2. Join the Network
Start other nodes by bootstrapping to the first one.
```bash
./nebulafs start --port 4000 --bootstrap :3000
```

### 3. Upload a File
Upload a file to the network. This will split, encrypt, and distribute chunks to peers.
```bash
# Upload a file using a temporary node on port 5001
./nebulafs upload --file ./my-secret-doc.pdf --bootstrap :3000 --port 5001
```
*Output will save a `.meta.json` file and provide an **Encryption Key**.*

### 4. Download a File
Retrieve a file using its metadata and key.
```bash
./nebulafs download \
  --meta my-secret-doc.pdf.meta.json \
  --key <YOUR_ENCRYPTION_KEY> \
  --out recovered-doc.pdf \
  --bootstrap :3000
```

## 🏗️ Architecture

1.  **Identity**: Authenticated encryption keys & Node IDs.
2.  **DHT**: Kademlia implementation for peer discovery and routing (`XOR` metric).
3.  **Storage**: Content-Addressable Storage (CAS) with local disk persistence.
4.  **Transport**: Custom P2P protocol over WebSockets.
5.  **Files**:
    *   **Chunking**: Fixed-size 1MB chunks.
    *   **Encryption**: AES-256-GCM.
    *   **Reassembly**: Verifies hash integrity on download.
