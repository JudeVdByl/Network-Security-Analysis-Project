# Network Security Analysis Project

## Objective
Conduct a comprehensive network security analysis using Nmap and Wireshark to identify potential vulnerabilities and anomalies within the network, and provide recommended solutions to mitigate these issues.

## Skills Learned
- Network topology analysis
- Vulnerability assessment
- Network protocol analysis
- Cybersecurity best practices

## Tools Used
- **Nmap**: For network discovery and security auditing.
- **Zenmap**: GUI version of Nmap for visualizing network topology.
- **Wireshark**: For network protocol analysis and identifying anomalies.

## Steps

### 1. Network Topology Discovery with Nmap
I used Zenmap to scan the network range `10.168.27.0/24`. The network topology identified was a Star topology, consisting of the following devices:

![Network Topology](https://github.com/user-attachments/assets/f17308df-b19d-4318-9dee-84c944feb018)


| Host          | State | Open Ports | OS                                            |
| ------------- | ----- | ---------- | --------------------------------------------- |
| 10.168.27.1   | Up    | 0          | N/A                                           |
| 10.168.27.10  | Up    | 8          | Microsoft Windows Server 2012 or 2012 R2      |
| 10.168.27.14  | Up    | 1          | Linux 2.6.32                                  |
| 10.168.27.15  | Up    | 10         | Microsoft Windows Server 2008 R2 or Windows 8.1 |
| 10.168.27.20  | Up    | 1          | Linux 2.6.32                                  |
| 10.168.27.132 | Up    | 1          | Linux 2.6.32                                  |

### 2. Vulnerability Assessment with Nmap
Nmap scans revealed several vulnerabilities within the network. Below is a summary of these vulnerabilities along with their potential implications:

#### Vulnerability 1: Unencrypted HTTP Traffic
- **Host**: 10.168.27.15
- **Issue**: Port 80 is open, which is used for unencrypted HTTP traffic.
- **Implications**: Unencrypted traffic can lead to various attacks such as SQL injections, cross-site scripting, and more. Sensitive data transmitted over HTTP is vulnerable to interception.

![Nmap Scan for Port 80](screenshot2.png)

#### Vulnerability 2: Insecure FTP Traffic
- **Host**: 10.168.27.15
- **Issue**: Port 21 is open, used for FTP, which transmits data in clear text.
- **Implications**: Data transmitted via FTP can be easily intercepted, compromising confidentiality and integrity. This can expose sensitive information and network credentials.

![Nmap Scan for Port 21](screenshot3.png)

#### Vulnerability 3: End of Life Operating System
- **Host**: 10.168.27.15
- **Issue**: The operating system detected is Microsoft Windows Server 2008 R2 or Windows 8.1, both of which are no longer supported.
- **Implications**: Using an unsupported OS exposes the system to unpatched vulnerabilities, increasing the risk of exploitation over time.

![Nmap Scan for OS](screenshot4.png)

### 3. Anomaly Detection with Wireshark
Using Wireshark, I analyzed the `Pcap3.pcapng` file and identified the following anomalies:

#### Anomaly 1: Unencrypted HTTP Traffic
- **Description**: Credentials are being transmitted over unencrypted HTTP traffic. This includes sensitive information such as passwords, which were detected using the term "TCP contains password."
- **Implications**: The transmission of credentials in plain text severely compromises security, making it easy for attackers to intercept and misuse this information.

![Wireshark Capture - Unencrypted HTTP Traffic](screenshot5.png)

#### Anomaly 2: TCP ACKed Unseen Segment
- **Description**: The trace indicates a missing segment that was acknowledged by an ACK packet. This could be due to Wireshark's limitations in capturing all packets, leading to potential oversight of data.
- **Implications**: Missing packets can result in undetected vulnerabilities or incomplete analysis, leading to a false sense of security.

![Wireshark Capture - TCP ACKed Unseen Segment](screenshot6.png)

### 4. Recommendations for Mitigating Vulnerabilities and Anomalies

#### Vulnerability 1: Unencrypted HTTP Traffic
- **Recommendation**: Switch to HTTPS to encrypt data using Transport Layer Security (TLS). This protects data in transit from being intercepted by unauthorized parties.

#### Vulnerability 2: Insecure FTP Traffic
- **Recommendation**: Encrypt FTP traffic using Secure FTP (SFTP), which tunnels FTP over an encrypted SSH connection, or use a Virtual Private Network (VPN) to secure the data transfer.

#### Vulnerability 3: End of Life Operating System
- **Recommendation**: Isolate systems running unsupported OS versions from the rest of the network. Consider upgrading to a supported OS version to ensure regular security patches.

#### Anomaly 1: Unencrypted HTTP Traffic with Credentials
- **Recommendation**: Implement HTTPS across the network to ensure all sensitive data, including credentials, are encrypted during transmission.

#### Anomaly 2: TCP ACKed Unseen Segment
- **Recommendation**: Improve packet capture accuracy by using Berkeley Packet Filters (BPF) to focus on relevant traffic and reduce load on the capturing device. Also, consider using multiple capture systems to handle high traffic volumes.

