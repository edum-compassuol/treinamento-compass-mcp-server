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
│   ├── api/                 # API client
│   │   ├── client.ts/py     # 4Devs API client
│   │   └── types.ts/py      # Type definitions
│   └── utils/               # Utilities
│       ├── validation.ts/py # Input validation
│       └── formatting.ts/py # Response formatting
├── tests/                   # Test files
├── build/                   # Compiled output (TS only)
├── package.json/setup.py    # Dependencies
└── README.md               # Documentation
```

## Key Technical Decisions

### API Integration Pattern

- **Single Endpoint**: All functionality accessed through one 4Devs endpoint
- **Action-Based**: Different tools determined by `acao` parameter
- **Form-Data**: All requests use POST with form-data encoding
- **No Authentication**: 4Devs API appears to be publicly accessible

### Tool Architecture

Each MCP tool follows this pattern:

1. **Input Validation**: Validate and sanitize user inputs
2. **API Request**: Format and send request to 4Devs API
3. **Response Processing**: Parse and format API response
4. **Error Handling**: Handle API errors gracefully
5. **Logging**: Comprehensive logging for debugging

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
- **Tool Registry**: Manages available tools
- **API Client**: Handles 4Devs API communication
- **Validation Layer**: Ensures data integrity
- **Error Handler**: Manages failures gracefully

### Tool Dependencies

- **Person Generator**: Standalone tool
- **City Loader**: Used by Person Generator for location data
- **Certificate Generator**: Standalone tool
- **CNH Generator**: Standalone tool
- **PIS Generator**: Standalone tool
- **Voter Generator**: Standalone tool

## Critical Implementation Paths

### Geographic Data Workflow

1. User requests person data for specific city
2. If city code unknown, call City Loader tool first
3. Extract city code from HTML response
4. Use city code in Person Generator request

### Error Handling Strategy

- **API Failures**: Retry with exponential backoff
- **Invalid Responses**: Parse and provide meaningful errors
- **Network Issues**: Timeout handling and user feedback
- **Rate Limiting**: Implement if needed based on API behavior

### Logging Strategy

- **Request Logging**: Log all API requests with parameters
- **Response Logging**: Log API responses (sanitized)
- **Error Logging**: Detailed error context and stack traces
- **Performance Logging**: Track API response times

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
