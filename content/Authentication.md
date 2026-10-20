## Register with Google

**1-Google Sign-up Trigger:**
- The user taps the Google icon on the Register screen.
- The app communicates with Google servers to retrieve the user's `displayName`, `email`, and unique `id`.
- The full name is split into `firstName` and `lastName` to match backend requirements.

**2- Deterministic Secure Password Generation:**
- To solve backend validation limitations and security concerns with hardcoded secrets, the app merges the Google `User ID` with a secure app `Secret Key`. 
- The combined string is hashed using **SHA-256**, encoded to **Base64**, and truncated to the first **10 characters**.
- A fixed validation prefix (`SuperFit@2026#`) is prepended to form a robust 24-character password that satisfies all backend security criteria (Uppercase, lowercase, numbers, and special symbols).

**3- Silent Login Check (Existing vs. New User):**
- Before sending the user through the onboarding questionnaire, the app silently attempts to `Login` using the generated credentials.
- **Case A (200 OK):** The user already exists. The app saves the token and navigates directly to the `Home Screen`.
- **Case B (404 / 400 Error):** The user is new. The app stores the Google data and generated password in the `RegisterCubit` and routes them to the first onboarding screen (`Select Gender`).

**4-Complete Profile Onboarding (Real Data Collection):**
- The user goes through the multi-step questionnaire screens in sequence: (`gender` ➡️ `age` ➡️ `weight` ➡️ `height` ➡️ `goal` ➡️ `activityLevel`).
- The `RegisterCubit` dynamically updates and holds each selected value in its state.

**5- Final Registration Request:**
- Upon completion of the last step (`Activity Level`), tapping `Next` triggers the final API call.
- All data—including Google profile details, the deterministically generated password, and the user's physical attributes—is bundled into the `RegisterRequestBody` and sent to the `Register Endpoint`.    
- Upon success, the account is created, and the user enters the app.

![[Pasted image 20261020172500.png]]


