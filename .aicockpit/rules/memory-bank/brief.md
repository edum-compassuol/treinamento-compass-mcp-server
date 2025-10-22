# 4Devs MCP Server Project

## Overview

The 4Devs MCP Server is a Model Context Protocol (MCP) server designed to integrate with the 4Devs API, a Brazilian web service that generates valid Brazilian documents and personal data for testing and development purposes.

## Main Objectives

- Provide seamless access to 4Devs API functionality through MCP protocol
- Enable generation of realistic Brazilian personal data including names, documents, and addresses
- Support multiple document types: CPF, RG, CNH, PIS, birth certificates, and voter registration
- Facilitate testing and development of Brazilian applications requiring valid document formats

## Key Features

### Document Generation Tools

- **Personal Data Generator**: Creates complete person profiles with name, age, documents, address, and contact information
- **Certificate Generator**: Generates various types of Brazilian certificates (birth, marriage, death)
- **CNH Generator**: Creates valid Brazilian driver's license numbers
- **PIS Generator**: Generates valid PIS (social security) numbers
- **Voter Registration Generator**: Creates valid voter registration numbers

### Geographic Data Tools

- **City Lookup**: Retrieves city lists by Brazilian state (UF)
- **Address Generation**: Creates realistic Brazilian addresses with proper formatting

### Customization Options

- Gender selection (male, female, random)
- Age specification or random generation
- Document formatting (with or without punctuation)
- Geographic targeting by state and city
- Batch generation (1-30 records)

## Technologies Used

- **Protocol**: Model Context Protocol (MCP)
- **API Integration**: 4Devs REST API (`https://www.4devs.com.br/ferramentas_online.php`)
- **Data Format**: JSON responses with form-data requests
- **Geographic Scope**: Brazilian states and municipalities

## Technical Architecture

The server interfaces with a single 4Devs API endpoint using POST requests with form-data payloads. Different functionalities are accessed through the `acao` parameter, supporting:

- `gerar_pessoa` - Generate person data
- `carregar_cidades` - Load cities by state
- `gerador_certidao` - Generate certificates
- `gerar_cnh` - Generate CNH numbers
- `gerar_pis` - Generate PIS numbers
- `gerar_titulo_eleitor` - Generate voter registration

## Project Significance

This MCP server addresses the critical need for realistic Brazilian test data in software development. It provides developers with:

- **Compliance Testing**: Valid document formats for Brazilian applications
- **Development Efficiency**: Quick access to realistic test data
- **Integration Simplicity**: MCP protocol compatibility with modern AI tools
- **Localization Support**: Proper Brazilian document and address formatting

The project bridges the gap between the 4Devs web service and modern development workflows, making Brazilian document generation accessible through standardized MCP tooling.

## Current Status

Project is in initial planning phase with comprehensive API documentation completed. Implementation of the MCP server and tool definitions is pending.
