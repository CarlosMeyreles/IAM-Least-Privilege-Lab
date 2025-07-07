# IAM-Least-Privilege-Lab

## Objective
This project demonstrates how to implement the principle of least privilege in AWS using IAM roles, custom policies, and live testing via EC2. The goal was to create narrowly scoped permissions allowing upload access to a single S3 bucket — and validate the setup through real-world testing on an EC2 instance.

This hands-on lab showcases key AWS Identity and Access Management (IAM) practices in action, moving beyond simulations by confirming behavior in a live environment.

### Skills Learned
- Understanding of IAM users, roles, and trust relationships
- Creation and application of scoped-down custom IAM policies
- Validation of default deny behavior and role permissions
- Use of the AWS CLI to confirm permissions in a real-world EC2 environment
- Familiarity with troubleshooting IAM policy simulation limitations

### Tools Used
- AWS IAM — to manage users, roles, and policies
- Amazon S3 — as the resource to restrict access to
- AWS EC2 — to test role-based access in a live cloud instance
- AWS CLI — to simulate actions from inside the EC2 instance
- AWS Policy Simulator — to attempt validation of permissions (with limitations noted)

## Steps ##

**Step 1: IAM Users Created**

Created two IAM users:

- `AdminUser` (granted temporary `AdministratorAccess`)  
  ![S2-Screenshot User list showing both users created](https://github.com/user-attachments/assets/fa4111d4-131d-4bc1-aed9-599e226f1282)

- `ReadOnlyUser` (granted AWS managed `ReadOnlyAccess`)  
   ![S2-ReaderOnlyUser Permission SC](https://github.com/user-attachments/assets/608ea1e3-5402-46ac-a1f6-0f526ac8e33a)



|

**Step 2: IAM Roles Created**

Created two IAM roles:
- `EC2S3ReadOnlyRole` — attached to AWS managed policy `AmazonS3ReadOnlyAccess`  
  ![S3-EC2S3ReadOnlyRole- Role with attached policy listed](https://github.com/user-attachments/assets/bbfe70e1-b23c-4cb4-bd66-784c6f711714)

- `CustomS3Uploader` — an empty role created for scoped upload access 
  ![S3-CustomS3Uploader-Empty role for testing scoped custom S3 upload policy](https://github.com/user-attachments/assets/dd4d3d3d-19e3-4c1a-ad7d-80a8bd1f85a4)

|

**Step 3: Custom Scoped-Down Policy Created**
- Created `carlos-iam-lab-bucket` as the target resource for the scoped IAM policy.
  ![S4 - Bucket creation confirmation screen](https://github.com/user-attachments/assets/62723a7c-b6d6-44fd-b786-6a81e3119e93)

- Defined a policy that allows only `s3:PutObject` to a single bucket.  
   ![2025-07-07_12-25-32](https://github.com/user-attachments/assets/84cdd444-c1ac-41da-8b72-2048785f8359)


|

**Step 4: Attached Policy to Role**


- Attached the `ScopedS3UploaderPolicy` to the `CustomS3Uploader` role. 
  ![S4-Role summary now showing attached ScopedS3UploaderPolicy](https://github.com/user-attachments/assets/8bc9ebbc-9cbb-4ef3-a95f-365e15e0699a)

|

**Step 5: Verified Default Deny Behavior**
Logged in as `ReadOnlyUser` and attempted unauthorized actions (like uploading to S3), which failed due to denied permissions.

- Confirmed I was logged in as a scoped IAM user (`ReadOnlyUser`)  
  ![S5-Logged-in console as ReadOnlyUser](https://github.com/user-attachments/assets/797770a3-0975-4398-a3a9-f061926c0a85)

- Attempted to upload to an S3 bucket and received an “Access Denied” error, demonstrating that permissions are denied unless explicitly granted.
  ![S5-Access Denied or unauthorized action screen](https://github.com/user-attachments/assets/ae3629ca-36af-4ead-a2c8-5e14d325d22e)


|

**Step 6: Simulated Role Permissions (Policy Simulator Limitations)**
- Used AWS Policy Simulator to test permissions for the `ReadOnlyUser` IAM user. Confirmed that certain actions like GetObject and ListBucket were allowed, while others like PutObject and DeleteObject were denied — demonstrating default deny behavior.  
  ![S6-Table showing which actions are allowed vs denied-ReadOnlyAccess](https://github.com/user-attachments/assets/b2b6aafd-8752-400c-9031-26cf2429bcc9)

|


**Step 7: Validated Role in a Live EC2 Environment**
Launched an EC2 instance with the `CustomS3Uploader` role attached, then confirmed identity and tested upload via the AWS CLI.

- Launched an EC2 instance with the CustomS3Uploader role attached.
  ![S6-ec2_instance_with_role png](https://github.com/user-attachments/assets/8fe37ed9-ac95-40ec-a98d-c4f6f5056bf1)

- Ran aws sts get-caller-identity to confirm the EC2 instance assumed the correct role (CustomS3Uploader), not using root or user credentials.
  ![S6-screenshotssts_caller_identity png](https://github.com/user-attachments/assets/4920e43f-048e-42ab-a8c9-2f41aebfdb17)

- Successfully uploaded test-upload.txt to the bucket, proving that the scoped s3:PutObject permission worked exactly as intended.
  ![S6-upload_success png](https://github.com/user-attachments/assets/d36fd8af-c6ed-4af0-8ac4-c9c9e759d7be)



## Project Summary

This project successfully demonstrated the implementation of least privilege principles using AWS IAM. Through a combination of custom scoped-down policies and live testing on an EC2 instance, I was able to:

- Show how IAM roles and trust policies control access  
- Confirm that IAM permissions default to "deny" unless explicitly granted  
- Work around the limitations of AWS Policy Simulator by testing policies live  
- Validate scoped `s3:PutObject` permissions in a real-world environment  

By designing, deploying, troubleshooting, and verifying IAM configurations from start to finish, I gained practical experience that directly applies to real cloud security operations.


