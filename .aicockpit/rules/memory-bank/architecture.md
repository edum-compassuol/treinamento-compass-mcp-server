# System Architecture - 4Devs MCP Server

## Overview

The 4Devs MCP Server is designed as a lightweight integration layer between the Model Context Protocol and the 4Devs Brazilian document generation API. The architecture follows MCP server patterns with a single API integration point.

## System Architecture

```txt
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   MCP Client    │    │  4Devs MCP      │    │   4Devs API     │
│   (AI Tools)    │◄──►│    Server       │◄──►│   Service       │
│                 │    │                 │    │                 │
└─────────────────┘    └─────────────────┘    └─────────────────┘
```

## Source Code Structure (Planned)

```txt
4devs-mcp-server/
├── src/
│   ├── index.ts/py          # Main server entry point
│   ├── server.ts/py         # MCP server implementation
│   ├── tools/               # Tool implementations
│   │   ├── person-generator.ts/py
│   │   ├── city-loader.ts/py
│   │   ├── certificate-generator.ts/py
│   │   ├── cnh-generator.ts/py
│   │   ├── pis-generator.ts/py
│   │   └── voter-generator.ts/py
│   ├── resources/           # MCP resources
│   │   └── brazilian-states.ts/py # UF list resource
│   ├── api/                 # API client
│   │   ├── client.ts/py     # 4Devs API client
│   │   └── types.ts/py      # Type definitions
│   ├── data/                # Static data
│   │   └── brazilian-ufs.json # Brazilian Federal Units list
│   └── utils/               # Utilities
│       ├── validation.ts/py # Input validation
│       └── formatting.ts/py # Response formatting
├── tests/                   # Test files
├── build/                   # Compiled output (TS only)
├── package.json/setup.py    # Dependencies
└── README.md               # Comprehensive documentation
```

## Key Technical Decisions

### API Integration Pattern

- **Single Endpoint**: All functionality accessed through `/ferramentas_online.php`
- **Action-Based**: Different tools determined by `acao` parameter
- **Form-Data**: All requests use POST with `multipart/form-data` encoding
- **No Authentication**: 4Devs API is publicly accessible
- **OpenAPI Specification**: Complete schema definitions available for all endpoints

### Tool Architecture

Each MCP tool follows this pattern:

1. **Input Validation**: JSON Schema validation based on OpenAPI specifications
2. **Parameter Mapping**: Convert MCP inputs to form-data parameters
3. **API Request**: POST request with multipart/form-data to 4Devs endpoint
4. **Response Processing**: Parse JSON/HTML responses according to OpenAPI schemas
5. **Error Handling**: Handle API errors gracefully with structured responses
6. **Logging**: Comprehensive logging following MCP development protocol

### MCP Development Protocol Compliance

- **Bootstrap**: Use `npx @modelcontextprotocol/create-server` for TypeScript or `pip install mcp` for Python
- **SDK Usage**: Implement using official MCP SDK
- **Comprehensive Logging**: Follow protocol logging patterns with `[Setup]`, `[API]`, `[Error]` prefixes
- **Testing Requirements**: Must test every tool individually before completion
- **Type Definitions**: Generate from OpenAPI schema for strong typing
- **MCP Inspector Testing**: Use official MCP Inspector for comprehensive server validation

### Data Flow

1. MCP client sends tool request
2. Server validates input parameters
3. Server formats request for 4Devs API
4. API call made with form-data payload
5. Response parsed and formatted
6. Structured data returned to MCP client

## Component Relationships

### Core Components

- **MCP Server**: Handles protocol communication
- **Tool Registry**: Manages available tools and resources
- **API Client**: Handles 4Devs API communication
- **Resource Provider**: Serves Brazilian UF data and other static resources
- **Validation Layer**: Ensures data integrity
- **Error Handler**: Manages failures gracefully

### Tool Dependencies

