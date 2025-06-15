## Create root certificate authority
1. Generate Root CA private key
```bash
openssl genrsa -out rootCA.key 4096
```
2. Generate Root CA certificate (self-signed)
```bash
openssl req -x509 -new -nodes -key rootCA.key -sha256 -days 3650 -out rootCA.crt
```
## Create intermediate CA(optional)
1. 
```bash
openssl genrsa -out intermediateCA.key 4096
```
2. Create certificate signing request CSR
```bash
openssl req -new -key intermediateCA.key -out intermediateCA.csr
```
3. Use Root CA to sign the Intermediate CA cert
```bash
cd ~/pki/root
openssl x509 -req -in ../intermediate/intermediateCA.csr -CA rootCA.crt -CAkey rootCA.key -CAcreateserial -out ../intermediate/intermediateCA.crt -days 1825 -sha256
```
## Create Server Certificate Signed by Intermediate CA
1. 
```bash
openssl genrsa -out server.key 2048
```
2. Create CSR for the server
```bash
openssl req -new -key server.key -out server.csr
```
3. Sign server certificate using Intermediate CA
```bash
openssl x509 -req -in ../server/server.csr -CA intermediateCA.crt -CAkey intermediateCA.key -CAcreateserial -out ../server/server.crt -days 825 -sha256
```
## Build Certificate Chain and Verify
`cat server.crt ../intermediate/intermediateCA.crt > server-chain.crt
` 