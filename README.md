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

## 🌐 Step 2: Download the Intermediate Certificate
```
# Extract the CA Issuer URL from the certificate
ca_url=$(openssl x509 -in public.pem -text -noout | grep 'CA Issuers' | awk '{print $4}' | sed 's/URI://g')

# Download the intermediate certificate
wget $ca_url
```

## 🧬 Step 3: Combine into a New PFX File
```
# Get the filename of the downloaded certificate
ca_name=$(basename "$ca_url")

# Define the output file name
new_pfx_name="updated.pfx"

# Create a new PFX file with the intermediate cert
openssl pkcs12 -export \
  -inkey private.pem \
  -in public.pem \
  -certfile "$ca_name" \
  -out "$new_pfx_name"
```

## ✅ Result
You now have an updated PFX file named updated.pfx that includes:

# 🔐 The private key

# 📄 The public certificate

# 🧾 The intermediate certificate (trust chain)
