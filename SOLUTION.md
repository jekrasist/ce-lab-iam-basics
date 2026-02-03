# IAM Basics Lab - Solution

**Student Name:** [Ahmet Erdogan]  
**Date Completed:** [03.02.2026]

---

## Exercise 1: IAM Groups

### Screenshots:
![Groups Created](<img width="1469" height="512" alt="image" src="https://github.com/user-attachments/assets/1380f46b-ba7a-4341-a24e-6a310b788427" />
)

### Groups Created:
- [x] Developers group
- [x] DevOps group

---

## Exercise 2: Group Permissions

### Developers Group:
![Developers Permissions](<img width="1689" height="632" alt="image" src="https://github.com/user-attachments/assets/e324c1cc-7aaf-4ef1-b32a-d1bceb7a02ed" />
)

**Policies Attached:**
- AmazonS3FullAccess
- AmazonEC2ReadOnlyAccess

### DevOps Group:
![DevOps Permissions](<img width="1554" height="621" alt="image" src="https://github.com/user-attachments/assets/3d378852-936e-410a-9484-cdd0163f9489" />
)

**Policies Attached:**
- AmazonS3FullAccess
- AmazonEC2FullAccess

---

## Exercise 3: IAM Users

### Screenshots:
![Users Created](<img width="1485" height="518" alt="image" src="https://github.com/user-attachments/assets/4c5061cf-699c-49b5-a974-cc449740f3d4" />
)

### Users Created:

| Username | Group | Console Access | Status |
|----------|-------|----------------|--------|
| alice | Developers | Yes | ✅ Created |
| bob | Developers | Yes | ✅ Created |
| charlie | DevOps | Yes | ✅ Created |

---

## Exercise 4: Permission Testing

### Alice's Access Tests:

**S3 Access:**
![Alice S3 Access](<img width="1436" height="663" alt="image" src="https://github.com/user-attachments/assets/70ef7b7a-eef1-4d59-bd09-2c529ec0b895" />
)
- Create bucket: ✅ SUCCESS
- Upload file: ✅ SUCCESS

**EC2 Access:**
![Alice EC2 Read-Only](<img width="1919" height="790" alt="image" src="https://github.com/user-attachments/assets/79287255-16d0-4322-8e1b-456eacf568d3" />
)
- View instances: ✅ SUCCESS
- Launch instance: ❌ DENIED (Expected)

### Bob's Access Tests:

**S3 Access:**
![Bob S3 Access](<img width="1899" height="734" alt="image" src="https://github.com/user-attachments/assets/cd63b9d8-0241-4d60-bed0-c66da5f1ef25" />
)
- Create bucket: ✅ SUCCESS

**EC2 Access:**
![Bob EC2 Denied](<img width="1919" height="748" alt="image" src="https://github.com/user-attachments/assets/a157025b-f1cd-495d-aac5-418fec3094c0" />
)
- View instances: ✅ SUCCESS
- Launch instance: ❌ DENIED (Expected)

### Charlie's Access Tests:

**Full Access:**
![Charlie Full Access](<img width="1886" height="713" alt="image" src="https://github.com/user-attachments/assets/e5bbbadb-9e6e-4496-96ad-1d644fde0f27" />
)
- S3 create bucket: ✅ SUCCESS
- EC2 launch instance: ✅ SUCCESS

### Summary of Test Results:

| User | S3 Full | EC2 View | EC2 Launch | Result |
|------|---------|----------|------------|--------|
| alice | ✅ | ✅ | ❌ | As expected |
| bob | ✅ | ✅ | ❌ | As expected |
| charlie | ✅ | ✅ | ✅ | As expected |

---

## Exercise 5: Custom Policy

### Policy JSON:
![Custom Policy](screenshots/custom-policy.png)

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "ListAllBuckets",
            "Effect": "Allow",
            "Action": "s3:ListAllMyBuckets",
            "Resource": "*"
        },
        {
            "Sid": "DevBucketAccess",
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket",
                "s3:GetObject",
                "s3:PutObject",
                "s3:DeleteObject"
            ],
            "Resource": [
                "arn:aws:s3:::dev-bucket",
                "arn:aws:s3:::dev-bucket/*"
            ]
        }
    ]
}
```

### Custom Policy Test:
![Bob Custom Policy Test](<img width="1901" height="751" alt="image" src="https://github.com/user-attachments/assets/23c60667-0e2d-4344-aa53-bd70d68fdf96" />).


**Bob's Access After Custom Policy:**
- Access dev-bucket: ✅ SUCCESS
- Access other buckets: ❌ DENIED (Expected)

---

## Exercise 6: MFA Configuration

![MFA Enabled](<img width="1453" height="673" alt="image" src="https://github.com/user-attachments/assets/6bc0a92a-f7bd-4d1a-9997-7d09474f21d1" />
)

**MFA Details:**
- User: [alice / admin user]
- Device type: Virtual MFA
- Authenticator app: [Google Authenticator / Microsoft Authenticator / Authy]
- Status: ✅ Active

---

## Bonus Challenges

### Challenge 1: Password Policy

![Password Policy](screenshots/password-policy.png)

**Policy Settings:**
- [x] Minimum length: 12 characters
- [x] Require uppercase letters
- [x] Require lowercase letters
- [x] Require numbers
- [x] Require symbols
- [x] Password expiration: 90 days

---

### Challenge 2: Access Analyzer

![Access Analyzer](screenshots/access-analyzer.png)

**Findings:**
- Number of findings: [X]
- Critical issues: [List any public access found]
- Recommendations: [Your notes]

---

### Challenge 3: CLI Access Keys

**Alice Access Key Created:** [Yes / No]

**CLI Test Output:**
```bash
$ aws s3 ls --profile alice
[Paste output here]
```

**Screenshot:** [If applicable]

---

## Reflection Questions

### 1. Why use groups instead of attaching policies directly to users?

**Your Answer:*It is more organized this way.*

[Explain benefits: easier management, consistency, scalability, etc.]

---

### 2. What are the risks of giving everyone AdministratorAccess?

**Your Answer:*It is a huge security risk.*

[Discuss: security risks, accidental changes, compliance issues, etc.]

---

### 3. How would you organize IAM for 50 developers across 5 projects?

**Your Answer:*I would assign them to different groups based on their relevant projects and give them role based access to ensure maximum efficiency*

[Propose structure: project-based groups, role-based access, tagging strategy, etc.]

---

### 4. What happens if you delete an IAM user? Can you recover their permissions?

**Your Answer:*No, but you can always create a new user with the same permissions.*

[Explain: user deletion is permanent, permissions can be recreated but history lost, etc.]

---

## Key Learnings

**What was most challenging about this lab?**

[I had connection issues.]

---

**What IAM best practice will you always follow?**

[Being careful with who I assign as admin.]

---

**How does IAM help implement the principle of least privilege?**

[It helps the owner give developer the needed access but nothing more than that.]

---

## Checklist

- [ ] All 3 users created (alice, bob, charlie)
- [ ] Both groups created (Developers, DevOps)
- [ ] Permissions tested for each user
- [ ] Custom policy created and tested
- [ ] MFA enabled for at least one user
- [ ] All screenshots captured
- [ ] All reflection questions answered
- [ ] Policy JSON file saved
- [ ] Work committed to Git
- [ ] Pull request created

---

**Completed By:** [Ahmet Erdogan]  
**Date:** [03.02.2026]
