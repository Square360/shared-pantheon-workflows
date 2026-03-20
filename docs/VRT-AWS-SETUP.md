# VRT AWS Setup Checklist

Everything that needs to be created/configured in AWS to support the Visual Regression Testing workflow.

---

## 1. S3 Bucket

- [x] Create bucket: `square360-vrt-reports` (or your preferred name)
- [x] Region: `us-east-1` (or set `AWS_S3_REGION` secret if different)
- [x] Block public access: **off** for the bucket (required for public report links)
- [x] Versioning: not required
- [x] Object ownership: ACLs disabled (use bucket policy for public access)

### Bucket Policy

Attach the following bucket policy to allow public read on reports and diffs only:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadReportsAndDiffs",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": [
        "arn:aws:s3:::square360-vrt-reports/*/report.html",
        "arn:aws:s3:::square360-vrt-reports/*/diff/*"
      ]
    }
  ]
}
```

> Live and RC screenshots are **not** public — only diffs and the HTML report are exposed.

---

## 2. IAM User

- [x] Create IAM user: `github-vrt-bot`
- [x] Access type: **Programmatic access only** (no console login needed)
- [x] Attach the following inline policy:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:PutObject",
        "s3:GetObject",
        "s3:ListBucket"
      ],
      "Resource": [
        "arn:aws:s3:::square360-vrt-reports",
        "arn:aws:s3:::square360-vrt-reports/*"
      ]
    }
  ]
}
```

- [x] Generate access key (Key ID + Secret) — save these immediately, the secret is shown once

---

## 3. GitHub Secrets

Store the following as **org-level** GitHub secrets (or repo-level if preferred):

| Secret name | Value |
|---|---|
| `AWS_ACCESS_KEY_ID` | Access key ID from the `github-vrt-bot` IAM user |
| `AWS_SECRET_ACCESS_KEY` | Secret access key from the `github-vrt-bot` IAM user |
| `AWS_S3_BUCKET` | Bucket name, e.g. `square360-vrt-reports` |
| `AWS_S3_REGION` | Region, e.g. `us-east-1` (optional — defaults to `us-east-1`) |

---

## 4. Verification

- [x] Upload a test file to the bucket using the IAM credentials to confirm write access:
  ```bash
  AWS_ACCESS_KEY_ID=xxx AWS_SECRET_ACCESS_KEY=yyy \
    aws s3 cp test.html s3://square360-vrt-reports/test/report.html
  ```
- [x] Confirm the file is publicly accessible at:
  `https://square360-vrt-reports.s3.amazonaws.com/test/report.html`
- [ ] Delete the test file when done (requires `s3:DeleteObject` permission — delete manually via AWS console)
