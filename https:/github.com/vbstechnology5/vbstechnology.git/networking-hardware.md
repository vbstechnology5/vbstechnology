# Networking Hardware

Layer 1

Modem

Hub



Layer 2

Switch

Wireless Access Point



Layer 2/3

MLS (Multilayer Switch)



Layer 3

Router





Firewall

Can be software based, or its own device



Functions on layers 2 3 4 and 7

A firewall can block packets from entering or leaving the network, and it does this through one of two methods. It can perform stateless inspection, in which the firewall examines every packet that enters or leaves the networks against a set of rules. Once the packet matches a rule, the specified action is taken. Alternatively, it may use stateful inspection, where the firewall only examines the state of a connection between networks, specifically when a connection is made from an internal network to an external network. The firewall does not examine any packets returning from the external connection; it only cares about the state of the connection. As a general rule, external connections are not allowed to be initiated with the internal network. Now, firewalls are the first line of defense in protecting the internal network from outside threats; you can consider the firewall as the police force of the network.

Then there is the intrusion detection system (IDS). An IDS is a passive system designed to identify when a network breach or attack against the network is occurring. IDS is usually designed to inform a network administrator when a breach or attack has occurred, and it does this through log files, text messages, and email notifications. IDS cannot prevent or stop a breach or attack on its own. The IDS receives a copy of all traffic and evaluates it against a set of standards, which may be signature-based, evaluating network traffic for known malware or attack signatures. Alternatively, the standard may be anomaly-based, evaluating network traffic for suspicious changes, or it may be policy-based, evaluating network traffic against a specific declared security policy. An IDS may be deployed at the host level; when deployed at the host level, it's called a host-based intrusion detection system (HIDS), which is more potent than the intrusion detection system.

The intrusion prevention system (IPS) is more potent than the intrusion detection system. The IPS is an active system designed to stop a breach or attack from succeeding and damaging the network. IPS is usually designed to perform an action or set of actions to stop malicious activity and inform a network administrator.
