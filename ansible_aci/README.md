# Ansible Modules used to Automate Cisco ACI
### *Idempotent Modules to manage ACI and much more!*

---
### Requirements
* ACI Fabric 1.0(3f)+
* Cobra SDK

---
### Modules

  * [aci_filter_entry - manages filter entries that will be assigned to a filter](#aci_filter_entry)
  * [aci_filter - manages top level filter objects](#aci_filter)
  * [aci_bridge_domain - manages bridge domains in an aci fabric](#aci_bridge_domain)
  * [aci_contract - manages initial contracts (does not include contract subjs)](#aci_contract)
  * [aci_tenant - manage tenants in an aci fabric](#aci_tenant)
  * [aci_rest - direct access to the apic api](#aci_rest)
  * [aci_epg - manages aci end point groups and related contracts](#aci_epg)
  * [aci_contract_subject - manages contract subjects](#aci_contract_subject)
  * [aci_context - manage private networks, contexts, in an aci fabric](#aci_context)
  * [aci_anp - manage top level application network profile objects](#aci_anp)

---

## aci_filter_entry
Manages filter entries that will be assigned to a filter

  * Synopsis
  * Options
  * Examples

#### Synopsis
 Manages filter entries that will be assigned to an already created filter

#### Options

| Parameter     | required    | default  | choices    | comments |
| ------------- |-------------| ---------|----------- |--------- |
| username  |   yes  |  admin  | <ul></ul> |  Username used to login to the switch  |
| dest_from_port  |   no  |  unspecified  | <ul></ul> |  supports keywords as APIC does, i.e. unspecified, https, http  |
| protocol  |   no  |  https  | <ul> <li>http</li>  <li>https</li> </ul> |  Dictates connection protocol  |
| name  |   yes  |  | <ul></ul> |  Name of the entry  |
| dest_to_port  |   no  |  unspecified  | <ul></ul> |  supports keywords as APIC does, i.e. unspecified, https, http  |
| proto  |   no  |  | <ul> <li>eigrp</li>  <li>egp</li>  <li>icmp</li>  <li>igmp</li>  <li>igp</li>  <li>l2tp</li>  <li>ospfigp</li>  <li>pim</li>  <li>tcp</li>  <li>udp</li> </ul> |  Protocol to be matched against for this entry  |
| allow_fragment  |   no  |  | <ul> <li>yes</li>  <li>no</li> </ul> |  Matches allow_fragment option from APIC  |
| host  |   yes  |  | <ul></ul> |  IP Address or hostname of APIC resolvable by Ansible control host  |
| filter  |   yes  |  | <ul></ul> |  Name of the filter this entry will be part of  |
| state  |   no  |  present  | <ul> <li>present</li>  <li>absent</li> </ul> |  Desired state of the entry  |
| src_to_port  |   no  |  unspecified  | <ul></ul> |  supports keywords as APIC does, i.e. unspecified, https, http  |
| password  |   yes  |  C1sco12345  | <ul></ul> |  Password used to login to the switch  |
| src_from_port  |   yes  |  unspecified  | <ul></ul> |  supports keywords as APIC does, i.e. unspecified, https, http  |
| tenant  |   yes  |  | <ul></ul> |  Name of the tenant this entry will be part of  |
| tcp_session_rules  |   no  |  | <ul> <li>ack</li>  <li>syn</li>  <li>est</li>  <li>rst</li>  <li>fin</li> </ul> |  Acknowledgment, Synchronize, Established, Reset, Finish  |


 
#### Examples

```

# entry for web filter
- aci_filter_entry: name=web_filter proto=tcp tenant=ACILab filter=Web_Filter dest_to_port=80 state=present host={{ inventory_hostname }} username={{ user }} password={{ pass }}

# entry showing varying src/dest ports
- aci_filter_entry: name=entry2 proto=tcp tenant=ACILab filter=Web_Filter src_to_port=unspecified dest_from_port=500 dest_to_port=1000 tcp_session_rules=rst host={{ inventory_hostname }} username={{ user }} password={{ pass }}


```


#### Notes

- If only a single source port is required (not to and from), either the src_to_port or src_from_port could be used.

- If only a single dest port is required (not to and from), either the dest_to_port or dest_from_port could be used.

- When neither *_to_port or *_from_port is not used, it becomes unspecified (as in ACI fabric)

- If ACI supports named protocols, the module is not idempotent, with the exception of http/https.

- Only a single tcp session rule is supported per entry

- If proto does not equal tcp/udp, the src/dest and tcp_session params are not used, even if set.


---


## aci_filter
Manages top level filter objects

  * Synopsis
  * Options
  * Examples

#### Synopsis
 Manages top level filter objects, i.e. not each entry

#### Options

| Parameter     | required    | default  | choices    | comments |
| ------------- |-------------| ---------|----------- |--------- |
| username  |   yes  |  admin  | <ul></ul> |  Username used to login to the switch  |
| protocol  |   no  |  https  | <ul> <li>http</li>  <li>https</li> </ul> |  Dictates connection protocol to use  |
| name  |   yes  |  | <ul></ul> |  Name of the filter  |
| descr  |   no  |  | <ul></ul> |  description of filter  |
| host  |   yes  |  | <ul></ul> |  IP Address or hostname of APIC resolvable by Ansible control host  |
| state  |   no  |  present  | <ul> <li>present</li>  <li>absent</li> </ul> |  Desired state of the filter  |
| password  |   yes  |  C1sco12345  | <ul></ul> |  Password used to login to the switch  |
| tenant  |   yes  |  | <ul></ul> |  name of tenant this filter will be part of  |


 
#### Examples

```

# name: ensure filters exist
aci_filter: name={{ item }} tenant=ACILab host={{ inventory_hostname }} username={{ user }} password={{ pass }}
with_items:
  - Web_Filter
  - App_Filter
  - DB_Filter



```



---


## aci_bridge_domain
Manages bridge domains in an ACI fabric

  * Synopsis
  * Options
  * Examples

#### Synopsis
 Manages bridge domains within an ACI fabric

#### Options

| Parameter     | required    | default  | choices    | comments |
| ------------- |-------------| ---------|----------- |--------- |
| username  |   yes  |  admin  | <ul></ul> |  Username used to login to the switch  |
| subnet  |   no  |  | <ul></ul> |  name of subnet that is paired to bridge domain  |
| protocol  |   no  |  https  | <ul> <li>http</li>  <li>https</li> </ul> |  Dictates connection protocol to use  |
| name  |   yes  |  | <ul></ul> |  Name of the bridge domain  |
| descr  |   no  |  | <ul></ul> |  description of bridge domain  |
| host  |   yes  |  | <ul></ul> |  IP Address or hostname of APIC resolvable by Ansible control host  |
| state  |   no  |  present  | <ul> <li>present</li>  <li>absent</li> </ul> |  Desired state of the bridge domain  |
| context  |   yes  |  | <ul></ul> |  name of context (private network / VRF)  |
| password  |   yes  |  C1sco12345  | <ul></ul> |  Password used to login to the switch  |
| tenant  |   yes  |  | <ul></ul> |  name of tenant this bridge domain will be part of  |


 
#### Examples

```

# ensure bridge domain 1 exists
- aci_bridge_domain: name=ACILab_BD1 context=ACILab_VRF subnet=10.10.10.1/24 tenant=ACILab state=present host={{ inventory_hostname }} username={{ user }} password={{ pass }}

# ensure bd 1 doesn't exist
- aci_bridge_domain: name=ACILab_BD1 context=ACILab_VRF tenant=ACILab state=absent host={{ inventory_hostname }} username={{ user }} password={{ pass }}


```


#### Notes

- Tenant and context must be exist prior to using this module

- One subnet can be added per task (per module call)

- state=absent removes complete bridge domain configuration including all subnets


---


## aci_contract
Manages initial contracts (does not include contract subjs)

  * Synopsis
  * Options
  * Examples

#### Synopsis
 Manages contract resource, but does not include subjects although subjects can be removed using this module

#### Options

| Parameter     | required    | default  | choices    | comments |
| ------------- |-------------| ---------|----------- |--------- |
| username  |   yes  |  admin  | <ul></ul> |  Username used to login to the switch  |
| protocol  |   no  |  https  | <ul> <li>http</li>  <li>https</li> </ul> |  Dictates connection protocol to use  |
| name  |   yes  |  | <ul></ul> |  Name of the contract  |
| prio  |   no  |  | <ul> <li>unspecified</li>  <li>level1</li>  <li>level2</li>  <li>level3</li> </ul> |  priority (qosclass) of contract  |
| host  |   yes  |  | <ul></ul> |  IP Address or hostname of APIC resolvable by Ansible control host  |
| state  |   no  |  present  | <ul> <li>present</li>  <li>absent</li> </ul> |  Desired state of the contract  |
| descr  |   no  |  | <ul></ul> |  description of contract  |
| scope  |   no  |  | <ul> <li>application-profile</li>  <li>context</li>  <li>global</li>  <li>tenant</li> </ul> |  scope of contract  |
| password  |   yes  |  C1sco12345  | <ul></ul> |  Password used to login to the switch  |
| tenant  |   yes  |  | <ul></ul> |  name of tenant this contract will be part of  |


 
#### Examples

```
# ensure contract exists
- aci_contract: name=web-contract descr='web contracy by ansible' tenant=customer_1 host={{ inventory_hostname }} username={{ user }} password={{ pass }}

# ensure contract exists with added params
- aci_contract: name=web-contract descr='web contracy by ansible' tenant=customer_1 prio=level2 scope=context host={{ inventory_hostname }} username={{ user }} password={{ pass }}

# ensure contract does not exist
- aci_contract: name=web-contract tenant=customer_1 state=absent host={{ inventory_hostname }} username={{ user }} password={{ pass }}


```


#### Notes

- Tenant and context must be exist prior to using this module

- state=absent removes complete contract including the contract subjects that were deployed with the aci_contract_subject module


---


## aci_tenant
Manage tenants in an ACI fabric

  * Synopsis
  * Options
  * Examples

#### Synopsis
 Offers ability to manage tenants

#### Options

| Parameter     | required    | default  | choices    | comments |
| ------------- |-------------| ---------|----------- |--------- |
| username  |   yes  |  admin  | <ul></ul> |  Username used to login to the switch  |
| protocol  |   no  |  https  | <ul> <li>http</li>  <li>https</li> </ul> |  Dictates connection protocol  |
| name  |   yes  |  | <ul></ul> |  Name of tenant  |
| descr  |   no  |  | <ul></ul> |  description of tenant  |
| state  |   no  |  present  | <ul> <li>present</li>  <li>absent</li> </ul> |  Desired state of the tenant  |
| host  |   yes  |  | <ul></ul> |  IP Address or hostname of APIC resolvable by Ansible control host  |
| password  |   yes  |  C1sco12345  | <ul></ul> |  Password used to login to the switch  |


 
#### Examples

```
# ensure tenant exists
- aci_tenant: name=ACILab descr='tenant by Ansible' host={{ inventory_hostname }} username={{ user }} password={{ pass }}

# ensure tenant does not exist on system
- aci_tenant: name=ACILab state=absent host={{ inventory_hostname }} username={{ user }} password={{ pass }}


```



---


## aci_rest
Direct access to the APIC API

  * Synopsis
  * Options
  * Examples

#### Synopsis
 Offers direct access to the APIC API

#### Options

| Parameter     | required    | default  | choices    | comments |
| ------------- |-------------| ---------|----------- |--------- |
| username  |   yes  |  admin  | <ul></ul> |  Username used to login to the switch  |
| config_file  |   no  |  | <ul></ul> |  name of the absolute path of the filname that includes the body of the http request being sent to the ACI fabric  |
| uri  |   yes  |  | <ul></ul> |  uri being used to execute API calls. Must end in .xml or .json  |
| host  |   yes  |  | <ul></ul> |  IP Address or hostname of APIC resolvable by Ansible control host  |
| action  |   yes  |  | <ul> <li>post</li>  <li>get</li> </ul> |  http verb, i.e. post or get  |
| protocol  |   no  |  https  | <ul> <li>http</li>  <li>https</li> </ul> |  Dictates connection protocol to use  |
| password  |   yes  |  C1sco12345  | <ul></ul> |  Password used to login to the switch  |


 
#### Examples

```

# add a tenant
- aci_rest: action=post uri=/api/mo/uni.xml config_file=/home/cisco/ansible/aci/configs/aci_config.xml host={{ inventory_hostname }} username={{ user }} password={{ pass }}

# get tenants
- aci_rest: action=get uri=/api/node/class/fvTenant.json host={{ inventory_hostname }} username={{ user }} password={{ pass }}

# configure contracts
- aci_rest: action=post uri=/api/mo/uni.xml config_file=/home/cisco/ansible/aci/configs/contract_config.xml host={{ inventory_hostname }} username={{ user }} password={{ pass }}


```


#### Notes

- Tenant must be exist prior to using this module


---


## aci_epg
Manages ACI end point groups and related contracts

  * Synopsis
  * Options
  * Examples

#### Synopsis
 Manages ACI end point groups and related contracts

#### Options

| Parameter     | required    | default  | choices    | comments |
| ------------- |-------------| ---------|----------- |--------- |
| username  |   yes  |  admin  | <ul></ul> |  Username used to login to the switch  |
| password  |   yes  |  C1sco12345  | <ul></ul> |  Password used to login to the switch  |
| protocol  |   no  |  https  | <ul> <li>http</li>  <li>https</li> </ul> |  Dictates connection protocol to use  |
| name  |   yes  |  | <ul></ul> |  Name of the application network profile  |
| prio  |   no  |  | <ul> <li>unspecified</li>  <li>level1</li>  <li>level2</li>  <li>level3</li> </ul> |  priority (qos class) for epg  |
| vmm_domain  |   no  |  | <ul></ul> |  desired vmm domain or list of vmm domains (VMware only)  |
| host  |   yes  |  | <ul></ul> |  IP Address or hostname of APIC resolvable by Ansible control host  |
| state  |   no  |  present  | <ul> <li>present</li>  <li>absent</li> </ul> |  Desired state of the epg  |
| anp  |   yes  |  | <ul></ul> |  name of the application profile this will be part of  |
| descr  |   no  |  | <ul></ul> |  description of the application network profile  |
| consumed_contracts  |   no  |  | <ul></ul> |  desired contract or list of consumed contracts  |
| bridge_domain  |   no  |  | <ul></ul> |  desired bridge domain or list of bridge domains  |
| tenant  |   yes  |  | <ul></ul> |  name of tenant this application network profile will be part of  |
| provided_contracts  |   no  |  | <ul></ul> |  desired contract or list of provided contracts  |


 
#### Examples

```

# ensure web epg exists
- aci_epg:
    name: Web_EPG
    consumed_contracts: Web_Con
    provided_contracts: App_Con
    bridge_domain: ACILab_BD1
    vmm_domain: My-vCenter
    anp: 3Tier_App
    tenant: ACILab
    state: present
    host: "{{ inventory_hostname }}"

# ensure app epg exists
- aci_epg:
    name: App_EPG
    consumed_contracts: App_Con
    provided_contracts: DB_Con
    bridge_domain: ACILab_BD1
    vmm_domain: My-vCenter
    anp: 3Tier_App
    tenant: ACILab
    state: present
    host: "{{ inventory_hostname }}"


```


#### Notes

- provided_contracts, consumed_contracts, vmm_domain, and bridge_domain could be supplied as a string or a list of names for that particular resource.  They also ensure each are in the desired state. This means if resources are already assigned and not in the new list (or str), they will be removed.


---


## aci_contract_subject
Manages contract subjects

  * Synopsis
  * Options
  * Examples

#### Synopsis
 Manages contract subjects that are a necessity for contracts

#### Options

| Parameter     | required    | default  | choices    | comments |
| ------------- |-------------| ---------|----------- |--------- |
| username  |   yes  |  admin  | <ul></ul> |  Username used to login to the switch  |
| in_filters  |   no  |  | <ul></ul> |  Filter or list of filters being applied inbound when the contract is applied as a policy between EPGs  |
| out_filters  |   no  |  | <ul></ul> |  Filter or list of filters being applied inbound when the contract is applied as a policy between EPGs  |
| protocol  |   no  |  https  | <ul> <li>http</li>  <li>https</li> </ul> |  Dictates connection protocol to use  |
| name  |   yes  |  | <ul></ul> |  Name of contract subject  |
| prio  |   no  |  | <ul> <li>unspecified</li>  <li>level1</li>  <li>level2</li>  <li>level3</li> </ul> |  priority (qos class) for subject (not per direction filters)  |
| apply_both_directions  |   no  |  True  | <ul> <li>true</li>  <li>false</li>  <li>yes</li>  <li>no</li> </ul> |  determines if the contract applies to both inbound and outbound traffic  |
| state  |   no  |  present  | <ul> <li>present</li>  <li>absent</li> </ul> |  Desired state of the contract subject  |
| contract  |   yes  |  | <ul></ul> |  Name of contract this subject will be applied to  |
| svc_graph  |   no  |  | <ul></ul> |  distinguished name of the service graph. The service graph is an image that shows the relationship between contracts and subjects. Not yet supported in this module.  |
| host  |   yes  |  | <ul></ul> |  IP Address or hostname of APIC (resolvable by Ansible control host)  |
| reverse_filter_ports  |   no  |  | <ul> <li>true</li>  <li>false</li>  <li>yes</li>  <li>no</li> </ul> |  Apply the same subject rule to the reverse filter ports when the contract applies in both directions.  apply_both_directions must be true to use this flag  |
| filters  |   no  |  | <ul></ul> |  Filter or list of filters being applied to the contract subject. To be used when a single filter is being applied in both directions.  |
| descr  |   no  |  | <ul></ul> |  description of contract subject  |
| password  |   yes  |  C1sco12345  | <ul></ul> |  Password used to login to the switch  |
| tenant  |   yes  |  | <ul></ul> |  Name of tenant the contract and subject will be applied to  |


 
#### Examples

```

# ensure contract subject for web exists
- aci_contract_subject: name=web_subject contract=Web_Con filters=Web_Filter tenant=ACILab host={{ inventory_hostname }}

# created a subject using a different filter for each direction
- aci_contract_subject: name=web_subject contract=Web_Con in_filters=arp out_filters=Web_Filter apply_both_directions=false descr='web subj2' tenant=ACILab host={{ inventory_hostname }}


```


#### Notes

- Tenant & Contract must be exist prior to using this module

- filters, in_filters, and out_filters can be a single filter or a list of filters.  In either case, it is the desired filters that should be applied to the contract subject.  This means if filters are already assigned and not in the new list, they will be removed.

- QOS class per filter "group" when not applying the same group of filters in not yet supported

- Service is currently not supported.


---


## aci_context
Manage private networks, contexts, in an ACI fabric

  * Synopsis
  * Options
  * Examples

#### Synopsis
 Offers ability to manage private networks. Each context is a private network associated to a tenant, i.e. VRF

#### Options

| Parameter     | required    | default  | choices    | comments |
| ------------- |-------------| ---------|----------- |--------- |
| username  |   yes  |  admin  | <ul></ul> |  Username used to login to the switch  |
| protocol  |   no  |  https  | <ul> <li>http</li>  <li>https</li> </ul> |  Dictates connection protocol to use  |
| name  |   yes  |  | <ul></ul> |  Name of context (private network / VRF)  |
| descr  |   no  |  | <ul></ul> |  description of context (private network)  |
| host  |   yes  |  | <ul></ul> |  IP Address or hostname of APIC resolvable by Ansible control host  |
| state  |   no  |  present  | <ul> <li>present</li>  <li>absent</li> </ul> |  Desired state of the context  |
| password  |   yes  |  C1sco12345  | <ul></ul> |  Password used to login to the switch  |
| tenant  |   yes  |  | <ul></ul> |  name of tenant the private network will be associated to  |


 
#### Examples

```
# ensure context for tenant exists (private network)
- aci_context: name=ACILab_VRF descr='ACILab VRF' tenant=ACILab host={{ inventory_hostname }} username={{ user }} password={{ pass }}

# ensure context for tenant exists (private network)
- aci_context: name=ACILab_VRF tenant=ACILab state=absent host={{ inventory_hostname }} username={{ user }} password={{ pass }}



```


#### Notes

- Tenant must be exist prior to using this module


---


## aci_anp
Manage top level application network profile objects

  * Synopsis
  * Options
  * Examples

#### Synopsis
 Manage top level application network profile object, i.e. this does not manage EPGs.

#### Options

| Parameter     | required    | default  | choices    | comments |
| ------------- |-------------| ---------|----------- |--------- |
| username  |   yes  |  admin  | <ul></ul> |  Username used to login to the switch  |
| protocol  |   no  |  https  | <ul> <li>http</li>  <li>https</li> </ul> |  Dictates connection protocol to use  |
| name  |   yes  |  | <ul></ul> |  Name of the application network profile  |
| descr  |   no  |  | <ul></ul> |  description of the application network profile  |
| host  |   yes  |  | <ul></ul> |  IP Address or hostname of APIC resolvable by Ansible control host  |
| state  |   yes  |  present  | <ul> <li>present</li>  <li>absent</li> </ul> |  Desired state of the application network profile  |
| password  |   yes  |  C1sco12345  | <ul></ul> |  Password used to login to the switch  |
| tenant  |   yes  |  | <ul></ul> |  name of tenant this application network profile will be part of  |


 
#### Examples

```

# ensure application network profile exists
- aci_anp: name=3Tier_App tenant=ACILab state=present host={{ inventory_hostname }} username={{ user }} password={{ pass }}


```



---


---
Created by Network to Code, LLC
For:
2015
