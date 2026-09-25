# redis (ot-container-kit redis-operator)

Misskey 用の Redis。nrt1 と同じく `ot-operators/redis-operator` を Helm で入れ、
`Redis` CR `ot-operators/redis` を置く。

## Install

* operator (Helm)
    ```
    $ helm repo add ot-helm https://ot-container-kit.github.io/helm-charts/
    $ helm upgrade --install redis-operator ot-helm/redis-operator \
        --namespace ot-operators --create-namespace --wait
    ```
* Redis
    ```
    $ kubectl kustomize manifests/redis/ > /dev/null
    $ kubectl apply -k manifests/redis/
    ```

## Memo

* nrt1 の Redis CR は Git 管理されていない。image / 資源量 / 永続化設定を nrt1 に
  揃える場合は、nrt1 で `kubectl get redis -n ot-operators redis -o yaml` の spec を
  確認すること。DR の同期 (dump-restore) には hnd1 の Redis バージョンが nrt1 以上で
  あることが必要 (`misskey-dr doctor` / `misskey-dr redis status` で確認できる)。
* Misskey の設定 (`manifests/misskey/default.yml`) はパスワードなしで接続するため、
  `kubernetesConfig.redisSecret` は設定しない。
