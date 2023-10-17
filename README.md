# Calling Card

This repo is the monorepo for a demonstration of combining identity management with Generative AI in the manner shown in this diagram:

```mermaid
sequenceDiagram
participant U as User  
participant W as Website
activate W
participant K as Kratos
participant H as Hydra
activate H
participant IPFS as IPFS
activate IPFS
participant DID as DID Registry
activate DID

U->>+W: Starts signup flow
W->>+K: Create user identity
K->>+DID: Generate decentralized id
deactivate DID
DID-->>-K: Returns decentralized id
K->>+H: Create access token
deactivate H  
H-->>-K: Access token with DID
K->>W: Signup complete
deactivate W
W-->>-U: Login with DID

activate H
U->>+W: Login with DID
W->>+H: Introspect token
deactivate H
H-->>-W: Valid token
W->>U: Login success
deactivate W 

activate IPFS  
U->>+W: Store context
W->>IPFS: Add encrypted SQLite file
deactivate IPFS
IPFS-->>-W: Returns CID
activate DID
W->>DID: Update DID doc \nwith SQLite CID
deactivate DID
DID-->>-W: DID doc updated
W-->>-U: Context stored
deactivate U

activate DID
U->>+W: Request context
W->>DID: Resolve DID
deactivate DID
DID-->>-W: Returns DID doc
activate IPFS
W->>IPFS: Get SQLite file
deactivate IPFS  
IPFS-->>-W: Returns SQLite file
W->>+LM: Queries SQLite \nfor context
LM-->>-W: Context for LM
deactivate LM
W-->>-U: Provides context
deactivate U
```