Role Name
=========

Installs Suricata IDS/IPS Role - http://suricata-ids.org/

Requirements
------------

None

Role Variables
--------------
````
---
# defaults file for ansible-suricata
config_suricata: false  #defines if suricata should be configured
scripts_dir: /opt/scripts
suricata_action_order:
  - pass
  - drop
  - reject
  - alert
suricata_af_packet_interfaces:
  - int: eth0
    threads: 1
    defrag: "yes"
    cluster_type: cluster_flow  #cluster_round_robin, cluster_flow or cluster_cpu
    cluster_id: 99
    copy_mode: ips
    copy_iface: eth1
    buffer_size: 64535
    disable_promisc: "no"  # Set to yes to disable promiscuous mode
    use_nmap: "yes"
  - int: eth1
    threads: 1
    defrag: "yes"
    cluster_type: cluster_flow  #cluster_round_robin, cluster_flow or cluster_cpu
    cluster_id: 98
    copy_mode: ips
    copy_iface: eth0
    buffer_size: 64535
    disable_promisc: "no"  # Set to yes to disable promiscuous mode
    use_nmap: "yes"
suricata_classification_file: /etc/suricata/classification.config
suricata_config_outputs: true
suricata_default_rule_path: /etc/suricata/rules
suricata_flow_timeouts:
  - name: default
    new: 30
    established: 300
    closed: 0
    emergency_new: 10
    emergency_established: 100
    emergency_closed: 0
  - name: tcp
    new: 60
    established: 3600
    closed: 120
    emergency_new: 10
    emergency_established: 100
    emergency_closed: 20
  - name: udp
    new: 30
    established: 300
    emergency_new: 10
    emergency_established: 100
  - name: icmp
    new: 30
    established: 300
    emergency_new: 10
    emergency_established: 100
suricata_host_mode: auto  #defines surica operating mode...Options are auto, router (IPS-Mode) or sniffer-only (IDS-Mode)
#    suricata_include_files:  #Files included here will be handled as if they were inlined in the configuration file.
#        - include1.yaml
#        - include2.yaml
suricata_interfaces:  #define the interfaces on your suricata host and define if offloading should be disabled.
  - int: eth0
    disable_offloading: false
    disable_features:
      - gro
      - gso
      - lro
      - rx
      - rxhash
      - rxvlan
      - sg
      - tso
      - tx
      - txvlan
      - ufo
  - int: eth1
    disable_offloading: false
    disable_features:
      - gro
      - gso
      - lro
      - rx
      - rxhash
      - rxvlan
      - sg
      - tso
      - tx
      - txvlan
      - ufo
suricata_log_dir: /var/log/suricata/
suricata_oinkmaster_rules_url: http://rules.emergingthreats.net/open/suricata/emerging.rules.tar.gz
suricata_outputs:
  - name: fast
    enabled: "yes"
    filename: fast.log
    append: "yes"
  - name: eve-log
    enabled: "yes"
    type: file  #file|syslog|unix_dgram|unix_stream
    filename: eve.json
    types:
      - name: "alert"
      - name: "http:"
        extended: "yes"  # enable this for extended logging information
      - name: "dns"
      - name: "tls:"
        extended: "yes"  # enable this for extended logging information
      - name: "files:"
        force_magic: "no"  # force logging magic on all logged files
        force_md5: "no"  # force logging of md5 checksums
