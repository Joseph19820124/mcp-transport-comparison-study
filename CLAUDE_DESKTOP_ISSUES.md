# Claude Desktop Remote MCP Server Support Issues

## Current Status (June 2025)

Claude Desktop has **limited native support** for remote MCP servers using HTTP/SSE transport.

## Confirmed Issues

### 1. No Direct SSE Configuration
- ❌ Cannot configure remote HTTP/SSE servers in `claude_desktop_config.json`
- ❌ Only STDIO transport is supported in configuration file
- ⚠️ Remote servers require workarounds or paid plans

### 2. Integration Settings Limitation
- ✅ Remote MCP supported through Settings > Integrations
- ❌ **Requires Claude Pro, Team, or Enterprise plans**
- ❌ Free tier users cannot use remote MCP servers natively

### 3. Community Evidence

From official GitHub discussions:

> **Question**: "How do I add MCP server using HTTP with SSE transport?"
> 
> **Official Response**: "This is not supported at the moment."

Source: [MCP Discussion #16](https://github.com/orgs/modelcontextprotocol/discussions/16)

## Workaround Solutions

### Option 1: mcp-remote Proxy (Recommended)
```json
{
  "mcpServers": {
    "github-proxy": {
      "command": "npx",
      "args": ["mcp-remote", "https://api.githubcopilot.com/mcp/"]
    }
  }
}
```

### Option 2: Supergateway
```bash
npx -y supergateway --sse "https://remote-server.com/sse"
```

### Option 3: Custom Bridge
Several community solutions:
- `mcp-proxy` by sparfenyuk
- `mcp-gateway` by lightconetech
- `boilingdata/mcp-server-and-gw`

## Testing Remote MCP Server Connection

### Using MCP Inspector
```bash
# Test remote connection
npx @modelcontextprotocol/inspector
# Navigate to http://localhost:5173
# Enter your remote MCP server URL
```

### Using mcp-remote-client
```bash
# Direct test of remote server
npx -p mcp-remote@latest mcp-remote-client https://remote.mcp.server/sse
```

## Future Outlook

### Expected Improvements
1. **Native Remote Support**: Claude Desktop will likely add native HTTP/SSE support
2. **Streamable HTTP**: New transport protocol may replace SSE
3. **Better Authentication**: Improved OAuth flows
4. **Free Tier Support**: Remote MCP may become available to free users

### Migration Path
1. **Current**: Use proxy solutions for remote access
2. **Short-term**: Upgrade to paid plans for native integration
3. **Long-term**: Wait for native support in free tier

## Troubleshooting

### Common Issues

#### 1. Proxy Authentication Fails
```bash
# Enable debug logging
npx mcp-remote https://server.com/sse --debug
```

#### 2. Network Connectivity
```bash
# Test direct connection
curl -H "Accept: text/event-stream" https://server.com/sse
```

#### 3. OAuth Flow Problems
- Clear browser cache
- Check token expiration
- Verify OAuth app configuration

### Debug Logs Location
- **macOS**: `~/Library/Logs/Claude/mcp-server-*.log`
- **Windows**: `%APPDATA%\Claude\logs\mcp-server-*.log`
- **Linux**: `~/.config/Claude/logs/mcp-server-*.log`

## Recommendations

### For Free Tier Users
1. Use STDIO servers for local functionality
2. Use mcp-remote proxy for essential remote services
3. Consider upgrading to Pro plan for better remote support

### For Paid Plan Users
1. Configure remote servers via Settings > Integrations
2. Use official GitHub MCP server directly
3. Fallback to proxy solutions if needed

### For Developers
1. Provide both STDIO and HTTP/SSE implementations
2. Document Claude Desktop compatibility clearly
3. Test with proxy solutions
4. Prepare for future native support

---

**Last Updated**: June 14, 2025  
**Status**: Ongoing monitoring of Claude Desktop updates
