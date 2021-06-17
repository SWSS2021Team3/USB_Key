* make rsa key with encryption
(passphrase: cmuteam3)
openssl genrsa -aes256 -out rootca.key.enc 2048
openssl genrsa -aes256 -out server.key.enc 2048
openssl genrsa -aes256 -out videodb.key.enc 2048
openssl genrsa -aes256 -out facedb.key.enc 2048
openssl genrsa -aes256 -out userdb.key.enc 2048
openssl genrsa -aes256 -out clientconf.key.enc 2048
openssl genrsa -aes256 -out client.key.enc 2048

* make decypted rsa key
openssl rsa -in rootca.key.enc -out rootca.key
openssl rsa -in server.key.enc -out server.key
openssl rsa -in clientconf.key.enc -out clientconf.key
openssl rsa -in videodb.key.enc -out videodb.key
openssl rsa -in facedb.key.enc -out facedb.key
openssl rsa -in userdb.key.enc -out userdb.key
openssl rsa -in client.key.enc -out client.key

* certificate request file (refer provied crt files. openssl x509 -in [input.crt] -text -noout)
openssl req -new -key server.key -out server.csr
openssl req -new -key videodb.key -out videodb.csr
openssl req -new -key facedb.key -out facedb.csr
openssl req -new -key userdb.key -out userdb.csr
openssl req -new -key clientconf.key -out clientconf.csr
openssl req -new -key client.key -out client.csr

* serial file and making certificate
echo -n '00' > serial
openssl x509 -req -days 356 -extensions v3_ca -CAserial serial -in rootca.csr -signkey rootca.key -out rootca.crt
openssl x509 -req -days 90 -extensions v3_user -in server.csr -CA rootca.crt -CAserial serial -CAkey rootca.key -out server.crt
openssl x509 -req -days 90 -extensions v3_user -in videodb.csr -CA rootca.crt -CAserial serial -CAkey rootca.key -out videodb.crt
openssl x509 -req -days 90 -extensions v3_user -in facedb.csr -CA rootca.crt -CAserial serial -CAkey rootca.key -out facedb.crt
openssl x509 -req -days 90 -extensions v3_user -in userdb.csr -CA rootca.crt -CAserial serial -CAkey rootca.key -out userdb.crt
openssl x509 -req -days 90 -extensions v3_user -in clientconf.csr -CA rootca.crt -CAserial serial -CAkey rootca.key -out clientconf.crt
openssl x509 -req -days 90 -extensions v3_user -in client.csr -CA rootca.crt -CAserial serial -CAkey rootca.key -out client.crt

* signing
openssl cms -sign -in clientconf.bin -signer clientconf.crt -inkey clientconf.key -outform DER -out clientconf.sign -binary

* verification
openssl cms -verify -in clientconf.sign -inform DER -CAfile rootca.crt -content clientconf.bin -binary

* encrypt/decrypt
openssl enc -aes-128-cbc -iv `cat userdb.iv | xxd -p`  -in [inputfile] -out [outputfile] -nosalt -K `cat userdb.cipherkey | xxd -p`
openssl dec -aes-128-cbc -iv `cat userdb.iv | xxd -p`  -in [inputfile] -out [outputfile] -nosalt -K `cat userdb.cipherkey | xxd -p`
