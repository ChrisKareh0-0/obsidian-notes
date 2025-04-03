
---

## **Phase 1: Virtual Environment Setup

### **Step 1: Install VirtualBox**

- Download VirtualBox for macOS from [https://www.virtualbox.org](https://www.virtualbox.org)
- Install using the `.dmg` file and follow the setup wizard.

### **Step 2: Set Up Virtual Machines**

- **Kali Linux**: [Download ISO](https://www.kali.org/get-kali/)
- **Ubuntu**: [Download ISO](https://ubuntu.com/download/desktop)
- **Metasploitable 2**: [Download VM](https://sourceforge.net/projects/metasploitable/)

Create each VM in VirtualBox:

1. Allocate ~2GB RAM and at least 20GB disk.
2. Use **Bridged Adapter** for network (Settings > Network > Adapter 1 > Bridged Adapter).
3. Mount ISO under Storage > Optical Drive for Kali and Ubuntu. For Metasploitable, just import the `.vmdk` disk if available.

### **Step 3: Confirm Network Communication**

- Boot all three VMs.
- In each VM, open terminal and run:
    
    ```bash
    ping <IP_ADDRESS_OF_OTHER_VM>
    ```
    
    Ensure all VMs can ping each other.


## **Phase 2: Install Network Security Tools (April 2025)**

### **On Ubuntu (Snort Machine)**

1. **Install Snort**:
    
    ```bash
    sudo apt update
    sudo apt install snort -y
    ```
    
2. **Test Snort**:
    
    ```bash
    sudo snort --daq afpacket -i eth0 -v -A fast
    ```
    

### **On Kali Linux (Attacker)**

1. **Install Nmap** (usually preinstalled):
    
    ```bash
    sudo apt update
    sudo apt install nmap -y
    ```
    

---

## **Phase 3: Traffic Simulation & Intrusion Detection (May 2025)**

### **Step 1: Download & Move Dataset**

On Ubuntu (Snort machine):

- Example: Download CICIDS2017 or KDD99 PCAP file.
- Move to home directory:
    
    ```bash
    mv dataset.pcap /home/ubuntu/
    ```
    

### **Step 2: Replay Traffic**

Install **tcpreplay**:

```bash
sudo apt install tcpreplay -y
sudo tcpreplay --intf1=eth0 /home/ubuntu/dataset.pcap
```

**Alternative (using Python & Scapy):**

```bash
sudo apt install python3-scapy -y
python3
```

Then in Python:

```python
from scapy.all import rdpcap, sendp
packets = rdpcap("/home/ubuntu/dataset.pcap")
for packet in packets:
    sendp(packet, iface="eth0", verbose=False)
print("Packets sent successfully!")
```

### **Step 3: Run Snort**

```bash
sudo snort -i eth0 -A fast -c /etc/snort/snort.conf
cat /var/log/snort/alert
```

---

## **Phase 4: Attack Simulation (June 2025)**

### **Step 1: Attack from Kali**

Use tools like:

```bash
nmap -sS <Metasploitable_IP>
msfconsole
```

Example attack:

```bash
use exploit/unix/ftp/vsftpd_234_backdoor
set RHOST <Metasploitable_IP>
run
```

### **Step 2: Observe on Snort**

Check Snort logs:

```bash
cat /var/log/snort/alert
```

---

## **Phase 5: Documentation & Presentation**

- Document:
    - Network topologies
    - Dataset used
    - Packet injection method
    - Snort alerts
    - Attack methods
    - Analysis and recommendations

Prepare your final report and slides for **June 10, 2025**.

---

If you want, I can help you:

- Write the Python script for packet sending
- Customize Snort rules
- Create your final report template or presentation

Let me know how you'd like to proceed next.