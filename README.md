# NetBox MCP Server - Read & Write Edition

**The first Model Context Protocol (MCP) server with full read AND write capabilities for NetBox.**

Unlike existing read-only NetBox MCP implementations, this server provides comprehensive CRUD (Create, Read, Update, Delete) operations, enabling you to not only query your NetBox data but also modify it directly through LLMs that support MCP.

## What Makes This Special

- **First Read-Write NetBox MCP**: The only MCP server that allows both reading from AND writing to NetBox
- **Full CRUD Operations**: Create, read, update, and delete any NetBox object type
- **Bulk Operations**: Efficiently handle multiple objects at once
- **Comprehensive Coverage**: Supports all major NetBox models (DCIM, IPAM, Circuits, Virtualization, etc.)
- **Production Ready**: Built with proper error handling, validation, and security practices

## Features

### Read Operations
- List and filter any NetBox object type
- Get detailed information about specific objects
- Access change history and audit trails
- Advanced filtering and search capabilities

### Write Operations
- Create new devices, IP addresses, sites, racks, and more
- Update existing object properties
- Delete objects (with proper safeguards)
- Bulk create, update, and delete operations

### Supported Object Types

**DCIM (Device and Infrastructure):**
- devices, device-types, device-roles, manufacturers
- sites, locations, racks, rack-roles
- cables, interfaces, power-ports, console-ports
- platforms, regions, virtual-chassis

**IPAM (IP Address Management):**
- ip-addresses, prefixes, vlans, vrfs
- asns, aggregates, services
- roles, rirs, route-targets

**Circuits:**
- circuits, circuit-types, providers
- circuit-terminations, provider-networks

**Virtualization:**
- virtual-machines, clusters, cluster-groups
- cluster-types, vm-interfaces

**And many more...**

## Installation

1. Clone this repository:
```bash
git clone https://github.com/alexkiwi1/netbox-mcp-rw.git
cd netbox-mcp-rw
```

2. Install dependencies:
```bash
# Using UV (recommended)
uv sync

# Or using pip
pip install -e .
```

3. Set environment variables:
```bash
export NETBOX_URL="https://your-netbox-instance.com/"
export NETBOX_TOKEN="your-api-token"
```

4. Test the server:
```bash
NETBOX_URL=https://netbox.example.com/ NETBOX_TOKEN=<your-token> uv run server.py
```

## MCP Client Configuration

### Claude Desktop (Mac/Windows)

Add to your Claude Desktop configuration:

```json
{
  "mcpServers": {
    "netbox-rw": {
      "command": "uv",
      "args": [
        "--directory",
        "/path/to/netbox-mcp-rw",
        "run",
        "server.py"
      ],
      "env": {
        "NETBOX_URL": "https://your-netbox-instance.com/",
        "NETBOX_TOKEN": "your-api-token"
      }
    }
  }
}
```

### Other MCP Clients

This server works with any MCP-compatible client. Adjust the command and arguments based on your client's requirements.

## Usage Examples

### Reading Data
```
"Show me all active devices in the NYC datacenter"
"List available IP addresses in the 10.0.1.0/24 subnet"
"What changes were made to devices last week?"
```

### Writing Data
```
"Create a new server called 'web-01' in rack R42 at site NYC-DC1"
"Add IP address 192.168.1.100/24 to device 'firewall-01'"
"Update device 'switch-01' status to maintenance mode"
"Create a new VLAN 100 named 'DMZ' at site headquarters"
```

### Bulk Operations
```
"Create 10 new servers with names web-01 through web-10"
"Update all Cisco devices to set the platform to 'ios'"
"Delete all IP addresses in the decommissioned subnet"
```

## Available Tools

### Device Management
- `netbox_get_objects` - List/filter any object type
- `netbox_get_object_by_id` - Get specific object details
- `netbox_create_object` - Create new objects
- `netbox_update_object` - Update existing objects
- `netbox_delete_object` - Delete objects
- `netbox_bulk_create_objects` - Bulk create operations
- `netbox_bulk_update_objects` - Bulk update operations
- `netbox_bulk_delete_objects` - Bulk delete operations

### Audit & History
- `netbox_get_changelogs` - Access change history and audit trails

## Security Features

- API tokens stored in environment variables (never hardcoded)
- SSL/TLS verification enabled by default
- Proper error handling and validation
- Audit trail preservation through NetBox's built-in changelog

## Requirements

- Python 3.13+
- NetBox instance with API access
- Valid NetBox API token with appropriate permissions

## Contributing

This is the first read-write NetBox MCP server - help us make it better:

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests
5. Submit a pull request

## Roadmap

- [ ] Add input validation with Pydantic models
- [ ] Implement caching for better performance
- [ ] Add async support
- [ ] Create comprehensive test suite
- [ ] Add support for custom fields and plugins

## License

This project is licensed under the Apache 2.0 License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments

Built on top of the excellent [FastMCP](https://github.com/pydantic/FastMCP) framework and NetBox's comprehensive REST API.

---

**Warning**: This server has write capabilities. Always test in a development environment first and ensure your API token has appropriate permissions. Use bulk operations carefully as they can modify many objects at once.