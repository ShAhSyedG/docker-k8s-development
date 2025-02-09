# Use CFSSL to generate certificates

More about [CFSSL here]("https://github.com/cloudflare/cfssl")

```

cd hashicorp\vault-2022\tls

docker run -it --rm -v ${PWD}:/work -w /work debian bash

sudo apt-get update && apt-get install -y curl &&
curl -L https://github.com/cloudflare/cfssl/releases/download/v1.6.1/cfssl_1.6.1_linux_amd64 -o /usr/local/bin/cfssl && \
curl -L https://github.com/cloudflare/cfssl/releases/download/v1.6.1/cfssljson_1.6.1_linux_amd64 -o /usr/local/bin/cfssljson && \
sudo chmod +x /usr/local/bin/cfssl && \
sudo chmod +x /usr/local/bin/cfssljson

#generate ca in /tmp
cfssl gencert -initca ca-csr.json | cfssljson -bare /tmp/ca

#generate certificate in /tmp
cfssl gencert \
  -ca=/tmp/ca.pem \
  -ca-key=/tmp/ca-key.pem \
  -config=ca-config.json \
  -hostname="vault,vault.vault.svc.cluster.local,vault.vault.svc,51.21.132.229,172.31.25.233,16.171.231.146,172.31.17.109" \
  -profile=default \
  ca-csr.json | cfssljson -bare /tmp/vault
```

view the files:

```
ls -l /tmp
```

access the files:

```
mv /tmp/* .
```