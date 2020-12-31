
# Routes
- System-generated:
  - Default route:
    - path for traffic to leave the VPC network
    - used for internet and Private Googel Access
    - applies to the whole VPC network
    - Can be removed or replaced
  - Subnet routes
    - at least one for each IP range (primary and secondary, if exists)
    - paths to reach VMs that use subnets
    - no other route can have a destination that matches or is more specific than the destination of a subnet route
    - applies to the whole VPC network
    - created, updated and removed automatically
- Custom routes
  - static routes
    - created manually
  - dynamic routes, 
    - maintained automatically by Cloud Routers
    - destination always represent external IP address ranges, received from a BGP peer
    - used by:
      - Dedicated Interconnect
      - Partner Interconnect
      - HA VPN tunnels
      - Classic VPN tunnels that use dynamic routing
    - VPC networks ignore destinations when:
      - it exactly matches a subnet IP address range
      - it fits within a subnet IP address range
- Peering subnet routes
  - Paths to resources using subnets in another VPC connected using VPC Network Peering
    - Peer networks always export their subnet routes
    - Peer networks automatically import subnet routes
    - Imported peering subnet routes cannot be removed, unless you disable the peering
    - When peering is disabled, all imported routes are automatically removed
    - GCP prevents IP ranges conflicts between peered networks
- Peering custom routes
  - You can configure peer networks to export or import custom routes (both static and dynamic)
    - It omits these types of routes:
      - static routes that use the `next-hop-gateway` (default internet gateway next hop)
      - static routes scoped to instances by network tag

## Dynamic routing mode
- Each VPC network has an associated dynamic routing mode
- Learn custom dynamic routes from connected networks when VPC network is connect to another network, using Cloud VPN tunnel that uses dynamic routing or by using Dedicated Interconnect or Partner Interconnect
- Types:
  - Regional dynamic routing
  - Global dynamic routing

## Static route parameters
- Name
- Description (optional)
- Network
- Destination range
- Priority
- Next hop
  - Next Hop Gateway (next-hop-gateway) -> default internet gateway, GCP instance or a Cloud VPN tunnel)
  - Next Hop Instance (next-hop-instance) -> existing instance in GCP by specifying name and zone
  - Next Hop Internal TCP/UDP Load Balancer (next-hop-ilb)
  - Next Hop IP (next-hop-address) -> internal IP address assigned to a VM
  - Next Hop VPN Tunnel (next-hop-vpn-tunnel)
- Tags (network tags)

## Applicability and order
### Applicable routes
- Subnet and peering subnet routes apply to all instances
- System-generated default route applies to all instances
- Custom static routes:
  - Apply to all instances if route has no network tag
  - Apply to specific instances if route has newtowrk tag (applies to isntances that have the same tag)
- Dynamic routes apply to instances based on the dynamic routing mode of the VPC network
  - if regional or global route
- For some load-balanced traffic, the applicable routes originate outside your VPC network

### Routing order
Route is selected according to the following order:
1. Subnet routes and peering subnet routes are considered first
1. Looks for a static, dynamic, or peering custom route that has the most specific destination
1. If more than one route has the same most specific destination:
   1. If your VPC network is connected to peer networks, Google Cloud eliminates all route candidates except for those from a single VPC network
   2. Google Cloud eliminates all route candidates except for those that have the highest priority. If this results in a single remaining route
   3. Google Cloud continues the elimination process using the route's next hop and route's type. If this results in a single remaining route, Google Cloud sends the packet to its next hop.
   4. If more than one route remains from the route candidates, those routes all exist within the same VPC network and have the same highest priority. Google Cloud distributes traffic among the next hops of the route candidates by using a five-tuple hash for affinity
1. If no applicable destination is found, Google Cloud drops the packet, replying with an ICMP destination or network unreachable error.

### Special return paths
- Special routes for certain services
- Defined outside VPC network
  - Don't appear in VPC network routing table
  - Cannot be removed or overridden
1. Load balancer return paths
2. IAP - A route for 35.235.240.0/20 is included to support TCP forwarding using IAP
3. Cloud DNS - A route to 35.199.192.0/19 supports connectivity to forwarding targets that use private routing or alternative name servers that use private routing

additional considerations:
https://cloud.google.com/vpc/docs/routes#instance_next_hops
