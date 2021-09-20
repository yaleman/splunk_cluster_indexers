# splunk_indexer_config

Splunk cluster indexer configuration - see https://github.com/yaleman/splunk_cluster_core for more information.

The configuration for storage in master-apps/_cluster on the cluster master.

Splunk Documentation here: https://docs.splunk.com/Documentation/Splunk/8.0.2/Indexer/Managecommonconfigurations

The contents of this repo should end up in `$SPLUNK_HOME/etc/master-apps` on the cluster master, which replicates to the indexers in the cluster.

i.e.: `git clone git@github.com:/yaleman/splunk_cluster_indexers /opt/splunk/etc/master-apps/`

Search this repo for `CHECKTHIS` and update the required configurations.

There should be a *very* simple configuration in `_cluster`. From the docs:

> Note the following:
> * The `/_cluster` directory is a special location for configuration files that need to be distributed across all peers:
> * The `/_cluster/default` subdirectory contains a default version of indexes.conf. Do not add any files to this directory and do not change any files in it. This peer-specific default indexes.conf has a higher precedence than the standard default indexes.conf, located under `$SPLUNK_HOME/etc/system/default`.
> * The `/_cluster/local` subdirectory is where you can put new or edited configuration files that you want to distribute to the peers.

Any any files in the root directory of this repo won't be distrubuted to the peers.

# Updating the config

1. On the CM, pull the config:
```
sudo su splunk
cd /opt/splunk/etc/master-apps/
git pull
```
2. Validate the new configuration:
```
/opt/splunk/bin/splunk validate cluster-bundle
```
This command returns a message confirming that bundle validation has started. In certain failure conditions, it also indicates the cause of failure.

3. To validate the bundle and check whether a restart is necessary, include the --check-restart parameter. This version of the command first validates the bundle. If validation succeeds, it then checks whether a peer restart is necessary.
```
/opt/splunk/bin/splunk validate cluster-bundle --check-restart
```

4. To view the status of bundle validation, run the splunk show cluster-bundle-status command:
```
/opt/splunk/bin/splunk show cluster-bundle-status
```
5. Once you've checked the above and there's no errors, apply the bundle. This'll ask about a rolling restart, which is what we'll need to do.
```
/opt/splunk/bin/splunk apply cluster-bundle
```
