Ansible Role: Windows Admin Center
=========

[![Build Status](https://ci.zollo.net/buildStatus/icon?job=github-joezollo%2Fansible-role-windows-admin-center%2Fmaster)](https://ci.zollo.net/job/github-joezollo/job/ansible-role-windows-admin-center/job/master/)

Installs or removes Windows Admin Center on Windows Server or Windows 10. Windows Admin Center is a locally deployed, browser-based app for managing servers, clusters, hyper-converged infrastructure, and Windows 10 PCs. It comes at no additional cost beyond Windows and is ready to use in production.

Requirements
------------
* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016
* Windows Server 2019

Role Variables
--------------

Available variables are listed below, along with default values (see `defaults/main.yml`):

    wac_state: present

The state of Windows Admin Center, present (default) will install the latest version, absent will remove it.

    wac_cleanup_installer: false

Determines if the Windows Admin Center installer is deleted after installation.

    wac_download_url: https://aka.ms/WACDownload

Download to the MSI Installation file, defaults to Microsoft's CDN which has the latest available version.

    wac_download_path: C:\\Windows\\Temp\\wac.msi

The folder and file path where the Windows Admin Center installation artifact is downloaded to.

    wac_sme_port: 8443

The port that Windows Admin Center's HTTP web server will listen on. WAC does not use IIS so watch out for port conflicts.

    wac_ssl_certificate_option: generate

Defines how SSL certificates are handled, installer can generate a self signed certificate (use the value 'generate', which is the default) or use one that's already installed (change value to 'installed').

    wac_sme_thumbprint: 

The thubmprint of the certificate you wish to use for Windows Admin Center's web server. If the certificate option is set to installed, wac_sme_thumbprint is required. Note: There is no default value here.

    wac_product_id: '{65E83844-8B8A-42ED-B78D-BA021BE4AE83}'

Windows Admin Center's MSI Installer ID, only change this if you know what you're doing.

    wac_constrained_delegation_enabled: false

When enabled, configures your Active Directory for constrained delegation on nodes specified in `wac_constrained_delegation_list`.

    wac_server_hostname: ""

The Active Directory Computer Object name of your Windows Admin Center Gateway Server, depending on how you structure your inventory file, it can often be set to `{{ inventory_hostname }}`.

    wac_constrained_delegation_ad_server: ""

Inventory name of an Active Directory Domain Controller, should line up with your inventory. Actions are delegated to this server so authentication will need to already be defined.

    wac_constrained_delegation_list: []

List of Active Directory Computer Objects that you wish to grant constrained delegation access to (via Windows Admin Center). Depending on how you structure your inventory file, it can often be set to a group list object `{{ groups['windows_group_name'] }}`.

Dependencies
------------

None

Example Playbook
----------------

    - hosts: win_servers
      vars:
        wac_state: present
      roles:
         - joezollo.windows_admin_center

    - hosts: win_servers
      vars:
        wac_state: present
        wac_constrained_delegation_enabled: true
        wac_constrained_delegation_ad_server: jz-srv01
        wac_server_hostname: zollo-srv09
        wac_constrained_delegation_list:
          - jz-srv01
          - jz-srv02
          - jz-srv03
          - jz-srv04
      roles:
         - joezollo.windows_admin_center


License
-------

WIP

Author Information
------------------

This role was created by Joseph Zollo (joe@zollo.net)
