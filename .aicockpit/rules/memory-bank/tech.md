# Technology Stack - 4Devs MCP Server

## Core Technologies

### Model Context Protocol (MCP)

- **Purpose**: Standard protocol for AI tool integration
- **Version**: Latest MCP SDK
- **Benefits**: Standardized interface, broad compatibility with AI tools
- **Documentation**: <https://modelcontextprotocol.io/>

### API Integration

- **Target API**: 4Devs Brazilian Document Generator
- **Endpoint**: `https://www.4devs.com.br/ferramentas_online.php`
- **Method**: POST with form-data encoding
- **Authentication**: None required (public API)
- **Rate Limiting**: Unknown - needs investigation during implementation

## Technology Stack Decision (Pending)

### Option 1: TypeScript/Node.js

**Pros:**

- Excellent web service integration
- Strong typing with TypeScript
- Rich ecosystem for HTTP clients
- Good MCP SDK support
- Familiar to web developers

**Cons:**

- More complex build process
- Larger runtime footprint

**Best For:**

- Web development teams
- JavaScript/TypeScript environments
- Integration with web applications

### Option 2: Python

**Pros:**

- Simple deployment
- Excellent for data processing
- Strong HTTP libraries (requests, httpx)
- Good MCP SDK support
- Familiar to data science teams

**Cons:**

- Dynamic typing (can use type hints)
- Potentially slower for high-throughput

**Best For:**

- Data science workflows
- ML/AI development environments
- Rapid prototyping

## Development Setup

### TypeScript Path

```bash
# Bootstrap
npx @modelcontextprotocol/create-server 4devs-mcp-server
cd 4devs-mcp-server
npm install

# Additional dependencies
npm install axios form-data
npm install -D @types/node typescript ts-node
```

### Python Path

```bash
# Setup
pip install mcp
# Or with uv (recommended)
uv add "mcp[cli]"

# Additional dependencies
pip install httpx python-multipart
```

## Dependencies

### Core Dependencies

- **MCP SDK**: Model Context Protocol implementation
- **HTTP Client**: For API requests (axios/httpx)
- **Form Data**: For multipart form encoding
- **Logging**: Comprehensive request/response logging

### Development Dependencies

- **TypeScript**: Type definitions and compilation (if TS)
- **Testing Framework**: Jest (TS) or pytest (Python)
- **Linting**: ESLint (TS) or ruff (Python)
- **Type Checking**: Built-in (TS) or mypy (Python)

## Technical Constraints

### 4Devs API Limitations

- **Single Endpoint**: All functionality through one URL
- **Form Data Only**: No JSON request bodies
- **Batch Limits**: Maximum 30 records per request
- **Geographic Dependencies**: Cities must be loaded by state first
- **Response Formats**: Mixed JSON and HTML responses

### MCP Protocol Requirements

- **Tool Definitions**: Each endpoint becomes an MCP tool
- **Input Schemas**: JSON Schema validation for all inputs
- **Error Handling**: Structured error responses
- **Logging**: Comprehensive debugging information

### Performance Considerations

- **Network Latency**: External API calls add latency
- **Rate Limiting**: Unknown limits - need graceful handling
- **Memory Usage**: Batch operations may use significant memory
- **Error Recovery**: Robust retry mechanisms needed

## Tool Usage Patterns

### Request Flow

1. **Input Validation**: JSON Schema validation
2. **Parameter Mapping**: Convert MCP inputs to form-data
3. **API Request**: POST to 4Devs endpoint
4. **Response Processing**: Parse JSON/HTML responses
5. **Output Formatting**: Structure for MCP client

### Error Handling Patterns

- **Network Errors**: Retry with exponential backoff
- **API Errors**: Parse error messages from responses
- **Validation Errors**: Clear user feedback
- **Timeout Handling**: Configurable request timeouts

### Logging Patterns

- **Request Logging**: Sanitized parameter logging
- **Response Logging**: Status codes and timing
- **Error Logging**: Full context and stack traces
- **Performance Metrics**: Response time tracking

## Configuration Management

### Environment Variables

- **LOG_LEVEL**: Control logging verbosity
- **REQUEST_TIMEOUT**: API request timeout
- **MAX_RETRIES**: Retry attempts for failed requests
- **RATE_LIMIT**: Optional rate limiting configuration

### MCP Configuration

```json
{
  "mcpServers": {
    "4devs": {
      "command": "node|python",
      "args": ["build/index.js|server.py"],
      "env": {
        "LOG_LEVEL": "info"
      },
      "disabled": false,
      "autoApprove": []
    }
  }
}
```

## Testing Strategy

### Unit Testing

- **Tool Functions**: Test each MCP tool individually
- **API Client**: Mock 4Devs API responses
- **Validation**: Test input validation logic
- **Error Handling**: Test error scenarios

### Integration Testing

- **Live API**: Test against real 4Devs API
- **MCP Protocol**: Test MCP client integration
- **End-to-End**: Full workflow testing

### Test Data

- **Valid Inputs**: Known good parameters
- **Edge Cases**: Boundary conditions
- **Error Cases**: Invalid inputs and API failures
- **Geographic Data**: Various Brazilian states/cities

## Deployment Considerations

### Local Development

- **Hot Reload**: Development server with auto-restart
- **Debug Logging**: Verbose logging for development
- **Mock API**: Optional mock server for offline development

### Production Deployment

- **Process Management**: Systemd or PM2 for process control
- **Monitoring**: Health checks and metrics collection
- **Logging**: Structured logging for production analysis
- **Error Reporting**: Centralized error tracking
