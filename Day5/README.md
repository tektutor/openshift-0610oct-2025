# Day 5

## Post Assessment 
<pre>
https://forms.cloud.microsoft/r/u7eNcEn0Ee
</pre>

## Feedback 
<pre>
https://forms.cloud.microsoft/r/mmgzeDtkiV
</pre>

## Info - Security your Openshift Cluster and applications deployed in Openshift

#### Authentication & Authorization
<pre>
- Authentication
  - Could setup Identity providers (IDPs) to authenticate openshift login
  - LDAP, GitHub, GitLab, Google, HTPasswd, Keycloak (OIDC)
- ServiceAccounts vs User accounts
- Token and OAuth flows (oc whoami -t)

- Authorization
  - RBAC (Role-Based Access Control)
  - Roles, ClusterRoles, RoleBindings, ClusterRoleBindings
  - Custom Roles vs. default roles (admin, view, edit, etc.)
  - Least privilege principle
</pre>

#### Network Security
<pre>  
- Pod-to-Pod & Pod-to-Service
- NetworkPolicy (namespaced) and ClusterNetworkPolicy (cluster-wide)
- Default deny-all and zero-trust networking
- Controlling ingress/egress with egress firewall rules
- Ingress Security
- Routes and TLS termination types 
- Securing routes with certificates 
</pre>

#### Cluster Security & Hardening
<pre>  
- Security Context Constraints (SCC) – OpenShift equivalent of PodSecurityPolicies
- restricted, anyuid, privileged SCCs
- Creating custom SCCs
- Admission Controllers and Validating/Mutating Webhooks
- Pod Security Standards (Baseline, Restricted)
- Running containers as non-root
- Disabling privileged escalation
</pre>

#### Image & Supply Chain Security
<pre>  
- ImageStreams and trusted registries
- image signature verification (oc adm policy add-scc-to-user, oc adm signature)
- Image scanning (e.g., Red Hat Quay, Clair, Trivy)
- Content Trust and ImagePolicy admission
- Immutable images and reproducible builds
- Security scanning in CI/CD pipelines
</pre>

#### Secrets & Config Security
<pre>  
- Kubernetes Secrets vs ConfigMaps
- Encryption at rest with KMS providers (e.g., AWS KMS, Vault)
- Sealed Secrets (Bitnami SealedSecrets)
- External secret managers (HashiCorp Vault, CyberArk, AWS Secrets Manager)
- Securing environment variables in deployments
</pre>

#### Platform Access & API Security
<pre>  
- API server authentication and RBAC for oc and kubectl users
- OpenShift Console access restrictions
- API audit logging
- Certificate management for API server and etcd
- OAuth token lifetimes and rotation
</pre>

#### Compliance & Monitoring
<pre>  
- Compliance Operator (CIS Benchmark for OpenShift)
- OpenSCAP scans and remediation
- Audit logging and forwarding (e.g., to ELK or SIEM)
- Security events in oc get events and cluster logs
- Cluster-wide alerts for security breaches
</pre>

#### Security Tools & Operators
<pre>  
- Compliance Operator – CIS, NIST profiles
- Gatekeeper / OPA – policy as code
- ACS (Red Hat Advanced Cluster Security) – runtime threat detection, network graph
- Falco – behavioral threat detection
- Trivy – image scanning in CI/CD
</pre>

#### Secure Multitenancy
<pre>  
- Namespace isolation best practices
- Project quotas and limits
- Network segmentation per tenant
- RBAC delegation to team leads
- Resource constraints to prevent noisy neighbor issues
</pre>

#### Security Updates & Patch Management
<pre>  
- OpenShift upgrade process & channels (fast, stable)
- Node OS updates (RHCOS)
- Operator lifecycle management (OLM) security
- CVE tracking and remediation using Red Hat Insights
</pre>

#### Runtime & Workload Security
<pre>  
- Pod security context:
  - runAsUser, fsGroup, readOnlyRootFilesystem
- Dropping Linux capabilities
  - Seccomp profiles and AppArmor (on supported platforms)
- Container resource limits to prevent DoS
- Detecting crypto-mining and lateral movement
</pre>

#### Ingress/Egress & Perimeter Security
<pre>  
- Securing ingress with TLS, WAF integration (e.g., F5, HAProxy)
- Rate limiting & DoS protection
- Egress control with EgressFirewall and proxies
- Bastion hosts & jump boxes for controlled access
</pre>

#### Backup & Disaster Recovery (Security Angle)
<pre>  
- etcd encryption and backup security
- Secrets encryption during backups
- Immutable backup storage
- RBAC restrictions for restore operations
</pre>

#### Zero Trust Architecture
<pre>  
- Network segmentation
- Identity-aware access to workloads
- Mutual TLS between services
- Policy enforcement using OPA/Gatekeeper
- Just-in-time access control
</pre>

