# Day 5

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
