# Cardano P2P Deployment

The below instruction covers how to deploy P2P functionality to Cardano relay nodes and disable legacy manual Topology-Updater functionality.

**IMPORTANT**: P2P on mainnet is only intended to be ran on a single relay at this time. (v1.35.7) Continue to run any other relays with P2P disabled using the legacy topology.

## P2P Configuration

terminal:~$ `cd $CNODE_HOME/files`

terminal:~$ `mv topology.json topology.json.bk`

terminal:~$ `wget https://book.world.dev.cardano.org/environments/mainnet/topology-p2p.json`

OR

terminal:~$ `wget https://github.com/MonedaCloud/Cardano-P2P/blob/main/topology-p2p.json`

terminal:~$ `mv topology-p2p.json topology.json`

### Add block producer and second relay to the localRoots section:

#Check our sample topology.json in the repository section for guidance.

terminal:~$ `vi topology.json`

```
  "localRoots": [
    {
      "accessPoints": [
        {
            "address": "BlockProducerIP",
            "port": 6001
        },
        {
            "address": "SecondRelayIP",
            "port": 6002
        }
      ],
      "advertise": false,
      "valency": 1
    }
  ],
  ```

terminal:~$ `cd $CNODE_HOME/files`

### Add P2P flag configuration to the config.json cofiguration file:

terminal:~$ `vi config.json`

`"EnableP2P": true,`

#There are additional settings that can be added to the the config.json file. Check those here.[^1].

## Disable legacy Topology-Updater functionality:

terminal:~$ `cd $CNODE_HOME/scripts`

terminal:~$ `chmod -x topologyUpdater.sh`

terminal:~$ `sudo systemctl stop cnode-tu-fetch.service`

terminal:~$ `sudo systemctl stop cnode-tu-restart.timer`

terminal:~$ `sudo systemctl stop cnode-tu-restart.service`

terminal:~$ `sudo systemctl disable cnode-tu-fetch.service`

terminal:~$ `sudo systemctl disable cnode-tu-restart.timer`

terminal:~$`sudo systemctl disable cnode-tu-restart.service`

terminal:~$`sudo systemctl restart cnode.service`

## References:

[^1]: https://github.com/input-output-hk/cardano-node/releases/tag/1.35.6

[^2]: https://github.com/input-output-hk/cardano-node/blob/master/doc/getting-started/understanding-config-files.md

[^3]: https://book.world.dev.cardano.org/environments.html

[^4]: https://docs.cardano.org/explore-cardano/cardano-network/p2p-networking

[^5]: https://iohk.io/en/blog/posts/2023/03/16/dynamic-p2p-is-coming-to-cardano/

[^6]: https://forum.cardano.org/t/docs-and-procedures-for-p2p-how-to-run-p2p/107087/12

[^7]: https://cardano-community.github.io/guild-operators/Scripts/topologyupdater/
