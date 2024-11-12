# CloudFix Deprovisioning Automation Script

This repository contains a Python script designed to automate the deprovisioning process for CloudFix subscriptions. The script interacts with Zendesk tickets, extracts necessary information using OpenAI's GPT, and performs database operations to deactivate subscriptions based on customer requests.

## Table of Contents

- [Features](#features)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configuration](#configuration)
- [Usage](#usage)
- [Error Handling](#error-handling)
- [Contributing](#contributing)
- [License](#license)

## Features

- **Zendesk Integration**: Fetches ticket information and updates ticket status and comments.
- **OpenAI GPT Integration**: Extracts necessary data from ticket descriptions using GPT models.
- **Google Sheets Logging**: Logs actions and statuses to a Google Sheet for record-keeping.
- **AWS DynamoDB**: Checks for duplicate runs and prevents multiple executions on the same ticket.
- **MySQL Database Operations**: Updates the CloudFix tenant database to deactivate subscriptions.
- **Scheduler Integration**: Adds future deprovisioning tasks to a scheduler Google Sheet.

## Prerequisites

- **Python 3.7 or higher**
- **AWS Account**: For DynamoDB and other AWS services.
- **Google Cloud Account**: For accessing Google Sheets API.
- **Zendesk Account**: For interacting with Zendesk tickets.
- **OpenAI Account**: For using GPT models.
- **MySQL Database**: Access to the CloudFix tenant database.

## Installation

1. **Clone the Repository**

   ```bash
   git clone https://github.com/yourusername/your-repo-name.git
   cd your-repo-name
   ```

2. **Create a Virtual Environment**

   ```bash
   python3 -m venv venv
   source venv/bin/activate  # On Windows use `venv\Scripts\activate`
   ```

3. **Install Dependencies**

   ```bash
   pip install -r requirements.txt
   ```

## Configuration

1. **Environment Variables**

   Create a `.env` file in the root directory and add the following environment variables:

   ```ini
   OPENAI_API_KEY=your_openai_api_key
   ZENDESK_API_KEY=your_zendesk_api_key
   aws_access_key_id=your_aws_access_key_id
   aws_secret_access_key=your_aws_secret_access_key
   region_name=your_aws_region
   DB_USER=your_database_user
   DB_PASSWORD=your_database_password
   DB_HOST=your_database_host
   DB_PORT=your_database_port
   DB_NAME=your_database_name
   ```

2. **Google Sheets Credentials**

   Place your Google Sheets API credentials JSON file in the root directory and update the `CREDENTIALS_FILE` variable in the script:

   ```python
   CREDENTIALS_FILE = 'path_to_your_credentials.json'
   ```

3. **Update Script Variables**

   - **Google Sheets IDs**

     ```python
     SPREADSHEET_ID = 'your_logging_sheet_id'
     SHEET_NAME = 'your_logging_sheet_name'
     ```

   - **Scheduler Sheet**

     ```python
     # Scheduler Google Sheet IDs
     ```

   - **DynamoDB Table**

     ```python
     table_name = 'your_dynamodb_table_name'
     ```

## Usage

The script is intended to be deployed as an AWS Lambda function but can also be run locally for testing.

### Running Locally

1. **Set Up AWS Credentials**

   Ensure your AWS credentials are configured locally, either via environment variables or AWS CLI configuration.

2. **Run the Script**

   ```bash
   python your_script_name.py
   ```

### AWS Lambda Deployment

1. **Package Dependencies**

   Use a tool like `zip` or AWS SAM to package your dependencies along with the script.

2. **Deploy to AWS Lambda**

   - Upload the package to AWS Lambda.
   - Set up the appropriate execution role with necessary permissions.
   - Configure environment variables within the Lambda function configuration.

## Error Handling

The script includes comprehensive error handling and logging:

- **Invalid Auth or Ticket Number**

  Logs the issue and updates the Zendesk ticket with an appropriate comment.

- **Duplicate Runs**

  Uses DynamoDB to prevent multiple executions on the same ticket within a specified time frame.

- **Database Errors**

  Catches MySQL errors and logs them, updating the ticket for manual intervention.

- **Google API Errors**

  Handles Google Sheets API errors gracefully, ensuring the script continues running.

## Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository.
2. Create a new branch: `git checkout -b feature/your-feature-name`.
3. Commit your changes: `git commit -am 'Add some feature'`.
4. Push to the branch: `git push origin feature/your-feature-name`.
5. Submit a pull request.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

*Disclaimer: Ensure all API keys and sensitive information are securely stored and not exposed in any commits or logs.*
