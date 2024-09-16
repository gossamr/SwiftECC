# Key Management

Creating new keys and loading exsisting keys

## 

### Create new Keys

Given a domain, you can generate new public / private key pairs.

#### Example

```swift
let domain = Domain.instance(curve: .EC384r1)
let (pubKey, privKey) = domain.generateKeyPair()
```

The private key is simply a random positive integer less than the domain order.
The public key is the domain generator point multiplied by the private key.

Given a private key, say `privKey`, you can generate the corresponding public key, like

```swift
let pubKey = ECPublicKey(privateKey: privKey)
```

Given a domain, say `dom` and a curve point, say `pt`, you can generate a public key, like

```swift
let pubKey = try ECPublicKey(domain: dom, w: pt)
```

### Load existing Keys

You can load existing keys from their PEM- or DER encodings.

#### Example

```swift
// Public key encoding - EC384r1 domain
let pubKeyPem =
"""
-----BEGIN PUBLIC KEY-----
MHYwEAYHKoZIzj0CAQYFK4EEACIDYgAEQW/MahMwMTFjwY95uOEdfBVC7HrQhTGGTwxiPlgDiARq
C6y6EQ1Ajkuhe4A02WOltRYQRXKytzspOR25UfgtagURAwxVFYzR9cmi6FRmvvq/Tsigd/dAi4FN
jniR7/Pg
-----END PUBLIC KEY-----
"""
let pubKey = try ECPublicKey(pem: pubKeyPem)

// Private key encoding in PKCS#8 format - EC384r1 domain
let privKeyPem =
"""
-----BEGIN PRIVATE KEY-----
MIG/AgEAMBAGByqGSM49AgEGBSuBBAAiBIGnMIGkAgEBBDBmpNziSYmGoWwl7apJM9ZdDBxkJqmx
MScHGXG45ZQXSv7fIuJlsSwxK76nUiiO7gigBwYFK4EEACKhZANiAARBb8xqEzAxMWPBj3m44R18
FULsetCFMYZPDGI+WAOIBGoLrLoRDUCOS6F7gDTZY6W1FhBFcrK3Oyk5HblR+C1qBREDDFUVjNH1
yaLoVGa++r9OyKB390CLgU2OeJHv8+A=
-----END PRIVATE KEY-----
"""
let privKey = try ECPrivateKey(pem: privKeyPem)

// See the key ASN1 structures
print(pubKey)
print(privKey)
```

giving:

```swift
Sequence (2):
  Sequence (2):
    Object Identifier: 1.2.840.10045.2.1
    Object Identifier: 1.3.132.0.34
  Bit String (776): 00000100 01000001 01101111 11001100 01101010 00010011 00110000 00110001 00110001 01100011 11000001 10001111 01111001 10111000 11100001 00011101 01111100 00010101 01000010 11101100 01111010 11010000 10000101 00110001 10000110 01001111 00001100 01100010 00111110 01011000 00000011 10001000 00000100 01101010 00001011 10101100 10111010 00010001 00001101 01000000 10001110 01001011 10100001 01111011 10000000 00110100 11011001 01100011 10100101 10110101 00010110 00010000 01000101 01110010 10110010 10110111 00111011 00101001 00111001 00011101 10111001 01010001 11111000 00101101 01101010 00000101 00010001 00000011 00001100 01010101 00010101 10001100 11010001 11110101 11001001 10100010 11101000 01010100 01100110 10111110 11111010 10111111 01001110 11001000 10100000 01110111 11110111 01000000 10001011 10000001 01001101 10001110 01111000 10010001 11101111 11110011 11100000

Sequence (4):
  Integer: 1
  Octet String (48): 66 a4 dc e2 49 89 86 a1 6c 25 ed aa 49 33 d6 5d 0c 1c 64 26 a9 b1 31 27 07 19 71 b8 e5 94 17 4a fe df 22 e2 65 b1 2c 31 2b be a7 52 28 8e ee 08
  [0]:
    Object Identifier: 1.3.132.0.34
  [1]:
    Bit String (776): 00000100 01000001 01101111 11001100 01101010 00010011 00110000 00110001 00110001 01100011 11000001 10001111 01111001 10111000 11100001 00011101 01111100 00010101 01000010 11101100 01111010 11010000 10000101 00110001 10000110 01001111 00001100 01100010 00111110 01011000 00000011 10001000 00000100 01101010 00001011 10101100 10111010 00010001 00001101 01000000 10001110 01001011 10100001 01111011 10000000 00110100 11011001 01100011 10100101 10110101 00010110 00010000 01000101 01110010 10110010 10110111 00111011 00101001 00111001 00011101 10111001 01010001 11111000 00101101 01101010 00000101 00010001 00000011 00001100 01010101 00010101 10001100 11010001 11110101 11001001 10100010 11101000 01010100 01100110 10111110 11111010 10111111 01001110 11001000 10100000 01110111 11110111 01000000 10001011 10000001 01001101 10001110 01111000 10010001 11101111 11110011 11100000
```