#### Best Practices & Standards
<pre>  
- CIS Benchmark for OpenShift
- NIST 800-53 alignment
- ISO 27001 security practices
- Regular pen-testing and red teaming
- Shift-left security in CI/CD
</pre>

## Flannel Network Model
<pre>
Flannel is a simple and easy way to configure a layer 3 network fabric designed for Kubernetes. 
It provides networking for container clusters by creating a virtual network that spans across all nodes.

Key Concepts
- Overlay Network
- Creates a virtual network layer on top of the existing host network
- Each node gets a subnet from a larger cluster network
- Pods can communicate across nodes using this overlay
- Network Backends
- VXLAN (Default)

Encapsulates packets in UDP
- Works across most network infrastructures
- Good performance with modern kernels

Host-Gateway
- Uses host routing tables
- Better performance but requires Layer 2 connectivity
- No packet encapsulation overhead

UDP
- Legacy backend
- User-space packet forwarding
- Lower performance but maximum compatibility
</pre>

#### Architecture
<pre>
┌─────────────────────────────────────┐    ┌─────────────────────────────────────┐
│              Node A                 │    │              Node B                 │
│          (10.0.1.100)               │    │          (10.0.1.101)               │
│                                     │    │                                     │
│ ┌─────────────┐    ┌─────────────┐  │    │ ┌─────────────┐    ┌─────────────┐  │
│ │    Pod1     │    │    Pod2     │  │    │ │    Pod3     │    │    Pod4     │  │
│ │             │    │             │  │    │ │             │    │             │  │
│ │ eth0:       │    │ eth0:       │  │    │ │ eth0:       │    │ eth0:       │  │
│ │10.244.1.10  │    │10.244.1.20  │  │    │ │10.244.2.10  │    │10.244.2.20  │  │
│ │             │    │             │  │    │ │             │    │             │  │
│ │ Route:      │    │ Route:      │  │    │ │ Route:      │    │ Route:      │  │
│ │ default via │    │ default via │  │    │ │ default via │    │ default via │  │
│ │10.244.1.1   │    │10.244.1.1   │  │    │ │10.244.2.1   │    │10.244.2.1   │  │
│ └─────────────┘    └─────────────┘  │    │ └─────────────┘    └─────────────┘  │
│        │                   │        │    │        │                   │        │
│        └───────┬───────────┘        │    │        └───────┬───────────┘        │
│                │                    │    │                │                    │
│         ┌─────────────┐             │    │         ┌─────────────┐             │
│         │   cni0      │             │    │         │   cni0      │             │
│         │ 10.244.1.1  │             │    │         │ 10.244.2.1  │             │
│         │ (bridge)    │             │    │         │ (bridge)    │             │
│         └─────────────┘             │    │         └─────────────┘             │
│                │                    │    │                │                    │
│         ┌─────────────┐             │    │         ┌─────────────┐             │
│         │  flannel.1  │             │    │         │  flannel.1  │             │
│         │ 10.244.1.0  │             │    │         │ 10.244.2.0  │             │
│         │ (vxlan)     │             │    │         │ (vxlan)     │             │
│         └─────────────┘             │    │         └─────────────┘             │
│                │                    │    │                │                    │
│         ┌─────────────┐             │    │         ┌─────────────┐             │
│         │    eth0     │             │    │         │    eth0     │             │
│         │ 10.0.1.100  │             │    │         │ 10.0.1.101  │             │
│         │ (physical)  │             │    │         │ (physical)  │             │
│         └─────────────┘             │    │         └─────────────┘             │
│                                     │    │                                     │
│ Routing Table (Node A):             │    │ Routing Table (Node B):             │
│ ┌─────────────────────────────────┐ │    │ ┌─────────────────────────────────┐ │
│ │ Destination    Gateway   Iface  │ │    │ │ Destination    Gateway   Iface  │ │
│ │ 10.244.1.0/24  0.0.0.0   cni0   │ │    │ │ 10.244.2.0/24  0.0.0.0   cni0   │ │
│ │ 10.244.2.0/24  10.244.2.0 fl.1  │ │    │ │ 10.244.1.0/24  10.244.1.0 fl.1  │ │
│ │ 10.244.0.0/16  0.0.0.0   fl.1   │ │    │ │ 10.244.0.0/16  0.0.0.0   fl.1   │ │
│ │ 10.0.1.0/24    0.0.0.0   eth0   │ │    │ │ 10.0.1.0/24    0.0.0.0   eth0   │ │
│ │ default        10.0.1.1  eth0   │ │    │ │ default        10.0.1.1  eth0   │ │
│ └─────────────────────────────────┘ │    │ └─────────────────────────────────┘ │
│                                     │    │                                     │
│ VXLAN FDB (Forwarding Database):    │    │ VXLAN FDB (Forwarding Database):    │
│ ┌─────────────────────────────────┐ │    │ ┌─────────────────────────────────┐ │
│ │ MAC Address       IP Address    │ │    │ │ MAC Address       IP Address    │ │
│ │ aa:bb:cc:dd:ee:02 10.0.1.101    │ │    │ │ aa:bb:cc:dd:ee:01 10.0.1.100    │ │
│ └─────────────────────────────────┘ │    │ └─────────────────────────────────┘ │
└─────────────────────────────────────┘    └─────────────────────────────────────┘
                     │                                           │
                     └─────────────────┬─────────────────────────┘
                                       │
                               Physical Network
                                (10.0.1.0/24)
