<div align="center">

# ⚡ Onyx Gate — C++ Integration Example

[![Language](https://img.shields.io/badge/Language-C++17-00599C?style=for-the-badge&logo=cplusplus&logoColor=white)](https://isocpp.org)
[![Platform](https://img.shields.io/badge/Platform-Windows-0078D6?style=for-the-badge&logo=windows&logoColor=white)](https://microsoft.com/windows)
[![IDE](https://img.shields.io/badge/IDE-Visual%20Studio%202022-5C2D91?style=for-the-badge&logo=visualstudio&logoColor=white)](https://visualstudio.microsoft.com)
[![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)](LICENSE)

<br/>

**A lightweight, zero-dependency C++ SDK and Dear ImGui authentication UI for [Onyx Gate](https://auth.script-kittens.com).**

</div>

---

## 🚀 Overview

This example demonstrates how to integrate Onyx Gate licensing into:
1. **Console Applications / Loaders** via `loader.cpp`
2. **DirectX / OpenGL GUI Applications (Dear ImGui)** via `imgui_auth.h`

### ✨ Features
- 🔒 **Zero-Dependency SDK** — Native Win32 API network calls (`WinINet` / `WinHTTP`), no `vcpkg`, no `cURL`, no `boost`.
- 🛡️ **Hardware ID (HWID) Locking** — Automatically captures machine fingerprint to prevent credential sharing.
- 🔑 **Key Redemption & Plan Gates** — Seamless verification of tier permissions (`IsPaid()`, expiry checks).
- 🎨 **Drop-in ImGui UI** — Ready-to-use login and key redemption modal.

---

## 📂 Project Structure

```text
├── SKAuth.h               # Core C++ SDK (drop-in single header)
├── imgui_auth.h           # Ready-to-use Dear ImGui Auth Window
├── loader.cpp             # CLI loader & console entrypoint example
├── skStr.h                # Compile-time string encryption helper
├── storage.hpp            # Local token & session caching
├── OnyxGate-CPP.sln       # Visual Studio 2022 Solution
└── OnyxGate-CPP.vcxproj   # Visual Studio Project File
```

---

## ⚡ Quick Start

### 1. Requirements
- Visual Studio 2022 with **Desktop development with C++**
- Windows 10 / 11 SDK (included with Visual Studio)

### 2. Setup
1. Clone the repository:
   ```bash
   git clone https://github.com/fegrial/Onyx-Gate-CPP-Example.git
   ```
2. Open `OnyxGate-CPP.sln` in Visual Studio 2022.
3. In `loader.cpp`, update `APP_ID` with your Application ID from the Onyx Gate Dashboard:
   ```cpp
   constexpr const char* APP_ID = "YOUR_APP_ID_HERE";
   ```
4. Build in **Release | x64** and run!

---

## 💻 Code Examples

### 1. Simple Console Authentication
```cpp
#include "SKAuth.h"

int main() {
    SKAuth auth("YOUR_APP_ID", "1.0");

    auto res = auth.Login("username", "password");
    if (res.ok) {
        std::cout << "[+] Logged in! Plan: " << res.plan << std::endl;
        std::cout << "[+] Expires: " << res.expires_at << std::endl;
        
        if (auth.IsPaid()) {
            // Launch protected feature
        }
    } else {
        std::cerr << "[-] Login failed: " << res.message << std::endl;
    }
    return 0;
}
```

### 2. ImGui Cheat Integration (3 lines)
```cpp
#include "imgui_auth.h"

// 1. In your cheat initialization (e.g. DllMain):
AuthSystem::Init("YOUR_APP_ID");

// 2. In your render loop:
AuthSystem::Render();

// 3. Gate features behind login / subscription:
if (AuthSystem::IsLoggedIn()) {
    RenderCheatMenu();
    
    if (AuthSystem::IsPaid()) {
        RenderVIPFeatures();
    }
}
```

---

## 🛡️ API Reference

| Method | Description |
| :--- | :--- |
| `SKAuth(app_id, version)` | Initializes auth client instance with App ID & version. |
| `auth.Login(user, pass)` | Authenticates user with credentials and checks HWID. |
| `auth.Register(user, pass, email, key)` | Registers new account with an optional license key. |
| `auth.RedeemKey(key)` | Extends or activates subscription plan via license key. |
| `auth.IsPaid()` | Returns `true` if active user holds a paid tier. |
| `auth.GetHWID()` | Generates SHA256 hardware identifier. |

---

<div align="center">
<sub>Powered by <b>Onyx Gate</b> • Maintained by <a href="https://github.com/fegrial">fegrial</a></sub>
</div>
