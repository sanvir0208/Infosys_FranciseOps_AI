# Franlytics - Multi-Unit Franchise Analytics Platform
### Infosys Springboard 7.0 Virtual Internship - Milestone 1

---

## Overview and Core Architecture
Franlytics is an enterprise-grade authentication and secure session tracking gateway engineered for distributed franchise business networks. This submission marks the complete delivery of Milestone 1, focusing entirely on the implementation of a reliable user access pipeline. This includes user onboarding, input validation defenses, JSON Web Token session security, and multi-factor credential recovery.

The core codebase isolates validation and authentication states to guarantee data integrity before any information is written to local persistent file structures.

---

## Technical Implementation

### 1. User Input Validation Engine
The platform implements rigid validation filters at the gateway to intercept malicious inputs, accidental keystrokes, and empty entries:
* Username Rules: Enforces a strict length requirement of 3 to 20 characters. It allows only alphanumeric characters and underscores, completely blocking special symbols.
* Password Requirements: To comply with enterprise security baselines, passwords must be at least 8 characters long and contain at least one uppercase letter, one lowercase letter, one number, and one special character.
* Spacebar Injection Filters: The framework screens incoming character inputs using structural checks. If a user enters only blank spaces into a username or password field, the system instantly flags the entry as an empty parameter and blocks the sequence.

### 2. JSON Web Tokens / JWT (Session Security)
Once user credentials pass validation checks, the platform avoids traditional, insecure cookie-based tracking and issues a cryptographically signed JSON Web Token (JWT) using the HS256 (HMAC-SHA256) signature algorithm.
* Mechanism: The authentication module generates a signed identity string containing the verified username and a strict expiration timestamp (2 hours). This token is monitored securely in the background. If a token is altered, missing, or expired, the session is terminated instantly and the user is redirected to the entry gate.
* Secrets Protection: The private signature seed (JWT_SECRET) is kept out of the codebase and managed entirely through environment variables to prevent source control leaks.

### 3. Forgot Password Framework and OTP Generation Engine
When a profile requires credential restoration, the application provides two isolated verification paths:
* Security Question Verification: Cross-checks answers against a low-case string map saved during signup. It includes a reuse filter that blocks users from changing a password back to the active one on file.
* Multi-Factor OTP Engine: Generates a single-use, non-deterministic 6-digit numeric verification code using random cryptographic integer parameters between 100000 and 999999.
* Delivery Protocol: Dispatches code payloads by initiating a secure network handshake via SMTP over SSL on port 465. This routes an enterprise HTML email template straight to the user's registered inbox.

---

## End-to-End User Interface Workflow and Screenshots

### 1. Secure Access Gate (Login View)
A minimalist, centered interface layout built using professional SaaS design choices. It features empty-field blocking alerts and real-time whitespace validation tracking.
The main Login interface card 
<img width="1644" height="737" alt="image" src="https://github.com/user-attachments/assets/afe30e0b-0538-4c34-9c46-6117144bd3e2" />
<img width="754" height="116" alt="image" src="https://github.com/user-attachments/assets/eab0dd11-5a9b-48e0-aaa7-62addeff91c4" />




---

### 2. Tenant Provisioning and Validation Constraints (Signup View)
The registration panel enforces strong edge-case checks to block bad data. Entering blank spaces instantly triggers an explicit alert.
Registration/Signup card view.
<img width="1421" height="702" alt="image" src="https://github.com/user-attachments/assets/d01cbd71-4899-47c9-81eb-1eea6ab06cc5" />
<img width="1175" height="681" alt="image" src="https://github.com/user-attachments/assets/957b9ee6-69c4-4b48-98f6-a0ba86ad0094" />

An explicit error alert handling an input edge case, such as a username blank space violation or password validation message.
<img width="1033" height="111" alt="image" src="https://github.com/user-attachments/assets/8d6334e2-d044-47ff-8b9e-b3e2e7467d07" />
<img width="1144" height="670" alt="image" src="https://github.com/user-attachments/assets/d59ffd65-967f-40db-b9e6-1f6f7669db78" />
<img width="1175" height="668" alt="image" src="https://github.com/user-attachments/assets/b195d7e7-0988-4164-afbb-d477aaed7e0d" />
<img width="1150" height="668" alt="image" src="https://github.com/user-attachments/assets/4f4f530c-d8e1-4cff-8a5e-6c0f9bb5ee95" />



---

### 3. Stateful Multifactor Recovery (Account Reset View)
The password reset flow offers a choice between a security question check or a 6-digit email OTP verify code. The OTP input field replaces the standard long text box with 6 individual, centered digit boxes.


The Password Recovery options screen. The layout after selecting Email OTP, aligned digit placeholder boxes.

Via Security Question
<img width="889" height="452" alt="image" src="https://github.com/user-attachments/assets/49dc93b0-393b-476f-9cb2-d45ae5e08780" />
<img width="1171" height="686" alt="image" src="https://github.com/user-attachments/assets/bca39492-570b-4212-a219-57f98037cc34" />

Via email otp
<img width="1058" height="658" alt="image" src="https://github.com/user-attachments/assets/1a4f49b9-52c5-49ad-8441-9a4254cc82d8" />
<img width="769" height="518" alt="image" src="https://github.com/user-attachments/assets/6e6f08c4-144f-469f-9009-8c2a258bd85e" />



---

### 4. Dynamic Workspace Dashboard Launch (Post-Authentication)
Once the login or recovery sequence clears successfully, the system reads the signed token payload and grants access to the dashboard.
The landing interface screen immediately after a successful login.
<img width="1356" height="574" alt="image" src="https://github.com/user-attachments/assets/a026cdf4-ca0d-4e46-8e7e-2fad9b5c5d91" />
<img width="362" height="464" alt="image" src="https://github.com/user-attachments/assets/7236c8be-3509-4907-9db4-d25b7db02685" />

