# Post Generator

## Overview
This n8n workflow is an AI-powered content creation system that automatically generates professional blog posts with accompanying marketing visuals. Users submit a form with their topic and target audience, and receive a complete blog post with a custom-generated image via email.

## Features
- 📝 **AI-Powered Content Creation**: Uses GPT-4 to generate professional blog posts
- 🔍 **Real-Time Research**: Integrates Google Search for up-to-date information
- 🎨 **Custom Image Generation**: Creates marketing visuals using OpenAI's image generation
- 📧 **Email Delivery**: Automatically sends completed content via Gmail
- 🎯 **Audience Targeting**: Tailors content to specific target audiences
- 🌐 **Web Form Interface**: Easy-to-use form for content requests

## Workflow Components

### 1. Form Trigger
- **Type**: Form Trigger
- **Purpose**: Collects user input for content generation
- **Fields**:
  - **Email**: User's email address (required)
  - **Topic of Post**: Subject matter for the blog post (required)
  - **Target Audience**: Intended readership (e.g., "young professionals") (required)
- **Webhook ID**: `48e79ae9-ffaf-469c-bc3f-3b578107465d`

### 2. Post Agent
- **Type**: LangChain Agent
- **Purpose**: Generates the main blog post content
- **Features**:
  - Real-time Google Search integration
  - Professional tone and structure
  - Source attribution
  - Relevant hashtags
  - Call-to-action endings
- **System Instructions**:
  - Conducts research using Google Search
  - Creates engaging, educational content
  - Includes proper source citations
  - Formats with hashtags and CTAs

### 3. OpenAI Chat Model
- **Type**: LangChain OpenAI Chat Model
- **Model**: GPT-4.1
- **Purpose**: Powers both content generation agents
- **Credentials**: OpenAI API account

### 4. Google Search Tool
- **Type**: SerpAPI Tool
- **Purpose**: Provides real-time search capabilities for research
- **Integration**: Connected to Post Agent for information gathering
- **Credentials**: SerpAPI account

### 5. Image Prompt Agent
- **Type**: LangChain Agent
- **Purpose**: Transforms blog content into visual prompt descriptions
- **Features**:
  - Analyzes blog post content
  - Creates marketing-style graphic prompts
  - Optimized for AI image generation
  - Professional, modern design focus
- **Output**: Detailed image generation prompts

### 6. Image Generation (HTTP Request)
- **Type**: HTTP Request
- **Method**: POST
- **Endpoint**: OpenAI Images API
- **Purpose**: Generates custom marketing visuals
- **Configuration**:
  - Model: `gpt-image-1`
  - Size: 1024x1024 pixels
  - Uses environment variables for security

### 7. File Conversion
- **Type**: Convert to File
- **Purpose**: Converts base64 image data to binary file
- **Source**: `data[0].b64_json` from OpenAI response
- **Output**: Binary image file for email attachment

### 8. Gmail Delivery
- **Type**: Gmail
- **Purpose**: Sends completed content to user
- **Features**:
  - Dynamic recipient (from form submission)
  - Includes generated blog post in email body
  - Attaches generated image
- **Credentials**: Gmail OAuth2 account

## Environment Variables

The workflow uses environment variables to securely store sensitive API information:

```env
# OpenAI API Configuration
OPENAI_API_KEY=your_openai_api_key_here

# OpenAI API Endpoints
OPENAI_IMAGE_API_URL=https://api.openai.com/v1/images/generations
```

### Setting Up Environment Variables

1. Create a `.env` file in your workflow directory
2. Add your actual OpenAI API key
3. Ensure the `.env` file is added to `.gitignore` for security
4. Never commit API keys to version control

## Content Generation Process

### 1. Research Phase
- Agent receives topic and target audience
- Performs Google Search for current information
- Gathers relevant data, statistics, and insights

### 2. Content Creation
- Structures information into professional blog post
- Includes engaging hooks and clear conclusions
- Adds source attributions and relevant hashtags
- Incorporates call-to-action elements

### 3. Visual Design
- Analyzes blog content for key messages
- Creates detailed prompt for image generation
- Focuses on marketing-style graphics
- Ensures brand-appropriate visual elements