</pre>

#### Packet Flow: Pod1 to Pod3 with Flannel VXLAN
Detailed Packet Capture Analysis
Scenario: Pod1 (10.244.1.10) pings Pod3 (10.244.2.10)
```
# Inside Pod1 - Original ICMP packet
tcpdump -i eth0 -nn icmp
```

Packet Structure:
<pre>
┌─────────────────────────────────────────────────────────────┐
│                    Ethernet Header                          │
├─────────────────────────────────────────────────────────────┤
│ Dst MAC: 02:42:0a:f4:01:01 (cni0 bridge)                    │
│ Src MAC: 02:42:0a:f4:01:0a (Pod1 eth0)                      │
│ EtherType: 0x0800 (IPv4)                                    │
├─────────────────────────────────────────────────────────────┤
│                      IP Header                              │
├─────────────────────────────────────────────────────────────┤
│ Version: 4, Header Length: 20 bytes                         │
│ Source IP: 10.244.1.10                                      │
│ Destination IP: 10.244.2.10                                 │
│ Protocol: 1 (ICMP)                                          │
│ TTL: 64                                                     │
├─────────────────────────────────────────────────────────────┤
│                     ICMP Header                             │
├─────────────────────────────────────────────────────────────┤
│ Type: 8 (Echo Request)                                      │
│ Code: 0                                                     │
│ Identifier: 1234                                            │
│ Sequence: 1                                                 │
│ Data: "Hello from Pod1"                                     │
└─────────────────────────────────────────────────────────────┘  
</pre>

Packet on Node A (before VXLAN encapsulation)
```
# On Node A - Capture on cni0 bridge
sudo tcpdump -i cni0 -nn icmp
# 15:30:45.123456 IP 10.244.1.10 > 10.244.2.10: ICMP echo request, id 1234, seq 1
```

VXLAN Encapsulated Packet on Physical Network
```
# On Node A - Capture on eth0 (physical interface)
sudo tcpdump -i eth0 -nn 'port 8472'
```

Encapsulated Packet Structure:
<pre>
┌─────────────────────────────────────────────────────────────┐
│                 Outer Ethernet Header                       │
├─────────────────────────────────────────────────────────────┤
│ Dst MAC: aa:bb:cc:dd:ee:02 (Node B eth0)                    │
│ Src MAC: aa:bb:cc:dd:ee:01 (Node A eth0)                    │
│ EtherType: 0x0800 (IPv4)                                    │
├─────────────────────────────────────────────────────────────┤
│                    Outer IP Header                          │
├─────────────────────────────────────────────────────────────┤
│ Version: 4, Header Length: 20 bytes                         │
│ Source IP: 10.0.1.100 (Node A)                              │
│ Destination IP: 10.0.1.101 (Node B)                         │
│ Protocol: 17 (UDP)                                          │
│ TTL: 64                                                     │
├─────────────────────────────────────────────────────────────┤
│                     UDP Header                              │
├─────────────────────────────────────────────────────────────┤
│ Source Port: 45678 (random)                                 │
│ Destination Port: 8472 (VXLAN)                              │
│ Length: 78 bytes                                            │
├─────────────────────────────────────────────────────────────┤
│                    VXLAN Header                             │
├─────────────────────────────────────────────────────────────┤
│ Flags: 0x08 (Valid VNI)                                     │
│ VNI: 1 (Virtual Network Identifier)                         │
│ Reserved: 0x000000                                          │
├─────────────────────────────────────────────────────────────┤
│                 Inner Ethernet Header                       │
├─────────────────────────────────────────────────────────────┤
│ Dst MAC: 02:42:0a:f4:02:01 (Node B cni0)                    │
│ Src MAC: 02:42:0a:f4:01:01 (Node A cni0)                    │
│ EtherType: 0x0800 (IPv4)                                    │
├─────────────────────────────────────────────────────────────┤
│                   Inner IP Header                           │
├─────────────────────────────────────────────────────────────┤
│ Version: 4, Header Length: 20 bytes                         │
│ Source IP: 10.244.1.10 (Pod1)                               │
│ Destination IP: 10.244.2.10 (Pod3)                          │
│ Protocol: 1 (ICMP)                                          │
│ TTL: 63 (decremented by 1)                                  │
├─────────────────────────────────────────────────────────────┤
│                     ICMP Header                             │
├─────────────────────────────────────────────────────────────┤
│ Type: 8 (Echo Request)                                      │
│ Code: 0                                                     │
│ Identifier: 1234                                            │
│ Sequence: 1                                                 │
│ Data: "Hello from Pod1"                                     │
└─────────────────────────────────────────────────────────────┘  
</pre>

