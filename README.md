# Using a Firewall to Secure Linux

Using a firewall to secure a Linux system involves setting up rules that control incoming and outgoing network traffic based on predefined security parameters. Here's a step-by-step guide:

---

## 1. Choose a Firewall Tool

Linux systems typically include firewall tools. Common ones are:

- **iptables**: Low-level, powerful but complex to configure.
- **ufw (Uncomplicated Firewall)**: User-friendly, ideal for beginners.
- **firewalld**: Dynamic and versatile, used in distributions like Fedora and CentOS.
- **nftables**: Modern replacement for iptables, simpler and more efficient.

Choose one based on your needs and system compatibility.

---

## 2. Install the Firewall Tool

If your chosen firewall isn't installed, you can install it using your package manager:

- **Debian/Ubuntu (ufw):**
  ```bash
  sudo apt update
  sudo apt install ufw