### 4. Image Generation
- Sends prompt to OpenAI's image generation API
- Creates 1024x1024 pixel marketing visual
- Converts to file format for email attachment

### 5. Delivery
- Compiles blog post and image
- Sends via email to requester
- Includes both text content and visual attachment

## Blog Post Structure

Generated posts typically include:

- **Engaging Hook**: Attention-grabbing opening
- **Professional Tone**: Business-appropriate language
- **Educational Content**: Valuable insights and information
- **Source Citations**: "According to [source]" attributions
- **Relevant Hashtags**: For social media visibility
- **Call-to-Action**: Encourages engagement and sharing

## Image Generation Guidelines

Visual prompts are designed to create:

- **Marketing-Style Graphics**: Professional, polished appearance
- **Modern Design Elements**: Clean typography, gradients, icons
- **Content Alignment**: Visually supports blog post message
- **Brand Consistency**: Appropriate for professional use
- **Engagement Focus**: Designed to attract and retain attention

## Setup Instructions

### Prerequisites
- n8n instance with LangChain nodes installed
- OpenAI API account and key
- SerpAPI account for Google Search
- Gmail account with OAuth2 setup

### Installation Steps

1. **Import Workflow**: Import the JSON file into your n8n instance
2. **Configure Environment**: Set up the `.env` file with your API keys
3. **Set Up Credentials**:
   - OpenAI API credentials
   - SerpAPI credentials
   - Gmail OAuth2 credentials
4. **Test Form**: Access the form URL to test submissions
5. **Activate Workflow**: Enable the workflow to start processing requests

## API Requirements

### OpenAI API
- **Required**: Valid API key with sufficient credits
- **Models Used**: GPT-4.1 for text, Image generation for visuals
- **Endpoints**: Chat completions and image generation

### SerpAPI
- **Required**: Active account with search credits
- **Purpose**: Real-time Google Search integration
- **Usage**: Research phase of content generation

### Gmail API
- **Required**: OAuth2 setup for sending emails
- **Permissions**: Send email capability
- **Purpose**: Delivery of generated content

## Customization Options

### Content Style
Modify the system message in the Post Agent to adjust:
- Writing tone and style
- Content structure preferences
- Source citation format
- Hashtag usage

### Image Style
Update the Image Prompt Agent system message to change:
- Visual design preferences
- Brand guidelines
- Color schemes
- Layout styles

### Form Fields
Add or modify form fields to collect:
- Additional content requirements
- Style preferences
- Brand guidelines
- Specific formatting requests

## Troubleshooting

### Common Issues

1. **API Key Errors**
   - Verify `OPENAI_API_KEY` is correct
   - Check API key has sufficient credits
   - Ensure environment variables are loaded

2. **Search Failures**
   - Verify SerpAPI credentials
   - Check search quota limits
   - Review search query formatting

3. **Email Delivery Issues**
   - Confirm Gmail OAuth2 setup
   - Check email permissions
   - Verify recipient email format

4. **Image Generation Failures**
   - Check OpenAI image API status
   - Verify prompt length limits
   - Review content policy compliance

### Debugging Tips

- Enable debug logging in Code nodes
- Test individual components separately
- Monitor API usage and limits
- Check n8n execution logs for errors

## Security Considerations

- ✅ **API Keys**: Stored securely in environment variables
- ✅ **No Hardcoded Secrets**: All sensitive data externalized
- ✅ **OAuth2**: Secure Gmail authentication
- ✅ **Form Validation**: Required fields prevent empty submissions
- ✅ **Content Filtering**: AI models include built-in content policies

## Usage Limits

### OpenAI API
- Text generation: Based on token usage
- Image generation: Per-image pricing
- Rate limits: Varies by account tier

### SerpAPI
- Search queries: Based on subscription plan
- Monthly limits apply

### Gmail API
- Daily sending limits
- OAuth2 token refresh requirements

## Cost Optimization

- Monitor OpenAI token usage
- Optimize prompt lengths
- Cache search results when possible
- Use appropriate model sizes for tasks

## License

This workflow is provided as-is for content generation purposes. Modify as needed for your specific requirements. Ensure compliance with all API terms of service.