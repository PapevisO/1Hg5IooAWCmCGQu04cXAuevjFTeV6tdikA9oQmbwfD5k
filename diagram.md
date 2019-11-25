          ┌─┐                                                                                                                   
          ║"│                                                                                                                   
          └┬┘                                                                                                                   
          ┌┼┐               ┌──────────┐          ┌──────────┐                                ┌─────────┐          ┌───────────┐
           │                │Location  │          │Location  │          ┌──────────┐          │cache eg.│          │PostgreSQL │
          ┌┴┐               │management│          │management│          │AMQP state│          │Redis or │          │replication│
      InComm                │helper app│          │api       │          │machine   │          │Memcached│          │master     │
      nsales team           └────┬─────┘          └────┬─────┘          └────┬─────┘          └────┬────┘          └─────┬─────┘
           │      sign in        │                     │                     │                     │                     │      
           │────────────────────>│                     │                     │                     │                     │      
           │                     │                     │                     │                     │                     │      
           │    grant access     │                     │                     │                     │                     │      
           │<────────────────────│                     │                     │                     │                     │      
           │                     │                     │                     │                     │                     │      
           │           post merchant's                 │                     │                     │                     │      
           │           locations collection            │                     │                     │                     │      
           │──────────────────────────────────────────>│                     │                     │                     │      
           │                     │                     │                     │                     │                     │      
           │                     │                     │                     │                     │                     │      
           │                     │                     │ ─────────────────────────────────────────>│                     │      
           │                     │                     │                     │                     │                     │      
           │                     │                     │                     │                     │                     │      
           │                     │                     │ ────────────────────>                     │                     │      
           │                     │                     │                     │                     │                     │      
           │                     │                     │                     │────┐                │                     │      
           │                     │                     │                     │    │ daemon         │                     │      
           │                     │                     │                     │<───┘ pending        │                     │      
           │                     │                     │                     │      db master      │                     │      
           │                     │                     │                     │      unlock         │                     │      
           │                     │                     │                     │                     │                     │      
           │                     │                     │                     │                     │                     │      
           │                     │                     │                     │                     │                     │      
           │                     │                     │                     │ ─────────────────────────────────────────>│      
           │                     │                     │                     │                     │                     │      
           │                     │                     │                     │                     │                     │      
           │                     │                     │                     │ <─────────────────────────────────────────│      
           │                     │                     │                     │                     │                     │      
           │                     │                     │                     │────┐                │                     │      
           │                     │                     │                     │    │ enqueue db     │                     │      
           │                     │                     │                     │<───┘ lock           │                     │      
           │                     │                     │                     │                     │                     │      
           │                     │                     │                     │                     │                     │      
           │                     │                     │                     │    fetch cached     │                     │      
           │                     │                     │                     │    collection       │                     │      
           │                     │                     │                     │ <───────────────────│                     │      
           │                     │                     │                     │                     │                     │      
           │                     │                     │                     │              post collection              │      
           │                     │                     │                     │              SQL query                    │      
           │                     │                     │                     │ ─────────────────────────────────────────>│      
           │                     │                     │                     │                     │                     │      
           │                     │                     │                     │────┐                │                     │      
           │                     │                     │                     │    │ enqueue db     │                     │      
           │                     │                     │                     │<───┘ propagation    │                     │      
           │                     │                     │                     │                     │                     │      
           │                     │                     │                     │                     │                     │      
           │                     │                     │                     │  flush collection   │                     │      
           │                     │                     │                     │ ───────────────────>│                     │      
           │                     │                     │                     │                     │                     │      
           │                     │                     │                     │────┐                │                     │      
           │                     │                     │                     │    │ enqueue db     │                     │      
           │                     │                     │                     │<───┘ unlock         │                     │      
           │                     │                     │                     │                     │                     │      
      InComm                ┌────┴─────┐          ┌────┴─────┐          ┌────┴─────┐          ┌────┴────┐          ┌─────┴─────┐
      nsal┌─┐team           │Location  │          │Location  │          │AMQP state│          │cache eg.│          │PostgreSQL │
          ║"│               │management│          │management│          │machine   │          │Redis or │          │replication│
          └┬┘               │helper app│          │api       │          └──────────┘          │Memcached│          │master     │
          ┌┼┐               └──────────┘          └──────────┘                                └─────────┘          └───────────┘
           │                                                                                                                    
          ┌┴┐                                                                                                                   
                                                                                                                                
