# Full Discovery Process for Complex/New Features

## Purpose
Comprehensive research and analysis for new features or complex integrations requiring architectural decisions.

## Execution Steps

### 1. Feature Classification
- **Type**: New feature (greenfield)
- **Complexity**: High
- **Scope**: Requires significant architectural decisions

### 2. Technology Stack Research

**For each technology layer (Frontend, Backend, Communication, Storage)**:

1. **Identify Options**:
   - Research 2-3 viable technology options
   - Use WebSearch for latest recommendations and trends (2026)
   - Consider project constraints from requirements

2. **Evaluate Options**:
   - **Pros/Cons**: Benefits and drawbacks of each option
   - **Compatibility**: Works with other selected technologies
   - **Community Support**: Active maintenance, documentation quality
   - **Performance**: Meets non-functional requirements
   - **Learning Curve**: Team familiarity or ease of adoption

3. **Verify Latest Information**:
   - Use WebFetch to read official documentation
   - Check for breaking changes in recent versions
   - Verify API stability and deprecation notices
   - Review known issues and workarounds

4. **Document Decision**:
   - Selected technology with version
   - Rationale based on requirements
   - Record in research.md

### 3. External Dependency Analysis

**For each external API, library, or service**:

1. **API Contract Verification**:
   - Use WebFetch to access official API documentation
   - Document endpoints, request/response formats, authentication
   - Note rate limits, quotas, usage restrictions

2. **Version Compatibility**:
   - Verify compatible versions with other dependencies
   - Check for breaking changes between versions
   - Document migration paths if needed

3. **Integration Requirements**:
   - Authentication/authorization mechanisms
   - Error handling patterns
   - Retry and timeout strategies

### 4. Architecture Pattern Research

1. **Identify Candidate Patterns**:
   - Based on requirements (real-time, scalability, simplicity)
   - Common patterns: Client-Server, MVC, Event-Driven, Microservices

2. **Evaluate Patterns**:
   - **Fit**: How well pattern matches requirements
   - **Complexity**: Implementation complexity vs. benefit
   - **Scalability**: Supports performance requirements
   - **Maintainability**: Long-term maintenance considerations

3. **Select Pattern**:
   - Choose best-fit pattern
   - Document rationale in research.md

### 5. Performance & Security Research

1. **Performance Best Practices**:
   - WebSearch for optimization strategies for selected technologies
   - Benchmark data for similar systems
   - Caching strategies, connection pooling, load distribution

2. **Security Best Practices**:
   - Authentication/authorization patterns
   - Input validation and sanitization techniques
   - Secure communication (TLS, WSS)
   - OWASP recommendations for technology stack

### 6. Risk Identification

1. **Technical Risks**:
   - Technology maturity and stability
   - Breaking changes in dependencies
   - Performance bottlenecks
   - Security vulnerabilities

2. **Integration Risks**:
   - Dependency conflicts
   - API availability and reliability
   - Network and latency issues

3. **Mitigation Strategies**:
   - For each risk, document mitigation in design
   - Fallback mechanisms
   - Monitoring and alerting

### 7. Document Findings

Create or update `.kiro/specs/{feature}/research.md` with:
- All research topics and sources
- Technology decisions with rationale
- External dependencies verified
- Architecture pattern evaluation
- Risks and mitigation strategies
- Design boundaries for parallel development

## Completion Criteria

- [ ] All technology layers researched and selected
- [ ] External dependencies verified with documentation
- [ ] Architecture pattern chosen with rationale
- [ ] Performance and security approaches defined
- [ ] Risks identified with mitigation strategies
- [ ] research.md created with comprehensive findings

## Output

Findings from this discovery process must be:
1. Documented in `research.md`
2. Integrated into `design.md` generation
3. Referenced in component definitions and API contracts
