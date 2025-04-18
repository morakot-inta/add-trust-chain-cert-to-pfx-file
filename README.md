# 🛠️ Adding a certificate trust chain to a PFX file
## 🔍 Step 1: Extract the Certificate and Private Key from the PFX
```bash
# Set your original PFX file
pfx_file="original.pfx"

# Extract the private Key (no certificates)
openssl pkcs12 -in $pfx_file -nocerts -out private.pem -nodes

# Extract the certificate (no private key)
openssl pkcs12 -in $pfx_file -clcerts -nokeys -out public.pem
```

## Download cert
```
ca_url=$(openssl x509 -in public.pem -text -noout | grep 'CA Issuers' | awk '{print $4}' | sed 's/URI://g')
wget $ca_url
```
## Creating the new pfx file
```
ca_name=$(basename http://certificates.godaddy.com/repository/gdig2.crt)
new_pfx_name="updated.pfx"
openssl pkcs12 -export \
  -inkey private.pem \
  -in public.pem \
  -certfile $ca_name \
  -out updated.pfx
```
