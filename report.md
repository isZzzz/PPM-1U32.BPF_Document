# 1. Vulnerability Overview
This report identifies a critical vulnerability in the PPM-1U32.BPF, which can lead to a Denial of Service (DoS) condition. The vulnerability occurs when the device receives a malformed request. Upon processing this specially crafted packet, the PPM-1U32.BPF becomes unresponsive to all network requests. Normal operation can only be restored through a manual power cycle.

# 2. Affected Versions
- **Vendor Name**: Siemens Building Technologies, BAU-NA
- **Vendor Identifier**: 7
- **Model Name**: PPM Series 1 BACnet ASC Controller
- **Firmware Revision**: Digital PPM V1.00

# 3. Steps to Reproduce the Vulnerability
- **Environment Setup**: Ensure the PPM-1U32.BPF device is in its default configuration and connected to a BACnet MS/TP network.
- **Packet Transmission**: Send the following malformed APDU (unknown APDU) to the device: `55ff0500370018090`.
- **Observation of Results**: (The address 0x03 is the PPM-1U32.BPF, and the address 0x37 is my fuzzer.)
  ![Packet Transmission Results](https://github.com/isZzzz/PPM-1U32.BPF_Document/blob/main/PPM_DoS.png)
  1. Initially, a request is sent to the target device, which responds normally.
  2. Subsequently, the mutated request is sent, marked as the 5543rd message in the sequence.
  3. Continuous requests to the target device yield no response after the transmission of the 5543rd message, demonstrating the DoS vulnerability.

# 4. Security Impact
The PPM device plays a pivotal role in connecting and managing hardware or software modules of specific devices or subsystems, such as sensors, actuators, and controllers, to the BACnet network. Its reliable operation is crucial for maintaining seamless communication within the network. This vulnerability renders the device unresponsive, which can severely impact network communication, especially in scenarios where constant network availability is critical.

# 5. PCAP File
The PCAP file demonstrating the change in device network communications following the transmission of the specific packet is available in the GitHub repository: [PPM_DoS.pcapng](https://github.com/isZzzz/PPM-1U32.BPF_Document/blob/main/PPM_DoS.pcapng)

# 6. Reporter
- **Name**: Qiguang Zhang
- **Email**: iszhzzzz@gmail.com
