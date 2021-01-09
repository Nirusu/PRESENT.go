# PRESENT.go
## **This is a fork based which allows to set an adjustable number of rounds for research purposes. This is obviously INSECURE when used without the default number of rounds, which is 31. You properly may not want to use this, but rather [the original](github.com/yi-jiayu/PRESENT.go) if you do not need a "broken" cipher for research.**

## About
Go implementation of the PRESENT ultra-lightweight block cipher as defined by Bogdanov et al. [1].

Not to be confused with the Go presentation tool [package](https://godoc.org/golang.org/x/tools/present) and [command](https://godoc.org/golang.org/x/tools/cmd/present).

## Usage
This package implements the [cipher.Block](https://golang.org/pkg/crypto/cipher/#Block) interface from [crypto/cipher](https://golang.org/pkg/crypto/cipher/), so it can be used with the block cipher modes implemented there, just like the [crypto/aes](https://godoc.org/crypto/aes) package.

### Example
One of the test vectors from the PRESENT paper:
```go
package main

import (
	"encoding/hex"
	"fmt"
	"log"

	"github.com/Nirusu/PRESENT.go"
)

func encodeHex(data []byte) string {
	dst := make([]byte, hex.EncodedLen(len(data)))
	hex.Encode(dst, data)
	return string(dst)
}

func main() {
	key := make([]byte, 10)
	fmt.Printf("%-10s : %s\n", "Key", encodeHex(key))
	cipher, err := present.NewCipher(key, 31) // Adjust 31 to choose how many rounds you want to use
	if err != nil {
		log.Fatal(err)
	}
	plaintext := make([]byte, 8)
	fmt.Printf("%-10s : %s\n", "Plaintext", encodeHex(plaintext))
	ciphertext := make([]byte, 8)
	cipher.Encrypt(ciphertext, plaintext)
	fmt.Printf("%-10s : %s\n", "Ciphertext", encodeHex(ciphertext))
}
```

Output:
```
Key        : 00000000000000000000
Plaintext  : 0000000000000000
Ciphertext : 5579c1387b228445
```

## References 
1. Bogdanov A. et al. (2007) PRESENT: An Ultra-Lightweight Block Cipher. In: Paillier P., Verbauwhede I. (eds) Cryptographic Hardware and Embedded Systems - CHES 2007. CHES 2007. Lecture Notes in Computer Science, vol 4727. Springer, Berlin, Heidelberg ([pdf](http://www.lightweightcrypto.org/present/present_ches2007.pdf))
