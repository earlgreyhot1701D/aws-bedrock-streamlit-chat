# Cal Poly AI Summer Camp: Interactive AI Chatbot with Streamlit

Welcome to our AI Summer Camp! This project will teach you how to build a web-based AI chatbot using Streamlit and AWS Bedrock. You'll learn how to create an interactive chat interface that connects directly to Claude, allowing users to have natural conversations with AI through a beautiful web application.

## Contact Information

**Instructor**: Ryan Gertz - rgertz@calpoly.edu

Feel free to reach out if you have questions about:
- Setting up AWS credentials
- Understanding Streamlit web development
- Troubleshooting chatbot functionality
- Ideas for extending this project
- General questions about AI and web applications

## What You'll Learn

- **Streamlit Web Development**: Creating interactive web applications with Python
- **Real-time Chat Interface**: Building a conversational UI with message history
- **AWS Bedrock Integration**: Connecting your web app to cloud-based AI services
- **Session State Management**: Maintaining conversation context across interactions
- **User Interface Design**: Creating intuitive chat experiences

## What This Code Does

This project demonstrates how to create a fully functional AI chatbot web application. The code:

1. **Creates a Web Interface**: Uses Streamlit to build a chat-like user interface
2. **Manages Conversations**: Maintains message history and context throughout the chat
3. **Connects to Claude**: Uses AWS Bedrock to send messages to Anthropic's Claude model
4. **Handles Real-time Updates**: Displays messages immediately and shows thinking indicators
5. **Provides Error Handling**: Gracefully manages connection issues and API errors

## Prerequisites

Before you start, you'll need:

