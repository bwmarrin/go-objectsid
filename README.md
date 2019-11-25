go-objectsid
====
[![Discord Gophers](https://img.shields.io/badge/Discord%20Gophers-%23info-blue.svg)](https://discord.gg/0f1SbxBZjYq9jLBk)

go-objectsid is a [Go](https://golang.org/) package for decoding binary
Active Directory ObjectSID value into a human-readable string format.

**For help with this package or general Go discussion, please join the [Discord 
Gophers](https://discord.gg/0f1SbxBZjYq9jLBk) chat server.**

## Status
I just wrote this after finding a few examples in other languages.  It seems to 
work based on my minimal testing.  Please report any bugs you find.
  
### Binary ObjectSID Format
```
byte[0]   - Revision Level
byte[1]   - count of Sub-Authorities
byte[2-7] - 48 bit Authority (big-endian)
byte[8+]  - n Sub-Authorities, 32 bits each (little-endian)
```

### String ObjectSID Format
`S-{Revision}-{Authority}-{SubAuthority1}-{SubAuthority2}...-{SubAuthorityN}`



## Getting Started

### Installing

This assumes you already have a working Go environment, if not please see
[this page](https://golang.org/doc/install) first.

```sh
go get github.com/bwmarrin/snowflake
```


### Usage

Import the package into your project then call the Decode function with a
binary objectSID.  It will return a SID object that contains each individual 
component of the SID along with a String() method to generate a properly formatted
SID string. 


**Example Program:**

```go
package main

import (
	"encoding/base64"
	"fmt"

	"github.com/bwmarrin/go-objectsid"
)

func main() {
	// A objectSID in base64
	// This is just here for the purpose of an example program
	b64sid := `AQUAAAAAAAUVAAAArC22DNydmGz4WUTnUAQAAA==`

	// Convert the above into binary form. This is the value you would
	// get from a LDAP quary on AD.
	bsid, _ := base64.StdEncoding.DecodeString(b64sid)

	// Decode the binary objectsid into a SID object
	sid := objectsid.Decode(bsid)

	// Print out just one component of the SID
	fmt.Println(sid.Authority)

	// Print out the relative identifier
	fmt.Println(sid.RID())

	// Print the entire ObjectSID
	fmt.Println(sid)
}
```

### Credit
This was wrote using the below page as a reference.

http://www.adamretter.org.uk/blog/entries/active-directory-ldap-users-primary-group.xml

### Reference
This page has a lot of useful information on the SID format.

https://docs.microsoft.com/en-us/windows/security/identity-protection/access-control/security-identifiers