- **Person Generator** (`gerar_pessoa`): Standalone tool with complex schema validation
  - Required: `acao`, `sexo`, `txt_qtde`
  - Optional: `pontuacao`, `idade`, `cep_estado`, `cep_cidade`
  - Dependencies: City Loader for specific city targeting
- **City Loader** (`carregar_cidades`): Geographic data provider
  - Required: `acao`, `cep_estado`
  - Returns: HTML string with city options
  - Used by: Person Generator for location-specific data
- **Certificate Generator** (`gerador_certidao`): Document number generator
  - Required: `acao`
  - Optional: `pontuacao`, `tipo_certidao`
  - Returns: Plain text certificate number
- **CNH Generator** (`gerar_cnh`): Driver's license number generator
  - Required: `acao` only
  - Returns: Plain text CNH number
- **PIS Generator** (`gerar_pis`): Social security number generator
  - Required: `acao`
  - Optional: `pontuacao`
  - Returns: Plain text PIS number
- **Voter Generator** (`gerar_titulo_eleitor`): Voter registration generator
  - Required: `acao`
  - Optional: `estado` (UF code)
  - Returns: Plain text voter registration number

### Resource Dependencies

- **Brazilian UF Resource**: Static list of Federal Units (states) in two-character format
- **City Data**: Dynamic data loaded via API based on UF selection
- **Documentation Resource**: Comprehensive README.md with usage examples and API details

## Critical Implementation Paths

### Geographic Data Workflow

1. User requests person data for specific city/state
2. If UF needed, provide Brazilian UF resource for selection
3. If city code unknown, call City Loader tool with UF parameter
4. Parse HTML response to extract city codes and names
5. Use city code in Person Generator request
6. Return structured person data with geographic information

### Brazilian UF Resource Workflow

1. MCP client requests Brazilian states list
2. Server provides static UF resource with all 27 Federal Units
3. Client uses UF codes for geographic-specific data generation
4. UF codes used in City Loader and Voter Generator tools

### Error Handling Strategy

- **API Failures**: Retry with exponential backoff
- **Invalid Responses**: Parse and provide meaningful errors based on OpenAPI schemas
- **Network Issues**: Timeout handling and user feedback
- **Rate Limiting**: Implement if needed based on API behavior
- **Schema Validation**: Validate all inputs against OpenAPI specifications
- **Response Validation**: Ensure responses match expected schemas

### Logging Strategy (MCP Protocol Compliant)

- **Setup Logging**: `[Setup] Initializing server...`
- **API Request Logging**: `[API] Request to endpoint: /ferramentas_online.php`
- **Error Logging**: `[Error] Failed with: {error details}`
- **Performance Logging**: Track API response times
- **Parameter Logging**: Sanitized form-data parameters
- **Response Logging**: Status codes and response types (JSON/HTML)

## Design Patterns

### Factory Pattern

- Tool creation based on action type
- Consistent interface across all tools

### Strategy Pattern

- Different validation strategies per tool
- Flexible response formatting

### Adapter Pattern

- Convert 4Devs API responses to MCP format
- Handle different response types (JSON, HTML)

## Scalability Considerations

### Performance

- Connection pooling for HTTP requests
- Response caching where appropriate
- Efficient JSON parsing

### Reliability

- Circuit breaker pattern for API failures
- Health check endpoints
- Graceful degradation

### Monitoring

- Metrics collection for tool usage
- API response time tracking
- Error rate monitoring
- Resource access patterns (UF usage statistics)

## MCP Resources Architecture

### Brazilian UF Resource

- **Type**: Static JSON resource
- **URI Pattern**: `uf://brazilian-states`
- **Content**: Complete list of Brazilian Federal Units (27 total)
- **Format**: Array of objects with UF code and full state name
- **Schema**: Based on OpenAPI enum values for `cep_estado` and `estado` parameters
- **Usage**: Referenced by City Loader and Voter Generator tools
- **Data**: AC, AL, AP, AM, BA, CE, DF, ES, GO, MA, MS, MT, MG, PA, PB, PR, PE, PI, RJ, RN, RS, RO, RR, SC, SP, SE, TO

