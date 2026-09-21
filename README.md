# Swap Maker

[![License: MIT](https://shields.io)](https://opensource.org)
[![Language: Rust](https://shields.io)](https://rust-lang.org)

**NeonSwap** is a lightweight, high-performance Linux system utility designed to create, resize, activate, and deactivate swap space through a modern, easy-to-use graphical interface (GUI). 

Built with **Rust** and powered by **egui**, it eliminates the need for complex terminal commands, allowing both users and developers to manage virtual memory safely and efficiently.

---

## 🚀 Key Features Explained

### 1. Smart Drive & Disk Space Scanning
*   **What it does:** Automatically detects all available drives on your Linux system (`lsblk` and `df` parsing).
*   **How it helps:** It displays total RAM, current swap usage, and available disk space in real-time, helping you decide exactly where and how much swap space to allocate.

### 2. Intelligent Btrfs Detection (NOCOW Protection)
*   **What it does:** Automatically checks if your target directory is on a **Btrfs** filesystem.
*   **How it helps:** Btrfs normally uses Copy-on-Write (COW), which severely degrades swapfile performance. NeonSwap automatically runs `chattr +C` to disable COW and turns off compression on the swapfile, ensuring maximum performance without manual configuration.

### 3. Asynchronous Execution Runtimes (Zero GUI Lag)
*   **What it does:** Heavy system processes (like creating huge files via `fallocate` or `dd`, formatting with `mkswap`) run on background worker threads.
*   **How it helps:** The GUI remains completely smooth and responsive, even when allocating 16GB+ of swap space. You get a real-time progress update without window freezes.

### 4. Direct "Scan to Permanent" fstab Integration
*   **What it does:** Automatically updates the `/etc/fstab` file to ensure your swap space survives system reboots.
*   **How it helps:** It safely formats block partitions using unique hardware **UUIDs** rather than fragile generic names (like `/dev/sda1`), making your setup highly stable. It also automatically skips temporary layouts like `zram`.

### 5. Automated Backups & Safe Deactivation
*   **What it does:** Before making any modifications to critical system storage maps, it creates an automatic fallback backup at `/etc/fstab.bak.neonswap`.
*   **How it helps:** If you want to delete a swap space, clicking **Deactivate** safely turns off the swap space (`swapoff`), unlinks it from the boot menu, and completely wipes the file to reclaim your hard drive space.

### 6. Seamless Root Privilege Escalation
*   **What it does:** Integrates with native Linux `pkexec` (PolicyKit) authentication popups.
*   **How it helps:** Root access is required to modify storage. NeonSwap safely requests system privileges only when executing backend actions, keeping the core app runtime separate and highly secure.

---

## 🛠️ Tech Stack

*   **Logic Engine:** Built with **Rust Framework** (1.98.1+) for memory safety and raw hardware speed.
*   **GUI Framework:** Powered by **egui** & **eframe** (`egui_glow` GLES renderer) for an ultra-fast, smooth dark-theme interface.
*   **Display Compositor Support:** Native support for both modern **Wayland** display instances (`smithay-client-toolkit`) and legacy **X11** window instances (`x11rb`).
*   **Event Handling:** Asynchronous loop execution managed via thread-channels (`calloop`).

---

## ⚡ Quick Start & Build

### Prerequisites
Make sure your Linux machine has standard library runtimes installed:
*   Standard system libraries (`libc.so.6`, `libgcc_s.so.1`).
*   PolicyKit system permission framework (`pkexec`).

### Installation

```bash
# 1. Clone the repository framework
git clone https://github.com[your-username]/swap-activator.git
cd swap-activator

# 2. Build the optimized production release binary
cargo build --release

# 3. Launch the standalone application
./target/release/swap-activator
```

*Note: If you run into any permission or window compositor environment conflicts under standard user groups, simply launch directly with sudo privileges:*
```bash
sudo ./target/release/swap-activator
```

---

## 📄 License

Distributed under the MIT License. See `LICENSE` for more information.

---
Crafted with 💻, 🦀, and ☕ by [[Your Name](https://github.com[your-username])]
