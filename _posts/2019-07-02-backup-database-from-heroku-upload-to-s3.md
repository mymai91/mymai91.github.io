---
layout: post
title:  "[Heroku-S3] How to backup Heroku postgres database and upload to AWS S3"
comments: true
date:   2018-03-12 15:07:33 +0700
categories: Heroku-S3
---

# How to backup Heroku postgres database and upload to AWS S3

Imaging that you have a requirement need to backup database hourly. This post will help you figure out how to do step by step.

Let make the DB dump from heroku and upload to s3.

## Step 1: Create bucket name on S3. 
For my perspective I think we should create new user only use for backup-db then create new bucket and set the policy with private value after that

### Create new user

<img width="919" alt="IAM_Management_Console" src="https://user-images.githubusercontent.com/6791942/60442976-cb135480-9c4c-11e9-86e2-477cf428afca.png">


<img width="1402" alt="IAM_Management_Console" src="https://user-images.githubusercontent.com/6791942/60443127-22b1c000-9c4d-11e9-96b0-6d4a7f1dd6ef.png">


<img width="1220" alt="IAM_Management_Console" src="https://user-images.githubusercontent.com/6791942/60443354-a9ff3380-9c4d-11e9-86fd-c2d6a2ad58f0.png">


<img width="1050" alt="IAM_Management_Console" src="https://user-images.githubusercontent.com/6791942/60443456-e0d54980-9c4d-11e9-9c50-7812c04eeb02.png">


<img width="1023" alt="IAM_Management_Console" src="https://user-images.githubusercontent.com/6791942/60443565-1ed26d80-9c4e-11e9-95cc-6baf452d5b44.png">

### Create bucket

<img width="2252" alt="S3_Management_Console" src="https://user-images.githubusercontent.com/6791942/60444764-678b2600-9c50-11e9-857a-bd3f168c57b0.png">

<img width="1892" alt="S3_Management_Console" src="https://user-images.githubusercontent.com/6791942/60444854-96090100-9c50-11e9-8c4a-12b8c20f1e18.png">

<img width="1599" alt="S3_Management_Console" src="https://user-images.githubusercontent.com/6791942/60444906-b042df00-9c50-11e9-8e8e-20e7e37de282.png">

<img width="1354" alt="S3_Management_Console" src="https://user-images.githubusercontent.com/6791942/60444968-d2d4f800-9c50-11e9-974e-16c2ad77f42a.png">

<img width="1725" alt="AWS_Policy_Generator" src="https://user-images.githubusercontent.com/6791942/60445452-cf8e3c00-9c51-11e9-8554-908414ba73bf.png">

<img width="1512" alt="AWS_Policy_Generator" src="https://user-images.githubusercontent.com/6791942/60445550-02d0cb00-9c52-11e9-93f5-9fee6d0b55f7.png">

If you don't know how to get user_name and account_id follow the way bellow

<img width="817" alt="IAM_Management_Console" src="https://user-images.githubusercontent.com/6791942/60445790-812d6d00-9c52-11e9-85bd-4072320b8651.png">


Or you can use this structure and change your ACCOUNT_ID and USERNAME_A

```
{
    "Version": "2012-10-17",
    "Id": "Policy1561992140151",
    "Statement": [
        {
            "Sid": "Stmt1561992110140",
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::ACCOUNT_ID:user/USERNAME_A"
            },
            "Action": "s3:*",
            "Resource": "arn:aws:s3:::bucket-dump-db"
        }
    ]
}
```

## Step 2: Get keys to add into Heroku
we get S3 package, S3 secket key  and add inside Heroku

APP_NAME="your-app-name"
PG_BACKUP_PASSWORD="password-use-for-encrypted"
BACKUP_S3_BUCKET="s3-bucket-name"
BACKUP_S3_KEY="s3-key"
BACKUP_S3_SECRET="s3-secret"


