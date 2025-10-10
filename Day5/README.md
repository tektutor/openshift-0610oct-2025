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

#### Network Security
- Pod-to-Pod & Pod-to-Service
- NetworkPolicy (namespaced) and ClusterNetworkPolicy (cluster-wide)
- Default deny-all and zero-trust networking
- Controlling ingress/egress with egress firewall rules
- Ingress Security
- Routes and TLS termination types 
- Securing routes with certificates 


#### Cluster Security & Hardening
- Security Context Constraints (SCC) – OpenShift equivalent of PodSecurityPolicies
- restricted, anyuid, privileged SCCs
- Creating custom SCCs
- Admission Controllers and Validating/Mutating Webhooks
- Pod Security Standards (Baseline, Restricted)
- Running containers as non-root
- Disabling privileged escalation

#### Image & Supply Chain Security
- ImageStreams and trusted registries
- image signature verification (oc adm policy add-scc-to-user, oc adm signature)
- Image scanning (e.g., Red Hat Quay, Clair, Trivy)
- Content Trust and ImagePolicy admission
- Immutable images and reproducible builds
- Security scanning in CI/CD pipelines

#### Secrets & Config Security
- Kubernetes Secrets vs ConfigMaps
- Encryption at rest with KMS providers (e.g., AWS KMS, Vault)
- Sealed Secrets (Bitnami SealedSecrets)
- External secret managers (HashiCorp Vault, CyberArk, AWS Secrets Manager)
- Securing environment variables in deployments

#### Platform Access & API Security
- API server authentication and RBAC for oc and kubectl users
- OpenShift Console access restrictions
- API audit logging
- Certificate management for API server and etcd
- OAuth token lifetimes and rotation

#### Compliance & Monitoring
- Compliance Operator (CIS Benchmark for OpenShift)
- OpenSCAP scans and remediation
- Audit logging and forwarding (e.g., to ELK or SIEM)
- Security events in oc get events and cluster logs
- Cluster-wide alerts for security breaches

#### Security Tools & Operators
- Compliance Operator – CIS, NIST profiles
- Gatekeeper / OPA – policy as code
- ACS (Red Hat Advanced Cluster Security) – runtime threat detection, network graph
- Falco – behavioral threat detection
- Trivy – image scanning in CI/CD

#### Secure Multitenancy
- Namespace isolation best practices
- Project quotas and limits
- Network segmentation per tenant
- RBAC delegation to team leads
- Resource constraints to prevent noisy neighbor issues

#### Security Updates & Patch Management
- OpenShift upgrade process & channels (fast, stable)
- Node OS updates (RHCOS)
- Operator lifecycle management (OLM) security
- CVE tracking and remediation using Red Hat Insights

#### Runtime & Workload Security
- Pod security context:
  - runAsUser, fsGroup, readOnlyRootFilesystem
- Dropping Linux capabilities
  - Seccomp profiles and AppArmor (on supported platforms)
- Container resource limits to prevent DoS
- Detecting crypto-mining and lateral movement

#### Ingress/Egress & Perimeter Security
- Securing ingress with TLS, WAF integration (e.g., F5, HAProxy)
- Rate limiting & DoS protection
- Egress control with EgressFirewall and proxies
- Bastion hosts & jump boxes for controlled access

#### Backup & Disaster Recovery (Security Angle)
- etcd encryption and backup security
- Secrets encryption during backups
- Immutable backup storage
- RBAC restrictions for restore operations

#### Zero Trust Architecture
- Network segmentation
- Identity-aware access to workloads
- Mutual TLS between services
- Policy enforcement using OPA/Gatekeeper
- Just-in-time access control

#### Best Practices & Standards
- CIS Benchmark for OpenShift
- NIST 800-53 alignment
- ISO 27001 security practices

Regular pen-testing and red teaming

Shift-left security in CI/CD