Packet Capture Commands and Output
On Node A:
```
# Capture original packet leaving Pod1
sudo tcpdump -i veth12345678 -nn icmp -v
# 15:30:45.123456 IP (tos 0x0, ttl 64, len 84) 10.244.1.10 > 10.244.2.10: 
#   ICMP echo request, id 1234, seq 1, length 64

# Capture VXLAN encapsulated packet on physical interface
sudo tcpdump -i eth0 -nn 'udp port 8472' -v
# 15:30:45.123460 IP (tos 0x0, ttl 64, len 134) 10.0.1.100.45678 > 10.0.1.101.8472: 
#   VXLAN, flags [I] (0x08), vni 1
#   IP (tos 0x0, ttl 63, len 84) 10.244.1.10 > 10.244.2.10: 
#     ICMP echo request, id 1234, seq 1, length 64
```

On Node B:
```
# Capture incoming VXLAN packet
sudo tcpdump -i eth0 -nn 'udp port 8472' -v
# 15:30:45.123465 IP (tos 0x0, ttl 64, len 134) 10.0.1.100.45678 > 10.0.1.101.8472: 
#   VXLAN, flags [I] (0x08), vni 1
#   IP (tos 0x0, ttl 63, len 84) 10.244.1.10 > 10.244.2.10: 
#     ICMP echo request, id 1234, seq 1, length 64

# Capture decapsulated packet on cni0
sudo tcpdump -i cni0 -nn icmp -v
# 15:30:45.123470 IP (tos 0x0, ttl 63, len 84) 10.244.1.10 > 10.244.2.10: 
#   ICMP echo request, id 1234, seq 1, length 64
```

Indide Pod3
```
# Final packet received by Pod3
tcpdump -i eth0 -nn icmp -v
# 15:30:45.123475 IP (tos 0x0, ttl 63, len 84) 10.244.1.10 > 10.244.2.10: 
#   ICMP echo request, id 1234, seq 1, length 64
```

Return Path (Pod3 → Pod1)
The ICMP reply follows the reverse path:

```
# Pod3 sends reply
# 15:30:45.123480 IP 10.244.2.10 > 10.244.1.10: ICMP echo reply, id 1234, seq 1

# Node B encapsulates and sends via VXLAN
# 15:30:45.123485 IP 10.0.1.101.54321 > 10.0.1.100.8472: 
#   VXLAN, flags [I] (0x08), vni 1
#   IP 10.244.2.10 > 10.244.1.10: ICMP echo reply, id 1234, seq 1

# Pod1 receives the reply
# 15:30:45.123490 IP 10.244.2.10 > 10.244.1.10: ICMP echo reply, id 1234, seq 1
```

## Info - Calico Openshift Network Plugin
<pre>
- Calico Network in OpenShift
- Calico is a popular Container Network Interface (CNI) plugin that provides networking and 
  network security for containerized applications in OpenShift clusters.
  What is Calico?
  - Calico is an open-source networking solution that delivers:

Pod-to-pod networking across nodes
- Network policy enforcement for security
- IP address management (IPAM)
- Service mesh integration
- Key Features in OpenShift
  1. Pure Layer 3 Networking
     - Uses standard IP routing instead of overlay networks
     - Better performance with lower latency
     - Simplified troubleshooting using standard networking tools
  2. Network Policies
     - Kubernetes-native network policies
     - Advanced Calico network policies with additional features

- Ingress and egress traffic control
  - Application-layer policy enforcement
- Scalability
  - Supports thousands of nodes
  - Efficient BGP routing protocols
  - Distributed architecture without single points of failure
- Security
  - Workload isolation at the network level
  - Encryption in transit
  - Integration with service mesh security

- Architecture Components
  - Felix Agent
    - Runs on each node as a DaemonSet
    - Programs routing tables and iptables rules
    - Handles network policy enforcement

  - BGP Client (BIRD)
    - Distributes routing information between nodes
    - Maintains network topology
    - Enables cross-node pod communication

  - Calico Controller
    - Watches Kubernetes API for network policy changes
    - Translates policies into Felix-readable format
    - Manages IP address allocation  

- Common Use Cases
  - Multi-tenant environments requiring strong network isolation  
  - High-performance applications needing low network latency
  - Hybrid cloud deployments with consistent networking policies
  - Compliance requirements demanding network traffic control
</pre>