<img width="1427" alt="quiet-hamlet-85721_·_Settings___Heroku" src="https://user-images.githubusercontent.com/6791942/60446237-8b9c3680-9c53-11e9-9eb8-c535f963c0b6.png">


## Step 3: From Heroku create Heroku Scheduler


<img width="1515" alt="quiet-hamlet-85721_·_Resources___Heroku" src="https://user-images.githubusercontent.com/6791942/60446415-e6ce2900-9c53-11e9-844d-f24ac58b6e2c.png">


<img width="581" alt="2019-07-02-backup-database-from-heroku-upload-to-s3_md_—_mymai91_github_io" src="https://user-images.githubusercontent.com/6791942/60446584-44627580-9c54-11e9-8220-e85d1071da0b.png">

## Step 4: The most important create shell script to dump and upload Debug


If you are using MacOS, you need to install `$ brew install -v gpg`

We will use GPG to encrypted the pg backup ( Add one more layer for security) 

From the terminal run `touch bin/pg_backup_to_s3` to create shell script file to write the script to dump and upload database on s3
```
set -eu
set -o pipefail

#!/bin/sh

# Download the latest backup from
# Heroku and gzip it

heroku pg:backups:download --output=/tmp/pg_backup.dump --app $APP_NAME
gzip /tmp/pg_backup.dump

# Encrypt the gzipped backup file
# using GPG passphrase

gpg --yes --batch --passphrase=$PG_BACKUP_PASSWORD -c /tmp/pg_backup.dump.gz

# Remove the plaintext backup file

rm /tmp/pg_backup.dump.gz

# Generate backup filename based
# With the timestamp on the current date

BACKUP_FILE_NAME="heroku-backup-$(date '+%Y-%m-%d_%H.%M').gpg"
# BACKUP_FILE_NAME="heroku-backup-$(date '+%Y-%m-%d_%H.%M').dump"

# Make sure to use the UTC
# date for S3 signature!

DATE=`date -R -u`

S3_PATH="${BACKUP_S3_BUCKET}/${BACKUP_FILE_NAME}"

# Generate S3 signature needed
# to upload file to the bucket

MD5="$(openssl md5 -binary < "/tmp/pg_backup.dump.gz.gpg" | base64)"
CONTENT_TYPE="application/octet-stream"
S3_STRING="PUT\n$MD5\napplication/octet-stream\n${DATE}\n${S3_PATH}"

S3_SIGNATURE="$(printf "PUT\n$MD5\n$CONTENT_TYPE\n$DATE\n/$S3_PATH" | openssl sha1 -binary -hmac "$BACKUP_S3_SECRET" | base64)"
# Upload the file to S3 using
# the signature auth header

curl -X PUT -T "/tmp/pg_backup.dump.gz.gpg" \
  -H "Host: ${BACKUP_S3_BUCKET}.s3-ap-southeast-1.amazonaws.com" \
  -H "Date: ${DATE}" \
  -H "Content-Type: application/octet-stream" \
  -H "Content-MD5: $MD5" \
  -H "Authorization: AWS ${BACKUP_S3_KEY}:${S3_SIGNATURE}" \
  https://${BACKUP_S3_BUCKET}.s3-ap-southeast-1.amazonaws.com/${BACKUP_FILE_NAME}

# Remove the encrypted backup file

rm /tmp/pg_backup.dump.gz.gpg
```

## Step 5: Test and try to download dump DB and restore in your app.

Go to S3 download database is stored then unzip the file and descrypt DB dump

```
gpg --passphrase [PG_BACKUP_PASSWORD] --output database.zip -d [path download database]
```

Go to `database.yml` to get database info

pg_restore --verbose --clean --no-acl --no-owner -h localhost -U [pg user] -d [DB name][db_dump]

Example

```
pg_restore --verbose --clean --no-acl --no-owner -h localhost -U developer -d demo-rails_development database-dump
```


^___^

Jany Mai
