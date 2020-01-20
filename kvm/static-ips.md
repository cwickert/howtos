## Using fixed IPs in KVM

### Problem
By default, KVM uses dnsmasq to resolve host names (DNS) and dynamically assign IP addresses (DHCP). This works out of the box and is fine until the lease expires. If you haven't used a VM for a while, it will be assigned a different IP next time you boot it.
Of course you can configure a static IP address and configuration DNS severs and gateway manually on the VM guest, but KVM will not be able to resolve host names for guests you configured manually.

#### Solution
There are two possible solutions to this problem:
 1. Use static IPs on the VM hosts. As this is configured by the guest's OS, it cannot be managed centrally and names of these host won't resolve.
 2. Use DHCP with fixed IP addresses. This can be configured through libvirt

## Let's get started

For the sake of this exercise, we create a new virtual network. As we don't want to write XML, we use virt-manager or virsh

Example 

    <network>
      <name>example</name>
      <uuid>## Using fixed IPs in KVM

### Problem
By default, KVM uses dnsmasq to resolve host names (DNS) and dynamically assign IP addresses (DHCP). This works out of the box and is fine until the lease expires. If you haven't used a VM for a while, it will be assigned a different IP next time you boot it.
Of course you can configure a static IP address and configuration DNS severs and gateway manually on the VM guest, but KVM will not be able to resolve host names for guests you configured manually.

#### Solution
There are two possible solutions to this problem:
 1. Use static IPs on the VM hosts. As this is configured by the guest's OS, it cannot be managed centrally and names of these host won't resolve.
 2. Use DHCP with fixed IP addresses. This can be configured through libvirt

## Let's get started

For the sake of this exercise, we create a new virtual network. As we don't want to write XML, we use virt-manager or virsh

Example 

    <network>
      <name>example</name>
      <uuid>a21487fa-7b01-4342-b9a0-70c5388cfa95</uuid>
      <forward mode='nat'/>
      <bridge name='virbr1' stp='on' delay='0'/>
      <mac address='52:54:00:53:86:4b'/>
      <domain name='example.com'/>
      <ip address='192.168.111.1' netmask='255.255.255.0'>
        <dhcp>
          <range start='192.168.111.128' end='192.168.111.254'/>
          <host mac='52:54:00:01:11:11' name='caasp-admin.example.com' ip='192.168.111.11'/>
          <host mac='52:54:00:01:11:12' name='caasp-master.example.com' ip='192.168.111.12'/>
          <host mac='52:54:00:01:11:31' name='caasp-admin.example.com' ip='192.168.111.31'/>
          <host mac='52:54:00:01:11:32' name='caasp-master.example.com' ip='192.168.111.32'/>
          <host mac='52:54:00:01:11:41' name='sles12.example.com' ip='192.168.111.41'/>
          <host mac='52:54:00:01:11:42' name='sled12.example.com' ip='192.168.111.42'/>
          <host mac='52:54:00:01:11:51' name='sles15.example.com' ip='192.168.111.51'/>
          <host mac='52:54:00:01:11:52' name='sled15.example.com' ip='192.168.111.52'/>
        </dhcp>
      </ip>
    </network>



### Additional work
KVM's dnsmasq only listens on KVM's virbrX interface, IPs won't  resolve with the system resolver. The easiest way is to add them to /etc/hosts.
</uuid>
      <forward mode='nat'/>
      <bridge name='virbr1' stp='on' delay='0'/>
      <mac address='52:54:00:53:86:4b'/>
      <domain name='example.com'/>
      <ip address='192.168.111.1' netmask='255.255.255.0'>
        <dhcp>
          <range start='192.168.111.128' end='192.168.111.254'/>
          <host mac='52:54:00:01:11:11' name='caasp-admin.example.com' ip='192.168.111.11'/>
          <host mac='52:54:00:01:11:12' name='caasp-master.example.com' ip='192.168.111.12'/>
          <host mac='52:54:00:01:11:31' name='caasp-admin.example.com' ip='192.168.111.31'/>
          <host mac='52:54:00:01:11:32' name='caasp-master.example.com' ip='192.168.111.32'/>
          <host mac='52:54:00:01:11:41' name='sles12.example.com' ip='192.168.111.41'/>
          <host mac='52:54:00:01:11:42' name='sled12.example.com' ip='192.168.111.42'/>
          <host mac='52:54:00:01:11:51' name='sles15.example.com' ip='192.168.111.51'/>
          <host mac='52:54:00:01:11:52' name='sled15.example.com' ip='192.168.111.52'/>
        </dhcp>
      </ip>
    </network>



### Additional work
KVM's dnsmasq only listens on KVM's virbrX interface, IPs won't  resolve with the system resolver. The easiest way is to add them to /etc/hosts.
