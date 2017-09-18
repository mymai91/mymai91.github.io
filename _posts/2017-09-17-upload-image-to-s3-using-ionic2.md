---
layout: post
title:  "[Ionic2] How to upload a file to S3 bucket from ionic 2 application?"
comments: true
date:   2017-09-17 15:07:33 +0700
categories: ionic
---

I backed with the topic so hot on Ionic2 forum. <b>How to upload a file to S3 bucket from ionic 2 application?</b>

To upload Photo to s3, need to config from 2 side (backend & frontend). In this topic I only talk about frontend.

### What infos we need config to upload ?
To upload from frontend. Backend serve should define for you API to get s3Signature.

This is params what s3 need:

```
key: [key value is configured from frontend],
AWSAccessKeyId: [AWSAccessKeyId value received from backend],
acl: [acl value received from backend],
policy: [policy value received from backend],
signature: [signature value received from backend],
```

Ionic2 provide `FileTransfer` to upload image. To use it should install this plugin

```
$ ionic cordova plugin add cordova-plugin-file-transfer
$ npm install --save @ionic-native/file-transfer
```

In this topic I suggest we should create a service for upload. (it mean create new provider for upload)

```
ionic g provider UploadService
```

In the uploadService provider. Defined the code like below. Should *read* code as well

```
import { Inject, Injectable } from '@angular/core';
import { Http, RequestOptions, Headers } from '@angular/http';
import { FileTransfer, FileUploadOptions, FileTransferObject } from '@ionic-native/file-transfer';
import { File } from '@ionic-native/file';

@Injectable()
export class UploadServiceProvider {
  apiUrl = `${your host to get api}`
  constructor(public http: Http, @Inject(EnvVariables) public envVariables, private transfer: FileTransfer, private file: File) {
  }

  //config S3 params
  s3UploadConfig(file, s3Params) {
    return{
      url: s3Params.bucket_name,
      method: 'POST',
      chunkedMode: false,
      headers: {
        connection: "close"
      },
      params : {
        key: `uploads/${file.substr(file.lastIndexOf('/')+1)}`,
        AWSAccessKeyId: s3Params.key,
        acl: s3Params.acl,
        policy: s3Params.policy,
        signature: s3Params.signature,
        'Content-Type' : "image/jpeg"
      }
    };
  }

  // Get Signature
  generateSignature(token) {
    const headers: Headers = new Headers();
    headers.append('Content-Type', 'application/x-www-form-urlencoded');
    headers.append('Accept', 'application/json');
    headers.append('Authorization-Token', token);

    const options: RequestOptions = new RequestOptions();
    options.headers = headers;
    // Call API to get Signature
    return this.http.get(`${this.apiUrl}/generate-signature`, options)
  }

  // Upload Image to s3
  upload(file,token): Promise<any>{
    return new Promise((resolve, reject) => {
      this.generateSignature(token)
        .map(response => response.json().data)
        .subscribe(
          response => {
            let s3Params = response;
            let serveConfig = this.s3UploadConfig(file, s3Params);
            let key = `uploads/${file.substr(file.lastIndexOf('/')+1)}`;
            const fileTransfer: FileTransferObject = this.transfer.create();

            fileTransfer.upload(file, encodeURI(s3Params.bucket_name), serveConfig)
              .then((result) => {
                // when finished upload photo. S3 will return a link of image.
                // This link is combined from `s3Params.bucket_name + key`
                resolve(s3Params.bucket_name + key);
              }, (error) => {
                resolve(error.json());
              });
          });
    });
  }

}
```

Take a look code in above. You can see what I defined:

- <b>(1)</b> Function config S3 params

- <b>(2)</b> Function to call API `generate-signature` to get Signature.

- <b>(3)</b> Function for upload photo on S3.

<b>(1) & (2)</b> quite easy to understand, isn't it? (if not put your comments, i will help)

* At (3) Why I returned Promise?

    - We use service to upload and when you call it in controller or another service etc. you possible get promise as well.


### Alright, Done. Above is the way to you upload photo via S3.

*Read comment code to understand more*

^___^

Jany Mai
