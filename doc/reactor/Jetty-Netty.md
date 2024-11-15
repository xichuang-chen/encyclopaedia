# Netty vs Jetty

## 1. 框架类型
### Netty：
   - **类型**：异步事件驱动的网络应用框架。  
   - **用途**：用于构建高性能、可扩展的网络应用程序。适合需要自定义协议和高并发处理的场景，如游戏服务器、聊天应用、RPC 框架等。 
### Jetty：
   - **类型**：轻量级的 Java Servlet 容器和 HTTP 服务器。  
   - **用途**：主要用于嵌入式的 Web 服务器，支持 Servlet、WebSocket 和其他标准 Java EE 组件。适合构建 Web 应用和 Restful 服务。
## 2. 设计理念
### Netty：
   - 采用异步非阻塞 I/O 模型，强调性能和可扩展性。通过事件驱动的架构，能够处理大量并发连接。  
   - 提供了丰富的 API 来支持各种协议（如 HTTP、WebSocket、TCP、UDP），开发者可以根据需要实现自定义协议。
### Jetty：
   - 设计为一个完整的 Web 服务器和 Servlet 容器，提供了对标准 Java EE 规范的支持。  
   - 支持嵌入式部署，允许开发者将 Jetty 嵌入到自己的应用程序中，而无需外部服务器。
## 3. 功能和特性
### Netty：
   - 高度灵活，支持多种协议和编码解码机制。
   - 提供了丰富的扩展性，适合构建复杂的网络应用。
   - 支持大量的并发连接，适合高性能、高吞吐量的场景。
### Jetty：
   - 支持 Servlet、JSP 和 WebSocket，符合 Java EE 标准。
   - 方便的嵌入式使用，适合微服务架构和小型应用。
   - 具有简单的配置和管理界面，易于集成。
## 4. 使用场景
### Netty：
   - 适合需要高性能、低延迟的应用，如在线游戏、实时通信应用、分布式系统等。
### Jetty：
   - 适合构建 Web 应用、RESTful API 和需要 Servlet 支持的应用，特别是在需要嵌入式 Web 服务器的情况下。
### 总结
   - 如果你需要构建高性能的网络应用，使用 Netty。
   - 如果你在寻找一个轻量级的 Web 服务器或 Servlet 容器，使用 Jetty。
   - 根据具体需求选择合适的框架将帮助你更有效地实现项目目标。


# Servlet 容器 和 Tomcat 容器
Netty：不使用传统的 Servlet 容器，专注于异步网络编程，适合高性能和低延迟需求的应用。  
Jetty：使用 Servlet 容器，提供标准的 Web 开发支持，适合构建基于 HTTP 的 Web 应用。  

**_Servlet 容器_**：
- Servlet 容器是一个用于运行 Java Servlet 的环境。它负责处理 HTTP 请求、管理 Servlet 的生命周期，并将请求和响应传递给 Servlet。  
- Servlet 容器遵循 Java EE（或Jakarta EE）规范，提供对 Servlet 和 JSP 的支持。
**_Tomcat 容器_**：
Tomcat 是一个具体实现的 Servlet 容器和 Web 服务器。它是 Apache 软件基金会开发的开源项目，广泛用于运行 Java Web 应用。  
Tomcat 既是一个 Servlet 容器，也是一个 JSP 容器，提供了处理 Java EE Web 应用所需的基本功能。
## 总结
  
**Servlet 容器**：是一个广泛的概念，指任何支持 Servlet 规范的环境。
**Tomcat 容器**：是一个具体的 Servlet 容器实现，提供全面的 Web 应用支持。

# SpringMVC 和 SpringWebflux
**_SpringWebflux 和 SpringMVC 分别使用什么容器_**  
### Spring MVC
**Servlet 规范**：Spring MVC 主要依赖于传统的 Servlet 容器，如 Apache Tomcat、Jetty 或者任何兼容 Servlet 的容器。它是基于阻塞 I/O 的模型，适合于大多数传统的 Web 应用。
### Spring WebFlux
反应式容器：Spring WebFlux 可以运行在多种环境中，包括：  
- Netty：这是 Spring WebFlux 默认的反应式服务器，能够处理大量并发请求。
- Servlet 3.1+ 容器：例如 Tomcat、Jetty 等，这些容器也支持反应式编程，但在性能和效率上不如 Netty。


