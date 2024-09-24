# Guide to Implement Strong Password and Account Lockout Policy Using GPO on Windows Server 2022

## Prerequisites
- Windows Server 2022 installed and configured as a domain controller.
- Administrator privileges to manage Group Policy.

## Step 1: Access Group Policy Management
1. Open Group Policy Management:
   - Press Windows + R, type `gpmc.msc`, and press Enter to open the Group Policy Management Console (GPMC).
2. Expand Your Domain:
   - In the GPMC window, expand your domain by clicking on the arrow next to it.

## Step 2: Create a New GPO for Password and Account Lockout Policy
1. Create a New GPO:
   - Right-click on the Group Policy Objects folder and select New.
   - Name the new policy (e.g., "Password and Lockout Policy").
2. Link the New GPO:
   - Right-click your domain (or a specific Organizational Unit (OU) if you want to apply it to specific users or computers) and select Link an Existing GPO.
   - Choose the GPO you just created.

## Step 3: Configure Strong Password Policy
1. Edit the GPO:
   - Right-click the new GPO (e.g., "Password and Lockout Policy") and select Edit.
2. Navigate to Password Policy:
   - In the Group Policy Management Editor, go to:
     ```
     Computer Configuration > Policies > Windows Settings > Security Settings > Account Policies > Password Policy
     ```
3. Set the Password Policy Options:
   - Double-click each setting to configure:
     - Enforce Password History: Set this to 24 passwords remembered (to prevent reuse of previous passwords).
     - Maximum Password Age: Set this to 60 days (users must change their password every 60 days).
     - Minimum Password Age: Set this to 1 day (users can change their password only once per day).
     - Minimum Password Length: Set this to 12 characters (to enforce longer, more secure passwords).
     - Password must meet complexity requirements: Set this to Enabled (this requires the password to include at least three of the following: uppercase, lowercase, numbers, and special characters).
     - Store passwords using reversible encryption: Set this to Disabled (to avoid weaker encryption storage).

## Step 4: Configure Account Lockout Policy
1. Navigate to Account Lockout Policy:
   - Still in the Group Policy Management Editor, go to:
     ```
     Computer Configuration > Policies > Windows Settings > Security Settings > Account Policies > Account Lockout Policy
     ```
2. Set the Account Lockout Policy Options:
   - Double-click each setting to configure:
     - Account Lockout Duration: Set this to 15 minutes (how long the account will remain locked).
     - Account Lockout Threshold: Set this to 5 invalid login attempts (how many failed attempts before the account is locked).
     - Reset Account Lockout Counter After: Set this to 15 minutes (the failed attempts counter resets after this time period).

## Step 5: Apply and Test the Policy
1. Apply the Policy:
   - Close the Group Policy Management Editor and the Group Policy Management Console.
   - Open Command Prompt and run the following command to force a Group Policy update:
     ```
     gpupdate /force
     ```
2. Test the Policy:
   - To test the password policy, attempt to change the password of a user account to one that doesn't meet the complexity requirements. You should see an error message.
   - To test the account lockout policy, attempt several failed login attempts (based on the threshold you set) until the account is locked.

## Additional Notes
- The password and lockout policies will apply to all domain users unless specific OUs are targeted.
- If you want to create more granular password policies for different groups of users, you can use Fine-Grained Password Policies via Active Directory Administrative Center.

## Best Practices
- Regularly review and adjust the password policy to align with security requirements.
- Encourage users to use Multi-Factor Authentication (MFA) for additional security.
- Use strong password generators and password managers to assist users with managing complex passwords.
