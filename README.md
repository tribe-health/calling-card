# Calling Card

This repo is the monorepo for a demonstration of combining identity management with Generative AI in the manner shown in this diagram:

```mermaid
sequenceDiagram
participant U as User
participant W as Website
participant K as Kratos
participant H as Hydra    
participant IPFS as IPFS
participant DID as DID Registry

U->>+W: Starts signup flow
W->>+K: Create user identity
K->>DID: Generate DID
DID-->>-K: Returns DID
K->>+H: Create access token 
H-->>-K: Access token with DID
K->>W: Signup complete
W-->>-U: Login with DID

U->>+W: Login with DID 
W->>+H: Introspect token
H-->>-W: Valid token
W->>U: Login success

U->>+W: Store context
W->>IPFS: Add encrypted SQLite file
IPFS-->>-W: Returns CID
W->>DID: Update DID doc \nwith SQLite CID
DID-->>-W: DID doc updated
W-->>-U: Context stored

U->>+W: Request context
W->>DID: Resolve DID 
DID-->>-W: Returns DID doc
W->>IPFS: Get SQLite file
IPFS-->>-W: Returns SQLite file  
W->>+LM: Queries SQLite \nfor context
LM-->>-W: Context for LM 
W-->>-U: Provides context
```