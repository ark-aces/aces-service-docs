
# Implementations

The ACES Service API defines a standard protocol to facilitate creation and execution
of numerous blockchain interoperability services. As such, it is a
high level abstraction of many different service contract types that
needs to be implemented and hosted independently. Unlike many conventional business APIs
that host their own proprietary services on a single server, ACES Services are
designed to be distributed across many servers, where third-party service providers are 
able to develop, host, monetize, and maintain individual services.

The Service API specifies only the base necessary capabilities for implementing a
blockchain interoperability and contracts in a generic way that can support many
different use-cases for interoperability services, including those that may utilize 
multiple different blockchains or interface with physical hardware.

All implementations conforming to the ACES Service API specification can be 
consumed using the common client libraries and processing logic, 
allowing different services to easily plugged into applications or listed on the
Service Marketplace.