Comparison
<pre>
+------------------+----------+---------------+----------------+
| Feature          | Calico   | OpenShift SDN | OVN-Kubernetes |
+------------------+----------+---------------+----------------+
| Performance      | High     | Medium        | Medium-High    |
| Network Policies | Advanced | Basic         | Basic          |
| Complexity       | Medium   | Low           | Medium         |
| BGP Support      | Yes      | No            | No             |
| Overlay Network  | Optional | Yes           | Yes            |
| eBPF Support     | Yes      | No            | Limited        |
| IPv6 Support     | Yes      | Limited       | Yes            |
| Windows Support  | Yes      | No            | Yes            |
| Encryption       | WireGuard| IPSec         | IPSec          |
| IPAM             | Custom   | Built-in      | Built-in       |
| Troubleshooting  | Standard | OpenShift     | OVN Tools      |
|                  | Tools    | Tools         |                |
+------------------+----------+---------------+----------------+  
</pre>

## Info - Openshift Weave Network Plugin
<pre>
- Weave Network Plugin in OpenShift
  - Weave Network (Weave Net) is a Container Network Interface (CNI) plugin that 
    can be used with OpenShift to provide networking between containers and pods across a cluster.
- What is Weave Network?
  - Weave Network creates a virtual network that connects Docker containers across multiple hosts 
    and enables their automatic discovery. It provides:
- Overlay Network: Creates a Layer 2 network overlay that spans multiple hosts
- Automatic IP Management: Assigns IP addresses to containers automatically
- Service Discovery: Built-in DNS-based service discovery
- Network Segmentation: Support for network policies and microsegmentation
- Key Features in OpenShift Context
  1. Multi-Host Networking
     - Connects pods across different OpenShift nodes
     - Handles pod-to-pod communication seamlessly
     - Supports both IPv4 and IPv6
  2. Network Policies
     - Implements Kubernetes NetworkPolicy resources
     - Provides microsegmentation capabilities
     - Allows traffic filtering between pods/namespaces
  3. Encryption
    - Optional encryption of inter-host traffic
    - Uses NaCl crypto library for performance
    - Helps secure pod communication across untrusted networks
  4. Integration with OpenShift
     - Works as a CNI plugin
     - Integrates with OpenShift's Software Defined Networking (SDN)
     - Compatible with OpenShift service mesh

Architecture Components
- Weave Router
- Runs as a DaemonSet on each node
- Handles packet forwarding and routing
- Maintains network topology information
- Weave Net Plugin
  - CNI plugin that interfaces with container runtime
  - Allocates IP addresses to pods
  - Configures network interfaces
  - IPAM (IP Address Management)
  - Distributes IP address ranges across nodes
  - Prevents IP conflicts
  - Supports custom IP ranges  
</pre>

#### Usecases
- Multi-cloud deployments where encryption is required
- Strict network segmentation requirements
- Legacy applications that need specific networking features
- Development environments requiring flexible networking
- Comparison with OpenShift SDN

#### Limitations
- Performance overhead especially with encryption enabled
- Additional complexity compared to default OpenShift SDN
- Resource consumption from running additional networking components
- Troubleshooting complexity due to overlay networking
- Weave Network provides a robust networking solution for OpenShift,
  particularly useful when advanced networking features like - encryption and comprehensive
  network policies are required.
  
<pre>
+------------------------+---------------------------+---------------------------+
| Feature                | Weave Network             | OpenShift SDN             |
+------------------------+---------------------------+---------------------------+
| Overlay Technology     | VXLAN/UDP                 | VXLAN                     |
+------------------------+---------------------------+---------------------------+
| Network Policies       | Full NetworkPolicy        | Limited (requires         |
|                        | support                   | OVN-Kubernetes)           |
+------------------------+---------------------------+---------------------------+
| Encryption             | Built-in optional         | Requires additional       |
|                        | encryption                | setup                     |
+------------------------+---------------------------+---------------------------+
| Performance            | Good, with encryption     | Optimized for OpenShift   |
|                        | overhead                  |                           |
+------------------------+---------------------------+---------------------------+
| Setup Complexity       | More complex setup        | Integrated, simpler       |
+------------------------+---------------------------+---------------------------+
| IP Address Management  | Distributed IPAM          | Centralized IPAM          |
+------------------------+---------------------------+---------------------------+
| Service Discovery      | Built-in DNS              | Standard Kubernetes DNS   |
+------------------------+---------------------------+---------------------------+
| Multi-tenancy          | Network segmentation      | Project-based isolation   |
+------------------------+---------------------------+---------------------------+
| Troubleshooting        | More complex due to       | Easier with OpenShift     |
|                        | overlay networking        | tooling                   |
+------------------------+---------------------------+---------------------------+
| Resource Consumption   | Higher (additional        | Lower (integrated)        |
|                        | components)               |                           |
+------------------------+---------------------------+---------------------------+
| Cross-cluster          | Native support            | Requires additional       |
| Communication          |                           | configuration             |
+------------------------+---------------------------+---------------------------+
| Monitoring & Logging   | External tools required   | Integrated with           |
|                        |                           | OpenShift monitoring      |
+------------------------+---------------------------+---------------------------+
| Support & Maintenance  | Community/Commercial      | Red Hat supported         |
|                        | support                   |                           |
+------------------------+---------------------------+---------------------------+
</pre>

