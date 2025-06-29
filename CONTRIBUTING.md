🤝 Contributing to Enterprise MCP Ecosystem
Thank you for your interest in contributing to our Enterprise MCP Ecosystem! This document provides guidelines and information for contributors.
🌟 Welcome Contributors
We welcome contributions from developers, marketers, business analysts, and anyone interested in improving enterprise automation through MCP servers.
📋 Table of Contents
Code of Conduct
Getting Started
Development Setup
Contribution Types
Development Workflow
Code Standards
Testing Requirements
Documentation Guidelines
Review Process
📜 Code of Conduct
Our Commitment
We are committed to providing a welcoming and inclusive environment for all contributors, regardless of background, experience level, or identity.
Expected Behavior
Respect: Treat all community members with respect and kindness
Collaboration: Work together constructively and share knowledge
Professionalism: Maintain professional communication in all interactions
Inclusivity: Welcome newcomers and help them succeed
Unacceptable Behavior
Harassment, discrimination, or exclusionary behavior
Spam, self-promotion, or off-topic discussions
Sharing private information without permission
Any behavior that disrupts the collaborative environment
🚀 Getting Started
Prerequisites
Before contributing, ensure you have:
Node.js 18+ installed
TypeScript knowledge (intermediate level)
Understanding of Model Context Protocol concepts
Git and GitHub familiarity
First Contribution Suggestions
Perfect for newcomers:
Fix documentation typos or improve clarity
Add code comments and improve JSDoc
Write additional unit tests
Create new example use cases
Improve error messages and user feedback
⚙️ Development Setup
1. Fork and Clone
bash
# Fork the repository on GitHub
# Then clone your fork
git clone https://github.com/[your-username]/enterprise-mcp-ecosystem.git
cd enterprise-mcp-ecosystem

# Add upstream remote
git remote add upstream https://github.com/[original-repo]/enterprise-mcp-ecosystem.git
2. Environment Setup
bash
# Install dependencies
npm install

# Install dependencies for all servers
npm run install-all

# Copy environment template
cp .env.template .env
# Edit .env with your API keys for testing
3. Build and Test
bash
# Build all servers
npm run build-all

# Run tests
npm run test-all

# Start development environment
npm run dev
4. Verify Setup
bash
# Health check all servers
npm run health-check

# Validate configuration
npm run validate-config
🎯 Contribution Types
🐛 Bug Fixes
Fix existing functionality issues
Improve error handling
Resolve performance problems
Address security vulnerabilities
✨ New Features
Add new MCP server capabilities
Create additional tool functions
Integrate new external services
Enhance existing server functionality
📚 Documentation
Improve README and guides
Add code documentation
Create tutorial content
Update API references
🧪 Testing
Add unit tests for servers
Create integration test scenarios
Improve test coverage
Add performance benchmarks
🎨 User Experience
Improve error messages
Enhance tool descriptions
Optimize response formats
Streamline configuration
🔄 Development Workflow
1. Issue First
Before starting work:
Check existing issues for duplicates
Create an issue describing your planned contribution
Wait for maintainer feedback and approval
Get assigned to the issue before starting work
2. Branch Strategy
bash
# Create feature branch from main
git checkout main
git pull upstream main
git checkout -b feature/your-feature-name

# For bug fixes
git checkout -b bugfix/issue-description

# For documentation
git checkout -b docs/improvement-description
3. Development Process
bash
# Make your changes
# Test thoroughly
npm run test-all

# Check code quality
npm run lint
npm run format

# Build to ensure no errors
npm run build-all
4. Commit Standards
Follow conventional commits:
bash
# Feature additions
git commit -m "feat(server-name): add new functionality"

# Bug fixes
git commit -m "fix(server-name): resolve issue description"

# Documentation
git commit -m "docs: improve README clarity"

# Tests
git commit -m "test(server-name): add unit tests for feature"
5. Pull Request Process
bash
# Push your branch
git push origin feature/your-feature-name

# Create Pull Request on GitHub with:
# - Clear title and description
# - Reference to related issue
# - Screenshots/examples if applicable
# - Checklist completion
📏 Code Standards
TypeScript Standards
typescript
// Use strict typing
interface ToolResult {
  success: boolean;
  data?: any;
  error?: string;
}

// Prefer async/await over Promises
async function performAction(): Promise<ToolResult> {
  try {
    const result = await externalService.call();
    return { success: true, data: result };
  } catch (error) {
    return { success: false, error: error.message };
  }
}

