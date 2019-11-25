          ┌─┐                                                                                                     
          ║"│                                                                                                     
          └┬┘               ┌──────────────┐                                                                      
          ┌┼┐               │Location      │          ┌─────────┐                                                 
           │                │management    │          │cache eg.│          ┌──────┐          ┌──────────┐         
          ┌┴┐               │GUI helper app│          │Redis or │          │event │          │AMQP state│         
      InComm                │and API       │          │Memcached│          │API   │          │machine   │         
      nsales team           └──────┬───────┘          └────┬────┘          └──┬───┘          └────┬─────┘         
           │       sign in         │                       │                  │                   │               
           │──────────────────────>│                       │                  │                   │               
           │                       │                       │                  │                   │               
           │     grant access      │                       │                  │                   │               
           │<──────────────────────│                       │                  │                   │               
           │                       │                       │                  │                   │               
           │ post merchant's       │                       │                  │                   │               
           │ locations collection  │                       │                  │                   │               
           │──────────────────────>│                       │                  │                   │               
           │                       │                       │                  │                   │               
           │                       │                       │                  │                   │               
           │                       │ ─────────────────────>│                  │                   │               
           │                       │                       │                  │                   │               
           │                       │                       │                  │                   │               
           │                       │ ─────────────────────────────────────────>                   │               
           │                       │                       │                  │                   │               
           │                       │                       │                  │                   │               
           │                       │                       │                  │ ──────────────────>               
           │                       │                       │                  │                   │               
           │                       │                       │                  │                   │────┐          
           │                       │                       │                  │                   │    │ daemon   
           │                       │                       │                  │                   │<───┘ pending  
           │                       │                       │                  │                   │      db master
           │                       │                       │                  │                   │      unlock   
           │                       │                       │                  │                   │               
      InComm                ┌──────┴───────┐          ┌────┴────┐          ┌──┴───┐          ┌────┴─────┐         
      nsal┌─┐team           │Location      │          │cache eg.│          │event │          │AMQP state│         
          ║"│               │management    │          │Redis or │          │API   │          │machine   │         
          └┬┘               │GUI helper app│          │Memcached│          └──────┘          └──────────┘         
          ┌┼┐               │and API       │          └─────────┘                                                 
           │                └──────────────┘                                                                      
          ┌┴┐                                                                                                     
                                                                                                                  
                                                                                                                  
                                                 ┌───────────┐          ┌───────────┐                 
     ┌──────┐          ┌──────────┐              │PostgreSQL │          │PostgreSQL │                 
     │event │          │AMQP state│              │replication│          │replication│          ┌─────┐
     │API   │          │machine   │              │master     │          │i-th slave │          │cache│
     └──┬───┘          └────┬─────┘              └─────┬─────┘          └─────┬─────┘          └──┬──┘
        │                   │────┐                     │                      │                   │   
        │                   │    │ daemon              │                      │                   │   
        │                   │<───┘ pending             │                      │                   │   
        │                   │      db master           │                      │                   │   
        │                   │      unlock              │                      │                   │   
        │                   │                          │                      │                   │   
        │                   │                          │                      │                   │   
        │                   │ replicate db queue empty?│                      │                   │   
        │                   │ <────────────────────────>                      │                   │   
        │                   │                          │                      │                   │   
        │      enqueue      │                          │                      │                   │   
        │      db lock      │                          │                      │                   │   
        │ <──────────────────                          │                      │                   │   
        │                   │                          │                      │                   │   
        │                   │                          │                      │                   │   
        │ ──────────────────>                          │                      │                   │   
        │                   │                          │                      │                   │   
        │                   │                          │  fetch cached        │                   │   
        │                   │                          │  collection          │                   │   
        │                   │ <────────────────────────────────────────────────────────────────────   
        │                   │                          │                      │                   │   
        │                   │ post collection SQL query│                      │                   │   
        │                   │ ─────────────────────────>                      │                   │   
        │                   │                          │                      │                   │   
        │    enqueue db     │                          │                      │                   │   
        │    propagation    │                          │                      │                   │   
        │ <──────────────────                          │                      │                   │   
        │                   │                          │                      │                   │   
        │                   │                          │                      │                   │   
        │ ──────────────────>                          │                      │                   │   
        │                   │                          │                      │                   │   
        │                   │                          │flush collection      │                   │   
        │                   │ ────────────────────────────────────────────────────────────────────>   
        │                   │                          │                      │                   │   
        │     enqueue db    │                          │                      │                   │   
        │     unlock        │                          │                      │                   │   
        │ <──────────────────                          │                      │                   │   
        │                   │                          │                      │                   │   
        │                   │                          │                      │                   │   
        │ ──────────────────>                          │                      │                   │   
     ┌──┴───┐          ┌────┴─────┐              ┌─────┴─────┐          ┌─────┴─────┐          ┌──┴──┐
     │event │          │AMQP state│              │PostgreSQL │          │PostgreSQL │          │cache│
     │API   │          │machine   │              │replication│          │replication│          └─────┘
     └──────┘          └──────────┘              │master     │          │i-th slave │                 
                                                 └───────────┘          └───────────┘                 
