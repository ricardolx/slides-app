Slides app - intelligent querying of Google Slides presentations. After granting access to drive and slides through Google's OAuth API, your Presentation notes and content are saved into a vector DB (Pinecone). You can ask questions about your presentations without specifying which presentation file contains the information - AI finds the best document for the information.

## Getting Started

First, run the development server:

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) with your browser to see the result

#### Firebase authentication required for access to slides page

## Deploy on Firebase static hosting

- firebase deploy --only hosting

# Backend 

## Pine cone

- Get pinecone api key
- Using with npm or pip package
   - Create Index 

## serverless functions

in the /functions dir
- **getAuthentication** - Get custom google OAuth URL with user profile, drive, slides scopes
- **oauthRedirect** - Handle redirect from Google OAuth, create firebase token for webApp authentication
- **getSlides** - Hit the Drive/Slides api and extract notes and slide content, save to Firestore
- **questionDocument** - Ask a question to the AI about your slides
- **onPresentationWritten** - When a slide is saved from getSlides to firestore, automatically create vector and store in Pinecone

### Function ENV variables /functions.env
CLIENT_ID= Google OAuth Client ID

CLIENT_SECRET= Google OAuth Client Secret

REDIRECT_PAGE= The webapp landing page

HANDLER= OAuth Redirect Handler function

OPENAI_KEY=OpenAI Api Key

VECTOR_MODEL= Embedding vector model (OpenAI)

PINECONE_KEY= Pinecone API key

PINECONE_DOCUMENT_INDEX= Pinecone Vector Index

PINECONE_ENVIRONMENT= Pinecone cloud environment

## Deploy backend

- firebase deploy --only functions

### Services to enable in GCP
- Google Drive API
- Google Slides API
- Google Oauth API
- Google Functions
- Firebase Auth
- Firestore
  - Semantic Search API
#### GCP Configuration and Permissions
- App Engine Service Account - Service Account Token Creator to create custom tokens
- OAuth 2.0 Client ID
   - Configure origins and redirects from WebApp
##### Function Permissions and Settings
- GetAuthenticaion (Retrieve Login) - Invocable by anonymous
- OauthRedirect (Handle Redirect) - callable by https
- GetSlides (Retrieve Slides) - Invocable by authenticated user
   - May need more than base 256MB to process slides
- QuestionDocument - Invocable by authenticated user
- onPresentationWritten - Invocable on document written to firestore Presentations collection
##### Firestore Collections
- Users - contains user OAuth token and refresh token
- Slides - contains users slide presentation content
- Embeddings - contains presentation content embeddings
  
# Architecture

![ArchP](https://github.com/ricardolx/slides-app/assets/37557051/0e7d9ce4-ee4e-4cfd-a4bc-fca3eb256835)