## Flannel vs Calico vs Weave
<pre>
+------------------------+---------------------------+---------------------------+---------------------------+
| Feature                | Calico                    | Flannel                   | Weave                     |
+------------------------+---------------------------+---------------------------+---------------------------+
| Overlay Technology     | BGP (L3), VXLAN, IPIP    | VXLAN, UDP, host-gw       | VXLAN, UDP                |
+------------------------+---------------------------+---------------------------+---------------------------+
| Network Policies       | Full NetworkPolicy +      | No native support         | Full NetworkPolicy        |
|                        | Calico policies           |                           | support                   |
+------------------------+---------------------------+---------------------------+---------------------------+
| Encryption             | WireGuard, IPSec          | No native encryption      | Built-in NaCl encryption  |
+------------------------+---------------------------+---------------------------+---------------------------+
| Performance            | Excellent (native BGP)    | Good (simple overlay)     | Good (with encryption     |
|                        |                           |                           | overhead)                 |
+------------------------+---------------------------+---------------------------+---------------------------+
| Setup Complexity       | Complex (BGP knowledge    | Simple                    | Moderate                  |
|                        | required)                 |                           |                           |
+------------------------+---------------------------+---------------------------+---------------------------+
| IP Address Management  | IPAM with IP pools        | Simple host-subnet        | Distributed IPAM          |
|                        |                           | allocation                |                           |
+------------------------+---------------------------+---------------------------+---------------------------+
| Service Discovery      | Standard Kubernetes DNS   | Standard Kubernetes DNS   | Built-in DNS + K8s DNS    |
+------------------------+---------------------------+---------------------------+---------------------------+
| Multi-tenancy          | Advanced policy engine    | Basic namespace           | Network segmentation      |
|                        |                           | isolation                 |                           |
+------------------------+---------------------------+---------------------------+---------------------------+
| Scalability            | Excellent (BGP routing)   | Good (limited by etcd)    | Good (mesh topology)      |
+------------------------+---------------------------+---------------------------+---------------------------+
| Cross-subnet Support   | Native (BGP)              | Limited (requires         | Good (automatic           |
|                        |                           | host-gw mode)             | discovery)                |
+------------------------+---------------------------+---------------------------+---------------------------+
| Windows Support        | Yes                       | Limited                   | No                        |
+------------------------+---------------------------+---------------------------+---------------------------+
| eBPF Support           | Yes (advanced features)   | No                        | No                        |
+------------------------+---------------------------+---------------------------+---------------------------+
| Resource Usage         | Low to moderate           | Very low                  | Moderate to high          |
+------------------------+---------------------------+---------------------------+---------------------------+
| Troubleshooting        | Complex (BGP knowledge)   | Simple                    | Moderate                  |
+------------------------+---------------------------+---------------------------+---------------------------+
| Enterprise Features    | Calico Enterprise         | Limited                   | Weave Cloud (deprecated)  |
|                        | (advanced security)       |                           |                           |
+------------------------+---------------------------+---------------------------+---------------------------+
| IPv6 Support           | Full dual-stack           | Basic                     | Yes                       |
+------------------------+---------------------------+---------------------------+---------------------------+
| Network Observability  | Flow logs, metrics        | Basic metrics             | Network map, metrics      |
+------------------------+---------------------------+---------------------------+---------------------------+
| Maturity               | Very mature               | Mature                    | Mature                    |
+------------------------+---------------------------+---------------------------+---------------------------+
| Best Use Cases         | - Large scale clusters    | - Simple deployments     | - Multi-cloud setups      |
|                        | - Advanced security       | - Learning/development    | - Encryption required     |
|                        | - Compliance requirements | - Resource constrained    | - Easy troubleshooting    |
|                        | - Multi-cloud             |   environments            | - Legacy app support      |
+------------------------+---------------------------+---------------------------+---------------------------+
| Vendor/Maintainer      | Tigera (formerly          | CoreOS/Red Hat            | Weaveworks                |
|                        | Project Calico)           |                           |                           |
+------------------------+---------------------------+---------------------------+---------------------------+
| OpenShift Support      | Yes (OVN-Kubernetes       | Limited                   | Third-party plugin        |
|                        | uses Calico for policies) |                           |                           |
+------------------------+---------------------------+---------------------------+---------------------------+
</pre>

#### Key Recommendations:
<pre>
Choose Calico when:
- You need advanced network policies and security
- Running large-scale production clusters
- Require compliance and audit capabilities
- Have networking expertise in your team

