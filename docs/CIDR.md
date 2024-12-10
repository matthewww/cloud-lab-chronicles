# CIDR (Classless Inter-Domain Routing) ranges

1. **CIDR Notation**:
   - CIDR is written as `IP/prefix` (e.g., `192.168.0.0/16`), where:
     - `IP` is the network address.
     - `prefix` indicates how many bits are reserved for the network (subnet mask).
   - The smaller the prefix (e.g., `/16`), the larger the range of IPs in that block.

2. **IP Address Range**:
   - Each `/X` corresponds to 2^(32-X) addresses for IPv4.
     - `/24` → 256 addresses (most common for subnets).
     - `/16` → 65,536 addresses (often for large VPCs).
     - `/28` → 16 addresses (small subnets for things like NAT gateways or bastions).
   - The first address is reserved for the **network identifier** and the last is for the **broadcast address**, leaving 2^(32-X) - 2 usable addresses.

3. **Common CIDR Blocks**:
   - For private subnets:
     - **10.0.0.0/8** (large range for private IPs).
     - **172.16.0.0/12** (medium range).
     - **192.168.0.0/16** (small range, often for home networks).

4. **AWS VPC and Subnetting**:
   - A **VPC CIDR range** must be between `/16` and `/28`.
   - Subnets divide the VPC CIDR into smaller blocks (subnet masks like `/24` or `/26`).

5. **Overlapping CIDRs**:
   - VPC peering or hybrid cloud setups (e.g., with on-premises networks) require non-overlapping CIDR ranges.

6. **Subnet Calculations**:
   - Know how to calculate available IPs and split a given CIDR range into subnets.
     Example: Splitting a `/16` (65,536) into `/24` (256) subnets gives 2^(24-16) = 256 subnets, each with 256 IPs.

### Things to Watch for in Exam Questions:
- Ensure subnets fit into the VPC CIDR range.
- Look for conflicts in CIDR blocks for multi-VPC or hybrid networking.
- When choosing subnets, remember to allocate sufficient IP addresses for future scaling (e.g., NAT, ALB, etc.).
