# Snort Installation and Configuration

Snort is a powerful, open-source network intrusion detection system (NIDS) capable of real-time traffic analysis and packet logging. This README file provides a step-by-step guide for installing and configuring Snort on a Linux system.

---

## Step 1: Update System Packages

ensure that your system packages are up-to-date.

```bash
sudo apt update && sudo apt upgrade -y
```

---

## Step 2: Install Dependencies

Snort requires several dependencies to function correctly. Install them using the following commands:

```bash
sudo apt install -y build-essential libpcap-dev libpcre3-dev libdnet-dev zlib1g-dev bison flex libssl-dev
```

## Step 3: Download and Install DAQ

Snort uses the DAQ (Data Acquisition) library for packet I/O. Download and install it using the command given below

```bash
wget https://www.snort.org/downloads/snort/daq-2.0.7.tar.gz
```

Extract and install DAQ:

```bash
tar -xvzf daq-2.0.7.tar.gz
cd daq-2.0.7
./configure && make && sudo make install
```

---

## Step 4: Install Snort

Download the latest version of Snort from the official website:

```bash
sudo apt install snort -y
```

Verify the Snort installation:

```bash
snort -V
```
### Download Rule Files

Obtain community rules or subscribe to Snort rules. Download the rules to `/etc/snort/rules`:

```bash
wget https://www.snort.org/downloads/community/community-rules.tar.gz -O /tmp/community-rules.tar.gz
sudo tar -xvzf /tmp/community-rules.tar.gz -C /etc/snort/rules
```

### Configure Snort

Edit the `snort.conf` file to match your environment:

```bash
sudo nano /etc/snort/snort.conf
```

Update the following variables:
- HOME_NET: Specify your internal network range (e.g., `var HOME_NET 192.168.1.0/24`)
- RULE_PATH: Set the path to your rules directory (e.g., `/etc/snort/rules`)

---

## Step 5: Test Snort

Run Snort in test mode to verify the configuration:

```bash
sudo snort -T -c /etc/snort/snort.conf
```

---

## Step 6: Run Snort

To run Snort in NIDS mode:

```bash
sudo snort -i eth0 -c /etc/snort/snort.conf -l /var/log/snort
```

Replace `eth0` with your network interface.

---

## Step 7: Automate Snort with Systemd (Optional)

Create a systemd service file for Snort:

```bash
sudo nano /etc/systemd/system/snort.service
```

Add the following content:

```ini
[Unit]
Description=Snort NIDS
After=network.target

[Service]
Type=simple
ExecStart=/usr/local/bin/snort -i eth0 -c /etc/snort/snort.conf -l /var/log/snort
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

Reload systemd and enable the service:

```bash
sudo systemctl daemon-reload
sudo systemctl enable snort.service
sudo systemctl start snort.service
```

Check Snort status:

```bash
sudo systemctl status snort.service
```

---

## Logs and Outputs

Snort logs will be stored in `/var/log/snort`. Use tools like `less`, `cat`, or `tail` to view logs:

```bash
tail -f /var/log/snort/alert
```

---

## References

- [Official Snort Documentation](https://www.snort.org/documents)
- [Community Rules](https://www.snort.org/downloads#rules)

---