Choose Flannel when:
- You want simplicity and ease of use
- Running smaller or development clusters
- Have limited networking requirements
- Want minimal resource overhead

Choose Weave when:
- You need built-in encryption
- Running multi-cloud deployments
- Require good troubleshooting tools
- Need automatic network discovery
</pre>

## Demo - Install Red Hat AMQ Broker Operator
```
mkdir -p openshift-jms-setup/{operators,broker,queues,application}
cd openshift-jms-setup
```

Install AMQ Broker Operator - operators/amq-operator-subscription.yaml
<pre>
apiVersion: operators.coreos.com/v1alpha1
kind: Subscription
metadata:
  name: amq-broker-rhel8
  namespace: openshift-operators
spec:
  channel: 7.11.x
  name: amq-broker-rhel8
  source: redhat-operators
  sourceNamespace: openshift-marketplace  
</pre>

Create AMQ Broker Instance broker/amq-broker-instance.yaml
<pre>
apiVersion: broker.amq.io/v1beta1
kind: ActiveMQArtemis
metadata:
  name: amq-broker
  namespace: jms-demo
spec:
  deploymentPlan:
    size: 1
    image: registry.redhat.io/amq7/amq-broker-rhel8:7.11
    requireLogin: false
    persistenceEnabled: true
  console:
    expose: true
  acceptors:
    - name: amqp
      protocols: amqp
      port: 5672
    - name: core
      protocols: core
      port: 61616  
</pre>

Create Queues - queues/order-queue.yaml
<pre>
apiVersion: broker.amq.io/v1beta1
kind: ActiveMQArtemisAddress
metadata:
  name: order-queue
  namespace: jms-demo
spec:
  addressName: order.queue
  queueName: order.queue
  routingType: anycast  
</pre>

Create Topics - queues/notification-topic.yaml
<pre>
apiVersion: broker.amq.io/v1beta1
kind: ActiveMQArtemisAddress
metadata:
  name: notification-topic
  namespace: jms-demo
spec:
  addressName: notification.topic
  queueName: notification.topic
  routingType: multicast  
</pre>

Create Application Configuration - application/configmap.yaml
<pre>
apiVersion: v1
kind: ConfigMap
metadata:
  name: jms-app-config
  namespace: jms-demo
data:
  application.yml: |
    spring:
      activemq:
        broker-url: tcp://amq-broker-hdls-svc:61616
        user: admin
        password: admin
      jms:
        pub-sub-domain: false
    logging:
      level:
        com.example.jms: DEBUG  
</pre>

## Lab - Deploying your JMS Producer
```
oc new-project jegan
oc create deploy jms-producer --image=tektutor/jms-producer:1.0
oc create deploy jms-consumer --image=tektutor/jms-consumer:1.0

oc get pods
oc logs -f your-jms-producer-pod-name
oc logs -f your-jms-consumer-pod-name

```

## Info - Openshift S2I Overview
<pre>
- In Kubernetes, we can deploy applications only using container images
- In Openshift, we can deploy applications
  - using container image
  - using GitHub, BitBucket, Gilab etc url that has your application source code (S2I)
- Openshift supports many S2I strategies
  - Docker
  - Source
  - Build
  - Custom
  - Pipeline ( Jenkinsfile )
</pre>

## Info - Openshift S2I Docker Strategy Overview
<pre>
- Source code from any version control can be used
- In our case, we will be using GitHub, hence our repository must be have the below for S2I Docker strategy
  - application source code
  - Dockerfile
- With the GitHub url, branch details, Openshift will generate a BuidConfig 
- Using the BuildConfig, Openshift Build Controller will create Build (Pod), 
  - clones your source code
  - follows the instructions mentioned in your Dockerfile
  - builds your application to create applicaiton executable
  - builds a custom container image with your applicaiton executable configured as default application
  - Pushes the image to Openshift's Internal Image Registry
  - It deploys the applicaiton using the custom image pushed recently into the Internal Image Registry
  - It creates a service for the deployed applicaton
</pre>

## Info - Openshift S2I Source Strategy Overview
<pre>
- Source code from any version control can be used
- In our case, we will be using GitHub, hence our repository must be have the below for S2I source strategy
  - application source code
- With the GitHub url, branch details, Openshift will generate a BuidConfig and a Dockerfile
- Using the BuildConfig, Openshift Build Controller will create Build (Pod), 
  - clones your source code
  - follows the instructions mentioned in your Dockerfile
  - builds your application to create applicaiton executable
  - builds a custom container image with your applicaiton executable configured as default application
  - Pushes the image to Openshift's Internal Image Registry
  - It deploys the applicaiton using the custom image pushed recently into the Internal Image Registry
  - It creates a service for the deployed applicaton 
</pre>

