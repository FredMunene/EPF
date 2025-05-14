# Execution Layer
- Block Validation
- 


```go
// state transaction function
func stf(parent types.Block, currentBlock types.Block, stateDB state.StateDB)(state.stateDB, error){
    // verify headers
    if err := core.VerifyHeaders(parent, currentBlock); err != nil {
        return nil, err
    }
    // errors may happen due to::
    // 1. gas limit : 1/2024 increase allowed
    // 2. block numbers not sequential
    for _, tx := range currentBlock.transaction(){
        re, err := vm.Run(currentBlock.Header(),tx, stateDB)
        if err != nil {
            //  transaction is invalid, block is invalid
            return nil, err
        }
        stateDB = res
    }

    return stateDB, nil
}
//  wrapper for state transaction function
func newPayLoad(executionPayload engine.ExecutionPayLoad) bool {
    //  stf -- state transaction function
    if _, err := stf(..); err != nil {
        return false
    }
    return true
}
```

## Build execution payload / block || Block Building
- Nodes gossip transactions
- 
```go
//  environment contains info, txPool list of ordered tx by gas amount,
func build( env Environment, pool txpool.Pool, state state.StateDB) (types.Block, state.StateDB, error){
    var (
        gasUsed = 0 // finite amount of gas
        txs []types.Transaction

    )
    // env.GasLImit 30_000_000
    //  control 1 / 1024 --> -gas-limit-target
    for ; gasUsed < 30_000_000 || !pool.Empty(); {
        tx := pool.Pop()
        res, gas, err := vm.Run(env,tx, state)
        if err != nil {
            // tx invalid
            continue
        }
        gasUsed += gas
        txs = append(txs,tx)
        state = rs
    }
    return core.Finalize(env, txs, state)
}

```



### Consensus Layer
- Beacon chain
