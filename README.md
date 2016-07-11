## Allow user to create DB and connect remotely

```
sudo -i -u postgres
psql
```

Run: `ALTER USER vagrant CREATEDB;`

```
sudo nano /etc/postgresql/9.3/main/postgresql.conf
```

Add: `listen_addresses = '*'`

```
sudo nano /etc/postgresql/9.3/main/pg_hba.conf
```

Add:

```
host    all             all             10.254.0.0/8            md5
```

```
sudo service postgresql restart
```

## Adding vagrant user’s SSH key for Rsync

vagrant ssh-config

cat /Users/neo/workspace/concourse-demo/.vagrant/machines/default/virtualbox/private_key | pbcopy

* copy to credentials.yml

mkdir caches

## Apply new pipeline

```
fly -t lite set-pipeline -p spotlight -c spotlight.yml --load-vars-from credentials.yml
fly -t lite unpause-pipeline -p spotlight
```