### 1. Python Installation
- Python 3.7 or higher installed on your computer
- You can download it from [python.org](https://www.python.org/downloads/)

### 2. AWS Account Setup
- An AWS account
- AWS credentials configured on your computer
- Access to AWS Bedrock service and Anthropic Claude model

### 3. Required Python Packages
Install the necessary packages by running this command in your terminal:
```bash
pip install streamlit boto3
```

## Understanding the Code

### Key Components

**Streamlit**: A Python library that makes it easy to create web applications for data science and AI projects. It handles all the web development complexity for you.

**Session State**: Streamlit's way of remembering information between user interactions. This is crucial for maintaining chat history.

**AWS Bedrock Converse API**: A newer, more efficient way to communicate with Claude that handles message formatting automatically.

**Real-time Updates**: The app updates immediately when users send messages, creating a smooth chat experience.

### The Functions Explained

#### Function 1: `get_bedrock_client()`
This function creates and caches the AWS connection:
- Uses Streamlit's `@st.cache_resource` decorator to avoid recreating the client
- Connects to AWS Bedrock in the us-west-2 region
- Returns a reusable client object for making API calls

#### Function 2: `invoke_model(messages: List[Dict[str, str]])`
This function handles communication with Claude:
- Takes the full conversation history as input
- Converts Streamlit message format to Bedrock's expected format
- Sends the conversation to Claude using the converse API
- Returns Claude's response or an error message if something goes wrong

#### Main Application Logic
The main part of the code handles the user interface:
- Sets up the page configuration and title
- Manages session state for conversation history
- Displays all previous messages in chronological order
- Handles new user input and generates AI responses
- Updates the interface in real-time

## How to Run the Code

1. **Save the code**: Clone this repo or copy the code into a file called `chatbot.py`

2. **Open your terminal/command prompt**

3. **Navigate to your project folder**:
   ```bash
   cd path/to/your/project
   ```

4. **Run the Streamlit app**:
   ```bash
   streamlit run chatbot.py
   ```

5. **Open your browser**: Streamlit will automatically open your default browser to `http://localhost:8501`

6. **Start chatting**: Type a message in the input box and press Enter to chat with the AI!

## Customizing The Code

### Change the App Appearance
Modify the page configuration:
```python
st.set_page_config(
    page_title="My Custom Chatbot", 
    page_icon="🚀",
    layout="wide",
    initial_sidebar_state="expanded"
)
st.title("🚀 My Custom AI Assistant")
st.write("Welcome to my personalized AI helper!")
```

### Adjust AI Response Settings
Modify the inference configuration:
```python
response = client.converse(
    modelId="anthropic.claude-3-5-sonnet-20241022-v2:0",
    messages=bedrock_messages,
    inferenceConfig={
        "maxTokens": 2000,  # Longer responses
        "temperature": 0.7,  # More creative responses
        "topP": 0.9,  # More diverse responses
    }
)
```

### Add System Instructions
Give your chatbot a personality:
```python
# Add this before the user messages
system_message = {
    "role": "system", 
    "content": "You are a helpful coding tutor who explains concepts clearly with examples."
}
bedrock_messages.insert(0, system_message)
```

### Add a Sidebar with Settings
Include user controls:
```python
with st.sidebar:
    st.header("Settings")
    temperature = st.slider("Creativity", 0.0, 1.0, 0.7)
    max_tokens = st.slider("Response Length", 100, 2000, 1000)
    
    if st.button("Clear Chat"):
        st.session_state.messages = []
        st.rerun()
```

## Understanding the Interface

### Chat Display
The app shows a conversation flow similar to popular messaging apps:
- **User messages**: Appear with a user icon
- **AI responses**: Appear with an assistant icon
- **Message history**: All previous messages remain visible
- **Thinking indicator**: Shows when the AI is processing

### Session Management
The app maintains conversation context:
- **Persistent history**: Messages stay visible across interactions
- **Context awareness**: The AI remembers previous parts of the conversation
- **Fresh start**: Restart the app to clear conversation history

## Common Issues and Solutions

### "No module named 'streamlit'"
Install the required package:
```bash
pip install streamlit
```

### "Port already in use"
If port 8501 is busy, Streamlit will automatically try other ports:
- Check the terminal output for the correct URL
- Or specify a different port: `streamlit run chatbot.py --server.port 8502`

### "No credentials found"
This means your AWS credentials aren't set up. You need:
- AWS Access Keys configured
- AWS CLI setup or environment variables
- IAM permissions for Bedrock

### "Access denied to model"
Your AWS account might not have permission to use Claude:
- Check your AWS Bedrock model access in the console
- Ensure you have the correct model ID
- Verify your IAM permissions include bedrock actions

### "Connection timeout"
Network or AWS service issues:
- Check your internet connection
- Try a different AWS region
- Verify AWS service status

### App doesn't update after sending message
This usually means the rerun() isn't working:
- Check for any errors in the terminal
- Restart the Streamlit app
- Ensure you're using the latest version of Streamlit

## Important Notes

- **Cost Awareness**: Each message exchange costs money (usually a few cents per conversation)
- **Rate Limits**: AWS has limits on how many requests you can make per minute
- **Session Persistence**: Messages are only saved while the app is running
- **Local Development**: The app runs on your computer by default

## Use Cases for AI Chatbots

This type of chatbot application is useful for:
- **Customer Support**: Automated assistance for common questions
- **Educational Tools**: Tutoring and learning assistance
- **Content Creation**: Writing help and brainstorming
- **Code Assistance**: Programming help and debugging
- **Research Assistant**: Information gathering and analysis
- **Personal Assistant**: Task management and reminders

## Getting Help

If you run into issues:
1. Check the terminal output for detailed error messages
2. Verify your AWS credentials are working
3. Ask our camp staff for assistance
4. Test with a simple message first
5. Check the Streamlit documentation for UI issues

## Example Conversations

Once running, you can try conversations like:
- **User**: "What is machine learning?"
- **AI**: "Machine learning is a subset of artificial intelligence..."
- **User**: "Can you give me an example?"
- **AI**: "Sure! A great example is email spam detection..."

The chatbot maintains context throughout the conversation, making it feel natural and helpful.

## Extending This Project

Ideas for taking this project further:
- **User Authentication**: Add login functionality
- **Message Export**: Save conversations to files
- **Multiple AI Models**: Let users choose different models
- **File Upload**: Allow users to upload documents for discussion
- **Voice Integration**: Add speech-to-text and text-to-speech
- **Deployment**: Host the app online using Streamlit Cloud
- **Custom Styling**: Add CSS for a unique look and feel
- **Analytics**: Track usage and conversation patterns

## Advanced Features to Add

```python
# File upload capability
uploaded_file = st.file_uploader("Upload a document", type=['txt', 'pdf', 'docx'])

# Model selection
model_choice = st.selectbox("Choose AI Model", 
    ["Claude 3.5 Sonnet", "Claude 3 Haiku", "Claude 3 Opus"])

# Export conversations
if st.button("Export Chat"):
    chat_export = json.dumps(st.session_state.messages, indent=2)
    st.download_button("Download Chat", chat_export, "chat.json")
```

## Resources for Further Learning

- [Streamlit Documentation](https://docs.streamlit.io/)
- [AWS Bedrock Documentation](https://docs.aws.amazon.com/bedrock/)
- [Streamlit Gallery](https://streamlit.io/gallery) - Example applications
- [Claude API Reference](https://docs.anthropic.com/claude/reference/)
- [Python Web Development](https://realpython.com/python-web-applications/)

---

Happy coding! 🚀