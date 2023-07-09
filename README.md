# 自己署名ルート認証局証明書
## CA の秘密鍵の生成
```bash
openssl genrsa -out ca.key 2048
```

## 証明書署名要求の生成
```bash
openssl req -new -sha256 -key ca.key -out ca.csr -config openssl.cnf
```

## 自己署名
```bash
openssl x509 -in ca.csr -days 365 -req -signkey ca.key -sha256 -out ca.crt -extfile ./openssl.cnf -extensions CA
```

# サーバ証明書の作成
## 秘密鍵の作成
```bash
openssl genrsa -out server.key 2048
```

## CSR の作成
```bash
openssl req -new -nodes -sha256 -key server.key -out server.csr -config openssl.cnf
```
### 注意点
commonName には localhost を指定

## 自己署名
```bash
openssl x509 -req -days 365 -in server.csr -sha256 -out server.crt -CA ca.crt -CAkey ca.key -CAcreateserial -extfile ./openssl.cnf -extensions Server
```

# クライアント証明書の作成
## 秘密鍵の作成
```bash
openssl genrsa -out client.key 2048
```

## CSR の作成
```bash
openssl req -new -nodes -sha256 -key client.key -out client.csr -config openssl.cnf
```

## 自己署名
```bash
openssl x509 -req -days 365 -in client.csr -sha256 -out client.crt -CA ca.crt -CAkey ca.key -CAcreateserial -extfile ./openssl.cnf -extensions Client
```