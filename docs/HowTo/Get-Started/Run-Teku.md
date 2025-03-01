---
title: Start Teku
sidebar_position: 2
---

# Start Teku

You can run Teku as a beacon node and validator in a single process, or as separate processes.

We recommend you run the beacon node and validator as a [single process] if they are to run on the same machine.

:::note

By default, Teku connects to `mainnet`. Use the [`--network`](../../Reference/CLI/CLI-Syntax.md#network) command line option to specify an alternate network.

:::

## Prerequisites

- [Teku installed](Installation-Options/Install-Binaries.md).
- [An execution client synced](Connect/Connect-To-Mainnet.md#2-start-the-execution-client).
- [Validator keystores and password files](Connect/Connect-To-Mainnet.md#3-generate-validator-keys-and-stake-eth).

## Start the clients in a single process

Start the beacon node and validator as a single process by specifying the validator options using the [`teku`](../../Reference/CLI/CLI-Syntax.md#options) command.

```bash title="Example"
teku \
  --ee-endpoint=http://localhost:8551                       \
  --ee-jwt-secret-file=jwtsecret.hex                        \
  --metrics-enabled=true                                    \
  --rest-api-enabled=true                                   \
  --validators-proposer-default-fee-recipient=0xFE3B557E8Fb62b89F4916B721be55cEb828dBd73 \
  --validator-keys=validator/keys/validator_888eef.json:validator/passwords/validator_888eef.txt
```

Use the [`--validator-keys`](../../Reference/CLI/CLI-Syntax.md#validator-keys) option to specify the directories or files to load the encrypted keystore file(s) and associated password file(s) from.

## Run the clients separately

Validators must connect to a beacon node to publish attestations or propose blocks. The beacon node requires internet access, but the connected validators can run on machines without internet access.

### Start the beacon node

Run Teku as a beacon node.

```bash title="Example"
teku \
    --ee-endpoint=http://localhost:8551 \
    --ee-jwt-secret-file=jwtsecret.hex  \
    --metrics-enabled=true              \
    --rest-api-enabled=true
```

Specify [`--rest-api-enabled`](../../Reference/CLI/CLI-Syntax.md#rest-api-enabled) to allow validators to connect to the beacon node.

:::caution

Don't pass the validator keys as a command line option to both the beacon node and validator client. This can cause a [slashable offense].

:::

By default, [validator clients] can connect to the beacon node at `http://127.0.0.1:5051`. Use the [`--rest-api-interface`](../../Reference/CLI/CLI-Syntax.md#rest-api-interface) and [`--rest-api-port`](../../Reference/CLI/CLI-Syntax.md#rest-api-port) options to update the address.

You can specify [`--rest-api-host-allowlist`](../../Reference/CLI/CLI-Syntax.md#rest-api-host-allowlist) to allow access to the REST API from specific hostnames.

### Start the validator

To run a validator, connect to a [running beacon node].

Use the [`validator-client`](../../Reference/CLI/Subcommands/Validator-Client.md#validator-client-vc) or [`vc`](../../Reference/CLI/Subcommands/Validator-Client.md#validator-client-vc) subcommand to run a Teku as a validator.

```title="Example"
teku validator-client \
    --beacon-node-api-endpoint=http://192.10.10.101:5051,http://192.140.110.44:5051 \
    --validator-keys=validator/keys:validator/passwords
```

:::warning

Ensure that the validator keys are only provided to the validator. Don't pass the validator keys as command line options to both the beacon node and validator client. This can a cause a [slashable offense].

:::

Specify one or more beacon nodes using the [`--beacon-node-api-endpoint`](../../Reference/CLI/Subcommands/Validator-Client.md#beacon-node-api-endpoint-beacon-node-api-endpoints) option.

:::info

You can supply multiple beacon node endpoints to the validator. The first one is used as the primary node, and others as failovers.

:::

## Confirm Teku is running

Use the [`/liveness`](https://consensys.github.io/teku/#operation/getTekuV1AdminLiveness) endpoint to check whether the node is up.

The endpoint returns the status `200 OK` if the node is up or syncing.

<!--tabs-->

# curl HTTP request

```bash
curl -I -X GET "http://192.10.10.101:5051/teku/v1/admin/liveness"
```

# Result

```bash
HTTP/1.1 200 OK
Date: Fri, 05 Feb 2021 03:58:30 GMT
Server: Javalin
Content-Type: application/json
Cache-Control: max-age=0
Content-Length: 0
```

<!--/tabs-->

<!-- links -->

[validator clients]: #start-the-validator
[running beacon node]: #start-the-beacon-node
[Validator keystores]: Connect/Connect-To-Testnet.md#generate-the-validators-and-send-the-deposits
[password files]: Connect/Connect-To-Testnet.md#create-a-password-file-for-each-validator-key
[slashable offense]: ../../Concepts/Slashing-Protection.md
[single process]: #start-the-clients-in-a-single-process
