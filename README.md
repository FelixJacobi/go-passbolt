# go-pbolt

<p align="center">
  <img src="https://raw.githubusercontent.com/pbolt-dev/.github/main/assets/pbolt-logo.svg" alt="pbolt logo" width="140" height="140" />
</p>

<p align="center">
  <strong>Fast, robust Go SDK and client library compatible with Passbolt.</strong>
</p>

<p align="center">
  <a href="https://pkg.go.dev/github.com/passbolt/go-passbolt"><img src="https://pkg.go.dev/badge/github.com/passbolt/go-passbolt.svg" alt="Go Reference" /></a>
  <a href="https://github.com/pbolt-dev/go-pbolt-cli"><img src="https://img.shields.io/badge/CLI-go--pbolt--cli-0bddec?style=flat-square&logo=go" alt="go-pbolt-cli" /></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/license-MIT-fdae15?style=flat-square" alt="License: MIT" /></a>
</p>

---

## ⚡ Overview

**`go-pbolt`** is a Go client SDK to interact with [Passbolt](https://www.passbolt.com/), an open-source password manager for teams. Both Passbolt Community and Pro server editions are supported.

There is also a companion CLI tool: [**`go-pbolt-cli`**](https://github.com/pbolt-dev/go-pbolt-cli).

This module is divided into two primary packages:
- **`api`**: Low-level client providing direct access to the Passbolt REST API endpoints.
- **`helper`**: High-level convenience functions executing multi-step cryptographic and administrative workflows (e.g. creating/decrypting resources, sharing passwords with users/groups, resolving folders).

---

## 🔒 API Compatibility

The Go module is designed to work with current Passbolt API versions.

> [!IMPORTANT]
> Compatibility is continuously tested against Passbolt API 5.6.1 and later.

---

## 💻 Installation

```bash
go get github.com/passbolt/go-passbolt
```

---

## 💡 Quick Example

```go
package main

import (
	"context"
	"fmt"

	"github.com/passbolt/go-passbolt/api"
	"github.com/passbolt/go-passbolt/helper"
)

const (
	address      = "https://passbolt.example.com"
	userPassword = "yourStrongPassword"
	userPrivateKey = `-----BEGIN PGP PRIVATE KEY BLOCK-----
...
-----END PGP PRIVATE KEY BLOCK-----`
)

func main() {
	ctx := context.TODO()

	client, err := api.NewClient(nil, "", address, userPrivateKey, userPassword)
	if err != nil {
		panic(err)
	}

	if err := client.Login(ctx); err != nil {
		panic(err)
	}
	defer client.Logout(ctx)
	fmt.Println("Successfully authenticated with Passbolt server!")

	// Create a new password resource
	resourceID, err := helper.CreateResource(
		ctx,
		client,
		"",                   // Folder ID (empty for root)
		"Database Server",   // Name
		"dbuser",            // Username
		"https://db.local",  // URI
		"s3cur3P@ssw0rd",    // Password
		"Production primary database credentials",
	)
	if err != nil {
		panic(err)
	}
	fmt.Printf("Created resource ID: %s\n", resourceID)
}
```

---

## ⚖️ Trademark Notice

*Passbolt is a registered trademark of Passbolt SA. **pbolt** is an independent community project and is not affiliated with, endorsed by, or sponsored by Passbolt SA.*

---

## 📄 License

This project is licensed under the [MIT License](LICENSE).
