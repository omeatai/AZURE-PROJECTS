# Azure certification foundation: from on-premises domains to cloud services

## Introduction

This document turns a course transcript into clear study notes for Microsoft Azure and identity exams. It explains how classic on-premises **Microsoft Active Directory** domains work (controllers, DNS, Group Policy, VPNs, DMZs, virtualization) and how that history connects to **Microsoft Azure** (infrastructure as a service), **Microsoft 365** (productivity and collaboration), and **Microsoft Entra ID** (cloud identity). When you finish, you should be able to describe on-prem versus cloud identity, name common protocols and edge designs, and explain **IaaS**, **PaaS**, and **SaaS** in plain language.

## Table of contents

- [Introduction](#introduction)
- [Why organizations moved from peer-to-peer to domains](#why-organizations-moved-from-peer-to-peer-to-domains)
- [Active Directory Domain Services (AD DS) on premises](#active-directory-domain-services-ad-ds-on-premises)
  - [Domain controllers and replication](#domain-controllers-and-replication)
  - [Authentication and directory protocols](#authentication-and-directory-protocols)
- [DNS and name resolution in a domain](#dns-and-name-resolution-in-a-domain)
- [Group Policy Objects (GPOs)](#group-policy-objects-gpos)
- [Firewalls, the internet, and the network edge](#firewalls-the-internet-and-the-network-edge)
- [Remote access with VPNs and RRAS](#remote-access-with-vpns-and-rras)
- [DMZ and perimeter networks for public-facing servers](#dmz-and-perimeter-networks-for-public-facing-servers)
- [Virtualization and on-premises elasticity](#virtualization-and-on-premises-elasticity)
- [How cloud hosting grew out of virtualization](#how-cloud-hosting-grew-out-of-virtualization)
- [Service models: IaaS, PaaS, and SaaS](#service-models-iaas-paas-and-saas)
- [Microsoft Azure and Microsoft 365 at a high level](#microsoft-azure-and-microsoft-365-at-a-high-level)
- [Microsoft Entra ID and hybrid identity](#microsoft-entra-id-and-hybrid-identity)
  - [Microsoft Entra Connect and Microsoft Entra Cloud Sync](#microsoft-entra-connect-and-microsoft-entra-cloud-sync)
- [Quick recap](#quick-recap)
- [Key terms](#key-terms)

## Why organizations moved from peer-to-peer to domains

- [x] In early PC networking, **peer-to-peer** meant every computer was equal, with no single place to manage users or settings—admins often touched each machine manually.
- [x] **File servers** and directory products (historically vendors such as Novell NetWare in the market) helped centralize files and management.
- [x] Microsoft introduced **Windows NT** and the idea of a **domain controller**: a server role that could manage other computers in a **domain** (a logical security boundary with shared accounts and policies).
- [x] **Active Directory** shipped with Windows Server 2000-era technology and became the standard for on-premises **Windows** enterprise identity and administration for many years.

## Active Directory Domain Services (AD DS) on premises

- [x] **Active Directory Domain Services (AD DS)** is the on-premises role that stores directory data (users, groups, computers, and more) in a database hosted on **domain controllers**.
- [x] People often say “Active Directory” to mean the whole on-prem directory system; in documentation, **AD DS** is the precise Windows Server role name.

### Domain controllers and replication

- [x] Organizations run **multiple domain controllers** for load distribution and **redundancy**—if one fails, others can still authenticate users.
- [x] Domain controllers **replicate** directory changes to each other so account and policy updates converge across the forest.

### Authentication and directory protocols

- [x] **Kerberos** is the primary authentication protocol used in modern Active Directory environments (ticket-based sign-in between clients and domain controllers).
- [x] **NTLM** (NT LAN Manager) is a legacy protocol kept for compatibility with very old scenarios; exam and security discussions still mention it, but Kerberos is the normal path today.
- [x] **LDAP** (Lightweight Directory Access Protocol) is the language clients and apps often use to **query** and interact with directory data.

## DNS and name resolution in a domain

- [x] **DNS** (Domain Name System) maps friendly names (for example `server01.contoso.com`) to **IP addresses** so computers can find each other.
- [x] In an AD DS environment, DNS is essential: clients locate domain controllers and services by name.
- [x] **DNS servers** hold zone data (records). Clients and servers **register** and **query** DNS so name resolution is centralized instead of manual.

## Group Policy Objects (GPOs)

- [x] **Group Policy Objects (GPOs)** are settings bundles linked to sites, domains, or organizational units that apply configuration to users and computers.
- [x] Examples include security baselines (firewall posture), software deployment, and desktop restrictions—GPOs replicate like other directory data so policy is consistent.

## Firewalls, the internet, and the network edge

- [x] A **firewall** filters traffic between a trusted internal network and untrusted networks (typically the **internet**), reducing exposure of internal servers and clients.
- [x] Classic on-prem designs placed internal resources behind a firewall and only allowed inbound traffic deliberately.

## Remote access with VPNs and RRAS

- [x] Remote workers should not “open every port” to the internet; that increases risk from attackers probing for weak services.
- [x] A **VPN** (virtual private network) builds an **encrypted tunnel** so a remote client can reach internal resources as if it were on the corporate network, without exposing raw internal protocols to the public internet.
- [x] Historically, Windows environments often used **RRAS** (Routing and Remote Access Service) on a server to terminate VPN connections (third-party VPN concentrators are also common).

## DMZ and perimeter networks for public-facing servers

- [x] Putting a public **web server** on the same trusted LAN as databases and file shares can be dangerous: if the web app is compromised, attackers may **pivot** to other internal systems.
- [x] A common mitigation is a **DMZ** (demilitarized zone), also called a **perimeter network**: a separate network segment between two firewalls, with only required ports (for example HTTP **80** and HTTPS **443**) allowed from the internet.
- [x] If a DMZ host is compromised, **firewall rules** still limit lateral movement into the internal corporate network.

## Virtualization and on-premises elasticity

- [x] A **hypervisor** is software that runs **virtual machines (VMs)** by emulating hardware; examples include **Microsoft Hyper-V** and other vendors’ platforms.
- [x] Virtualization reduces the need for a physical server per workload and enables **snapshots/checkpoints** for rollback during changes.
- [x] On a virtual host, **CPU** and **RAM** can be **overcommitted** or pooled so workloads share unused capacity—a small-scale analogy to **elasticity** in the cloud.

## How cloud hosting grew out of virtualization

- [x] Large providers operate huge **data centers**; offering **hosted VMs** and services for a fee is the business roots of **public cloud** IaaS.
- [x] Customers pay to offload facility power, cooling, hardware procurement, and part of the operational burden—similar to paying a mechanic instead of only changing your own oil.

## Service models: IaaS, PaaS, and SaaS

- [x] **IaaS** (Infrastructure as a Service): you rent foundational building blocks—virtual machines, networks, storage, load balancers, and related primitives—while you still manage guest OS and application responsibilities depending on the offering.
- [x] **PaaS** (Platform as a Service): the provider runs a **managed platform** (runtime, middleware, scaling hooks); you bring application logic and configuration, with less infrastructure plumbing than raw IaaS.
- [x] **SaaS** (Software as a Service): a **ready-to-use application** delivered over the internet under a subscription (web mail, collaboration suites, and so on).

## Microsoft Azure and Microsoft 365 at a high level

- [x] **Microsoft Azure** is Microsoft’s public cloud; it is strongly associated with **IaaS** (VMs, networks, storage) but also includes many **PaaS** and **SaaS**-style services across the catalog.
- [x] **Microsoft 365** bundles productivity and collaboration services (for example Exchange Online, SharePoint Online, Microsoft Teams, client apps) and is largely **SaaS/PaaS** from an admin perspective.
- [x] Microsoft 365 relies on Azure infrastructure; customer experiences are linked through shared identity and licensing models.

## Microsoft Entra ID and hybrid identity

- [x] **Microsoft Entra ID** is Microsoft’s cloud **identity and access management** service (formerly branded **Azure Active Directory** or **Azure AD**). Many older articles still say “Azure AD.”
- [x] It holds cloud **users**, **groups**, **app registrations**, and conditional access policies—similar concepts to on-prem AD, but not a literal copy of every AD feature.
- [x] **Microsoft Intune** provides **mobile device management (MDM)** and **mobile application management (MAM)** and is the modern cloud counterpart to many scenarios once handled only with on-prem **GPOs** (both can coexist in hybrid shops).

### Microsoft Entra Connect and Microsoft Entra Cloud Sync

- [x] **Microsoft Entra Connect** (successor name for what was commonly called **Azure AD Connect**) is the traditional on-premises **sync** tool that publishes selected on-prem AD objects to Entra ID and supports hybrid scenarios such as **password hash sync**, **pass-through authentication**, or **federation** depending on design.
- [x] **Microsoft Entra Cloud Sync** is Microsoft’s **lighter-weight** agent-based option for many directory sync needs; Microsoft documents decision guidance for Connect versus Cloud Sync.
- [x] **Single sign-on (SSO)** aims for one identity to reach both on-prem and cloud resources smoothly when hybrid identity is configured.
- [x] Hybrid sync is primarily **from AD to the cloud** for objects you choose to synchronize; cloud-created users are not simply “reversed” into on-prem AD with classic Connect the way the course described—always verify current Microsoft Learn behavior for your scenario.

## Quick recap

- [x] On-prem **AD DS** centralizes accounts and policy; **DNS** makes names work; **GPOs** push configuration.
- [x] **VPNs** and **DMZs** address remote access and public service exposure without flattening all security boundaries.
- [x] **Virtualization** led to efficient sharing of hardware and foreshadowed cloud elasticity.
- [x] **Azure** emphasizes infrastructure and a huge service catalog; **Microsoft 365** emphasizes productivity services; **Entra ID** is the shared cloud identity plane for Microsoft’s cloud ecosystem.

## Key terms

| Term | Meaning |
| --- | --- |
| AD DS | Active Directory Domain Services—the Windows Server on-premises directory role. |
| Kerberos | Ticket-based authentication protocol used in Active Directory. |
| LDAP | Lightweight Directory Access Protocol—directory query/access language. |
| DNS | Domain Name System—maps names to IP addresses. |
| GPO | Group Policy Object—bundle of managed settings applied in AD. |
| VPN | Virtual private network—encrypted tunnel for remote access. |
| DMZ / perimeter network | Segmented network for internet-facing systems between security boundaries. |
| Hypervisor | Software that runs virtual machines (for example Hyper-V). |
| IaaS / PaaS / SaaS | Infrastructure, platform, and software as service—who manages what stack layers. |
| Microsoft Entra ID | Microsoft cloud identity service (formerly Azure AD). |
| Microsoft Entra Connect / Cloud Sync | Tools to synchronize on-premises AD with Entra ID (different footprints and features). |

---

Last reviewed: 2026-04-20 (aligned with Microsoft Learn naming: Microsoft Entra ID, Microsoft Entra Connect, Microsoft Entra Cloud Sync; course transcript used older spoken names in places).
