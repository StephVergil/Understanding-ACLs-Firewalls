# Understanding ACLs & Firewalls: A Hands-On Guide

This guide provides an in-depth exploration of **Access Control Lists (ACLs)** and **Firewalls**. It explains the concepts of stateful and stateless firewalls, demonstrates practical configurations with **IPTables**, and teaches users how to manage network traffic effectively.

---

## Project Links

- [Understanding ACLs & Firewalls Part 1](https://github.com/StephVergil/Understanding-ACLs-Firewalls/blob/main/Understanding%20ACLs%20%26%20Firewalls%20part%201.docx)
- [Understanding ACLs & Firewalls Part 2](https://github.com/StephVergil/Understanding-ACLs-Firewalls/blob/main/Understanding%20ACLs%20Firewalls%20part%202.docx)

---

## Overview

### What Are Firewalls?

Firewalls are security systems that monitor and control network traffic based on predefined security rules. There are two main types:
- **Stateful Firewalls**: These track active connections and apply rules based on the context of the connection (e.g., IPTables, Windows Firewall).
- **Stateless Firewalls**: These evaluate each packet independently, applying rules without considering connection state, prioritizing speed over context.

### Objectives of the Lab

1. Understand the differences between stateful and stateless firewalls.
2. Configure **IPTables** to block, allow, and track network traffic.
3. Analyze network activity using tools like `nmap` and `netcat`.
4. Learn to create and manage firewall rules effectively.

---

## Key Concepts

### **1. Stateful vs. Stateless Firewalls**
- **Stateful Firewalls**: Maintain a table of active connections and analyze traffic context. They provide robust security by recognizing and handling established or related traffic.
- **Stateless Firewalls**: Apply rules packet-by-packet without tracking state, making them faster but less secure.

### **2. IPTables**
IPTables is a Linux-based firewall utility used to manage and configure network traffic rules. It supports:
- **Chains**: INPUT, OUTPUT, FORWARD.
- **Rules**: Specific conditions to allow or block traffic.
- **Protocols**: TCP, UDP, ICMP, and more.

### **3. Connection States**
Traffic in IPTables can be in one of the following states:
- **NEW**: Initiating a new connection.
- **ESTABLISHED**: Part of an active two-way connection.
- **RELATED**: Associated with an existing connection (e.g., FTP).
- **INVALID**: Cannot be identified or is part of an unknown connection.

---

## Step-by-Step Guide

### Part 1: Basic IPTables Configuration

#### **1.1 Exploring IPTables**
1. Launch the Ubuntu machine and log in as `sysadmin`.
2. Open the terminal and view IPTables options:
   ```bash
   iptables -h
   ```
3. Check detailed documentation:
   ```bash
   man iptables
   ```

   **Why?** These commands familiarize you with IPTables and its capabilities.

#### **1.2 Clearing and Viewing Rules**
1. View existing IPTables rules:
   ```bash
   sudo iptables -L
   ```
2. Flush all current rules:
   ```bash
   sudo iptables -F
   ```
3. Confirm the chains are empty:
   ```bash
   sudo iptables -L
   ```

   **Why?** Starting with a clean slate ensures old rules do not interfere with new configurations.

#### **1.3 Setting Basic Rules**
1. Block all incoming, outgoing, and forwarded traffic:
   ```bash
   sudo iptables -P INPUT DROP
   sudo iptables -P OUTPUT DROP
   sudo iptables -P FORWARD DROP
   ```
2. Allow traffic on the loopback interface:
   ```bash
   sudo iptables -A INPUT -i lo -j ACCEPT
   sudo iptables -A OUTPUT -o lo -j ACCEPT
   ```

   **Why?** These rules establish a secure "deny all" baseline and selectively allow necessary traffic.

---

### Part 2: Advanced IPTables Management

#### **2.1 Saving and Managing Rules**
1. Save rules to a file:
   ```bash
   sudo iptables-save > /etc/iptables/rules.v4
   ```
2. View detailed rule information:
   ```bash
   sudo iptables -n -v -L --line-numbers
   ```

   **Why?** Saving rules ensures they persist after a reboot, and detailed views aid in debugging.

#### **2.2 Deleting and Disabling Rules**
1. Delete a specific rule by its line number:
   ```bash
   sudo iptables -D INPUT 7
   ```
2. Disable the firewall by flushing all rules:
   ```bash
   sudo iptables -F
   ```

   **Why?** Deleting or disabling rules allows flexibility for testing and troubleshooting.

#### **2.3 Testing Open Ports**
1. Open port 5000 on Ubuntu:
   ```bash
   ncat -4 --keep-open --listen --source-port 5000
   ```
2. Connect to the port from Kali:
   ```bash
   ncat 192.168.1.4 5000
   ```
3. Check the IPTables hit counter:
   ```bash
   sudo iptables -vL INPUT
   ```

   **Why?** Testing open ports verifies rule configurations and ensures proper traffic handling.

---

## Tools and Commands

### Tools Used
- **IPTables**: Firewall rule configuration.
- **Ncat**: Network tool for testing connections.
- **Nmap**: Scanning tool for identifying open ports.

### Important Commands
- **Viewing Rules**: `sudo iptables -L`
- **Flushing Rules**: `sudo iptables -F`
- **Saving Rules**: `sudo iptables-save > /etc/iptables/rules.v4`
- **Adding Rules**:
  ```bash
  sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT
  ```
- **Deleting Rules**:
  ```bash
  sudo iptables -D INPUT 3
  ```

---

## Resources

- [IPTables Documentation](https://netfilter.org/projects/iptables/)
- [Ncat Guide](https://nmap.org/ncat/)
- [Nmap Reference](https://nmap.org/book/)
- [Linux Man Pages](https://man7.org/linux/man-pages/)

---

## Key Takeaways

1. **Stateful vs. Stateless**: Understand when to use each firewall type based on security and performance needs.
2. **Secure Configurations**: Learn to build "deny all" baselines and selectively allow necessary traffic.
3. **Monitoring Traffic**: Use tools like `ncat` and `nmap` to validate firewall rules and ensure functionality.

---

### Disclaimer
This project was conducted in a controlled environment. Unauthorized use of these techniques or tools outside such an environment may violate ethical guidelines and legal regulations.

---

> **Author**: Stephanie Vergil
