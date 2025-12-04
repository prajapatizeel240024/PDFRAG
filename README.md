# PDF Conversational AI with LangChain and FAISS

![image](https://github.com/user-attachments/assets/e0985bb6-83fd-441c-a56a-e4fe7b03b917)


## **Overview**
This project provides an AI-powered conversational system to interact with PDF documents. Leveraging OpenAI's GPT model, **LangChain** for conversational retrieval, and **FAISS** as a vector store for embeddings, this application enables users to upload a PDF, extract its content, and engage in an interactive Q&A session via a streamlined **Streamlit** interface.

## **Features**
- Extract text from uploaded PDF files.
- Split text into manageable chunks for efficient processing.
- Generate vector embeddings and store them in a FAISS database for retrieval.
- Create a conversational retrieval system for interactive Q&A with the document.
- User-friendly chat interface with customizable templates.

---

## **Requirements**
- Python 3.7 or higher.
- OpenAI API key (stored in a `.env` file).

---

## **Installation and Setup**

1. **Set Up Environment Variables**:
   - Create a `.env` file in the root directory of your project.
   - Add your OpenAI API key to the file:
     ```plaintext
     OPENAI_API_KEY=your_openai_api_key
     ```

2. **Install Dependencies**:
   Install the required Python packages by running:
   ```bash
   pip install -r requirements.txt
   ```

3. **Run the HTML Templates Setup**:
   Before launching the main application, set up the chat templates by executing:
   ```bash
   python htmlTemplates.py
   ```

4. **Run the Streamlit Application**:
   Start the application using Streamlit:
   ```bash
   streamlit run app.py
   ```

---

## **Usage**

### **Step 1: Upload a PDF**
- In the Streamlit app, use the file uploader to upload a PDF document.

### **Step 2: Processing the PDF**
- The system extracts text from the uploaded PDF and splits it into chunks for efficient retrieval.

### **Step 3: Interactive Chat**
- Once processing is complete, you can interact with the PDF content by typing questions in the chat box.
- Example:
  ```plaintext
  You: What is the main topic of the document?
  Assistant: The document primarily discusses the principles of AI and its applications.
  ```

### **Step 4: Exit**
- To stop the chat session, type `exit`.

---

## **Project Architecture**

1. **PDF Upload and Text Extraction**:
   - The app reads and extracts text from uploaded PDF files using `PyPDF2`.

2. **Text Chunking**:
   - The extracted text is split into smaller chunks using `LangChain`'s `CharacterTextSplitter`.

3. **Embeddings Generation**:
   - The text chunks are embedded into vector space using `OpenAIEmbeddings`.

4. **FAISS Vector Store**:
   - The embeddings are stored in a FAISS database for quick and efficient retrieval.

5. **Conversational Retrieval Chain**:
   - A LangChain-based conversational retrieval chain connects the vector database to the chatbot interface.

6. **Streamlit Chat Interface**:
   - Users interact with the chatbot through a web-based interface powered by Streamlit.

---

## **Project Structure**

```plaintext
.
├── app.py                # Main Streamlit application
├── htmlTemplates.py      # Contains the CSS and HTML templates for the chat interface
├── requirements.txt      # List of dependencies
├── README.md             # Documentation
├── .env                  # API key for OpenAI (not included in the repository)
```

---

## **Features and Customizations**

1. **Temperature Setting**:
   - The chatbot's creativity can be adjusted by modifying the `temperature` parameter in the `app.py`.

2. **Chunk Size and Overlap**:
   - Adjust `chunk_size` and `chunk_overlap` in the `CharacterTextSplitter` to handle documents of varying lengths.

3. **CSS and HTML Customization**:
   - Modify `htmlTemplates.py` for personalized chatbot styles.

---

## **Example Workflow**

### Uploading and Interacting with a PDF

1. **Upload**:
   - Drag and drop a PDF or select one using the file uploader in the Streamlit interface.

2. **Interact**:
   - Ask questions such as:
     ```plaintext
     You: What is the primary focus of the document?
     Assistant: The document is focused on the implementation of AI in education.
     ```

3. **Exit**:
   - To stop the interaction, simply type `exit`.

---

## **Dependencies**

Add the following to your `requirements.txt` file:
```plaintext
streamlit
python-dotenv
PyPDF2
langchain
openai
faiss-cpu
```

Install all dependencies using:
```bash
pip install -r requirements.txt
```

---

## **License**
This project is licensed under the MIT License.

---

## **Acknowledgments**
- **OpenAI**: For GPT-powered conversational AI.
- **LangChain**: For providing tools for conversational retrieval.
- **FAISS**: For efficient vector-based storage and retrieval.
- **Streamlit**: For an easy-to-use interactive interface.

# Fiancé Visa (K-1) - Frontend Integration Guide

## Overview

The Fiancé Visa service allows U.S. citizens to bring their foreign fiancé(e) to the United States for marriage. This guide explains how to integrate the frontend with the backend API.

---

## Service Details

### Service Information
- **Service Slug**: `"fiance-visa"`
- **Service Title**: `"Fiancé Visa"`
- **Category**: `"Family"`
- **Icon**: `"heart"`
- **Forms**: `["I-129F"]`

### Pricing
- **Service Fee**: $600.00 (60,000 cents)
- **USCIS Filing Fee**: $675.00 (67,500 cents) - paid separately during Review & Sign step

---

## Process Flow

The fiance visa service follows this step-by-step process:

### 1. Prerequisite Check
**Step Type**: `PREREQUISITE`

Before starting the service, users must answer two eligibility questions:

1. **"Is petitioner a U.S. citizen?"**
   - If **No** → User is **ineligible** (only U.S. citizens can file)
   - Error message: "You are not eligible for Fiancé Visa"
   - Description: "Only U.S. citizens are eligible to file a fiancé(e) visa petition. Green Card holders are not permitted to file this type of application."

2. **"Have you met in person within the last two years?"**
   - If **No** → **Consultation required**
   - Message: "Live consultation required"
   - Description: "Please book a live chat with our immigration expert"

**API Endpoint**: Check service eligibility via the order creation endpoint

---

### 2. Questionnaire (Combined)
**Step Type**: `QUESTIONNAIRE`
**Step Slug**: `"fiancee-visa-questionnaire"`

This is a **single combined step** that includes both:
- **Petitioner Form**: `"i-129f-pet"`
- **Beneficiary Form**: `"i-129f-ben"`

**Important**: Unlike other services that have separate questionnaire steps, the fiance visa has ONE questionnaire step that contains both forms.

**Form Sections - Petitioner (`i-129f-pet`)**:
1. Name and Name Change
2. Other Information (Petitioner)
3. General Questions (Petitioner)
4. Biographic Information
5. Five Years Addresses
6. Employment History
7. Parent Information
8. Prior Marital History
9. Children (Petitioner)

**Form Sections - Beneficiary (`i-129f-ben`)**:
1. Name and Name Change
2. Other Information (Beneficiary)
3. Five Years Addresses (Beneficiary)
4. Employment History (Beneficiary)
5. Parent Information
6. Prior Marital History
7. Children (Beneficiary)

**API Endpoints**:
- Get questionnaire milestone: `GET /api/v1/milestone/:milestoneId`
- Submit form section: `POST /api/v1/form-submission`
- Get form sections: Use the milestone's step resources

---

### 3. Service Fee
**Step Type**: `SERVICE_FEE`
**Step Slug**: `"g-service-fee"`

User pays the $600 service fee.

**API Endpoints**:
- Get transaction: `GET /api/v1/transaction/:transactionId`
- Update transaction: `PUT /api/v1/transaction/:transactionId`
- Payment processing via Square

---

### 4. Checklist (Document Upload)
**Step Type**: `SUPPORTING_DOCUMENT`
**Step Slug**: `"fiancee-visa-checklist"`

Users upload supporting documents. This step includes **ID Photos** as part of the checklist.

**Required Documents**:
1. **Evidence of U.S. Legal Status of Petitioner**
   - U.S. passport, birth certificate, or naturalization certificate

2. **Beneficiary Passport**
   - Valid passport copy

3. **Beneficiary Birth Certificate**
   - Original or certified copy

4. **Proof of Termination of Prior Marriages**
   - Divorce decrees, death certificates, annulment papers (if applicable)

5. **Bona Fide Relationship Evidence**
   - Photographs together
   - Passport stamps and boarding passes
   - Hotel reservations
   - Chat logs/text messages
   - Phone records
   - Emails
   - Affidavits from friends/family
   - Joint bank accounts or financial support evidence
   - Engagement ring receipts
   - Wedding invitations
   - Wedding-related expense receipts

6. **Engagement or Wedding Plans**
   - Engagement ring receipts/invoices
   - Wedding invitations
   - Wedding-related expense receipts (venue, catering, etc.)

7. **ID Photos**
   - Passport-style photos for both petitioner and beneficiary

8. **Additional Documents**
   - Any other relevant supporting documents

**API Endpoints**:
- Get document templates: Check milestone's step resources
- Upload documents: `POST /api/v1/document-upload`
- Get uploaded documents: `GET /api/v1/document-upload/:documentId`

---

### 5. Review and Sign (with Filing Fee)
**Step Type**: `REVIEW_AND_SIGN`
**Step Slug**: `"fiancee-visa-review-and-sign-with-filing-fee"`

**Important**: This is a **combined step** that includes:
- Form review and signing
- **Filing fee payment** ($675)

Users must:
1. Review all completed forms
2. Sign the forms
3. Pay the USCIS filing fee ($675)

**API Endpoints**:
- Submit for review: `PUT /api/v1/milestone/:milestoneId` with status `"AWAITING_ADMIN"`
- Get filing fee transaction: Check for transaction associated with this milestone
- Update filing fee transaction: `PUT /api/v1/transaction/:transactionId`

---

### 6. Application Processed
**Step Type**: `APPLICATION_PROCESSED`
**Step Slug**: `"g-application-processed"`

Final step - application has been processed and submitted.

---

## API Integration Details

### Creating an Order

```typescript
POST /api/v1/order
{
  "service": "fiance-visa",
  "prerequisiteResponses": {
    "fv-prerequisite-a": "yes",  // Is petitioner a U.S. citizen?
    "fv-prerequisite-b": "yes"  // Have you met in person within the last two years?
  }
}
```

**Response**: Order object with process milestones

---

### Getting Process Steps

The order will contain milestones in this order:

```typescript
[
  {
    step: {
      slug: "fiancee-visa-questionnaire",
      type: "QUESTIONNAIRE",
      resources: [/* form IDs for i-129f-pet and i-129f-ben */]
    },
    status: "AWAITING_USER"
  },
  {
    step: {
      slug: "g-service-fee",
      type: "SERVICE_FEE"
    },
    status: "PENDING"
  },
  {
    step: {
      slug: "fiancee-visa-checklist",
      type: "SUPPORTING_DOCUMENT",
      resources: [/* document template IDs */]
    },
    status: "PENDING"
  },
  {
    step: {
      slug: "fiancee-visa-review-and-sign-with-filing-fee",
      type: "REVIEW_AND_SIGN"
    },
    status: "PENDING"
  },
  {
    step: {
      slug: "g-application-processed",
      type: "APPLICATION_PROCESSED"
    },
    status: "PENDING"
  }
]
```

---

### Form Submission

For the questionnaire step, you need to submit data for **both forms**:

```typescript
// Petitioner form submission
POST /api/v1/form-submission
{
  "milestone": "<milestone-id>",
  "section": "<section-id>",
  "responses": {
    // Form field responses
  }
}

// Beneficiary form submission
POST /api/v1/form-submission
{
  "milestone": "<milestone-id>",
  "section": "<section-id>",
  "responses": {
    // Form field responses
  }
}
```

**Note**: Both forms are part of the same questionnaire milestone, but they have different section IDs.

---

### Document Upload

For the checklist step:

```typescript
POST /api/v1/document-upload
{
  "milestone": "<milestone-id>",
  "documentTemplate": "<document-template-id>",
  "file": <File>
}
```

**Document Template Slugs**:
- `"fiance-visa-bona-fide-relationship-evidence"`
- `"fiance-visa-engagement-or-wedding-plans"`
- Plus standard document templates (passport, birth certificate, etc.)
- Plus ID photos template

---

### Form Download (Admin)

For downloading completed forms:

```typescript
POST /api/v1/admin/download-all-forms
{
  "orderId": "<order-id>",
  "service": "fiance-visa",
  "formArray": ["i-129f"]  // Will be auto-set to ["i-129f"]
}
```

Or for full application package:

```typescript
POST /api/v1/admin/download-application-package
{
  "orderId": "<order-id>",
  "service": "fiance-visa",
  "formArray": ["i-129f"]  // Will be auto-set to ["i-129f"]
}
```

---

## Key Differences from Other Services

### 1. Combined Questionnaire Step
- **Most services**: Separate questionnaire steps for petitioner and beneficiary
- **Fiancé Visa**: **Single combined questionnaire step** with both forms

### 2. ID Photos in Checklist
- **Most services**: Separate ID photos step
- **Fiancé Visa**: ID photos are **included in the checklist step**

### 3. Combined Review & Filing Fee
- **Most services**: Separate Review & Sign step and Filing Fee step
- **Fiancé Visa**: **Combined step** that includes both review/sign and filing fee payment

---

## Form Field IDs

### Prerequisite Fields
- `"fv-prerequisite-a"`: Is petitioner a U.S. citizen?
- `"fv-prerequisite-b"`: Have you met in person within the last two years?

### Form Sections (Petitioner)
- `"name-and-name-change"`: Name information
- `"other-information-i-129f-pet"`: Other petitioner information
- `"general-questions-i-129f-pet"`: General questions
- `"biographic-information"`: Biographic details
- `"five-year-addresses"`: Address history
- `"employment-history"`: Employment history
- `"fv-parent-information"`: Parent information
- `"fv-prior-marital-history"`: Prior marital history
- `"fv-children-pet"`: Children information (petitioner)

### Form Sections (Beneficiary)
- `"name-and-name-change"`: Name information
- `"other-information-i-129f-ben"`: Other beneficiary information
- `"fv-five-year-addresses-ben"`: Address history (beneficiary)
- `"fv-employment-history-ben"`: Employment history (beneficiary)
- `"fv-parent-information"`: Parent information
- `"fv-prior-marital-history"`: Prior marital history
- `"fv-children-ben"`: Children information (beneficiary)

---

## UI/UX Recommendations

### 1. Prerequisite Screen
- Show two clear questions with Yes/No radio buttons
- Display error messages immediately if user selects "No" for U.S. citizenship
- Show consultation prompt if user hasn't met in person

### 2. Questionnaire Screen
- **Important**: Show both forms in the same step
- Use tabs or accordion to switch between "Petitioner" and "Beneficiary" forms
- Or show them side-by-side if screen space allows
- Clearly indicate which form the user is currently filling
- Show progress: "Petitioner Form (1/2)" and "Beneficiary Form (2/2)"

### 3. Checklist Screen
- Group documents logically:
  - **Identity Documents** (Passport, Birth Certificate, ID Photos)
  - **Relationship Evidence** (Bona Fide Relationship, Engagement/Wedding Plans)
  - **Legal Documents** (Marriage Termination, U.S. Status)
  - **Additional Documents**
- Show upload progress for each document
- Allow multiple files for relationship evidence

### 4. Review & Sign Screen
- Show all completed forms for review
- Display filing fee amount prominently ($675)
- Integrate payment flow within the same screen
- Only allow submission after both:
  - Forms are signed
  - Filing fee is paid

---

## Error Handling

### Prerequisite Failures
- **Not U.S. Citizen**: Block service access, show ineligibility message
- **Haven't Met**: Show consultation requirement, allow booking

### Form Validation
- Ensure all required fields are completed for both forms
- Validate dates, addresses, and other format-specific fields
- Show clear error messages for each form separately

### Document Upload
- Validate file types (PDF, images)
- Check file sizes
- Show upload progress
- Handle upload failures gracefully

---

## Testing Checklist

- [ ] Prerequisite check works (U.S. citizen requirement)
- [ ] Prerequisite check works (met in person requirement)
- [ ] Combined questionnaire shows both forms
- [ ] Can submit data for both petitioner and beneficiary forms
- [ ] Service fee payment works
- [ ] All document types can be uploaded
- [ ] ID photos upload works (in checklist step)
- [ ] Review & Sign shows all forms
- [ ] Filing fee payment works (in Review & Sign step)
- [ ] Form download works for admin
- [ ] Application package download works

---

## Constants Reference

All constants are available in: `src/constants/fianceeVisa.ts`

You can import and use:
- `FIANCE_VISA_SERVICE` - Service-level constants
- `FIANCE_VISA_STEPS` - Process step constants
- `FIANCE_VISA_PREREQUISITE` - Prerequisite constants

---

## Support

For questions or issues, refer to:
- Backend API documentation
- Service definition: `src/db_seed/services/fianceeVisa.ts`
- Process steps: `src/db_seed/processSteps/fiancee.ts`
- Constants: `src/constants/fianceeVisa.ts`

# Fiancé Visa - Quick Reference for Frontend

## Service Slug
```
"fiance-visa"
```

## Process Steps (In Order)

1. **Prerequisite** → Check eligibility
2. **Questionnaire** → Combined step (both forms)
3. **Service Fee** → Pay $600
4. **Checklist** → Upload documents (includes ID photos)
5. **Review & Sign** → Review, sign, and pay filing fee ($675)
6. **Application Processed** → Final step

## Key Step Slugs

```typescript
const STEP_SLUGS = {
  QUESTIONNAIRE: "fiancee-visa-questionnaire",
  SERVICE_FEE: "g-service-fee",
  CHECKLIST: "fiancee-visa-checklist",
  REVIEW_AND_SIGN: "fiancee-visa-review-and-sign-with-filing-fee",
  APPLICATION_PROCESSED: "g-application-processed"
};
```

## Form Slugs

```typescript
const FORM_SLUGS = {
  PETITIONER: "i-129f-pet",
  BENEFICIARY: "i-129f-ben"
};
```

## Prerequisite Questions

```typescript
const PREREQUISITE = {
  US_CITIZEN: "fv-prerequisite-a",      // "Is petitioner a U.S. citizen?"
  MET_IN_PERSON: "fv-prerequisite-b"   // "Have you met in person within the last two years?"
};
```

## Document Template Slugs

```typescript
const DOCUMENTS = {
  BONA_FIDE: "fiance-visa-bona-fide-relationship-evidence",
  ENGAGEMENT: "fiance-visa-engagement-or-wedding-plans",
  // Plus standard documents: passport, birth certificate, ID photos, etc.
};
```

## Important Notes

⚠️ **Combined Questionnaire**: One step contains both petitioner and beneficiary forms

⚠️ **ID Photos in Checklist**: Not a separate step, included in document upload

⚠️ **Filing Fee in Review**: Combined with review & sign step, not separate

## API Endpoints

```typescript
// Create order
POST /api/v1/order
{ service: "fiance-visa", prerequisiteResponses: {...} }

// Submit form
POST /api/v1/form-submission
{ milestone, section, responses }

// Upload document
POST /api/v1/document-upload
{ milestone, documentTemplate, file }

// Submit for review (includes filing fee)
PUT /api/v1/milestone/:milestoneId
{ status: "AWAITING_ADMIN" }
```

## Pricing

- Service Fee: **$600** (Step 3)
- Filing Fee: **$675** (Step 5, combined with Review & Sign)


