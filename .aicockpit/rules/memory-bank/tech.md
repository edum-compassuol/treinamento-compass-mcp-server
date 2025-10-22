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

### TypeScript Path (Web Services/JavaScript Integration)

```bash
# Bootstrap using MCP development protocol
npx @modelcontextprotocol/create-server 4devs-mcp-server
cd 4devs-mcp-server
npm install

# Additional dependencies for 4Devs API integration
npm install axios form-data
npm install -D @types/node typescript ts-node

# OpenAPI type generation
npm install -D openapi-typescript
```

### Python Path (Data Science/ML Workflows)

```bash
# Setup using MCP development protocol
pip install mcp
# Or with uv (recommended)
uv add "mcp[cli]"

# Additional dependencies for 4Devs API integration
pip install httpx python-multipart

# OpenAPI schema validation
pip install pydantic[email] openapi-schema-validator
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

- **Single Endpoint**: All functionality through `/ferramentas_online.php`
- **Form Data Only**: No JSON request bodies, only `multipart/form-data`
- **Batch Limits**: Maximum 30 records per request (Person Generator)
- **Geographic Dependencies**: Cities must be loaded by state first
- **Response Formats**: Mixed JSON and HTML responses based on OpenAPI spec

### MCP Protocol Requirements

- **Tool Definitions**: Each endpoint becomes an MCP tool with OpenAPI-based schemas
- **Resource Definitions**: Brazilian UF resource and documentation resource
- **Input Schemas**: JSON Schema validation based on OpenAPI specifications
- **Error Handling**: Structured error responses following MCP patterns
- **Logging**: MCP development protocol compliant logging with prefixes
- **Testing Requirements**: Every tool must be tested individually before completion

### OpenAPI Schema Constraints

- **Person Generator**: 6 parameters with complex validation rules
- **UF Validation**: All UF parameters must match enum: [AC, AL, AP, AM, BA, CE, DF, ES, GO, MA, MS, MT, MG, PA, PB, PR, PE, PI, RJ, RN, RS, RO, RR, SC, SP, SE, TO]
- **Batch Limits**: txt_qtde parameter limited to 1-30 range
- **Certificate Types**: tipo_certidao enum: [nascimento, casamento, casamento_religioso, obito, Indiferente]
- **Response Types**: JSON arrays for Person Generator, HTML strings for City Loader, plain text for others

### Performance Considerations

- **Network Latency**: External API calls add latency
- **Rate Limiting**: Unknown limits - need graceful handling
- **Memory Usage**: Batch operations may use significant memory
- **Error Recovery**: Robust retry mechanisms needed

## Tool Usage Patterns

### Request Flow (OpenAPI Compliant)

1. **Input Validation**: JSON Schema validation based on OpenAPI specifications
2. **Parameter Mapping**: Convert MCP inputs to multipart/form-data parameters
3. **API Request**: POST to `/ferramentas_online.php` with proper form encoding
4. **Response Processing**: Parse JSON/HTML responses according to OpenAPI schemas
5. **Output Formatting**: Structure data for MCP client with type safety

### Error Handling Patterns

- **Network Errors**: Retry with exponential backoff
- **API Errors**: Parse error messages from responses based on OpenAPI error schemas
- **Validation Errors**: Clear user feedback with OpenAPI constraint details
- **Timeout Handling**: Configurable request timeouts
- **Schema Validation Errors**: Detailed validation failure messages

### Logging Patterns (MCP Development Protocol)

- **Setup Logging**: `[Setup] Initializing server...`
- **API Request Logging**: `[API] Request to endpoint: /ferramentas_online.php`
- **Error Logging**: `[Error] Failed with: {error details}`
- **Parameter Logging**: Sanitized form-data parameters (exclude sensitive data)
- **Response Logging**: Status codes, response types (JSON/HTML), and timing
- **Performance Metrics**: Response time tracking and API call statistics

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

## Resource Management

### Brazilian UF Resource

- **Data Source**: Static JSON file with all 27 Brazilian Federal Units
- **Format**: Array of objects with UF code and full state name
- **URI Pattern**: `uf://brazilian-states`
- **Usage**: Referenced by City Loader and Voter Generator tools
- **Maintenance**: Static data, updated only if Brazilian states change

### Documentation Resource

- **Source**: README.md file
- **Content**: Complete application information, API details, usage examples
- **Format**: Markdown with comprehensive sections
- **Updates**: Maintained with each feature addition or API change

## API Response Handling

### Person Generator Response

- **Format**: JSON array of person objects
- **Fields**: Complete demographic and document data
- **Processing**: Direct JSON parsing, no transformation needed

### City Loader Response

- **Format**: HTML string with `<option>` tags
- **Processing**: Parse HTML to extract city codes and names
- **Transformation**: Convert to structured JSON for MCP client

### Document Generator Responses

- **Certificate/CNH/PIS/Voter**: Plain text strings
- **Processing**: Direct string return with optional formatting
- **Validation**: Ensure proper Brazilian document format

## Testing Strategy (MCP Development Protocol Compliant)

### Critical Testing Requirements

⛔️ **BLOCKER**: Must test EVERY tool individually before completion

- Test each tool with valid inputs
- Verify output format matches OpenAPI specifications
- Confirm success from user for each test
- Document test results

### Unit Testing

- **Tool Functions**: Test each MCP tool individually with OpenAPI-compliant inputs
- **API Client**: Mock 4Devs API responses based on OpenAPI examples
- **Validation**: Test input validation logic against OpenAPI schemas
- **Error Handling**: Test error scenarios with proper MCP error responses
- **Schema Validation**: Test OpenAPI schema compliance for all inputs/outputs

