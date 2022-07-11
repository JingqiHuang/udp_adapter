# Udp Adapter


This adapter is to allow multiple SMFs to communicate with UPF 

## Adapter Overview

This adapter sits between SMF and UPF (Arch: SMF <-> UDP Adapter <-> UPF).

For SMF
- It acts as a HTTP client/server to SMF
- It receives HTTP packets with PFCP message information from SMFs
- It retrieves PFCP message information
- It reassembles the corresponding PFCP message

For UPF
- It sends the reassambled PFCP message to UPF
