This activity was assigned to us as part of the Google Cybersecurity Professional Certificate program.

# Intro
In this activity, you will analyze DNS and ICMP traffic in transit using data from a network protocol analyzer tool. You will identify which network protocol was utilized in assessment of the cybersecurity incident. 

# Scenario
You are a cybersecurity analyst working at a company that specializes in providing IT services for clients. Several customers of clients reported that they were not able to access the client company website www.yummyrecipesforme.com, and saw the error “destination port unreachable” after waiting for the page to load. 

You are tasked with analyzing the situation and determining which network protocol was affected during this incident. To start, you attempt to visit the website and you also receive the error “destination port unreachable.” To troubleshoot the issue, you load your network analyzer tool, tcpdump, and attempt to load the webpage again. To load the webpage, your browser sends a query to a DNS server via the UDP protocol to retrieve the IP address for the website's domain name; this is part of the DNS protocol. Your browser then uses this IP address as the destination IP for sending an HTTPS request to the web server to display the webpage  The analyzer shows that when you send UDP packets to the DNS server, you receive ICMP packets containing the error message: “udp port 53 unreachable.” 

![image](https://github.com/L0rdB43lish/Analyse-Network-Layer-communication/blob/7349526b1fd0b010d27b06e04918700e4a6e5c01/atividade%20analyse%20network%20activity.jpg)

In the tcpdump log, you find the following information:

The first two lines of the log file show the initial outgoing request from your computer to the DNS server requesting the IP address of yummyrecipesforme.com. This request is sent in a UDP packet.

The third and fourth lines of the log show the response to your UDP packet. In this case, the ICMP 203.0.113.2 line is the start of the error message indicating that the UDP packet was undeliverable to port 53 of the DNS server.

In front of each request and response, you find timestamps that indicate when the incident happened. In the log, this is the first sequence of numbers displayed: 13:24:32.192571. This means the time is 1:24 p.m., 32.192571 seconds.

After the timestamps, you will find the source and destination IP addresses. In the first line, where the UDP packet travels from your browser to the DNS server, this information is displayed as: 192.51.100.15 > 203.0.113.2.domain. The IP address to the left of the greater than (>) symbol is the source address, which in this example is your computer’s IP address. The IP address to the right of the greater than (>) symbol is the destination IP address. In this case, it is the IP address for the DNS server: 203.0.113.2.domain. For the ICMP error response, the source address is 203.0.113.2 and the destination is your computers IP address 192.51.100.15.

After the source and destination IP addresses, there can be a number of additional details like the protocol, port number of the source, and flags. In the first line of the error log, the query identification number appears as: 35084. The plus sign after the query identification number indicates there are flags associated with the UDP message. The "A?" indicates a flag associated with the DNS request for an A record, where an A record maps a domain name to an IP address. The third line displays the protocol of the response message to the browser: "ICMP," which is followed by an ICMP error message.

The error message, "udp port 53 unreachable" is mentioned in the last line. Port 53 is a port for DNS service. The word "unreachable" in the message indicates the UDP message requesting an IP address for the domain "www.yummyrecipesforme.com" did not go through to the DNS server because no service was listening on the receiving DNS port.

The remaining lines in the log indicate that ICMP packets were sent two more times, but the same delivery error was received both times. 

Now that you have captured data packets using a network analyzer tool, it is your job to identify which network protocol and service were impacted by this incident. Then, you will need to write a follow-up report. 

As an analyst, you can inspect network traffic and network data to determine what is causing network-related issues during cybersecurity incidents. Later in this course, you will demonstrate how to manage and resolve incidents. For now, you only need to analyze the situation. 

This event, in the meantime, is being handled by security engineers after you and other analysts have reported the issue to your direct supervisor. 

# My response
<b>Part 1: Provide a summary of the problem found in the DNS and ICMP traffic log.</b>

Several customers reported being unable to access the website "www.yummyrecipesforme.com", receiving the error message "destination port unreachable" when attempting to load the page. By analysing network traffic using "tcpdump", it becomes evident that the more detailed error message is actually "udp port 53 unreachable". Port 53 is used for DNS communication, which suggests that the issue may be associated with a DNS resolution failure.

<b>Part 2: Explain your analysis of the data and provide at least one cause of the incident.</b>

The incident occurred early in the afternoon when various customers reported they were unable to access the website "www.yummyrecipesforme.com". Security engineers analysed the logs using "tcpdump", which indicate repeated ICMP responses from the DNS server, specifically stating "udp port 53 unreachable". The issue appears to stem from a network-level block, likely caused by a firewall rule preventing UDP traffic to port 53 on the DNS server. This block may have been implemented to prevent unauthorized DNS traffic or it could be the result of a misconfigured firewall rule.

The next step should be to review firewall rules to identify whether a rule is blocking UDP traffic on port 53 to the DNS server. If such a rule exists, further investigation is needed to determine if it was intentional or a misconfiguration. In the case of a misconfiguration, the rule should be modified to allow DNS traffic to pass through to the DNS server.

Additionally, perform DNS queries to alternate DNS servers, such as Google DNS (8.8.8.8), to verify if the issue is specific to the DNS server (203.0.113.2) or if it affects all DNS requests. If other DNS servers resolve queries without issues, this reinforces the theory that the issue lies with the specific DNS server or firewall misconfiguration.

If the firewall rule is confirmed as the cause, work with the network or security team to either adjust the firewall rule or whitelist the DNS server in question.
