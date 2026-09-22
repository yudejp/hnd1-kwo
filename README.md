# k8s.dr

🐙 Kubernetes manifests for DR availablity zone

## Initalizing the cluster

### Prerequisites

* Reachability to tun.y2e.org (192.168.30.0/24) and assigned hostname within this network.
* Running Debian 13 (trixie).
* Reachability to the Internet.
* At least 16GB RAM.

### Setup k3s worker

```
$ ansible-playbook -i "hnd1kwoX.tun.y2e.org," playbooks/init-worker.yaml
```

### Setup k3s agent

* [クイックスタートガイド | K3s](https://docs.k3s.io/ja/quick-start)
    ```
    $ curl -sfL https://get.k3s.io | sh -s - server --server https://192.168.30.244:6443 --token <redacted> --node-name hnd1kwoN --node-ip 192.168.30.Y --flannel-iface <nic name> --tls-san 192.168.30.244 --tls-san 192.168.30.X --tls-san hnd1kwo0.tun.y2e.org --tls-san hnd1-kwo.tail5b1c5.ts.net --tls-san hnd1-tailscale-operator.tail5b1c5.ts.net
    ```
    * `token`: 他の worker で `cat /var/lib/rancher/k3s/server/token`
    * `X`: other k3s worker node
    * `Y`: set up target node
    * `N`: set up target node

## License

MIT
