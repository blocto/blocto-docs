---
description: Query and mutate Flow blockchain
---

# Blockchain Interactions

We are assuming you have read the [Scripts Documentation](https://docs.onflow.org/fcl/reference/scripts/) before this, as transactions are sort of scripts with more required things.

There are two different operations to interact with Flow blockchain:

1. Query: Send arbitrary Cadence scripts to the chain and receive decoded values. Can be performed without user login
2. Mutate: Use transaction to send Cadence code with specify authorizer to perform permanently state changes on chain. Must be performed after user logged in

### Query

```kotlin
// Pass your Cadence script as a parameter
when (val result = Fcl.query(script)) {
    is Result.Success -> {
        val queryResult = result.value.toString()
    }
    is Result.Failure -> {
        // Handle error
    }
}
```

### Mutate

```kotlin
when (val result = Fcl.mutate(
    cadence = script,
    arguments = args, 
    limit = 300u,
    authorizers = listOf(FlowAddress(userAddress)
)) {
    is Result.Success -> {
        val transactionId = result.value
    }
    is Result.Failure -> {
        // Handle error
    }
}
```
