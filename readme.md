# PDF Document Processing Workflow

An N8n workflow that automatically processes PDF documents by extracting key information using ChatGPT and organizing the data in a Google Spreadsheet.

## 🎯 What This Workflow Does

This workflow monitors a Google Drive folder for new PDF documents and automatically:
1. **Monitors** an "Incoming" folder for new PDF files
2. **Downloads** and extracts text from uploaded PDFs
3. **Uses ChatGPT** to identify and extract key business data
4. **Saves** extracted data to a Google Sheets spreadsheet
5. **Moves** processed files to a "Processed" folder

## 📊 Data Extracted

The workflow extracts the following information from each PDF:
- **Processed Date** - When the workflow ran
- **Document Type** - Tax Invoice, Delivery Docket, or PO Reprint
- **Document Date** - The main date from the document
- **Purchase Order Number** - PO numbers or order references
- **Vendor Name** - Company/supplier name
- **Original File Name** - For reference

## 🛠️ Setup Instructions

### Step 1: Create Google Drive Folders
1. Create an "Incoming" folder in Google Drive
2. Create a "Processed" folder in Google Drive
3. Note down the folder IDs from the URLs:
   - Open each folder in Google Drive
   - Copy the ID from the URL (the long string after `/folders/`)

### Step 2: Create Google Spreadsheet
1. Create a new Google Spreadsheet called "Invoices"
2. Add these headers to the first row:
   - A1: `processedDate`
   - B1: `documentType`
   - C1: `documentDate`
   - D1: `purchaseOrderNumber`
   - E1: `vendorName`
   - F1: `originalFileName`
3. Note down the spreadsheet ID from the URL (between `/d/` and `/edit`)

### Step 3: Configure N8n Credentials
Set up the following credentials in N8n:
- **Google Drive OAuth2 API** (for folder monitoring and file operations)
- **Google Sheets OAuth2 API** (for spreadsheet access)
- **OpenAI API** (for ChatGPT document analysis)

### Step 4: Update the Workflow
After importing the workflow, update these placeholder values:

**In "Monitor Incoming Folder" node:**
- Replace `INCOMING_FOLDER_ID` with your actual incoming folder ID

**In "Add to Invoices Spreadsheet" node:**
- Replace `INVOICES_SPREADSHEET_ID` with your actual spreadsheet ID

**In "Move to Processed Folder" node:**
- Replace `PROCESSED_FOLDER_ID` with your actual processed folder ID

**Update credential references:**
- Replace `id_google_drive` with your actual Google Drive credential ID
- Replace `id_google_sheets` with your actual Google Sheets credential ID  
- Replace `id_chatgpt` with your actual OpenAI credential ID

## 🔧 How to Deploy

1. **Import the workflow** into your N8n instance
2. **Set up credentials** as described above
3. **Replace placeholder IDs** with your actual folder/spreadsheet IDs
4. **Test the workflow** by uploading a PDF to your incoming folder
5. **Activate the workflow** to start automatic processing

## 🧪 Testing

To test the workflow:
1. Upload a PDF document to your "Incoming" folder
2. Check the N8n execution log for any errors
3. Verify that data appears in your "Invoices" spreadsheet
4. Confirm the PDF was moved to your "Processed" folder

## 🚨 Troubleshooting

**Common issues:**
- **No text extracted**: The PDF might be image-based. Consider adding OCR capability
- **ChatGPT parsing errors**: Check the debug node output to see what text was extracted
- **Permission errors**: Ensure your Google credentials have access to the specified folders/spreadsheets
- **Workflow not triggering**: Check that the folder ID is correct and the workflow is activated

**Debug tips:**
- The workflow includes a debug node that logs extracted text to help troubleshoot
- Check N8n's execution history for detailed error messages
- Verify your credential permissions in Google Drive/Sheets

## 📝 Notes

- The workflow runs every minute to check for new files
- Only PDF files are processed
- Error records are created in the spreadsheet if processing fails
- The workflow uses GPT-4 for document analysis (configurable)
- Files are moved (not copied) to the processed folder

## 🔒 Security Considerations

- API keys and credentials are stored securely in N8n's credential system
- Folder and spreadsheet IDs are visible in the workflow but don't grant access without proper credentials
- Consider limiting Google Drive folder permissions to specific users
- Monitor your OpenAI API usage to avoid unexpected charges

## 📄 License

This workflow is provided as-is for educational and personal use. Modify as needed for your specific requirements.