# Generate Certificates with CA

### 1- Commands to generate Certificates

```
openssl genrsa -out ca.key 4096

openssl req -new -x509 -days 365 -key ca.key -subj "/C=IN/ST=KA/L=BLR/O=PhonePE /CN=PhonePE Root CA " -out ca.crt

openssl req -newkey rsa:4096 -nodes -keyout server.key -subj "/C=IN/ST=KA/L=BLR/O=PhonePE/CN=phonepeaerospike" -out server.csr

openssl x509 -req -extfile <(printf "subjectAltName=DNS:phonepeaerospike,IP:10.57.14.26, IP:10.57.14.57") -days 365 -in server.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out server.crt
```

### 2- Command to check certificate details

```
openssl x509 -in /etc/aerospike/ssl/ca.crt -noout -text
openssl x509 -in /etc/aerospike/ssl/server.crt -noout -text
```