// Use descriptive variable names
const userContactInformation = await hubspot.getContact(contactId);
const marketingCampaignMetrics = await analytics.getCampaignData();
File Organization
src/
├── index.ts          # Main server entry point
├── tools/            # Tool implementations
│   ├── contacts.ts   # Contact management tools
│   ├── campaigns.ts  # Campaign tools
│   └── analytics.ts  # Analytics tools
├── services/         # External service clients
├── types/            # Type definitions
├── utils/            # Utility functions
└── constants/        # Configuration constants
Error Handling
typescript
// Consistent error handling pattern
export async function createContact(data: ContactData): Promise<ToolResult> {
  try {
    // Validate input
    if (!data.email) {
      return {
        success: false,
        error: "Email is required for contact creation"
      };
    }

    // Perform operation
    const contact = await hubspotClient.contacts.create(data);
    
    return {
      success: true,
      data: {
        id: contact.id,
        email: contact.properties.email,
        createdAt: contact.createdAt
      }
    };
  } catch (error) {
    // Log error for debugging
    console.error('Contact creation failed:', error);
    
    return {
      success: false,
      error: `Failed to create contact: ${error.message}`
    };
  }
}
🧪 Testing Requirements
Unit Tests
Every new function must include unit tests:
typescript
// Example test structure
describe('Contact Management', () => {
  describe('createContact', () => {
    it('should create contact with valid data', async () => {
      const contactData = {
        email: 'test@example.com',
        firstname: 'John',
        lastname: 'Doe'
      };
      
      const result = await createContact(contactData);
      
      expect(result.success).toBe(true);
      expect(result.data.email).toBe(contactData.email);
    });

    it('should reject contact without email', async () => {
      const contactData = { firstname: 'John' };
      
      const result = await createContact(contactData);
      
      expect(result.success).toBe(false);
      expect(result.error).toContain('Email is required');
    });
  });
});
Integration Tests
Test server interactions:
typescript
describe('HubSpot Server Integration', () => {
  it('should handle complete campaign workflow', async () => {
    // Test full workflow from creation to analytics
    const campaign = await createCampaign(campaignData);
    const contacts = await addContactsToCampaign(campaign.id, contactList);
    const metrics = await getCampaignMetrics(campaign.id);
    
    expect(campaign.success).toBe(true);
    expect(contacts.success).toBe(true);
    expect(metrics.data.totalContacts).toBe(contactList.length);
  });
});
Test Coverage Requirements
Minimum 80% code coverage for new features
All public functions must have unit tests
Critical paths must have integration tests
Error scenarios must be tested
📖 Documentation Guidelines
Code Documentation
typescript
/**
 * Creates a new marketing campaign in HubSpot
 * 
 * @param campaignData - Campaign configuration and content
 * @param campaignData.name - Campaign name (required)
 * @param campaignData.type - Campaign type (email, social, etc.)
 * @param campaignData.targetAudience - Audience segmentation criteria
 * @returns Promise resolving to campaign creation result
 * 
 * @example
 * ```typescript
 * const result = await createCampaign({
 *   name: "Q1 Product Launch",
 *   type: "email",
 *   targetAudience: { segment: "new-customers" }
 * });
 * 
 * if (result.success) {
 *   console.log(`Campaign created with ID: ${result.data.id}`);
 * }
 * ```
 */
export async function createCampaign(campaignData: CampaignData): Promise<ToolResult> {
  // Implementation
}
README Updates
When adding new features:
Update the main README.md feature list
Add business use case examples
Update configuration documentation
Include new tool descriptions
API Documentation
Maintain comprehensive API docs:
Tool function signatures
Parameter descriptions
Return value formats
Usage examples
Error scenarios
👥 Review Process
Pull Request Requirements
Your PR must include:
 Clear description of changes
 Reference to related issue
 All tests passing
 Code coverage maintained/improved
 Documentation updates
 No breaking changes (or clearly documented)
Review Criteria
Reviewers will check:
Functionality: Does it work as intended?
Code Quality: Is it well-written and maintainable?
Testing: Are there adequate tests?
Documentation: Is it properly documented?
Performance: Does it maintain system performance?
Security: Are there any security implications?
Review Timeline
Initial review within 48 hours
Feedback response expected within 1 week
Final approval within 1 week of all requirements met
Merge Requirements
To be merged, PRs need:
✅ At least 1 approving review from maintainer
✅ All CI checks passing
✅ Up-to-date with main branch
✅ All conversations resolved
🎯 Development Tips
Best Practices
Start Small: Begin with minor improvements
Test Thoroughly: Test both success and failure cases
Document Everything: Code should be self-explanatory
Performance Matters: Consider impact on response times
Security First: Validate all inputs and secure API calls
Common Pitfalls
Don't forget to update documentation
Avoid breaking existing functionality
Don't skip testing error scenarios
Remember to handle API rate limits
Consider backward compatibility
Getting Help
GitHub Discussions: Ask questions and share ideas
Issues: Report bugs or request features
Discord/Slack: Real-time community support
Maintainer Contact: Direct contact for complex issues
🏆 Recognition
Contributor Credits
All contributors are recognized in:
README.md contributors section
Release notes for their contributions
Annual contributor highlights
Contribution Levels
🌟 First-time contributor: Welcome badge
🚀 Regular contributor: 5+ merged PRs
💎 Core contributor: Significant feature additions
🎯 Maintainer: Ongoing project stewardship
📞 Support
Need help contributing?
GitHub Discussions: General questions and community support
Issues: Bug reports and feature requests
Email: maintainers@enterprise-mcp.dev
Documentation: Check our Wiki
Thank you for contributing to the Enterprise MCP Ecosystem! 🚀
Your contributions help create better automation tools for businesses worldwide.
