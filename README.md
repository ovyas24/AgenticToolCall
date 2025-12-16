# AgenticToolCall

An AI-powered personal assistant chatbot that uses OpenAI's GPT-4o-mini with agentic tool calling capabilities. The chatbot acts as a personal representative, answering questions about background, experience, and skills while intelligently recording user interactions via Discord webhooks.

## Overview

This application creates an interactive web-based chatbot using Gradio that impersonates a person (Om Vyas) and can:
- Answer questions about career, background, and experience using information from LinkedIn PDF and summary text
- Use OpenAI's function calling (tool calling) to execute specific actions
- Send notifications to Discord when users provide contact information or ask unanswerable questions
- Engage users professionally and encourage them to get in touch

## How It Works

### Core Architecture

1. **Profile Loading**: On initialization, the `Me` class loads:
   - LinkedIn profile information from `me/linkedin.pdf` (using PyPDF)
   - A summary from `me/summary.txt`

2. **Agentic Tool Calling**: The chatbot uses OpenAI's function calling feature with two custom tools:
   - `record_user_details`: Records user contact information (email, name, notes)
   - `record_unknown_question`: Records questions the bot couldn't answer

3. **Chat Loop**: The conversation flow works as follows:
   - User sends a message through the Gradio web interface
   - System prompt includes the loaded profile information
   - OpenAI GPT-4o-mini processes the message with available tools
   - If GPT decides to call a tool, the function executes and sends a Discord notification
   - The bot continues the loop until it has a final response
   - Response is returned to the user via Gradio

4. **Discord Notifications**: When tools are called, the `discord_notify` function:
   - Sends an HTTP POST request to a Discord webhook URL
   - Includes formatted information about the tool execution
   - Allows real-time monitoring of user interactions

## Technologies Used

### Core Libraries

- **OpenAI**: GPT-4o-mini model for natural language understanding and generation with function calling
- **Gradio**: Web-based chat interface for user interaction
- **python-dotenv**: Environment variable management for API keys and webhook URLs
- **pypdf (PdfReader)**: PDF parsing to extract LinkedIn profile information
- **requests**: HTTP client for sending Discord webhook notifications

### Key Concepts

- **Agentic Tool Calling**: LLM decides when to execute predefined functions
- **Function Calling**: OpenAI's native tool calling API with JSON schema definitions
- **Webhook Integration**: Discord webhooks for real-time notifications

## Setup Instructions

### Prerequisites

- Python 3.7+
- OpenAI API key
- Discord webhook URL (optional, for notifications)

### Installation

1. Clone the repository:
```bash
git clone https://github.com/ovyas24/AgenticToolCall.git
cd AgenticToolCall
```

2. Install dependencies:
```bash
pip install -r requirements.txt
```

3. Create a `.env` file in the project root with:
```env
OPENAI_API_KEY=your_openai_api_key_here
DISCORD_WEBHOOK_URL=your_discord_webhook_url_here
```

4. Create the required file structure:
```
AgenticToolCall/
├── app.py
├── requirements.txt
├── .env
└── me/
    ├── linkedin.pdf    # Your LinkedIn profile as PDF
    └── summary.txt     # A text summary of your background
```

### Required Files

- `me/linkedin.pdf`: LinkedIn profile exported as PDF (used to provide career/background context)
- `me/summary.txt`: A text file containing a summary of your background, skills, and experience

## Usage

Run the application:
```bash
python app.py
```

The Gradio interface will launch and provide a URL (typically `http://localhost:7860`). Open this URL in your browser to interact with the chatbot.

### What the Chatbot Does

- **Answers Career Questions**: Uses the LinkedIn profile and summary to answer questions about experience, skills, projects, etc.
- **Records Contact Info**: When users provide their email, it records the information and sends a Discord notification
- **Logs Unknown Questions**: Questions the bot can't answer are recorded via Discord for follow-up
- **Stays in Character**: Maintains a professional persona throughout the conversation
- **Encourages Engagement**: Tries to steer conversations toward getting user contact information

## Tool Definitions

### record_user_details
Records user contact information when they express interest in connecting.

**Parameters:**
- `email` (required): User's email address
- `name` (optional): User's name
- `notes` (optional): Additional context about the conversation

### record_unknown_question
Records questions the chatbot couldn't answer.

**Parameters:**
- `question` (required): The question that couldn't be answered

## Environment Variables

- `OPENAI_API_KEY`: Your OpenAI API key for GPT-4o-mini access
- `DISCORD_WEBHOOK_URL`: Discord webhook URL for receiving notifications

## Customization

To customize this for your own use:

1. Update the `self.name` variable in the `Me` class constructor
2. Replace `me/linkedin.pdf` with your LinkedIn profile PDF
3. Update `me/summary.txt` with your personal summary
4. Modify the system prompt in `system_prompt()` method to change the bot's behavior
5. Add or modify tools in the `tools` list to extend functionality

## Contributing

Contributions are welcome! Please feel free to submit issues or pull requests.

## License

[Add your license information here]

## Contact

For questions or support, please open an issue on GitHub.