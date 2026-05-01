# AI Vishing Simulation

An advanced, voice-enabled social engineering simulation environment designed for Security Awareness Training. This tool allows security professionals to demonstrate how modern AI can be weaponized to conduct high-pressure Vishing (Voice Phishing) attacks using real-time voice synthesis and adaptive LLM reasoning.

## Prerequisites

Before running the simulation, ensure you have the following accounts and API keys configured:

### 1. Google Services
*   **Google AI Studio Key:** Required for the Gemini 2.5 Flash model. Obtain it at aistudio.google.com.
*   **Google Drive:** An account to store the session transcripts. The script will request to mount your drive at runtime.

### 2. ElevenLabs
*   **API Key:** Required for high-fidelity voice synthesis. Sign up at elevenlabs.io.
*   **Voice ID:** The script defaults to a professional persona. Ensure your account has access to the "Adam" voice or update the VOICE_ID in the code.

### 3. Colab Configuration
*   In your Google Colab notebook, click the **Secrets** (key icon) tab and add:
    *   GEMINI_API_KEY
    *   ELEVENLABS_API_KEY
*   **Important:** Toggle "Notebook Access" to **ON** for both secrets.

## System Context Overview

The simulation operates as a linear processing pipeline. To maintain the realism of a phone call, each stage is optimized for low latency to ensure the AI "hears" and "responds" without suspicious delays.



### The Data Journey
1.  **Audio Capture (The Ear):** The browser’s Web Speech API captures raw analog audio from the microphone.
2.  **Speech-to-Text (STT):** The browser converts that audio into a text string locally. This bypasses the need for high-bandwidth audio file uploads, significantly reducing lag.
3.  **Contextual Prompting (The Brain):** The text is injected into a persistent chat session with Gemini 2.5 Flash. System instructions force the AI to maintain its persona as an urgent IT specialist.
4.  **Speech Synthesis (The Voice):** The AI’s text response is sent to ElevenLabs. They return a high-fidelity digital audio stream.
5.  **Output & Persistence:** The audio plays automatically in the browser while the interaction is simultaneously logged and saved to Google Drive.

---

## Technical Function Guide

### drive.mount()
Establishes a secure connection to your personal Google Drive. This is critical because Colab environments are ephemeral; mounting Drive ensures that your training logs are saved permanently to /MyDrive/vishing_transcript.txt.

### start_browser_stt()
Since Python cannot directly access your laptop's microphone through a web browser, this function uses a JavaScript bridge to the browser's native Speech Recognition API. It includes a Watchdog Timer (6 seconds) to ensure that if the mic doesn't hear a clear sentence, it automatically resets the UI.

### process_interaction(message)
The central routing hub. It manages the is_processing state, ensuring the user cannot accidentally trigger multiple AI responses at once. It coordinates the handoff between the Gemini API and the ElevenLabs API.

### eleven_client.text_to_speech.convert()
This function communicates with the ElevenLabs ultra-low-latency model (eleven_flash_v2_5). It handles the conversion of text into a specific voice profile to provide a consistent, believable attacker persona.

### save_log()
The finalization function. When the user types 'exit', this function flushes the conversation buffer and writes the entire timestamped conversation to a text file in your mounted Google Drive for later review.

---

## Defending Against Social Engineering

This lab is designed to teach employees to recognize Vishing Red Flags:
*   **Spoofed Identity:** Realizing that a professional-sounding caller claiming to be from IT can be entirely computer-generated.
*   **Urgency Bias:** Recognizing when an attacker uses fear (e.g., "Your account has been compromised!") to make you bypass standard security protocols.
*   **Verification Protocol:** Teaching the "Hang Up and Call Back" rule. Never trust an incoming call; always hang up and call the official, verified company helpdesk number from the corporate directory.

---

## Disclaimer
**FOR EDUCATIONAL AND RESEARCH PURPOSES ONLY.**
This code is intended strictly for authorized security awareness training and academic research. Unauthorized use of this tool to deceive, defraud, or harm individuals or organizations is strictly prohibited and carries severe legal consequences. Always obtain explicit written consent before performing social engineering simulations on others.