### Documentation Resource

- **Type**: Markdown documentation
- **Location**: README.md file
- **Content**: Complete application information, usage examples, API details
- **OpenAPI Integration**: Include schema documentation and examples
- **Maintenance**: Updated with each feature addition or API change

## OpenAPI Schema Integration

### Request Schema Validation

- **Person Generator**: Complex schema with 6 parameters, batch limits (1-30)
- **City Loader**: Simple schema with UF validation
- **Certificate Generator**: Optional parameters with enum validation
- **CNH Generator**: Minimal schema (action only)
- **PIS Generator**: Optional punctuation parameter
- **Voter Generator**: Optional state parameter with UF validation

### Response Schema Handling

- **JSON Responses**: Person generator returns array of person objects
- **HTML Responses**: City loader returns HTML option tags
- **Text Responses**: Document generators return plain text strings
- **Schema Validation**: Ensure all responses match OpenAPI specifications

### Type Generation

- **TypeScript**: Generate interfaces from OpenAPI schemas
- **Python**: Generate Pydantic models from OpenAPI schemas
- **Validation**: Runtime validation using generated types
- **Documentation**: Auto-generated API documentation from schemas

## MCP Inspector Integration

### Official Testing Tool

The 4Devs MCP Server must be tested using the official MCP Inspector from `https://github.com/modelcontextprotocol/inspector`

### Testing Architecture

- **Interactive UI Mode**: Visual testing interface for development and debugging
- **CLI Mode**: Programmatic testing for automation and CI/CD integration
- **Transport Support**: Full support for stdio, SSE, and streamable HTTP transports
- **Authentication**: Built-in session token authentication for security
- **Configuration Export**: Generate `mcp.json` files for client integration

### Inspector Integration Workflow

1. **Development Testing**: Use UI mode for interactive tool and resource testing
2. **Automated Testing**: Use CLI mode for continuous integration validation
3. **Configuration Generation**: Export server configurations for client deployment
4. **Schema Validation**: Automatic validation against MCP protocol specifications
5. **Error Analysis**: Visual error reporting and request history tracking

### Testing Commands for 4Devs Server

```bash
# Basic server testing
npx @modelcontextprotocol/inspector node build/index.js

# Test all 6 tools individually
npx @modelcontextprotocol/inspector --cli node build/index.js --method tools/call --tool-name gerar_pessoa --tool-arg sexo=I --tool-arg txt_qtde=1
npx @modelcontextprotocol/inspector --cli node build/index.js --method tools/call --tool-name carregar_cidades --tool-arg cep_estado=SC
npx @modelcontextprotocol/inspector --cli node build/index.js --method tools/call --tool-name gerador_certidao --tool-arg tipo_certidao=nascimento
npx @modelcontextprotocol/inspector --cli node build/index.js --method tools/call --tool-name gerar_cnh
npx @modelcontextprotocol/inspector --cli node build/index.js --method tools/call --tool-name gerar_pis --tool-arg pontuacao=S
npx @modelcontextprotocol/inspector --cli node build/index.js --method tools/call --tool-name gerar_titulo_eleitor --tool-arg estado=SP

# Test Brazilian UF resource
npx @modelcontextprotocol/inspector --cli node build/index.js --method resources/list
```

### Inspector Benefits for 4Devs Server

- **OpenAPI Compliance**: Validates all tool schemas against OpenAPI specifications
- **Brazilian UF Resource Testing**: Verifies UF resource availability and format
- **Geographic Data Workflow**: Tests city loading and person generation integration
- **Error Handling Validation**: Ensures proper error responses for invalid inputs
- **Performance Monitoring**: Tracks API response times and resource usage
