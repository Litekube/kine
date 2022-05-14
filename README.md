# Kine (Kine is not etcd)
==========================

_NOTE: this repository has been recently (2020-11-19) moved out of the github.com/rancher org to github.com/k3s-io
supporting the [acceptance of K3s as a CNCF sandbox project](https://github.com/cncf/toc/pull/447)_.

---

Kine is an etcdshim that translates etcd API to sqlite, Postgres, Mysql, and dqlite

## run
* HTTP

  * start server
    ```shell
    ./kine-server --listen-address="0.0.0.0:2379"
    ```
  
  * validate
  
    ```shell
    curl http://{host_ip}:2379/version
    
    # get
    {"etcdserver":"3.5.0","etcdcluster":"3.5.0"}
    ```

* HTTPS

  * install cfssl，refer to [script](https://github.com/Litekube/info/blob/main/aim1/scripts/install-cfssl.sh)

  * generate certificates，refer to [script](https://github.com/Litekube/info/blob/main/aim1/scripts/generate-kine-cert.sh)

  * start server
    ```shell
    ./kine-server --listen-address="0.0.0.0:2379" --server-cert-file=kine-cert/server.pem  --server-key-file=kine-cert/server-key.pem
    ```

  * validate

    ```shell
    curl -k https://{host_ip}:2379/version --cert kine-cert/client.pem --key kine-cert/client-key.pem
    
    # get
    {"etcdserver":"3.5.0","etcdcluster":"3.5.0"}

## Features
- Can be ran standalone so any k8s (not just k3s) can use Kine
- Implements a subset of etcdAPI (not usable at all for general purpose etcd)
- Translates etcdTX calls into the desired API (Create, Update, Delete)
- Backend drivers for dqlite, sqlite, Postgres, MySQL
