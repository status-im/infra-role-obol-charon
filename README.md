# Description

This role deployes [Charon Distributed Validator Middleware](https://github.com/ObolNetwork/charon) created by [Obol Collective](https://obol.org/).

It is an HTTP middleware client for Ethereum Staking that enables you to safely run a single validator across a group of independent nodes.

# Configuration

Basic config requires endpoints of Exec and Consensus layer nodes.
```yaml
obol_charon_beacon_node_urls: ['http://localhost:8545']
obol_charon_execution_client_rpc_url: ['http://localhost:9300']
obol_charon_validator_api_port: 3600
obol_charon_monitoring_port: 3620
```
The Validator API port is what the Validator Client needs to connect to in order to allow Charon to relay requests to the Beacon Node.

# Management

Service runs via `systemd`:
```
 > sudo systemctl status obol-charon
● obol-charon.service - Obol Distributed Validator Charon Middleware
     Loaded: loaded (/etc/systemd/system/obol-charon.service; enabled; vendor preset: enabled)
     Active: active (running) since Tue 2025-08-12 11:47:56 UTC; 1h 19min ago
       Docs: https://github.com/ObolNetwork/charon
    Process: 956069 ExecStart=/usr/local/bin/charon run --log-color=disable --log-format=json --log-level=debug --beacon-node-endpoints=http://loc>
   Main PID: 956069 (code=exited, status=1/FAILURE)
        CPU: 24ms
```

# Security

The role [generates a private key](https://docs.obol.org/run-a-dv/start/create-a-dv-with-a-group?q=private+keyk#step-1-get-your-enr) that is saved as `.charon/charon-enr-private-key` in the node working directory.

This key is used to generate an ENR address that is then used to [accept an invite to an Obol DVT cluster](https://docs.obol.org/run-a-dv/start/create-a-dv-with-a-group?q=join#step-2-create-a-cluster-or-accept-an-invitation-to-a-cluster). It is also saved in `enr` file in the service directory.
