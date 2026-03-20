---
title: Concept
has_children: false
nav_order: 2
---

# Concept

Doggy is a web application with a graphical user interface that allows users to upload a dog photo and receive AI-based breed identification together with practical care recommendations.
The system works in a request/response flow: after an image is uploaded, the backend validates the input, runs dog detection and breed prediction models, generates advice, and returns the result.

From a use case perspective, the users are dog owners and dog lovers using a desktop or browser.
They interact with the app during short analysis sessions, mainly through standard web controls:

1. The user selects a local image file (any common image format is supported).
2. The system starts analysis and shows a loading state.
3. After processing, the app displays the detected breed, top predictions, and AI-generated recommendations.
4. If processing fails, the user gets an error message and can try again with another image.

The application does not rely on permanent user data.
No registration, login, or long-term user history is required.
Uploaded images are used only for the current analysis; temporary files are removed after processing, and results are session-based.

There are two main roles in the system:

- **User**: uploads dog photos and consumes the identification/advice output.
- **System**: handles image validation, dog presence check, breed classification, advice generation, error handling, and response delivery.
