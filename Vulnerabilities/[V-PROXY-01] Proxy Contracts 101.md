# Proxies and Upgradeable Smart Contract Problems

## Summary

Proxies and Upgradeable Smart Contracts is a brilliant inovation in the blockchain, it breaks the sentence of 

```
Once it's deployed, it cannot be altered
```

stigma of blockchain. While that is true for historical transaction information, it is not true for the Storage and Addresses in Smart Contract. The main idea behind it, is to make a middle-contract; the one that will transact with the actual smart contract behing it and acting as a `proxy` and the `actual smart contract` that is behind this `proxy` and can always be upgrade by deploying a newer version and assign the address of the new version to the `proxy` to interact with.

It's not using a simple `call` but instead it use a specific variant of `call` called `delegatecall`, in which the `msg.sender` and its' attribute remains the same as the original.

For details on how to assess the security aspect of `Proxies and Upgradeables`, please refer to [Proxies Security](https://github.com/Quillhash/Proxies-Security) by [QuillAudits](https://www.quillaudits.com/)

## References

- [Proxies Security by QuillAudit](https://github.com/Quillhash/Proxies-Security)