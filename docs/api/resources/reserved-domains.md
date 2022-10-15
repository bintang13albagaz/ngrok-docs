import ReservedDomainsCreateRequest from './examples/_reserved_domains_create_request.md';
import ReservedDomainsCreateResponse from './examples/_reserved_domains_create_response.md';
import ReservedDomainsDeleteRequest from './examples/_reserved_domains_delete_request.md';
import ReservedDomainsGetRequest from './examples/_reserved_domains_get_request.md';
import ReservedDomainsGetResponse from './examples/_reserved_domains_get_response.md';
import ReservedDomainsListRequest from './examples/_reserved_domains_list_request.md';
import ReservedDomainsListResponse from './examples/_reserved_domains_list_response.md';
import ReservedDomainsUpdateRequest from './examples/_reserved_domains_update_request.md';
import ReservedDomainsUpdateResponse from './examples/_reserved_domains_update_response.md';
import ReservedDomainsDeleteCertificateManagementPolicyRequest from './examples/_reserved_domains_delete_certificate_management_policy_request.md';
import ReservedDomainsDeleteCertificateRequest from './examples/_reserved_domains_delete_certificate_request.md';

# Reserved Domains
------------



## Create Reserved Domain
Create a new reserved domain.

### Request

POST /reserved_domains

<ReservedDomainsCreateRequest />


#### Parameters

