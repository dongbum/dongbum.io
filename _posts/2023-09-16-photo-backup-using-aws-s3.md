---
title: AWS S3로 동기화 스크립트
date: 2023-09-16T00:32:36+09:00
categories:
  - AWS
tags:
  - Amazon
  - AWS
  - S3
  - Cloud
---

NAS로 가족사진을 백업하며 사진들을 날려버리지는 않을까 항상 조마조마한 마음이 있었다. AWS S3에 가족들의 사진을 백업하게 되면서 이 불안함이 많이 사라졌다.

S3 에 백업하려면 기본적인 S3 사용법을 알아야한다. 버킷의 개념... 등등.

항상 NAS의 사진 디렉토리와 S3의 버킷이 동기화할 수 있도록 다음의 스크립트를 작성해서 생각날 때마다 한번씩 돌려주고 있다. (NAS는 리눅스 서버이다.)

### 기본 스크립트

```
aws s3 sync /disk3/BackupTargetDirectory s3://backup-bucket \
        --storage-class STANDARD_IA \
        --exclude "*" \
        --include "backup1/*" \
        --include "backup2/*" \
        --include "backup3/*"
```

이 스크립트의 가장 큰 목적은, 디렉토리내에 백업할 여러 자료들이 섞여있기 때문에 디렉토리내의 모든 디렉토리를 제외하고 백업하고자하는 특정디렉토리만을 선정하여 백업한다는 것이다.

### 저장 수준

자료들의 저장수준은 --storage-class 라는 옵션을 통하여 지정할 수 있다.

storage class 옵션에 대한 설명은 아래 참고자료의 링크에 있다.
한글페이지에는 아직 이 내용이 없는듯하다. 아래 링크의 메뉴얼에 대부분 옵션에 대한 설명이 되어있다.

* STANDARD
* REDUCED_REDUNDANCY
* STANDARD_IA
* ONEZONE_IA
* INTELLIGENT_TIERING
* GLACIER
* DEEP_ARCHIVE
* GLACIER_IR

### 참고자료

* <https://docs.aws.amazon.com/cli/latest/reference/s3/sync.html>

