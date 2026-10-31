<div align="center">
<img width="1200" height="475" alt="GHBanner" src="https://ai.google.dev/static/site-assets/images/share-ais-513315318.png" />
</div>

# Run and deploy your AI Studio app

This contains everything you need to run your app locally.

View your app in AI Studio: https://ai.studio/apps/e4e3e40a-e2e3-4259-8f82-796e897ce1ed

## Run Locally

**Prerequisites:**  Node.js


1. Install dependencies:
   `npm install`
2. Set the `GEMINI_API_KEY` in [.env.local](.env.local) to your Gemini API key
3. Run the app:
   `npm run dev`

## Consultation recording and transcription

The doctor workspace includes a consent-gated consultation recorder. It records audio in the browser, allows playback, sends the audio to the Express backend only when the user selects **Transcribe with Google**, and lets the clinician review/correct the returned text before adding it to the existing reconciliation workflow.

### Configure Google Cloud Speech-to-Text

1. Enable the **Cloud Speech-to-Text API** in your Google Cloud project and configure billing/permissions as required.
2. Create a server-side API key restricted to the Speech-to-Text API (or replace the API-key implementation with service-account authentication for your deployment).
3. Set this environment variable on the backend host; do not put it in frontend code:

   `GOOGLE_CLOUD_SPEECH_API_KEY=your_google_cloud_speech_api_key`

4. Install the new dependencies with `npm install`, then run `npm run dev`.
5. Open the app on `https://` or `localhost`, give microphone permission, obtain consent from both speakers, record, stop, and select **Transcribe with Google**.

The endpoint requests Indian English (`en-IN`) with Hindi (`hi-IN`) as an alternative language and automatic punctuation. Recognition of code-switching/Hinglish depends on the speech, API configuration and Google model; review the transcript before using it clinically.

### Current limitations and patient safety

- The current implementation is **record first, then transcribe**; live streaming transcription is not implemented yet. The recorder has pause/resume and playback.
- Google batch transcription may reject recordings that exceed its synchronous audio/request limits. For long consultations, implement Google Cloud long-running transcription or streaming recognition before production use.
- Speaker diarization is not currently enabled, so the transcript is not guaranteed to label doctor vs patient.
- Patient/case records remain in-memory in the existing app. Authentication, patient-specific authorization, encrypted persistent storage, audit logging, retention/deletion controls and clinician approval gating are still required before handling real patient information.
- The consent checkboxes are a UI safeguard, not a legally sufficient consent-record management system. Follow applicable institutional policy and Indian privacy/health-data requirements.