### Integration Testing

- **Live API**: Test against real 4Devs API with all 6 endpoints
- **MCP Protocol**: Test MCP client integration with proper tool/resource definitions
- **End-to-End**: Full workflow testing including geographic data dependencies
- **OpenAPI Compliance**: Verify all responses match OpenAPI specifications

### Test Data (OpenAPI Based)

- **Valid Inputs**: Known good parameters for all 6 API endpoints based on OpenAPI examples
- **Edge Cases**: Boundary conditions (txt_qtde: 1-30, all UF codes, certificate types)
- **Error Cases**: Invalid inputs and API failures with proper error handling
- **Geographic Data**: All 27 Brazilian states from OpenAPI UF enum
- **Resource Testing**: Brazilian UF resource availability and OpenAPI schema format
- **Documentation Testing**: README.md completeness with OpenAPI integration
- **Schema Validation**: Test all OpenAPI schema constraints and validations

### Testing Workflow (MCP Protocol)

1. **Individual Tool Testing**: Test each of the 6 tools separately
2. **Schema Validation Testing**: Verify OpenAPI compliance
3. **Resource Testing**: Test Brazilian UF resource and documentation
4. **Integration Testing**: Test tool interactions and dependencies
5. **Error Scenario Testing**: Test all error conditions
6. **Performance Testing**: Verify response times and resource usage
7. **MCP Inspector Testing**: Use official MCP Inspector for comprehensive server validation

### MCP Inspector Testing

**Official Testing Tool**: Use the MCP Inspector from `https://github.com/modelcontextprotocol/inspector`

**Setup and Usage**:

```bash
# Test the MCP server using the Inspector
npx @modelcontextprotocol/inspector node build/index.js

# With environment variables
npx @modelcontextprotocol/inspector -e LOG_LEVEL=debug node build/index.js

# With configuration file
npx @modelcontextprotocol/inspector --config mcp.json --server 4devs-server
```

**Inspector Features for Testing**:

- **Interactive UI**: Visual testing interface at `http://localhost:6274`
- **Tool Testing**: Form-based parameter input with real-time response visualization
- **Resource Exploration**: Interactive browser with hierarchical navigation
- **Schema Validation**: Automatic validation against MCP protocol specifications
- **Error Visualization**: Request history and visualized error responses
- **CLI Mode**: Programmatic testing for automation and CI/CD integration

**CLI Testing Examples**:

```bash
# List all available tools
npx @modelcontextprotocol/inspector --cli node build/index.js --method tools/list

# Test Person Generator tool
npx @modelcontextprotocol/inspector --cli node build/index.js --method tools/call --tool-name gerar_pessoa --tool-arg sexo=I --tool-arg txt_qtde=1

# Test City Loader tool
npx @modelcontextprotocol/inspector --cli node build/index.js --method tools/call --tool-name carregar_cidades --tool-arg cep_estado=SC

# List available resources (Brazilian UF resource)
npx @modelcontextprotocol/inspector --cli node build/index.js --method resources/list

# Test all document generators
npx @modelcontextprotocol/inspector --cli node build/index.js --method tools/call --tool-name gerar_cnh
npx @modelcontextprotocol/inspector --cli node build/index.js --method tools/call --tool-name gerar_pis --tool-arg pontuacao=S
npx @modelcontextprotocol/inspector --cli node build/index.js --method tools/call --tool-name gerador_certidao --tool-arg tipo_certidao=nascimento
npx @modelcontextprotocol/inspector --cli node build/index.js --method tools/call --tool-name gerar_titulo_eleitor --tool-arg estado=SP
```

**Inspector Configuration for 4Devs Server**:

- **Authentication**: Inspector includes session token authentication by default
- **Transport Support**: Supports stdio, SSE, and streamable HTTP transports
- **Export Configuration**: Generate `mcp.json` configuration files for client integration
- **Real-time Validation**: Immediate feedback on tool calls and resource access

## Deployment Considerations

### Local Development

- **Hot Reload**: Development server with auto-restart
- **Debug Logging**: Verbose logging for development
- **Mock API**: Optional mock server for offline development
- **Resource Loading**: Local Brazilian UF data file
- **Documentation**: Local README.md serving

### Production Deployment

- **Process Management**: Systemd or PM2 for process control
- **Monitoring**: Health checks and metrics collection
- **Logging**: Structured logging for production analysis
- **Error Reporting**: Centralized error tracking
- **Resource Optimization**: Efficient UF data loading
- **Documentation Serving**: README.md accessibility for clients

## Brazilian Federal Units (UF) Support

### Complete UF List

The system supports all 27 Brazilian Federal Units:

- **States (26)**: AC, AL, AP, AM, BA, CE, DF, ES, GO, MA, MT, MS, MG, PA, PB, PR, PE, PI, RJ, RN, RS, RO, RR, SC, SP, SE, TO
- **Federal District (1)**: DF

### UF Usage Patterns

- **Person Generator**: Optional UF for geographic targeting
- **City Loader**: Required UF parameter for city listing
- **Voter Generator**: Optional UF for state-specific voter registration
- **Resource Access**: Direct UF list access via MCP resource

### Geographic Data Integration

- **Two-Step Process**: UF selection → City selection → Data generation
- **Validation**: Ensure UF codes match Brazilian standards
- **Fallback**: Random selection when UF not specified