## Lab - Deploying your application into Openshift using S2I docker strategy
```
oc delete project jegan
oc new-project jegan

oc new-app --name=hello https://github.com/tektutor/spring-ms.git --strategy=docker
```
<img width="1920" height="1168" alt="image" src="https://github.com/user-attachments/assets/82dfd08a-2053-4ff8-a686-498e5a69f490" />
<img width="1920" height="1168" alt="image" src="https://github.com/user-attachments/assets/b29800f1-f24b-4416-9897-4e2fe9da8313" />
<img width="1920" height="1168" alt="image" src="https://github.com/user-attachments/assets/32aa9d75-fa7f-424e-8888-dfae6a9a8e41" />
<img width="1920" height="1168" alt="image" src="https://github.com/user-attachments/assets/86e4b2e9-5880-468a-bf99-1d895ae0e7ad" />


## Lab - Deploying your application into Openshift using S2I source strategy
```
oc delete project jegan
oc new-project jegan

oc new-app --name=hello registry.access.redhat.com/ubi8/openjdk-17~https://github.com/tektutor/spring-ms.git --strategy=source
```
<img width="1920" height="1168" alt="image" src="https://github.com/user-attachments/assets/df2f2e37-6ae0-4aa2-b7f8-24359f17d27e" />
<img width="1920" height="1168" alt="image" src="https://github.com/user-attachments/assets/a403298f-40f7-4f1b-a2ed-149e9e7f8302" />
<img width="1920" height="1168" alt="image" src="https://github.com/user-attachments/assets/32aa9d75-fa7f-424e-8888-dfae6a9a8e41" />
<img width="1920" height="1168" alt="image" src="https://github.com/user-attachments/assets/86e4b2e9-5880-468a-bf99-1d895ae0e7ad" />

## Lab - Rolling update
```
oc delete project jegan
oc new-project jegan


oc create deploy nginx --image=image-registry.openshift-image-registry.svc:5000/openshift/tektutor-nginx:1.0 --replicas=3
oc get pods -o yaml | grep image
oc expose deploy/nginx --port=80
oc expose svc/nginx

curl  http://nginx-jegan.apps.ocp4.palmeto.org

# rolling update - from v1.0 to v2.0
oc edit deploy/nginx # update the image version from 1.0 to 2.0
oc get pods -o yaml | grep image
curl  http://nginx-jegan.apps.ocp4.palmeto.org

## rolling update - status
oc rollout status deploy/nginx
oc rollout undo deploy/nginx
oc rollout history deploy/nginx
```

## Info - Helm Overview
<pre>
- Helm  is a package manager for Kubernetes/Openshift
- Using Helm we can download and deploy application into K8s/Openshift
- Helm packaged application is called chart
- Helm is opensource tool that can be installed on your system as a stand-alone command-line tool
- Helm supports
  - download from the helm community and install them into your Kubernetes/Openshift cluster
  - could package your custom application using Helm as charts and deploy them into Kubernetes/Openshift cluster
  - uninstalling applications deployed using Helm
  - upgrading applications deployed using helm from one version to other 
</pre>

## Lab - Packaging our custom wordpress and maraiab application as helm chart and deploying into openshift
```
cd ~/openshift-0610oct-2025
git pull
cd Day5/helm-chart
helm create wordpress
tree wordpress
rm -rf wordpress/templates/*

# Find your nfs shared folders
showmount -e | grep jegan

# Update the values.yaml file with your NFS Server IP, your mysql nfs path and your wordpress nfs path
cat values.yaml
cp values.yaml wordpress
cp scripts/* wordpress/templates

helm package wordpress
ls
helm install wordpress wordpress-0.1.0.tgz
helm list
oc get pods
```

Once you are done with this exercise, you can uninstall wordpress using helm
```
helm list
helm uninstall wordpress
oc get deploy,rs,po,pv,pvc,svc,route
```

## Lab - CI/CD Pipeline with Jenkins, Ansible and OpenShift

For step by step instruction to install and configure Jenkins, you may refer my blog
<pre>
https://www.tektutor.org/ci-cd-with-maven-github-docker-jenkins/  
</pre>

Let's start Jenkins from command-line, you may to give a different port in case you get binding error
```
cd ~
cp /tmp/jenkins.war ~
java -jar ~/jenkins.war --httpPort=9090
```

Accessing the Jenkins Dashboard, the port depend
```
http://localhost:9090
```

### Jenkins Job

##### General section
<pre>
Description - CICD Pipeline Demo
</pre>

#### Triggers section
```
H/02 * * * *
```

#### Pipeline section
<pre>
Definition - Pipeline Script from SCM
SCM - Git
Repository Url - https://github.com/tektutor/openshift-0610oct-2025.git
Branch specifier - */main
</pre>

#### Script Path
<pre>
Day5/CICD-Demo/Jenkinsfile  
</pre>
