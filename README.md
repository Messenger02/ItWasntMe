# Network Intrusion Analysis: Investigating phishing attacks
### Tools: Wireshark, Azure Labs (Linux VM)
### Context: Completed as part of the CodePath Intermediate Cybersecurity Program
Exploring network traffic analysis using Wireshark. Inspired by learning from CodePath
## Project Overview
The Mission: I conducted a forensic analysis of captured network traffic (PCAP) to identify and document evidence of a malicious insider distributing spam and phishing emails within a simulated corporate environment.
### Environment & Security (OpSec)
Initially, I planned to perform this analysis in a local VMware environment. However, due to the presence of live malware samples within the PCAP files. Instead, I pivoted to a cloud-based Azure Lab courtesy of CodePath. This ensured isolation and safety, preventing any potential malware execution from impacting my host machine, a critical practice in professional digital forensics. 
### Methodology: The Investigation
To find the evidence across three PCAP files, I utilized the following workflow:
1. Protocol filtering: Since the objective involved email, I focused on the TCP transport layer and specifically filtered for SMTP, port 25, and DNS traffic to identify mail server communication.
2. Traffic Volumetrics: I analyzed the frequency of outgoing packets to identify anamalous spikes in traffic, which often indicate automated spam scripts.
3. Payload inspection: I performed deep packet inspection on the SMTP payloads to identify the specific social engineering themes. I successfully flagged three malicious subject lines: "I can destroy everything!", "your private data!", and "safe your privacy."
## Analysis & Findings
To identify the malicous activity, I moved beyond general TCP filters and focused on the SMTP (Simple Mail Transfer Protocol) layer. 
* The Filter: I utilized smtp.data.fragment to isolate the body and headers of the email transmissoin. this allowed me to bypass the handshake noise and focus on the actual payloads being sent through the network.
* The Source: I identified a single internal host at IP 10.6.1.104 generating an anomolous volume of outbound mail.
* The Evidence: upon inspecting the SMTP data, I discovered several highly suspicious subject lines used in a social engineering campaign:
    - "I can destroy everything!"
    - "your private data!"
    - "safe your privacy."
* The payload: While no suspicious attachments, like .exe or .zip file, were found, the body of the emails contained links to a suspicous external website, a classic indicator of credential harvesting or phishing attack.
## Challenges and solutions
* The Challenge: Initially, the volume of traffic in the three PCAP files were overwhelming. Viewing "all TCP" traffic made it difficult to distinguish between legitimate background noise and the specific malicious activity.
* The Solution: I consulted documentation and used advanced display filters. By narrowing the scope to smtp.data.fragments, I was able to reconstruct the "conversation" the attacker was having. This taught me that in a SOC environment, knowing the right filter is the difference between hours of manual searching and seconds in automated discovery.
## Next Steps
Future improvements: In a real world response, my next step would be to pivot from Wireshark to a tool like NetworkMiner to automatically extract URLs from the PCAP. I would then perform a WHOIS lookup and check the reputation of the suspicious URL on VirusTotal to see if it has been flagged by other security researchers."

## Acknowledgments
Big thanks to **[CodePath](https://www.codepath.org/)** for providing this project and valuable learning resources in cybersecurity!
