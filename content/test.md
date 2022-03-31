+++
+++

# Docker environment variables
Here's a sample example of what environment variables in a `.env` file could look like :
```bash
BACKUP_PATH=/home/vincent/ethereum2/backup
IMPORT_PATH=/home/vincent/ethereum2/import
CONF_PATH=/home/vincent/ethereum2/conf
DOCKER_VOL_PATH=/mnt/disks/docker
PUBLIC_IP=34.79.131.148
GETH_FALLBACK_PROVIDER=https://goerli.infura.io/v3/f03d676810c84b5e8e7fbd107790104f
```

`BACKUP_PATH` is where data from the validator is exported. It must have `0700` permissions.
`IMPORT_PATH` is where data to import into a validator is stored. The tree is as such :
>/
>/keys/ `contains .json key files`
>/account_password `contains the password used for the account keys`
>/wallet_password	`contains the password used for the wallet`

`CONF_PATH` is where the configuration files for the metrics tools (prometheus, grafana) are stored
`DOCKER_VOL_PATH` is the path of the volume to be used by Docker
`PUBLIC_IP` is the WAN-visible IP of the container/VM, it is used for P2P in the Eth2 beacon
`GETH_FALLBACK_PROVIDER` is a third-party GEth endpoint to be used as fallback

# Geth
Just run `docker-compose up -d geth`.

# Beacon
Just run `docker-compose up -d beacon`. Beacon depends on geth, so you could just run beacon if you want to deploy both at once.

# Validator
Run with `docker-compose up -d validator`.

## Export accounts
Currently, export can't be fully automatized and you must go through the Prysm CLI,
but it's stlightly simplified here.

To export the accounts, use this command. You'll be prompted to enter the wallet password and asked to select the specific accounts you want to export.
`docker-compose run validator-export-accounts`

To export the slasher history, use :
`docker-compose run validator-export-slasher-history`

You can do **both at the same time** with `docker-compose run validator-export`

Accounts will be backed up in a `backup.zip` file, the slasher-history is backed up to a .json file.

## Import accounts
Just put the necessary files in the IMPORT_PATH as defined above, follow the tree structure and run `docker-compose run validator-import`.

## List accounts
`docker-compose validator-list-accounts`

## Delete accounts
`docker-compose run validator-delete-accounts` and select accounts by hand
