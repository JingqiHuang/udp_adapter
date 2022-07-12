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


- UDP Adapter Code
    - Use Same SMF codebase to create one more executable. like udp-adapter
    
    - create 2 executable. smf (existing), udp_adapter (new)
    
    - Helm chart changes to start udp adapter from smf container image

UDP Pod Data structures
    - Unique incremental sequence number : assign for every new request message
    
    - Keep transaction table at UDP pod: sequence number ==> trans struct


UDP-SMF Message Structure:

    - PFCP Encoded Message
    
    - UFP Identifier

Incoming Messages from SMF Pod:
    - Send PFCP Association ? How ? This has relation with config at UDP Pod.
    
        - Do we want config downloaded to UDP adapter? [No]
        
        - Send PFCP association when SMF sends first message towards UPF
        
        - UPF association modify/delete can be done later stages
        
        - UDP Pod can assign sequence number for pfcp association request message

    - Request Message Received fron SMF Pod
    
        - If association not formed then first initiated association and then only send actual request message
        
        - UDP Pod can assign sequence number for every new request message.
        
        - Create transaction.
        
        - Send Request message towards UPF

    - Response Message Received from UPF
    
        - Locate transaction based on pfcp sequence number
        
        - Post response message on SMF http thread
        
        - If association response is received then go through outstanding queued message and dispatch them towards UPF