#            - name: "drop"
      - name: "ssh"
  - name: unified2-alert
    enabled: "yes"
    filename: unified2.alert
    limit: 32mb  # File size limit.  Can be specified in kb, mb, gb.
    xff:
      - var: enabled
        val: "no"
      - var: mode
        val: extra-data
      - var: header
        val: X-Forwarded-For
  - name: http-log
    enabled: "yes"
    filename: http.log
    append: "yes"
  - name: tls-log
    enabled: "no"
    filename: tls.log
    append: "yes"
    certs_log_dir: certs
  - name: dns-log
    enabled: "no"
    filename: dns.log
    append: "yes"
  - name: pcap-info
    enabled: "no"
  - name: pcap-log
    enabled: "no"
    filename: log.pcap
    limit: 1000mb
    max_files: 2000
    mode: normal  # normal or sguil
    use_stream_depth: "no"  #If set to "yes" packets seen after reaching stream inspection depth are ignored. "no" logs all packets
  - name: alert-debug
    enabled: "no"
    filename: alert-debug.log
    append: "yes"
  - name: alert-prelude
    enabled: "no"
    profile: suricata
    log_packet_content: "no"
    log_packet_header: "yes"
  - name: stats
    enabled: "yes"
    filename: stats.log
    interval: 8
  - name: syslog
    enabled: "no"
    facility: local5
    level: Info  # possible levels: Emergency, Alert, Critical, Error, Warning, Notice, Info, Debug
  - name: drop
    enabled: "no"
    filename: drop.log
    append: "yes"
  - name: file-store
    enabled: "no"
    log_dir: files
    force_magic: "no"
    force_md5: "no"
  - name: file-log
    enabled: "no"
    filename: files-json.log
    append: "yes"
    force_magic: "no"
    force_md5: "no"
suricata_pfring_interfaces:
  - int: eth0
    threads: 1
    cluster_id: 99
    cluster_type: cluster_flow
  - int: eth1
    threads: 1
    cluster_id: 93
    cluster_type: cluster_flow
suricata_reference_config_file: /etc/suricata/reference.config
suricata_rules:
  - botcc.rules
  - ciarmy.rules
  - compromised.rules
  - drop.rules
  - dshield.rules
  - emerging-activex.rules
  - emerging-attack_response.rules
  - emerging-chat.rules
  - emerging-current_events.rules
  - emerging-dns.rules
  - emerging-dos.rules
  - emerging-exploit.rules
  - emerging-ftp.rules
  - emerging-games.rules
  - emerging-icmp_info.rules
#        - emerging-icmp.rules
  - emerging-imap.rules
  - emerging-inappropriate.rules
  - emerging-malware.rules
  - emerging-misc.rules
  - emerging-mobile_malware.rules
  - emerging-netbios.rules
  - emerging-p2p.rules
  - emerging-policy.rules
  - emerging-pop3.rules
  - emerging-rpc.rules
  - emerging-scada.rules
  - emerging-scan.rules
  - emerging-shellcode.rules
  - emerging-smtp.rules
  - emerging-snmp.rules
  - emerging-sql.rules
  - emerging-telnet.rules
  - emerging-tftp.rules
  - emerging-trojan.rules
  - emerging-user_agents.rules
  - emerging-voip.rules
  - emerging-web_client.rules
  - emerging-web_server.rules
  - emerging-web_specific_apps.rules
  - emerging-worm.rules
  - tor.rules
  - decoder-events.rules # available in suricata sources under rules dir
  - stream-events.rules  # available in suricata sources under rules dir
  - http-events.rules    # available in suricata sources under rules dir
  - smtp-events.rules    # available in suricata sources under rules dir
  - dns-events.rules     # available in suricata sources under rules dir
  - tls-events.rules     # available in suricata sources under rules dir
suricata_ubuntu_ppa: ppa:oisf/suricata-stable  #Options are ppa:oisf/suricata-stable, ppa:oisf/suricata-beta or ppa:oisf/suricata-daily
suricata_unix_command:
  enabled: "no"
#        filename: custom.socket

````

Dependencies
------------

None

Example Playbook
----------------

    - hosts: all
      roles:
         - { role: mrlesmithjr.suricata }

Vagrant testing
---------------

Included is a Vagrantfile in order to test using Vagrant. Adjust node settings in nodes.yml if required. http://everythingshouldbevirtual.com/learning-vagrant-and-ansible-provisioning

###### suricata.yml (change config_suricata: true to actually configure suricata)

````
---
- hosts: all
  remote_user: vagrant
  sudo: true
  vars:
    - config_suricata: false
  roles:
    - { role: mrlesmithjr.suricata }
````
###### To spin up Vagrant node(s)

````
vagrant up
vagrant ssh
cd /vagrant
ansible-playbook -i hosts suricata.yml
````

License
-------

BSD

Author Information
------------------

Larry Smith Jr.
- @mrlesmithjr
- http://everythingshouldbevirtual.com
- mrlesmithjr [at] gmail.com
