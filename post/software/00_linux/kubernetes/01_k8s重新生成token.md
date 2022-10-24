---
title:  centos7 离线安装 kubernets (k8s)
date: 2022-04-23 15:07:32
categories: ['']
tags: ['', '', '']
draft: false

---

# k8s 重新生成 token

---

k8s 安装之后会自动生成一个有效期为 24h 的 token，超时之后就需要重新生成新的 token

## 方法

**生成 token**

```shell
kubeadm token create
```

**获取ca证书 hash 值**

```shell
openssl x509 -pubkey -in /etc/kubernetes/pki/ca.crt | openssl rsa -pubin -outform der 2>/dev/null | openssl dgst -sha256 -hex | sed 's/^.* //'
```

** 查看 token **

```shell
kubeadm token list
```

上述步骤完成之后就可以添加新的 node 节点到 master 了

```shell
kubeadm join <master_ip>:6443 --token <TOKEN> --discovery-token-ca-cert-hash  share256:<HASH>
```

