# Active-Directory-Lab-Account-Lockouts-Security-Logs

This lab demonstrates how Active Directory handles account lockouts, password resets, enabling/disabling accounts, and event logging across a Domain Controller and Client machine.

## Lab Objectives
  - Configure Group Policy for account lockouts.
  - Simulate brute force login attempts.
  - Practice unlocking & resetting accounts.
  - Explore disabling & enabling accounts.
  - Review Windows Event Logs on DC and Client machines.

## Lab Setup
  - Domain Controller (DC-1): Windows Server 2022
  - Client-1: Windows 10, joined to domain mydomain.com
  - Users: Created in previous lab under _EMPLOYEES OU

### Part 1: Simulating Account Lockouts    

1. Simulating Account Lockouts
  - Log into DC-1.
  - Pick a random user from _EMPLOYEES (e.g., alice.smith).
  - On Client-1, attempt to log in 10 times with the wrong password.
    
2. Configure Group Policy
  - On DC-1, open Group Policy Management.
  - Edit Default Domain Policy → Account Policies → Account Lockout Policy.
  - Set:
    - Account lockout threshold: 5 invalid attempts
    - Account lockout duration: 15 minutes (or custom)
    - Reset account lockout counter after: 15 minutes
    - 
      <img width="1406" height="620" alt="image" src="https://github.com/user-attachments/assets/4578f6f7-1487-45f9-b825-783bc7d9dde1" />

  - Run:
    - gpupdate /force
      
      <img width="896" height="180" alt="image" src="https://github.com/user-attachments/assets/e80918d6-9fca-4fe7-8cb5-4ccc34691a82" />

      
3. Test Account Lockout
  - On Client-1, attempt login with bad password 6 times.
  - Observe: Account is locked out.
    
    <img width="241" height="282" alt="image" src="https://github.com/user-attachments/assets/ef2c7af6-650c-4cb7-922a-b9c3805917a3" />

  - On DC-1, open Active Directory Users and Computers (ADUC) → User account will show as locked.

    <img width="372" height="500" alt="image" src="https://github.com/user-attachments/assets/d2a437c5-47ea-4cf5-a96c-00f7afabd349" />


### Part 2: Unlocking & Resetting Passwords
1. In ADUC on DC-1:
   - Right-click locked account → Properties → Account.
   - Uncheck Account is locked out.
   - Reset password to a known value.

2. Attempt login on Client-1 with the new password → Success


### Part 3: Enabling & Disabling Accounts
1. In ADUC, Disable the same account.

   <img width="468" height="447" alt="image" src="https://github.com/user-attachments/assets/0dccba15-6069-434b-83da-eb96b1350ddf" />

2. Attempt login on Client-1 → Observe error “Your account has been disabled. Please see your system administrator.”

   <img width="240" height="254" alt="image" src="https://github.com/user-attachments/assets/2dc50482-c261-43f9-aab3-1d1eaea3ca37" />

3. Re-enable account.
4. Attempt login again → Successful.


### Part 4: Observing Logs
**On Domain Controller (DC-1)**
1. Open Event Viewer → Windows Logs → Security.
2. Look for:
   - Event ID 4740 → Account locked out
   - Event ID 4725 → User account disabled
   - Event ID 4722 → User account enabled
   - Event ID 4723 / 4724 → Password reset

**On Client-1**
1. Open Event Viewer → Windows Logs → Security.
2. Look for failed login attempts:
   - Event ID 4625 → Failed login

     <img width="942" height="306" alt="image" src="https://github.com/user-attachments/assets/fc20370a-aa61-47a4-b73f-8e3e5c458d57" />

   - Event ID 4624 → Successful login

     <img width="955" height="304" alt="image" src="https://github.com/user-attachments/assets/1f686a76-cebf-4480-a72a-8a0db127de24" />
