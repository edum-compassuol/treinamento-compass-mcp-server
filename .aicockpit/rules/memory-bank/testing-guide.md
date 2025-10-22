# MCP Server Testing Guide

This guide provides a comprehensive testing roadmap for the MCP Server created in AI Cockpit Reasoning. You will test both packages: the npm package and the Docker image.

## Testing Roadmap

### 1. Initial Setup and Understanding

1. **Load and read the README.md** of the created MCP Server to understand its functionalities and how to use it.
2. **Read the MCP server configuration file** existing in AI Cockpit Reasoning to understand how to configure the created MCP Server.
3. **Use best practices** for user terminal and operating system commands.

### 2. Testing the MCP Server using npm package

1. **Configure the MCP Server** from the npm package in the MCP servers configuration file according to the README.md instructions.
2. **Use the MCP Server functionalities** as described in the README.md, performing some test requests.
3. **At the end, remove the MCP Server configuration** from the MCP servers configuration file.

### 3. Testing the MCP Server using Docker image

1. **Configure the MCP Server** from the Docker image in the MCP servers configuration file according to the README.md instructions.
2. **Use the MCP Server functionalities** as described in the README.md, performing some test requests.
3. **At the end, remove the MCP Server configuration** from the MCP servers configuration file.

## Detailed Testing Steps

### Pre-Testing Requirements

- Ensure AI Cockpit Reasoning is properly configured
- Have access to the MCP servers configuration file
- Verify npm and Docker are installed and accessible
- Review the project's README.md for specific setup instructions

### npm Package Testing

#### Configuration Phase

1. Open the MCP servers configuration file in AI Cockpit Reasoning
2. Add the npm package configuration as specified in README.md
3. Verify the configuration syntax is correct
4. Save and reload the configuration

#### Testing Phase

1. Test basic server connectivity
2. Execute sample requests for each available tool
3. Verify response formats match expected schemas
4. Test error handling with invalid inputs
5. Monitor server logs for any issues

#### Cleanup Phase

1. Remove the npm package configuration from the MCP servers file
2. Verify the server is no longer accessible
3. Clean up any temporary files or logs

### Docker Image Testing

#### Configuration Phase

1. Pull the Docker image (if not already available)
2. Configure the Docker-based MCP server in the configuration file
3. Verify Docker container can start properly
4. Check network connectivity and port mappings

#### Testing Phase

1. Test basic server connectivity through Docker
2. Execute the same sample requests used in npm testing
3. Compare responses with npm package results
4. Test container restart and persistence
5. Monitor Docker logs and resource usage

#### Cleanup Phase

1. Remove the Docker configuration from the MCP servers file
2. Stop and remove the Docker container
3. Clean up Docker images if needed
4. Verify no residual containers or networks remain

## Test Validation Criteria

### Functional Tests

- All documented tools respond correctly
- Response formats match OpenAPI specifications
- Error handling works as expected
- Resource access functions properly

### Performance Tests

- Response times are acceptable
- Memory usage is within reasonable limits
- No memory leaks during extended testing
- Concurrent requests are handled properly

### Integration Tests

- MCP protocol compliance
- Proper integration with AI Cockpit Reasoning
- Configuration changes take effect correctly
- Server lifecycle management works properly

## Troubleshooting Common Issues

### npm Package Issues

- Verify Node.js version compatibility
- Check npm package installation
- Review environment variables
- Validate file permissions

### Docker Image Issues

- Verify Docker daemon is running
- Check port availability
- Review container logs
- Validate volume mounts and networking

### Configuration Issues

- Validate JSON syntax in configuration files
- Check file paths and permissions
- Verify environment variable substitution
- Review server startup logs

## Documentation Requirements

After completing the tests, document:

- Test results for both deployment methods
- Any issues encountered and their resolutions
- Performance observations
- Recommendations for production deployment
- Comparison between npm and Docker deployment methods

This testing guide ensures comprehensive validation of the MCP Server functionality across different deployment scenarios while maintaining proper cleanup and documentation practices.
