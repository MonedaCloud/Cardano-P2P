# Cardano P2P Deployment

The below instruction covers how to deploy P2P functionality to Cardano relay nodes and disable legacy manual Topology-Updater functionality.

## P2P Configuration

terminal:~$ `cd $CNODE_HOME/files`

terminal:~$ `mv topology.json topology.json.bk`

terminal:~$ `wget https://book.world.dev.cardano.org/environments/mainnet/topology-p2p.json`

terminal:~$ `mv topology-p2p.json topology.json`

### Add block producer and second relay to the localRoots section:

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
            "port": 6001
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