|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `name` | string | the domain name to reserve. It may be a full domain name like app.example.com. If the name does not contain a '.' it will reserve that subdomain on ngrok.io. |
| `region` | string | reserve the domain in this geographic ngrok datacenter. Optional, default is us. (au, eu, ap, us, jp, in, sa) |
| `description` | string | human-readable description of what this reserved domain will be used for |
| `metadata` | string | arbitrary user-defined machine-readable data of this reserved domain. Optional, max 4096 bytes. |
| `certificate_id` | string | ID of a user-uploaded TLS certificate to use for connections to targeting this domain. Optional, mutually exclusive with `certificate_management_policy`. |
| `certificate_management_policy` | [ReservedDomainCertPolicy](#api-reserved-domains-create-parameters-reserved-domain-cert-policy) | configuration for automatic management of TLS certificates for this domain, or null if automatic management is disabled. Optional, mutually exclusive with `certificate_id`. |

#### ReservedDomainCertPolicy parameters

|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `authority` | string | certificate authority to request certificates from. The only supported value is letsencrypt. |
| `private_key_type` | string | type of private key to use when requesting certificates. Defaults to rsa, can be either rsa or ecdsa. |


### Response

Returns a 201 response  on success

<ReservedDomainsCreateResponse />


#### Fields

|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `id` | string | unique reserved domain resource identifier |
| `uri` | string | URI of the reserved domain API resource |
| `created_at` | string | timestamp when the reserved domain was created, RFC 3339 format |
| `description` | string | human-readable description of what this reserved domain will be used for |
| `metadata` | string | arbitrary user-defined machine-readable data of this reserved domain. Optional, max 4096 bytes. |
| `domain` | string | hostname of the reserved domain |
| `region` | string | reserve the domain in this geographic ngrok datacenter. Optional, default is us. (au, eu, ap, us, jp, in, sa) |
| `cname_target` | string | DNS CNAME target for a custom hostname, or null if the reserved domain is a subdomain of *.ngrok.io |
| `certificate` | [Ref](#api-reserved-domains-create-fields-ref) | object referencing the TLS certificate used for connections to this domain. This can be either a user-uploaded certificate, the most recently issued automatic one, or null otherwise. |
| `certificate_management_policy` | [ReservedDomainCertPolicy](#api-reserved-domains-create-fields-reserved-domain-cert-policy) | configuration for automatic management of TLS certificates for this domain, or null if automatic management is disabled |
| `certificate_management_status` | [ReservedDomainCertStatus](#api-reserved-domains-create-fields-reserved-domain-cert-status) | status of the automatic certificate management for this domain, or null if automatic management is disabled |
| `acme_challenge_cname_target` | string | DNS CNAME target for the host _acme-challenge.example.com, where example.com is your reserved domain name. This is required to issue certificates for wildcard, non-ngrok reserved domains. Must be null for non-wildcard domains and ngrok subdomains. |

#### Ref fields

|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `id` | string | a resource identifier |
| `uri` | string | a uri for locating a resource |

#### ReservedDomainCertPolicy fields

|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `authority` | string | certificate authority to request certificates from. The only supported value is letsencrypt. |
| `private_key_type` | string | type of private key to use when requesting certificates. Defaults to rsa, can be either rsa or ecdsa. |

#### ReservedDomainCertStatus fields

|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `renews_at` | string | timestamp when the next renewal will be requested, RFC 3339 format |
| `provisioning_job` | [ReservedDomainCertJob](#api-reserved-domains-create-fields-reserved-domain-cert-job) | status of the certificate provisioning job, or null if the certificiate isn't being provisioned or renewed |

#### ReservedDomainCertJob fields

|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `error_code` | string | if present, an error code indicating why provisioning is failing. It may be either a temporary condition (INTERNAL_ERROR), or a permanent one the user must correct (DNS_ERROR). |
| `msg` | string | a message describing the current status or error |
| `started_at` | string | timestamp when the provisioning job started, RFC 3339 format |
| `retries_at` | string | timestamp when the provisioning job will be retried |


## Delete Reserved Domain
Delete a reserved domain.

### Request

DELETE /reserved_domains/{id}

<ReservedDomainsDeleteRequest />


### Response

Returns a 204 response with no body on success


## Get Reserved Domain
Get the details of a reserved domain.

### Request

GET /reserved_domains/{id}

<ReservedDomainsGetRequest />


### Response

Returns a 200 response  on success

<ReservedDomainsGetResponse />


#### Fields

|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `id` | string | unique reserved domain resource identifier |
| `uri` | string | URI of the reserved domain API resource |
| `created_at` | string | timestamp when the reserved domain was created, RFC 3339 format |
| `description` | string | human-readable description of what this reserved domain will be used for |
| `metadata` | string | arbitrary user-defined machine-readable data of this reserved domain. Optional, max 4096 bytes. |
| `domain` | string | hostname of the reserved domain |
| `region` | string | reserve the domain in this geographic ngrok datacenter. Optional, default is us. (au, eu, ap, us, jp, in, sa) |
| `cname_target` | string | DNS CNAME target for a custom hostname, or null if the reserved domain is a subdomain of *.ngrok.io |
| `certificate` | [Ref](#api-reserved-domains-get-fields-ref) | object referencing the TLS certificate used for connections to this domain. This can be either a user-uploaded certificate, the most recently issued automatic one, or null otherwise. |
| `certificate_management_policy` | [ReservedDomainCertPolicy](#api-reserved-domains-get-fields-reserved-domain-cert-policy) | configuration for automatic management of TLS certificates for this domain, or null if automatic management is disabled |
| `certificate_management_status` | [ReservedDomainCertStatus](#api-reserved-domains-get-fields-reserved-domain-cert-status) | status of the automatic certificate management for this domain, or null if automatic management is disabled |
| `acme_challenge_cname_target` | string | DNS CNAME target for the host _acme-challenge.example.com, where example.com is your reserved domain name. This is required to issue certificates for wildcard, non-ngrok reserved domains. Must be null for non-wildcard domains and ngrok subdomains. |

#### Ref fields

|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `id` | string | a resource identifier |
| `uri` | string | a uri for locating a resource |

#### ReservedDomainCertPolicy fields

|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `authority` | string | certificate authority to request certificates from. The only supported value is letsencrypt. |
| `private_key_type` | string | type of private key to use when requesting certificates. Defaults to rsa, can be either rsa or ecdsa. |

#### ReservedDomainCertStatus fields

|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `renews_at` | string | timestamp when the next renewal will be requested, RFC 3339 format |
| `provisioning_job` | [ReservedDomainCertJob](#api-reserved-domains-get-fields-reserved-domain-cert-job) | status of the certificate provisioning job, or null if the certificiate isn't being provisioned or renewed |

#### ReservedDomainCertJob fields

|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `error_code` | string | if present, an error code indicating why provisioning is failing. It may be either a temporary condition (INTERNAL_ERROR), or a permanent one the user must correct (DNS_ERROR). |
| `msg` | string | a message describing the current status or error |
| `started_at` | string | timestamp when the provisioning job started, RFC 3339 format |
| `retries_at` | string | timestamp when the provisioning job will be retried |


## List Reserved Domains
List all reserved domains on this account.

### Request

GET /reserved_domains

<ReservedDomainsListRequest />


### Response

Returns a 200 response  on success

<ReservedDomainsListResponse />


#### Fields

|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `reserved_domains` | [ReservedDomain](#api-reserved-domains-list-fields-reserved-domain) | the list of all reserved domains on this account |
| `uri` | string | URI of the reserved domain list API resource |
| `next_page_uri` | string | URI of the next page, or null if there is no next page |

#### ReservedDomain fields

|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `id` | string | unique reserved domain resource identifier |
| `uri` | string | URI of the reserved domain API resource |
| `created_at` | string | timestamp when the reserved domain was created, RFC 3339 format |
| `description` | string | human-readable description of what this reserved domain will be used for |
| `metadata` | string | arbitrary user-defined machine-readable data of this reserved domain. Optional, max 4096 bytes. |
| `domain` | string | hostname of the reserved domain |
| `region` | string | reserve the domain in this geographic ngrok datacenter. Optional, default is us. (au, eu, ap, us, jp, in, sa) |
| `cname_target` | string | DNS CNAME target for a custom hostname, or null if the reserved domain is a subdomain of *.ngrok.io |
| `certificate` | [Ref](#api-reserved-domains-list-fields-ref) | object referencing the TLS certificate used for connections to this domain. This can be either a user-uploaded certificate, the most recently issued automatic one, or null otherwise. |
| `certificate_management_policy` | [ReservedDomainCertPolicy](#api-reserved-domains-list-fields-reserved-domain-cert-policy) | configuration for automatic management of TLS certificates for this domain, or null if automatic management is disabled |
| `certificate_management_status` | [ReservedDomainCertStatus](#api-reserved-domains-list-fields-reserved-domain-cert-status) | status of the automatic certificate management for this domain, or null if automatic management is disabled |
| `acme_challenge_cname_target` | string | DNS CNAME target for the host _acme-challenge.example.com, where example.com is your reserved domain name. This is required to issue certificates for wildcard, non-ngrok reserved domains. Must be null for non-wildcard domains and ngrok subdomains. |

#### Ref fields

|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `id` | string | a resource identifier |
| `uri` | string | a uri for locating a resource |

#### ReservedDomainCertPolicy fields

|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `authority` | string | certificate authority to request certificates from. The only supported value is letsencrypt. |
| `private_key_type` | string | type of private key to use when requesting certificates. Defaults to rsa, can be either rsa or ecdsa. |

#### ReservedDomainCertStatus fields

|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `renews_at` | string | timestamp when the next renewal will be requested, RFC 3339 format |
| `provisioning_job` | [ReservedDomainCertJob](#api-reserved-domains-list-fields-reserved-domain-cert-job) | status of the certificate provisioning job, or null if the certificiate isn't being provisioned or renewed |

#### ReservedDomainCertJob fields

|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `error_code` | string | if present, an error code indicating why provisioning is failing. It may be either a temporary condition (INTERNAL_ERROR), or a permanent one the user must correct (DNS_ERROR). |
| `msg` | string | a message describing the current status or error |
| `started_at` | string | timestamp when the provisioning job started, RFC 3339 format |
| `retries_at` | string | timestamp when the provisioning job will be retried |


## Update Reserved Domain
Update the attributes of a reserved domain.

### Request

PATCH /reserved_domains/{id}

<ReservedDomainsUpdateRequest />


#### Parameters

|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `id` | string |  |
| `description` | string | human-readable description of what this reserved domain will be used for |
| `metadata` | string | arbitrary user-defined machine-readable data of this reserved domain. Optional, max 4096 bytes. |
| `certificate_id` | string | ID of a user-uploaded TLS certificate to use for connections to targeting this domain. Optional, mutually exclusive with `certificate_management_policy`. |
| `certificate_management_policy` | [ReservedDomainCertPolicy](#api-reserved-domains-update-parameters-reserved-domain-cert-policy) | configuration for automatic management of TLS certificates for this domain, or null if automatic management is disabled. Optional, mutually exclusive with `certificate_id`. |

#### ReservedDomainCertPolicy parameters

|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `authority` | string | certificate authority to request certificates from. The only supported value is letsencrypt. |
| `private_key_type` | string | type of private key to use when requesting certificates. Defaults to rsa, can be either rsa or ecdsa. |


### Response

Returns a 200 response  on success

<ReservedDomainsUpdateResponse />


#### Fields

|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `id` | string | unique reserved domain resource identifier |
| `uri` | string | URI of the reserved domain API resource |
| `created_at` | string | timestamp when the reserved domain was created, RFC 3339 format |
| `description` | string | human-readable description of what this reserved domain will be used for |
| `metadata` | string | arbitrary user-defined machine-readable data of this reserved domain. Optional, max 4096 bytes. |
| `domain` | string | hostname of the reserved domain |
| `region` | string | reserve the domain in this geographic ngrok datacenter. Optional, default is us. (au, eu, ap, us, jp, in, sa) |
| `cname_target` | string | DNS CNAME target for a custom hostname, or null if the reserved domain is a subdomain of *.ngrok.io |
| `certificate` | [Ref](#api-reserved-domains-update-fields-ref) | object referencing the TLS certificate used for connections to this domain. This can be either a user-uploaded certificate, the most recently issued automatic one, or null otherwise. |
| `certificate_management_policy` | [ReservedDomainCertPolicy](#api-reserved-domains-update-fields-reserved-domain-cert-policy) | configuration for automatic management of TLS certificates for this domain, or null if automatic management is disabled |
| `certificate_management_status` | [ReservedDomainCertStatus](#api-reserved-domains-update-fields-reserved-domain-cert-status) | status of the automatic certificate management for this domain, or null if automatic management is disabled |
| `acme_challenge_cname_target` | string | DNS CNAME target for the host _acme-challenge.example.com, where example.com is your reserved domain name. This is required to issue certificates for wildcard, non-ngrok reserved domains. Must be null for non-wildcard domains and ngrok subdomains. |

#### Ref fields

|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `id` | string | a resource identifier |
| `uri` | string | a uri for locating a resource |

#### ReservedDomainCertPolicy fields

|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `authority` | string | certificate authority to request certificates from. The only supported value is letsencrypt. |
| `private_key_type` | string | type of private key to use when requesting certificates. Defaults to rsa, can be either rsa or ecdsa. |

#### ReservedDomainCertStatus fields

|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `renews_at` | string | timestamp when the next renewal will be requested, RFC 3339 format |
| `provisioning_job` | [ReservedDomainCertJob](#api-reserved-domains-update-fields-reserved-domain-cert-job) | status of the certificate provisioning job, or null if the certificiate isn't being provisioned or renewed |

#### ReservedDomainCertJob fields

|&nbsp;| &nbsp;| &nbsp;|
|---|---|---|
| `error_code` | string | if present, an error code indicating why provisioning is failing. It may be either a temporary condition (INTERNAL_ERROR), or a permanent one the user must correct (DNS_ERROR). |
| `msg` | string | a message describing the current status or error |
| `started_at` | string | timestamp when the provisioning job started, RFC 3339 format |
| `retries_at` | string | timestamp when the provisioning job will be retried |


## Detach Certificate Management Policy from Reserved Domain
Detach the certificate management policy attached to a reserved domain.

### Request

DELETE /reserved_domains/{id}/certificate_management_policy

<ReservedDomainsDeleteCertificateManagementPolicyRequest />


### Response

Returns a 204 response with no body on success


## Detach Certificate from Reserved Domain
Detach the certificate attached to a reserved domain.

### Request

DELETE /reserved_domains/{id}/certificate

<ReservedDomainsDeleteCertificateRequest />


### Response

Returns a 204 response with no body on success