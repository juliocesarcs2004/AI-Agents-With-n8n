# AI-Agents-With-n8n

AI-Agents-with-n8n is a collection of practical workflows built with n8n to create AI-powered agents. The repository showcases real-world use cases such as email automation, data processing, decision-making, and integrations with LLMs, APIs, and external services, focusing on scalable and reusable automation patterns.

## Workflows

### 1. Customer Feedback Automation With Telegram

<img width="894" height="348" alt="Customer-Feedback-Automation-With-Telegram" src="https://github.com/user-attachments/assets/7251be2b-83e3-46c3-83cb-52110032578d" />

An intelligent customer feedback processing workflow that handles refund requests by analyzing form submissions, looking up customer data, assessing sentiment, and routing responses based on customer value and feedback tone.

#### Features

- **Form Integration**: Receives customer feedback and refund requests via webhook
- **AI-Powered Analysis**: Uses AI to extract and analyze form data with structured output
- **Customer Database Lookup**: Integrates with Google Sheets to verify customer information
- **Sentiment Analysis**: Evaluates customer emotion (positive, neutral, negative, very negative)
- **Smart Routing**: Routes responses based on multiple factors:
  - Customer lifetime value (VIP vs regular customers)
  - Days since purchase (warranty period enforcement)
  - Sentiment level (escalates complaints to management)
- **Multi-Channel Notifications**: Sends responses via Gmail and alerts team via Telegram

#### Workflow Components

1. **Webhook** - Receives form submissions from Tally form platform
2. **AI Agent** - LangChain-based agent that processes form data and performs analysis
3. **Google Gemini Chat Model** - LLM that extracts and interprets customer feedback
4. **Get row(s) in sheet in Google Sheets** - Looks up customer data by email in customer database
5. **Structured Output Parser** - Formats AI output into structured JSON
6. **If (Multiple Conditions)** - Routes based on days since purchase (>7 days = out of warranty)
7. **If1** - Routes common customers with negative sentiment
8. **If2** - Routes VIP customers (total spent ≥ R$3000)
9. **If3** - Routes very negative sentiment cases for escalation
10. **Send a message (3x)** - Sends personalized email responses to customers
11. **Send a text message (2x)** - Sends alerts to team via Telegram for follow-up

#### Extracted Data Points

The AI Agent analyzes and extracts:
- **nome**: Customer name from form
- **email**: Customer email (used as lookup key)
- **produto_reembolso**: Product requested for refund
- **comentario**: Customer's written comment
- **sentimento**: Sentiment analysis (positive, neutral, negative, very_negative)
- **cliente_encontrado**: Whether customer exists in database (true/false)
- **nome_completo**: Full name from customer database
- **total_gasto_cliente**: Lifetime value from database
- **dias_desde_compra**: Days since last purchase (calculated from database)

#### Routing Logic

**Out of Warranty (>7 days):** Customers whose purchase is beyond the 7-day guarantee period
- Regular customers: Automatic denial with explanation
- VIP customers (≥R$3000 spent): Manual review escalation to finance team via Telegram
- Very negative sentiment: High-priority escalation to management with full case details

**Within Warranty (≤7 days):** Processed through standard approval workflow

#### Setup Requirements

- n8n instance with webhook enabled
- Google Gemini API credentials
- Gmail OAuth2 credentials for customer responses
- Telegram Bot Token for team notifications
- Google Sheets with customer database:
  - Columns: email, nome_cliente, total_gasto_cliente, data_ultima_compra
- Tally form connected to webhook endpoint

#### How It Works

1. Customer submits refund request form via Tally form webhook
2. AI Agent receives form data and processes it
3. Agent uses Google Sheets tool to look up customer by email
4. Structured Output Parser formats the extracted data into JSON
5. First If node checks if purchase is within 7-day warranty period
6. Based on conditions (warranty status, customer value, sentiment):
   - Regular customers get automatic response
   - VIP customers get manual review escalation
   - Highly negative sentiment gets management review
7. Appropriate email response is sent to customer
8. Team notifications are sent via Telegram with relevant details

#### Customization

The workflow can be customized with:
- Warranty period threshold (currently 7 days)
- VIP customer threshold (currently R$3000 lifetime value)
- Sentiment classification rules
- Custom response messages for each scenario
- Different routing destinations (email, Slack, SMS, etc.)
- Additional customer data fields from database


### 2. AI-Powered Email Reply Automation

<img width="894" height="310" alt="AI-Powered Email Reply Automation" src="https://github.com/user-attachments/assets/856f6605-3778-4664-8618-90a55fbb12ab" />

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

---
