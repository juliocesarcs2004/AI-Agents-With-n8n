# AI-Agents-With-n8n

AI-Agents-with-n8n is a collection of practical workflows built with n8n to create AI-powered agents. The repository showcases real-world use cases such as email automation, data processing, decision-making, and integrations with LLMs, APIs, and external services, focusing on scalable and reusable automation patterns.

## Workflows

### 1. AI-Powered Email Reply Automation

An intelligent email automation workflow that autonomously reads incoming Gmail messages and generates intelligent, context-aware responses using AI.

#### Features

- **Gmail Integration**: Continuously monitors Gmail inbox for new messages
- **Smart Filtering**: Filters out emails from specific senders using conditional logic
- **AI-Powered Responses**: Generates intelligent email replies using Google Gemini API
- **Memory Management**: Maintains conversation context using memory buffers for personalized responses
- **Automatic Sending**: Automatically sends AI-generated replies to incoming email messages

#### Workflow Components

1. **Gmail Trigger** - Monitors Gmail inbox for new incoming messages (polls every minute)
2. **If (Conditional)** - Filters emails by sender domain (excludes hashtag.com emails)
3. **AI Agent** - LangChain-based agent that processes email content and generates responses
4. **Google Gemini Chat Model** - LLM that generates intelligent, context-aware replies
5. **Simple Memory** - Manages conversation history per email thread
6. **Reply to a message** - Sends the AI-generated response back via Gmail

#### Setup Requirements

- n8n instance with Gmail OAuth2 credentials configured
- Google Gemini API credentials
- Active Gmail account
- n8n LangChain nodes library

#### How It Works

1. Gmail Trigger detects new incoming messages every minute
2. If node checks the sender domain to filter out internal hashtag.com emails
3. AI Agent receives the email content and uses Gemini LLM to generate a contextual response
4. Memory buffer maintains conversation history per thread for more intelligent replies
5. Generated response is automatically sent back as a Gmail reply

#### Customization

The workflow can be customized with:
- Different filtering rules in the If node
- Custom AI system prompts (currently configured for sales/course support)
- Alternative LLM providers by swapping the Chat Model node
- Different polling intervals for the Gmail trigger
