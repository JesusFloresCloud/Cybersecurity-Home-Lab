# EC2 Windows Domain Controller Network Recovery

## Issue
Windows Server Domain Controller became unreachable through:
- RDP
- Ping
- AWS Systems Manager

The instance continued booting normally and displayed the Windows login screen.

## Symptoms
- EC2 instance was running
- Security groups were verified
- SSM Agent was offline
- Remote connectivity was unavailable

## Root Cause
The Windows network adapter had been configured with a static IP configuration.

The adapter was set to:
- DHCP Disabled
- Static IPv4 address assigned manually
- Static DNS configuration

This caused the Windows network configuration to no longer align with AWS EC2 networking, which relies on the Elastic Network Interface (ENI) and DHCP for instance network configuration.

## Recovery Process
1. Attached the affected EC2 root volume to a helper Windows instance.
2. Loaded the offline SYSTEM registry hive.
3. Navigated to:

HKLM\DC01\ControlSet001\Services\Tcpip\Parameters\Interfaces\<NIC GUID>

4. Changed:

EnableDHCP = 0

to:

EnableDHCP = 1

5. Removed the static IP configuration entries.
6. Detached the volume and restored it to the original EC2 instance.

## Validation
After boot:
- SSM Agent returned online
- Network connectivity restored
- RDP access restored
- Instance became reachable

## Lessons Learned
In AWS environments, private IP assignments should generally be managed through the EC2 Elastic Network Interface (ENI), while the Windows adapter remains configured for DHCP.

This allows AWS to manage:
- IP assignment
- Gateway configuration
- DNS settings
- Network routing

## Preventive Action
If a static private IP is required, assign it through the AWS ENI configuration rather than manually setting a static IP inside Windows.
