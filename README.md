# hnd1-kwo

🐙 Kubernetes manifests for DR availablity zone

## Initalizing the cluster

### Prerequisites

* Reachability to tun.y2e.org (192.168.30.0/24) and assigned hostname within this network.
* Running Debian 13 (trixie).
* Reachability to the Internet.
* At least 16GB RAM.

### Setup k3s worker

```
$ ansible-playbook -i "hnd1kwoX.tail5b1c5.ts.net," playbooks/init-worker.yaml
```

### Setup k3s agent

* [クイックスタートガイド | K3s](https://docs.k3s.io/ja/quick-start)
    ```
    $ TUN_IFACE=`ip -4 route get 192.168.30.1 | awk 'NR==1 {print $3}'` \
    TUN_ADDR=`ip -4 -o addr show dev ${TUN_IFACE} scope global primary | awk '{split($4,a,"/"); print a[1]}'` \
    TUN_VIP_ADDR=$(sed -n '/virtual_ipaddress[[:space:]]*{/,/}/p' /etc/keepalived/keepalived.conf \
  | awk '$1 ~ /^[0-9]+\./ {sub(/\/.*/, "", $1); print $1; exit}')
    $ curl -sfL https://get.k3s.io | sh -s - server --server https://${TUN_VIP_ADDR}:6443 \
    --token <redacted> \
    --node-name `cat /etc/hostname` \
    --node-ip ${TUN_ADDR} \
    --flannel-iface ${TUN_IFACE} \
    --tls-san ${TUN_VIP_ADDR} \
    --tls-san ${TUN_ADDR} \
    --tls-san hnd1kwo0.tun.y2e.org \
    --tls-san hnd1-kwo.tail5b1c5.ts.net \
    --tls-san hnd1-tailscale-operator.tail5b1c5.ts.net
    ```
    * `token`: 他の worker で `cat /var/lib/rancher/k3s/server/token`
    * `Y`: set up target node
    * `N`: set up target node

## License

MIT
