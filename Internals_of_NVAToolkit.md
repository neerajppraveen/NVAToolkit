
# Internals of NVAToolkit

NVAToolkit is a tool for detecting malicious traffic, which analyzes incoming network traffic or stored traffic in pcap format to determine its maliciousness. It uses 80 flow features as input to our ML model. 

![Logo](https://github.com/neerajppraveen/NVAToolkit/blob/ca5f1b5ca1dcf05bb6d6640885b4b8a733d269fa/nvatoolkit_image.png?raw=true)


## Working

It consists of two servers.

     Main app server

     Feature server

The user interacts with the main app server, which consists of a UI module. There are two modes of operation supported by NVAToolkit.

    Analysis of pcap

    Live packet capture and analysis

In first mode, pcap files are forwarded to the features server via FSBM (Feature server binding module), which interacts with fetaures server and drops invalid pcap files. Communication between two servers is using the REST API. The feature server consists cicflowmeter and CSV Verification Module. CICFlowmeter converts pcap to a csv file, which contains 80 flow features. The CSV Verfication module drops irrelevent columns from pcap (e.g., source ip, dest ip, source port, and low_id—these factors are not relevant with respect to the ML model, but these features are printed if traffic is malicious). Then modified CSV was sent to the ML model, which predicts type of attack and it was displayed on the UI. 

In second mode, the Live Capture server captures traffic on an interface specified by the user at a specified time (default: 40 seconds). Then captured traffic is stored as pcap, then forward it into the feature server. The rest of the operation similar to the first mode.



