# New Features

`v2.1` does not include any breaking API changes or new features, but is mainly focussed on fixing technical debt, increasing our future development velocity and improving code organization.

[[toc]]

## TypeScript

The major change in `v2.1` is a full migration to TypeScript, a typed flavor of JavaScript better suited for larger codebases. Older plugins may need to be rewritten, but TypeScript now provides a smoother development experience for core and community developers. Plugin writers do not need to fear another migration, TypeScript will be used for the foreseeable future.

## Debranding

BridgeChains and private chains reusing the Swipechain codebase previously had to debrand the codebase themselves, meaning they had to remove mentions of `Swipechain` from their configuration files and other hardcoded locations. `v2.1` is blockchain-brand agnostic. In general, all variables are named the same, except that the Swipechain has been removed from the name.

For example:

```bash
SWIPECHAIN_CORE_DB_XXX
```

has now been renamed to

```bash
CORE_DB_XXX
```

in `~/.swipechain/.env` and other files. We have also made editing these easier by collapsing different configuration files into a single network config.

### Comprehensive Variable Changelist

The following variables were renamed/added to the configuration of a network. The changes can be found in your `~/.swipechain/.env` and `plugins.js` file.

Note:

- If the second or third column is left empty, the configuration variable must be directly edited in the plugins.js configuration file, instead of being obtained from the `env.process`.
- `core-transaction-pool-mem` was renamed to `core-transaction-pool`.
- The `dynamicFees` key was added to `core-transaction-pool`, which must be configured in the plugins.js file.

| Variable                                                 | v2.0.19                             | v2.1                                 | default                                                     |
| :------------------------------------------------------- | :---------------------------------- | :----------------------------------- | :---------------------------------------------------------- |
| core-logger-winston.transports.console.options.level     | SWIPECHAIN_LOG_LEVEL                       | CORE_LOG_LEVEL                       | 'debug'                                                     |
| core-logger-winston.transports.dailyRotate.options.level | SWIPECHAIN_LOG_LEVEL                       | CORE_LOG_LEVEL                       | 'debug'                                                     |
| core-database-postgres.connection.host                   | SWIPECHAIN_DB_HOST                         | CORE_DB_HOST                         | 'localhost'                                                 |
| core-database-postgres.connection.port                   | SWIPECHAIN_DB_PORT                         | CORE_DB_PORT                         | 5432                                                        |
| core-database-postgres.connection.database               | SWIPECHAIN_DB_DATABASE                     | CORE_DB_DATABASE                     | ${process.env.CORE_TOKEN}\_${process.env.CORE_NETWORK_NAME} |
| core-database-postgres.connection.user                   | SWIPECHAIN_DB_USERNAME                     | CORE_DB_USERNAME                     | 'swipechain'                                                       |
| core-database-postgres.connection.password               | SWIPECHAIN_DB_PASSWORD                     | CORE_DB_PASSWORD                     | 'password'                                                  |
| core-transaction-pool.enabled                            | SWIPECHAIN_TRANSACTION_POOL_DISABLED       | CORE_TRANSACTION_POOL_DISABLED       | true                                                        |
| core-transaction-pool. enabled                           | SWIPECHAIN_TRANSACTION_POOL_MAX_PER_SENDER | CORE_TRANSACTION_POOL_MAX_PER_SENDER | 300                                                         |
| core-transaction-pool.allowedSenders                     |                                     |                                      | []                                                          |
| core-p2p.host                                            | SWIPECHAIN_P2P_HOST                        | CORE_P2P_HOST                        | "0.0.0.0"                                                   |
| core-p2p.port                                            | SWIPECHAIN_P2P_PORT                        | CORE_P2P_PORT                        | 4001                                                        |
| core-blockchain.fastRebuild                              |                                     |                                      | false                                                       |
| core-api.enabled                                         | SWIPECHAIN_API_DISABLED                    | CORE_API_DISABLED                    | true                                                        |
| core-api.host                                            | SWIPECHAIN_API_HOST                        | CORE_API_HOST                        | "0.0.0.0"                                                   |
| core-api.port                                            | SWIPECHAIN_API_PORT                        | CORE_API_PORT                        | 4003                                                        |
| core-api.whitelist                                       |                                     |                                      | ['*']                                                       |
| core-webhooks.enabled                                    | SWIPECHAIN_WEBHOOKS_ENABLED                | CORE_WEBHOOKS_ENABLED                | false                                                       |
| core-webhooks.server.enabled                             | SWIPECHAIN_WEBHOOKS_API_ENABLED            | CORE_WEBHOOKS_API_ENABLED            | false                                                       |
| core-webhooks.server.host                                | SWIPECHAIN_WEBHOOKS_HOST                   | CORE_WEBHOOKS_HOST                   | '0.0.0.0'                                                   |
| core-webhooks.server.port                                | SWIPECHAIN_WEBHOOKS_PORT                   | CORE_WEBHOOKS_PORT                   | 4004                                                        |
| core-webhooks.server.whitelist                           |                                     |                                      | ["127.0.0.1", "::ffff:127.0.0.1"]                           |
| core-graphql.enabled                                     | SWIPECHAIN_GRAPHQL_ENABLED                 | CORE_GRAPHQL_ENABLED                 | false                                                       |
| core-graphql.host                                        | SWIPECHAIN_GRAPHQL_HOST                    | CORE_GRAPHQL_HOST                    | '0.0.0.0'                                                   |
| core-graphql.port                                        | SWIPECHAIN_GRAPHQL_PORT                    | CORE_GRAPHQL_PORT                    | 4005                                                        |
| core-forger.hosts                                        |                                     |                                      | [`http://127.0.0.1:${process.env.CORE_P2P_PORT|| 4001}`]    |
| core-json-rpc.enabled                                    | SWIPECHAIN_JSON_RPC_ENABLED                | CORE_JSON_RPC_ENABLED                | false                                                       |
| core-json-rpc.host                                       | SWIPECHAIN_JSON_RPC_HOST                   | CORE_JSON_RPC_HOST                   | '0.0.0.0'                                                   |
| core-json-rpc.port                                       | SWIPECHAIN_JSON_RPC_PORT                   | CORE_JSON_RPC_PORT                   | 8080                                                        |
| core-json-rpc.allowRemote                                |                                     |                                      | false                                                       |
| core-json-rpc.whitelist                                  |                                     |                                      | ["127.0.0.1", "::ffff:127.0.0.1"]                           |
| ~/.swipechain/.env                                              | SWIPECHAIN_NETWORK_NAME                    | CORE_NETWORK_NAME                    | 'swipechain'                                                       |
| ~/.swipechain/.env                                              |                                     | CORE_TOKEN                           | Ѧ                                                           |
